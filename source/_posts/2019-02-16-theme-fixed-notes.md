---
layout: post
date: 2019-02-16 02:31
title: "FreeSky主题访问量统计修复"
categories: 其它
tag:
  - Hexo
  - FreeSky
  - 主题
comment: true
---

首先说明一下，本主题源自Hexo Next主题，经过笔者长期以来的修改，源代码中很多地方都有所变动，因此衍生出一个新的花名`FreeSky`，目前版本为`FreeSky v0.4.0`，GitHub地址：[csuldw/FreeSky](https://github.com/csuldw/FreeSky)。起初博主并没有意料到会有其他人也使用这一主题，毕竟修改的地方有点多，而且略微粗糙，没有去仔细修正，适合自用。最近在浏览GitHub的时候，看到用户ooobug提了一个[issue](https://github.com/csuldw/FreeSky/issues/1)，这也让我开始怀疑，这个BUG是否已经存在很久了。

<!-- more -->

主题涉及的技术有：

- Nodejs
- Swig模板引擎
- yaml
- html
- css
- JavaScript
- 其他插件


## 访问量修复

记得06年的时候，本站的访问量已经26w+，比自己的CSDN站点的访问量略大，现在CSDN站点的访问量已经突破百万，而修复之后的站点才29w+，可见这个BUG确实早已诞生，只是因为自己没注意到，就忽略了。其实修复起来很简单，主要是`busuanzi`的`js`文件路径引用路径变化了。首先找到`Freesky\layout\_partials`目录下的`footer.swig`文件,footer.swig文件内容如下：

```
<div class="copyright" >
  {% set current = date(Date.now(), "YYYY") %}
  &copy; {% if theme.since and theme.since != current %} {{ theme.since }} - {% endif %}
  <span itemprop="copyrightYear">{{ current }}</span>
  <span class="with-love">
    <i class="icon-next-heart fa fa-heart"></i>
  </span>
  <span class="author" itemprop="copyrightHolder">{{ config.author }}</span>
</div>

<div class="powered-by">
  {{ __('footer.powered', '<a class="theme-link" href="http://hexo.io">Hexo</a>') }}
</div>

<div class="theme-info">
  {{ __('footer.theme') }} -
  <a class="theme-link" href="#">
    FreeSky{% if theme.scheme %}.{{ theme.scheme }}{% endif %}
  </a>(Reserved)

  
  <span id="busuanzi_container_site_uv">
     &nbsp; | &nbsp;  用户量: <span id="busuanzi_value_site_uv"></span>
  </span>
  <span id="busuanzi_container_site_pv">
    &nbsp; | &nbsp;  总访问量: <span id="busuanzi_value_site_pv"></span>
  </span>

  
</div>

{% block footer %}{% endblock %}
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

我们将最后一行的内容替换为下面这行就OK了。

```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

## 结束语

最后感谢GitHub用户ooobug的提示，目前本站的总访问量是`29w+`，访问用户量是`16w+`，这个统计数漏掉了很长一段时间，不过也没太大关系。在后续的时间里，还是以提高博文质量为首要目标，多总结、多输出，主题的折腾就暂且放下吧，有时间再来集中更新一版(#^.^#)！

