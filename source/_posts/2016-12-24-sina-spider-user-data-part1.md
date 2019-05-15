---
layout: post
date: 2016-12-24 16:10
title: "新浪微博数据爬取Part 1：用户个人信息"
categories: Spider
tag:
  - 爬虫
  - Python
  - Spider
  - Sina
  - 微博
comment: true
---

从上一篇博文到现在，已有一月有余，期间发生了许多事情，庆幸地是博主终于想开了，有的时候，那些无法改变的人或事，就让TA 去吧，不必多多挂怀，趁着还有时间，做些自己喜欢的事情。此前在[模拟新浪微博登录：从原理分析到实现](http://www.csuldw.com/2016/11/10/2016-11-10-simulate-sina-login/)这篇博文中讲解了如何登陆新浪微博，虽然模拟登录看似比较复杂，但将其过程理解透彻之后，你会觉得它其实也比较简单。实现了登录，接下来就是新浪数据的爬取。本文是数据爬取的第一部分，以Python实现新浪用户个人信息的爬取，其余篇章将在后续博文中陆续给出。

<!-- more -->

新浪微博数据的爬取主要有两种方法，当然也可以说博主只知道这两种方法，一种是使用新浪API获取，另一种是结合正则直接爬取页面信息。第一种方法虽然官方封装甚好，给出的数据也比较丰富，但说到底还是限制太多，很多接口只能获取当前登录用户的信息，无法获取好友的信息（你若不信，可以实践一下），所以在爬取数据的过程中干脆放弃了。本文主要介绍第二种方法，即如何结合正则爬取页面信息。


## 登录微博

首先是登录微博，博主使用的是urllib2（当然你也可以使用requests），说明一下，有关爬取的相关代码，都写在`SinaClient`这个类中，login方法如下：

```
#使用urllib2模拟登录过程
def login(self, username=None, password=None):
    self.status = False #重新将登录状态设置为False
    self.logger.info("Start to login...")
    #根据用户名和密码给默认参数赋值,并初始化post_data
    self.setAccount(username, password) 
    self.setPostData() 
    self.enableCookie() 
    #登录时请求的url
    login_url = r'https://login.sina.com.cn/sso/login.php?client=ssologin.js(v1.4.15)'
    headers = self.headers
    request = urllib2.Request(login_url, urllib.urlencode(self.post_data), headers)
    resText = urllib2.urlopen(request).read()
    try:        
        jsonText = json.loads(resText)
        if jsonText["retcode"] == "0":
            self.logger.info("Login success!")
            self.status = True
            #将cookie加入到headers中
            cookies = ';'.join([cookie.name + "=" + cookie.value for cookie in self.cookiejar])
            headers["Cookie"] = cookies
        else:
            self.logger.error("Login Failed --> " + jsonText["reason"])
    except Exception, e:
        print e
        self.logger.info("Login Failed2!")
    self.headers = headers
    return self
```

上面用到了几个方法，与先前的[模拟新浪微博登录：从原理分析到实现](http://www.csuldw.com/2016/11/10/2016-11-10-simulate-sina-login/)这篇博文的源码类似，可以参考模拟登录新浪的源码[https://github.com/csuldw/WSpider/tree/master/SinaLogin](https://github.com/csuldw/WSpider/tree/master/SinaLogin)。本文的完整源码，待后续整理完整后，也会在该github的`WSpider`仓库中给出。

登录之后，就可以进行数据爬取了。

## 获取用户个人信息

为了方便，博主将请求ULR的内容写在了openURL方法里，该方法返回的是该url链接的源码，代码如下：

```
#打开url时携带headers,此header需携带cookies
def openURL(self, url, data=None):
    req = urllib2.Request(url, data=data, headers=self.headers)
    text = urllib2.urlopen(req).read()
    return text 
```

爬取用户个人信息时，为了得到更多的信息，我们需请求多个地址，博主在爬取时访问了如下四个：

- url_app = "http://weibo.cn/%s/info" %uid
- app_page = "http://weibo.cn/%s" %uid
- url_web = "http://weibo.com/%s/info" %uid
- tag_url = "http://weibo.cn/account/privacy/tags/?uid=%s" %uid


温馨提示：`%uid`是新浪微博用户ID，若想查看四个页面的信息，将`%s`替换成用户ID即可。比如将url_app中的uid赋值为`1669282904`，则网址为[http://weibo.cn/1669282904/info](http://weibo.cn/1669282904/info)。

在上述网址中，从url_app中可以得到昵称、性别、地区、生日、简介、性取向、婚姻状况、首页链接八个字段；从app_page中可以得到用户的关注量、粉丝量、微博量；从url_web中可以获取用户的注册日期；从tag_url中则可以得到用户的标签信息。将这些信息合并到一起，加上uid，共可得14个字段。爬取过程中有的字段取值因用户没填写而造成结果不存在，为了统一字段数量，我们将这些不存在的字段统一置为空串。请求一个页面时，我们可以将页面的源码保存下来，然后使用BeautifulSoup进行解析，再结合正则找到需要的字段值。整个代码如下：

```
from bs4 import BeautifulSoup as BS
def getUserInfos(self, uid):
    url_app = "http://weibo.cn/%s/info" %uid
    text_app = self.openURL(url_app)
    soup_app = unicode(BS(text_app, "html.parser"))
    nickname = re.findall(u'\u6635\u79f0[:|\uff1a](.*?)<br', soup_app)  # 昵称
    gender = re.findall(u'\u6027\u522b[:|\uff1a](.*?)<br', soup_app)  # 性别
    address = re.findall(u'\u5730\u533a[:|\uff1a](.*?)<br', soup_app)  # 地区（包括省份和城市）
    birthday = re.findall(u'\u751f\u65e5[:|\uff1a](.*?)<br', soup_app)  # 生日
    desc = re.findall(u'\u7b80\u4ecb[:|\uff1a](.*?)<br', soup_app)  # 简介
    sexorientation = re.findall(u'\u6027\u53d6\u5411[:|\uff1a](.*?)<br', soup_app)  # 性取向
    marriage = re.findall(u'\u611f\u60c5\u72b6\u51b5[:|\uff1a](.*?)<br', soup_app)  # 婚姻状况
    homepage = re.findall(u'\u4e92\u8054\u7f51[:|\uff1a](.*?)<br', soup_app)  #首页链接
    #根据app主页获取数据
    app_page = "http://weibo.cn/%s" %uid
    text_homepage = self.openURL(app_page)
    soup_home = unicode(BS(text_homepage, "html.parser"))
    tweets_count = re.findall(u'\u5fae\u535a\[(\d+)\]', soup_home)
    follows_count = re.findall(u'\u5173\u6ce8\[(\d+)\]', soup_home)
    fans_count = re.findall(u'\u7c89\u4e1d\[(\d+)\]', soup_home)
    #根据web用户详情页获取注册日期
    url_web = "http://weibo.com/%s/info" %uid
    text_web = self.openURL(url_web)
    reg_date = re.findall(r"\d{4}-\d{2}-\d{2}", text_web)
    #根据标签详情页获取标签        
    tag_url = "http://weibo.cn/account/privacy/tags/?uid=%s" %uid
    text_tag = self.openURL(tag_url)      
    soup_tag = BS(text_tag, "html.parser")
    res = soup_tag.find_all('div', {"class":"c"})
    tags = "|".join([elem.text for elem in res[2].find_all("a")])
    
    #将用户信息合并        
    userinfo = {}
    userinfo["uid"] = uid
    userinfo["nickname"] = nickname[0] if nickname else ""
    userinfo["gender"] = gender[0] if gender else ""
    userinfo["address"] = address[0] if address else ""
    userinfo["birthday"] = birthday[0] if birthday else ""
    userinfo["desc"] = desc[0] if desc else ""
    userinfo["sex_orientation"] = sexorientation[0] if sexorientation else ""
    userinfo["marriage"] = marriage[0] if marriage else ""
    userinfo["homepage"] = homepage[0] if homepage else ""
    userinfo["tweets_count"] = tweets_count[0] if tweets_count else "0"
    userinfo["follows_count"] = follows_count[0] if follows_count else "0"
    userinfo["fans_count"] = fans_count[0] if fans_count else "0"
    userinfo["reg_date"] = reg_date[0] if reg_date else ""
    userinfo["tags"] = tags if tags else ""
    return userinfo
```

上述方法传入的只有一个用户ID`uid`，最终返回的是一个dict，也就是json串。之所以以JSON串返回是因为这会方便我们后续的数据处理，比如存储至本地或者写入到数据库中。结果字段值如下：

```
uid:用户ID
nickname:昵称
address:地址
sex:性别
birthday:生日
desc:简介
marriage:婚姻状况
follows_count:关注数
fans_count:粉丝数
tweets_count:微博数
homepage:首页链接
reg_date:注册时间
tag:标签
sex_orientation:性取向
```

## 实例

运行代码后，最终的结果将以JSON串返回，传入uid为`1669282904`的参数，返回结果如下：

![](/assets/articleImg/sina-userinfo.png)

目前只包含这14个字段，如果需要更多的信息，可去新浪微博网址中仔细查看相关字段，然后将想要的信息加入到userinfo中即可。在后续博文中，将给出用户粉丝爬取、用户关注爬取、用户微博爬取等相关信息，敬请期待！最后，预祝大家平安夜快乐，圣诞快乐吧~

## References

- [模拟新浪微博登录：从原理分析到实现](http://www.csuldw.com/2016/11/10/2016-11-10-simulate-sina-login/)
- [小试牛刀：使用Python模拟登录知乎](http://www.csuldw.com/2016/11/05/2016-11-05-simulate-zhihu-login/)