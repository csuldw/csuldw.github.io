---
layout: post
date: 2016-12-24 19:10
title: "新浪微博数据爬取Part 2：好友关系与用户微博"
categories: Spider
tag:
  - Python
  - Spider
  - Sina
  - 微博
comment: true
---

上一篇博文[新浪微博数据爬取Part 1：用户个人信息](http://www.csuldw.com/2016/12/24/2016-12-24-sina-spider-user-data-part1/)介绍了如何爬取用户个人资料，使用了BeautifulSoup以及正则表达式，最后得到了与用户有关的14个字段。在这篇文章里，将继续介绍如何爬取微博数据，爬取的内容包括用户的粉丝、用户的关注者、用户个人发表的微博三部分信息。博主知道，没有完整的项目源码支持，对于初学者来说，确实是一件不容易的事。所以，笔者打算先将每个部分的代码单独抽取出来，等到下一篇附上源码的同时，介绍如何运行整个爬虫项目。

<!-- more -->

本文继续使用第二种方法进行数据爬取，与登录有关的信息,此处就不重复介绍了，读者可参考前面的文章[模拟新浪微博登录：从原理分析到实现](http://www.csuldw.com/2016/11/10/2016-11-10-simulate-sina-login/) AND [新浪微博数据爬取Part 1：用户个人信息](http://www.csuldw.com/2016/12/24/2016-12-24-sina-spider-user-data-part1/)。

说明一下，以下所有方法都是写在SinaClient类中，关于SinaClient类所有方法，本文不会细说，不过会在后续文章中给出，还请见谅。

## 获取用户关注者

登录微博之后，可以使用浏览器观察某个用户关注者的url链接，最后你会发现，用户`1669282904`的关注者url为[http://weibo.cn/1669282904/follow?page=1](http://weibo.cn/1669282904/follow?page=1)，后面的`page=1`为参数，所以我们需要做的就是获取所有page的源码，并将关注着的uid取出来，然后合并到一起。整个代码如下：

```
#爬取单个用户的follow，ulr = http://weibo.cn/%uid/follow?page=1
def getUserFollows(self, uid, params="page=1"):
    time.sleep(2)   
    url = "http://weibo.cn/%s/follow?%s" %(uid, params)
    self.logger.info("page is: " + url)
    text = self.openURL(url)
    soup = BS(text, "html.parser")
    res = soup.find_all('table')
    reg_uid = r"uid=(\d+)&" #匹配uid
    follows = {"uid": uid, "follow_ids": list(set([y for x in [re.findall(reg_uid, str(elem)) for elem in res] for y in x ]))}
    next_url = re.findall('<div><a href="(.*?)">下页</a>&nbsp', text) #匹配"下页"内容
    if len(next_url) != 0:
        url_params = next_url[0].split("?")[-1] 
        follows['follow_ids'].extend(self.getUserFollows(uid, params=url_params)["follow_ids"]) #将结果集合并
    return follows
```

温馨提示：在获取用户关注者时，有的用户因不明原因被sina确定为广告用户，所以不会显示出来，因此在得到的follows列表里，uid的总数会有所缩减。最后返回一个JSON字符串，包括uid、follow_ids两个字段，即：

```
uid:用户ID
follow_ids:关注人ID列表
```

## 获取用户粉丝

同理，可以使用浏览器观察某个用户粉丝的url链接，以用户`1669282904`为例，粉丝的url链接为[http://weibo.cn/1669282904/fans?page=1](http://weibo.cn/1669282904/fans?page=1)，后面的`page=1`为参数，所以我们需要做的也是获取所有page的源码，并将用户粉丝的uid取出，最后合并到一起。整个代码如下：


```
#获取用户粉丝对象UID列表 ulr = http://weibo.cn/%uid/fans?page=1
def getUserFans(self, uid, params="page=1"):
    time.sleep(2)
    url = "http://weibo.cn/%s/fans?%s" %(uid, params)
    self.logger.info("page is: " + url)
    text = self.openURL(url)
    soup = BS(text, "html.parser")
    res = soup.find_all('table')
    reg_uid = r"uid=(\d+)&" #正则匹配uid
    fans = {"uid": uid, "fans_ids": list(set([y for x in [re.findall(reg_uid, str(elem)) for elem in res] for y in x ]))}
    next_url = re.findall('<div><a href="(.*?)">下页</a>&nbsp', text) #匹配"下页"内容
    if len(next_url) != 0:
        url_params = next_url[0].split("?")[-1]
        fans['fans_ids'].extend(self.getUserFans(uid, params=url_params)["fans_ids"]) #将结果集合并
    return fans
```

温馨提示：在获取用户粉丝时，有的用户同样因不明原因被sina确定为广告用户，所以不会显示出来，因此在得到的fans列表里，uid的总数会也有所缩减。最后返回一个JSON字符串，包括uid、fans_ids两个字段,即：

```
uid:用户ID
fans_ids:粉丝列表
```

## 获取个人发表微博

为了获取用户发表的微博，我们同样需要知道显示微博的url，在这里我们使用的网址是[http://weibo.cn/](http://weibo.cn/)，该url内容相对而言易于爬取。值得一提的是，爬取时需要注意的细节比较多，尤其是用户发微博的时间，需要仔细斟酌。下面是获取用户tweets的源码，每个tweets结果包括用户ID、创建时间、评论数、转发数、内容、发表来源、转发理由、点赞量、tweets类型（转发、原创）等字段。完整的代码如下：

```
"""
@func: 获取用户的发的微博信息
"""
def getUserTweets(self, uid, tweets_all, params="page=1"):
    self.switchUserAccount(myconf.userlist)
    url = r"http://weibo.cn/%s/profile?%s" %(uid, params)
    self.logger.info("URL path is: " + url)
    text = self.openURL(url)
    soup = BS(text, "html.parser")
    res = soup.find_all("div", {"class":"c"})
    #规则：如果div中子div数量为1，则为一个原厂文本说说；数量为2，则根据cmt判断是原创图文还是转发文本说说；数量为3，则为转发图文
    tweets_list = []
    for elem in res:
        tweets = {}
        unicode_text = unicode(elem)
        sub_divs = elem.find_all("div")
        today = time.strftime('%Y-%m-%d',time.localtime(time.time()))
        if len(sub_divs) in [1, 2, 3]:
            tweets["uid"] = uid
            tweets["reason"] = "null"
            #tweets["content"] = re.findall("<span class=\"ctt\">(.*?)</span>", str(elem.find("span", {"class": "ctt"})))[0]
            tweets["content"] = elem.find("span", {"class": "ctt"}).text
            soup_text = elem.find("span", {"class": "ct"}).text
            created_at = re.findall("\d\d\d\d-\d\d-\d\d \d\d:\d\d:\d\d", unicode(soup_text))
            post_time = re.findall("\d\d:\d\d", unicode(soup_text))
            split_text = unicode(soup_text).split(u"\u5206\u949f\u524d")
            if not created_at:
                created_at = re.findall(u"\d\d\u6708\d\d\u65e5 \d\d:\d\d", unicode(soup_text))
                tweets["created_at"] = time.strftime("%Y-",time.localtime()) + unicode(created_at[0]).replace(u"\u6708", "-").replace(u"\u65e5", "") + ":00"
                tweets["source"] = soup_text.split(created_at[0])[-1].strip(u"\u00a0\u6765\u81ea")
            elif created_at:
                tweets["created_at"] = unicode(created_at[0]).replace(u"\u6708", "-").replace(u"\u65e5", "")
                tweets["source"] = soup_text.split(created_at[0])[-1].strip(u"\u00a0\u6765\u81ea")
            elif post_time:
                tweets["created_at"] = today + " " + post_time[0] + ":00"
                tweets["source"] = soup_text.split(post_time[0])[-1].strip(u"\u00a0\u6765\u81ea")
            elif len(split_text) == 2:
                tweets["created_at"] = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time() - int(split_text[0])*60))
                tweets["source"] = split_text[-1].strip(u"\u00a0\u6765\u81ea")
            tweets["like_count"] = re.findall(u'\u8d5e\[(\d+)\]', unicode_text)[-1]
            tweets["repost_count"] = re.findall(u'\u8f6c\u53d1\[(\d+)\]', unicode_text)[-1]
            tweets["comment_count"] = re.findall( u'\u8bc4\u8bba\[(\d+)\]', unicode_text)[-1]
        if len(sub_divs) == 0:
            pass
        elif len(sub_divs) == 1:
            #self.logger.info("text")
            tweets["type"] = "original_text"
        elif len(sub_divs) == 2:
            #self.logger.info("image")
            tweets["type"] = "original_image"
            #根据cmt的存在判断是否为转发的文字和原创的图文说说
            cmt = elem.find_all("span", {"class": "cmt"})
            if cmt: 
                tweets["type"] = "repost_text"
                tweets["reason"] = re.findall("</span>(.*?)<a", str(sub_divs[1]))[0]
        elif len(sub_divs) == 3:
            #self.logger.info("repost")
            tweets["type"] = "repost_image"
            tweets["reason"] = re.findall("</span>(.*?)<a", str(sub_divs[2]))[0]
        else:
            self.logger.error("parse error")
            pass
        if tweets:
            tweets_list.append(json.dumps(tweets))
    self.output("\n".join(tweets_list), "output/" + uid + "/" + uid + "_tweets_" + params.replace("=", "") + ".json")
    
    next_url = re.findall('<div><a href="(.*?)">下页</a>&nbsp', text) #匹配"下页"内容
    if len(next_url) != 0:
        url_params = next_url[0].split("?")[-1]
        tweets_all.extend(tweets_list)
        self.getUserTweets(uid, tweets_all, params=url_params)
    return tweets_list
```

getUserTweets 方法传入三个参数，外部调用时只需传入前两个参数即可，一个是uid，一个是类型为list的tweets_list。最后返回就是该list，每个元素都是一个JSON字符串，字段如下：


```
uid：用户ID
content：微博内容
created_at：发表时间
source：发布工具/平台
comment_count：评论数
repost_count：转载数
type：微博类型（原创/转发）
like_count：点赞量
reason：转发理由（原创博文无理由取值为空）
```


## 结果

根据上面的代码运行可得到如下样例结果：

**1.getUserFollows**

```
{"follow_ids": ["6049590367", "2239044693", "1974808274", "1674175865"], "uid": "1669282904"}
```

**2.getUserFans**

```
{"fans_ids": ["5883245860", "3104658267"], "uid": "1669282904"}
```

**3.getUserTweets**

```
{"uid": "1669282904", "created_at": "2016-12-16 11:42:00", "comment_count": "0", "repost_count": "0", "content": "#\u5317\u4eac\u7a81\u53d1#\u3010\u96fe\u973e\u8fdb\u57ce\u4e86\u2026[\u6cea]\u3011@\u6c14\u8c61\u5317\u4eac \uff1a\u96fe\u973e\u5df2\u5230\u5317\u4eac\uff0c\u6b64\u523b\u80fd\u89c1\u5ea6\u5357\u5317\u5dee\u8ddd\u5f88\u5927\uff0c\u5357\u90e8\u6c61\u67d3\u7269\u6d53\u8f83\u9ad8\uff0c\u901a\u5dde\u3001\u95e8\u5934\u6c9f\u548c\u6d77\u6dc0\u8fd8\u80fd\u770b\u89c1\u84dd\u5929\uff0c\u4f46\u5927\u5174\u90e8\u5206\u5730\u533a\u80fd\u89c1\u5ea6\u5df2\u4e0d\u8db32\u516c\u91cc\u3002\u3002\u3002\u5927\u5bb6\u4e00\u5b9a\u8bb0\u5f97\u6234\u53e3\u7f69\uff0c\u8fd9\u51e0\u5929\u4e5f\u5c3d\u91cf\u4e0d\u8981\u5f00\u7a97\u901a\u98ce\u3002", "source": "iPhone 6s Plus", "reason": "\u6709\u79cd\u707e\u96be\u7247\u7684\u8d76\u811a[\u611f\u5192] ", "like_count": "0", "type": "repost_image"}
{"uid": "1669282904", "created_at": "2016-12-01 21:03:00", "comment_count": "3", "repost_count": "0", "content": "\u679c\u7136\u6211\u5927\u6570\u636e\u4e07\u80fd\u7684\uff0c\u505a\u5b8cweb\u505aapp\uff0c\u505a\u5b8capp\u505a\u5e7f\u544a[\u4e8c\u54c8][\u4e8c\u54c8]", "source": "iPhone 6s Plus", "reason": "null", "like_count": "1", "type": "original_text"}

```

匆忙之际赶出本文，篇幅有些简洁，代码部分只给出了核心内容，如有任何问题，可在下面留言。对于微博数据爬取的全部内容，将在下一篇文章中介绍，敬请期待！

## References

- [新浪微博数据爬取Part 1：用户个人信息](http://www.csuldw.com/2016/12/24/2016-12-24-sina-spider-user-data-part1/)
- [模拟新浪微博登录：从原理分析到实现](http://www.csuldw.com/2016/11/10/2016-11-10-simulate-sina-login/)
- [小试牛刀：使用Python模拟登录知乎](http://www.csuldw.com/2016/11/05/2016-11-05-simulate-zhihu-login/)