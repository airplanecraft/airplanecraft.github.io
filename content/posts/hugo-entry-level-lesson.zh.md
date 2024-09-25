+++
Categories = ["Technology"]
Description = "Hogo sample websites and themes"
tags = ["hugo", "themes"]
date = "2019-11-26T10:40:37+08:00"
menu = "posts"
title = "手把手使用Hugo搭建网站-初级篇"
toc = true
+++

<a id="toc_anchor" name="#11-手把手使用hugo搭建网站初级篇"></a>

## 1.1. 手把手使用hugo搭建网站初级篇

<a id="toc_anchor" name="#11.1-Demo使用hugo的网站"></a>

### 1.1.1. Demo使用hugo的网站
  1. https://www.flysnow.org/
  2. https://www.gohugo.io
  3. https://www.netlify.com/
  4. https://gohugo.io/showcase/
  5. https://www.smashingmagazine.com/

<a id="toc_anchor" name="#11.2-部分主题"></a>

### 1.1.2. 部分主题
  1. https://github.com/Vimux/Mainroad 
  2. https://github.com/kingfsen/Mainroad
  3. https://themes.gohugo.io/academic/
  4. https://themes.gohugo.io/beautifulhugo/
  5. https://themes.gohugo.io/hyde/
  6. https://themes.gohugo.io/hugo-theme-even/
  7. https://github.com/rujews/maupassant-hugo



<a id="toc_anchor" name="#11.3-hugo跟wordpress其他建站工具的对比"></a>

### 1.1.3. hugo跟wordpress其他建站工具的对比
- wordpress 全球31%的网站使用wordpress，尽管他有各种主题和插件，但是也有非常多的缺点，安全性，seo不够友好，定制麻烦
- hugo 最快的静态生成工具,seo友好，静态更安全，方便定制模板，缺点没有插件，如果要定制模板只能懂一点go的语法
- wordpress是动态的并且还需要托管数据库，所以托管费用比较昂贵
- hugo是生成静态的页面，在本地生成后上传到服务器就可以了，托管费用非常便宜，可以直接用免费的github托管
- hugo不可以在线编辑

- wordpress vs hugo 

| Tables        | 静态/动态     | 托管       | 安全性       | 访问速度     | 在线编辑 | markdown 
| ------------- |:-------------:| ---------:| -----:      | ------------:|------------:|------------:|
| wordpress     | 动态          | 复杂       | 需要经常升级 | 快           | 可以  | 不支持 |
| hugo          | 静态          | 简单       | 不需要打补丁 | 非常快       | 不可以| 支持|



<a id="toc_anchor" name="#11.4-分钟0基础使用hugo搭建个人博客网站（20m）"></a>

### 1.1.4. 分钟0基础使用hugo搭建个人博客网站（20m）
- 安装hugo

  1. 下载 https://github.com/gohugoio/hugo/releases/download/v0.58.3/hugo_extended_0.58.3_Windows-64bit.zip
  2. 解压到e:\hugo\bin
  3. 运行命令行程序cmd,cd e:\hugo\bin
  4. 执行hugo version
  5. set PAHT=%PATH%;e:\hugo\bin

- 安装git 并clone 代码
  1. 下载并安装git
- 运行hugo默认的theme
  1. cd e:\hugo\sites
  2. hugo new site example.com
  3. cd example.com\theme
  4. git clone  https://github.com/spf13/hyde.git 

- 创建个人博客：支持菜单和tag
   1. cd e:\hugo\sites\example.com
   2. 打开config.toml
      ```
      baseURL = "http://clouda3.github.io/"
      [menu]
      [[menu.main]]
          identifier = "blog"
          name = "Blog"
          url = "/post/"
      ``` 
   3. hugo new post/first.md
   4. hugo server --theme=hyde -v -D
   5. 访问http://127.0.0.1:1313



<a id="toc_anchor" name="#11.5-详解hugo使用的工具Git"></a>

### 1.1.5. 详解hugo使用的工具Git
- Git 是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。1Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
- GitHub是一个开源的代码托管平台，使用GitHub可以查看别人的项目、可以建立静态网页、可以管理插件、可以在线编译、可以托管代码等等 

   1. 注册github www.github.com 
   2. 登录:username or email account:xxx@xxx.com /password
   3. 创建repositories:create https://github.com/clouda3/test
   4. cd e:\hugo\sites\example.com,执行..\..\hugu.exe
   5. cd  e:\hugo\sites\example.com\public
   6. git init
   7. git remote add origin https://github.com/clouda3/test.git
   8. git add .
   9.  git commint -m "init commit"
   10. git push -u origin master

<a id="toc_anchor" name="#11.6-使用githubpages托管个人网站"></a>

### 1.1.6. 使用githubpages托管个人网站
 1. 上一步将网站生成的代码提交到gihub上，利用已经提交的代码托管
 2. github repository->setting ->github pages ->source :master branch
 3. 访问一下https://clouda3.github.io/test
 4. 绑定域名test.clouda3.com，我用的是亚马逊的route53.CNAME value:https://clouda3.github.io
 5. github pages->setting ->custom domain test.clouda3.com 
 6. 访问一下test.clouda3.com

<a id="toc_anchor" name="#11.7-解析域名到githubpages"></a>

### 1.1.7. 解析域名到github pages
 1. 在aws route53申请域名（或者aliyun或者腾讯云申请)
 2. 域名的cname指向 test.clouda3.com
 3. 在[github](https://clouda3.github.io/test) settings  ->github pages ->custom domain 绑定域名www.xxxx.com
 4. 访问www.xxxx.com就可以访问到网站了

<a id="toc_anchor" name="#11.8-icon/robot.txt/404"></a>

### 1.1.8. icon/robot.txt/404

 1. icon 要放再static 或者theme/static目录下
   ```
      <head>
         <link rel="icon" type="image/png" sizes="32x32" href="/images/favicon-32x32.png">
         <link rel="icon" type="image/png" sizes="16x16" href="/images/favicon-16x16.png">
      </head>
   ```
 2. 404页面生成需要模板， 在theme/layouts/404.html
   ```
   {{ partial "head" . }}
      <body>
      {{ partial "header" . }}
      <div class="body404">
         <div class="info404">
            <header id="header404" class="clearfix">
                  <div class="site-name404"><i>404</i></div>
            </header>
            <section>
                  <div class="title404">
                     <p>页面去月球了吗？回去找找看</p>
                  </div>
                  {{ partial "related" . }}
                  <a rel="nofollow" href="{{ .Site.BaseURL }}" class="index404">回首页看看</a>
            </section>
         </div>
      </div>
    {{ partial "footer" . }}
   ```
   3. robot.txt生成也需要模板layouts/robot.txt,同时config.toml：enableRobotsTXT = true，layout/robot.txt

   ```
         User-agent: Baiduspider
         Disallow: 

   ```

<a id="toc_anchor" name="#11.9-创建网站地图"></a>

### 1.1.9. 创建网站地图

  1. hugo 默认有internal sitemap 模板
  2. 如果需要定制sitemap模板可以在layouts/sitemap.xml

   ```
      {{ printf "<?xml version=\"1.0\" encoding=\"utf-8\" standalone=\"yes\" ?>" | safeHTML }}
      <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
      xmlns:xhtml="http://www.w3.org/1999/xhtml">
      {{ range .Data.Pages }}
      <url>
         <loc>{{ .Permalink }}</loc>{{ if not .Lastmod.IsZero }}
         <lastmod>{{ safeHTML ( .Lastmod.Format "2006-01-02T15:04:05-07:00" ) }}</lastmod>{{ end }}{{ with .Sitemap.ChangeFreq }}
         <changefreq>{{ . }}</changefreq>{{ end }}{{ if ge .Sitemap.Priority 0.0 }}
         <priority>{{ .Sitemap.Priority }}</priority>{{ end }}{{ if .IsTranslated }}{{ range .Translations }}
         <xhtml:link
                     rel="alternate"
                     hreflang="{{ .Lang }}"
                     href="{{ .Permalink }}"
                     />{{ end }}
         <xhtml:link
                     rel="alternate"
                     hreflang="{{ .Lang }}"
                     href="{{ .Permalink }}"
                     />{{ end }}
      </url>
      {{ end }}
      </urlset>
   ```

<a id="toc_anchor" name="#11.10-查看hugo生成静态页面的结构###"></a>

### 1.1.10. 查看hugo生成静态页面的结构 ###
