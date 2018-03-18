---
title: 笔记-使用Hexo搭建博客
date: 2018-03-04 11:58:16
tags: [Hexo, Blog，Learn]
categories: 笔记
---

[TOC]

@[TOC]

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1859310&auto=1&height=66"></iframe>

## 常用页面添加

- 新增一个 404 页面：`hexo new page 404`
- 新增一个 about 页面：`hexo new page about`
- 新增一个分类页面：`hexo new page categories`
- 新增一个 tag 标签云页面：`hexo new page tags`

  在source/tags/index.md编辑:

  ```bash
  ---
  title: tags
  date: 2018-03-05 11:11:11
  type: "tags"
  ---
  ```

##  插件使用

插件官网：<https://hexo.io/plugins/>

### 基本命令

- 安装插件：`npm install 插件名 --save`
- 卸载插件：`npm uninstall 插件名 --save`
- 更新插件和博客框架（需要在 hexo 目录下）：

  ```
  npm update
  ```

  - 实质是通过根目录下 package.json 文件更新各大组件

### 必备插件

- 支持RSS：`npm install hexo-generator-feed --save`

- 文章加密：`npm install hexo-blog-encrypt`

- 本地搜索支持，需要主题有对应的设置：`npm install hexo-generator-search --save`

- TOC设置`npm install hexo-renderer-markdown-it-plus`

- HTML 压缩：`npm install hexo-html-minifier --save`

- JavaScript 压缩：`npm install hexo-uglify --save`

- CSS 压缩插件：`npm install hexo-clean-css --save`

- 图片懒加载法案一：`npm install hexo-lazyload-image --save`

  __选择性__


- SEO优化：`npm install hexo-generator-seo-friendly-sitemap --save`
- 生成站点地图：`npm install hexo-generator-sitemap --save`
- 生成百度站点地图：`npm install hexo-generator-baidu-sitemap --save`
- 图片懒加载方案二：`npm install hexo-renderer-marked-lazy --save` 
  - 可能会导致无法显示外链图片

## 插件使用

###  RSS

在hexo的_config.yml添加：

```bash
#RSS 订阅插件
plugin:
- hexo-generator-feed
#RSS 插件配置
feed:
  type: rss2
  path: atom.xml #atom.xml或者rss.xml，放根目录下
  limit: 20
  hub:
  content: true
```

###  TOC设置

[参考文章](http://knowhard.com/2017/10/01/Hexo-Theme-NexT/#6-toc-%E8%AE%BE%E7%BD%AE)

- 安装hexo-renderer-markdown-it-plus

```bash
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

- 站点配置文件_config.yml

```
markdown_it_plus:
     highlight: true
     html: true
     xhtmlOut: true
     breaks: true
     langPrefix:
     linkify: true
     typographer:
     quotes: “”‘’
     pre_class: highlight
```

- 在文档任意位置输入：`@[TOC]`，即可生成目录和锚点



###  Hexo-lazyload-image，图片懒加载方案一，推荐

然后修改 _config.yml 文件

```
lazyload:
  enable: true 
  onlypost: false
  loadingImg: # eg. ./images/loading.png 
```

###  Hexo-blog-encrypt，文章加密插件

修改_config.yml,

```
# Security
encrypt:
    enable: true # 开启为true，关闭为false
    default_abstract: 这是篇受保护的文章！#默认文章描述，可选
    default_message: 请输入文章密码！#默认密码输入提示，可选
    default_template: 
                    <script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
                    <div id="security">
                        <div class="input-container">
                            <input type="password" class="form-control" id="pass" placeholder=" {{message}} " />
                            <label for="pass"> {{message}} </label>
                            <div class="bottom-line"></div>
                        </div>
                    </div>
                    <div id="encrypt-blog" style="display:none">
                        {{content}}
                    </div>
                    #默认的文章详情页密码输入提示框，可选
```

`default_template` : 这个是指在文章详情页，我们看到的输入密码阅读的模板，同理，这个也是针对所有文章的。
最后的 `content` 显示 `div` 的 `id` 必须 是 ‘encrypt-blog’，同时为了好看，也希望进行隐藏。
同时，必须要有一个 `input` 输入框，`id` 必须是`"pass"`，用来供用户输入密码。
输入密码之后，务必要有一个触发器，用来调用 ‘decryptAES’ 函数。样例中以 `button` 来触发。

- 针对某篇文章加密

  1. 在Front-Matter添加`password: 密码`即可。
  2. 或者如下：

  ```
  ---
  title: hello world
  date: 
  tags:
      - tags1
      - tags2
  password: password #此项必需，下面信息为可选
  abstract: 这是篇受保护的文章！
  message: 请输入文章密码！
  template:
          <script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
          <div id="security">
              <div class="input-container">
                  <input type="password" class="form-control" id="pass" placeholder=" {{message}} " />
                  <label for="pass"> {{message}} </label>
                  <div class="bottom-line"></div>
              </div>
          </div>
          <div id="encrypt-blog" style="display:none">
              {{content}}
          </div>
  ---
  ```

###  本地搜索，hexo-generator-search

站点_config.yml添加:

```
search:
  path: search.xml
  field: post
```

- **path** - file path. By default is `search.xml` . If the file extension is `.json`, the output format will be JSON. Otherwise XML format file will be exported.
- __field__ - the search scope you want to search, you can chose:
  - **post** (Default) - will only covers all the posts of your blog. 只索引文章(post)
  - **page** - will only covers all the pages of your blog. 只索引页面
  - **all** - will covers all the posts and pages of your blog. 全部索引




###  html5播放器aplayer

[项目地址](https://github.com/MoePlayer/hexo-tag-aplayer)


```bash
#安装
npm install hexo-tag-aplayer --save`
#需要音乐的地方插入
{% aplayer "歌曲名称" "作者" "音乐_url" "封面图片_url" "autoplay" %}
```



### hexo-admin

在hexo的_config.yml添加

```bash
  admin:
    username: myfavoritename #登录名
    password_hash: be121740bf988b2225a313fa1f107ca1 #密码对应的md5 hash值
    secret: a secret something #用于保证cookie安全
```

域名/admin，进入后台



## 其他功能实现

### 插入多媒体

- 插入视频

  - 同网易云音乐，生成iframe类型外链播放器来引用。

- 插入音乐

  - 网易云音乐网页端，生成外链播放器，将代码复制到博文任意位置

    ```
    #本文的范例
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1859310&auto=1&height=66"></iframe>

    ```

- 插入图片

  - 博文中引用外链图片 `![标题，可选](图片链接)`


### Gitment评论系统

[Hexo-Next 添加 Gitment 评论系统](https://ryanluoxu.github.io/2017/11/27/Hexo-Next-%E6%B7%BB%E5%8A%A0-Gitment-%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F/)

[Hexo博客Next主题添加Gitment评论系统坑点](https://github.com/swlfigo/Study/wiki/Hexo%E5%8D%9A%E5%AE%A2Next%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0Gitment%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F%E5%9D%91%E7%82%B9)

---

## 我的博客

__网站__
- [x] Theme: Next
- [x] Tags
- [x] Categories
- [x] RSS
- [x] 本地搜索文章
- [x] hexo-admin 后台管

__文章功能__
- [x] TOC设置
- [x] 加密模块
- [x] 图片懒加载（非必要不安装）


__其他__

- [x] SNS: Github, E-mail
- [x] 公益404
- [x] Donate
- [x] Gitment评论系统


---

## 参考链接

[使用 Github 空间搭建 Hexo 技术博客](http://www.youmeek.com/hexo/)

[Hexo-lazyload-image图片懒加载](https://www.jianshu.com/p/cb2f0a882d08)

[使用Encrypt插件给Hexo博客文章加密](https://wenzizhou.com/%E4%BD%BF%E7%94%A8Encrypt%E6%8F%92%E4%BB%B6%E7%BB%99Hexo%E5%8D%9A%E5%AE%A2%E6%96%87%E7%AB%A0%E5%8A%A0%E5%AF%86/)

[hexo之加密插件hexo-blog-encrypt](http://cocofe.cn/2016/11/13/hexo%E4%B9%8B%E5%8A%A0%E5%AF%86%E6%8F%92%E4%BB%B6hexo-blog-encrypt/)

[Hexo 主题 —— NexT 设置](http://knowhard.com/2017/10/01/Hexo-Theme-NexT/#6-toc-%E8%AE%BE%E7%BD%AE)