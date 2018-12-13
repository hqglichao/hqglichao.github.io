# HBlog


致谢
====================================
+ 感谢[Github](https://github.com/)提供的代码维护和发布平台
+ 感谢[Jekyll](https://jekyllrb.com/)团队做出如此优秀的产品
+ 感谢[Freud Kang](https://github.com/luoyan35714/LessOrMore)提供的样式模板，我的博客选用了此样式模版


关于统计和索引
====================================
* 统计
  *  百度统计  
     去[百度统计网站](https://tongji.baidu.com/web/welcome/login)上生成自己的百度统计ID，替换`_config.yml`中的`baidu_analysis`。
     本网站中的代码
     ```bash
     var _hmt = _hmt || [];
     (function() {
         var hm = document.createElement("script");
         hm.src = "//hm.baidu.com/hm.js?{{ site.baidu_analysis }}";
         var s = document.getElementsByTagName("script")[0]; 
         s.parentNode.insertBefore(hm, s);
     })();
     ```
* 不蒜子统计
    不蒜子统计是显示网站右上角的访问次数的。
    只需添加代码
    ```bash
    <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
    <span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
    ```

* 谷歌索引
  使用谷歌索引，有两个文件要上传。先通过`site:https://hqglichao.github.io/`判断谷歌是不是已经录入，如果没有显示你的网站，则需要到[Google Search Console](https://www.google.com/webmasters/tools/home?hl=zh-TW)把需要确认的文件上传到自己网站的根目录。例如本网站中的`google4db5de0255abbcec.html`。
  其次到[xml-sitemaps](https://www.xml-sitemaps.com/)生成自己的网站地图，上传到[Sitemap](https://www.google.com/webmasters/tools/sitemap-list?hl=zh_TW&siteUrl=https://hqglichao.github.io/#MAIN_TAB=0&CARD_TAB=-1)上。
