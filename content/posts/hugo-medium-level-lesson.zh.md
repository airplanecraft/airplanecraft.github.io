+++
Categories = ["Technology"]
Description = "Hogo sample websites and themes"
tags = ["hugo", "themes"]
date = "2019-11-26T11:40:37+08:00"
menu = "posts"
title = "手把手使用Hugo搭建网站-中级篇"
toc = true
+++

<a id="toc_anchor" name="#12-手把手使用hugo搭建网站进阶篇"></a>

## 1.2. 手把手使用hugo搭建网站进阶篇

<a id="toc_anchor" name="#12.1-自定义菜单"></a>

### 1.2.1. 自定义菜单

- 配置定义菜单栏
   1. layouts/partials/sidebar.html
    ```
     <aside class="sidebar">
      <div class="container sidebar-sticky">
         <div class="sidebar-about">
            <a href="{{ .Site.BaseURL }}"><h1>{{ .Site.Title }}</h1></a>
            <p class="lead">
            {{ with .Site.Params.description }} {{.}} {{ else }}{{end}}
            </p>
         </div>

         <nav>
         
            <ul class="sidebar-nav">
            {{ $currentPage := . }}
               {{ range .Site.Menus.main -}}
               <li>   <a class="sidebar-nav-item{{if or ($currentPage.IsMenuCurrent "main" .) ($currentPage.HasMenuCurrent "main" .) }} active{{end}}" href="{{ .URL }}" title="{{ .Title }}">{{ .Name }}</a>
      </li>
            {{- end }}
            
            </ul>
         <ul class="sidebar-nav"><br/><br/><br/><br/><br/><br/><br/></ul>
         </nav>

         <p>{{ with .Site.Params.copyright }}{{.}}{{ else }}&copy;  xxx 公司 {{ now.Format "2006"}}.  All rights reserved. {{end}} </p>
      </div>
      </aside>
    ```
   2. site/config.toml
   ```
      baseURL = "http://clouda3.github.io/"
      languageCode = "en-us"
      title = "Clouda3"
      theme = "hyde"
      enableRobotsTXT="true"
      [permalinks]
      post = "/:filename/"

      [imaging]
      quality = 99

      [params]
      description = ""
      homeMetaContent = "clouda3 personal blog"
      footer = "clouda3的个人网站"
      date = "2019-04-10 14:05:50"
      codePenUser = "someUser"
      katex = true
      

      [menu]
      [[menu.main]]
         identifier = "home"
         name = "home"
         url = "/"
         weight = 1
      [[menu.main]]
         identifier = "blog"
         name = "Blog"
         url = "/posts/"
         weight = 2
      [[menu.main]]
         identifier = "tags"
         name = "Tags"
         url = "/tags/"
         weight = 3
      [[menu.main]]
         identifier = "about"
         name = "About"
         url = "/about"
         weight = 4
      [[menu.main]]
         identifier = "rss"
         name = "RSS"
         url = "/index.xml"
         weight = 5	
   ```
- 内容页面关联菜单
  1. content/posts 目录下面添加文章
  2. hugo new post post/abc.md
<a id="toc_anchor" name="#12.2-自定义tag/category/keyword"></a>

### 1.2.2. 自定义tag/category/keyword
- 无论是tag,category 还是keyword 都是用来分类的，在hugo里面没有太多的区别，都可以自定义,先演示页面keywords
  1. 自定义页面的keywords,xxx.md
   ```
      ---
   title: "First"
   date: 2019-11-01T16:08:13+08:00
   draft: false
   tags: ["aws"]
   keywords: ["aws","first"]
   ---

   ```
   2. layouts/partials/head.html

   ```
     {{ with .Params.keywords }}<meta name="keywords" content="{{ range $i, $e := . }}{{ if $i }} {{ end }}{{ $e }}{{ end }}">{{ end }}
   ```
- tag和categories
   1. xxx.md
   ```
      ---
      title: "First"
      date: 2019-11-01T16:08:13+08:00
      draft: false
      categories: ["亚马逊云","认证考试"]
      tags: ["aws"]
      keywords: ["aws","first"]
      ---

   ```

  2. 显示所有的tag,定义模板layouts/tags/index.html
   ```
      {{ define "main" -}}
      <ul>
         {{ $type := .Type }}
         {{$type}}
         {{ range $key, $value := .Site.Taxonomies.tags.Alphabetical }}
            {{ $name := .Name }}
            {{ $count := .Count }}
            {{ with $.Site.GetPage (printf "/%s/%s" $type $name) }}
                  <li><a href="{{ .Permalink }}">{{ $name }}</a> {{ $count }}11111111</li>
            {{ end }}
         {{ end }}
      </ul>

     {{- end }}
   ```
  3. 创建tag显示页面tags/index.md
   ```
         ---
      title: "Posts"
      date: 2019-11-01T16:10:12+08:00
      type: "tags"
      layout: "index"
      ---
   ``` 
   4. 显示每个tag关联的页面,创建模板layouts/taxonomy/tag.html
   ```
      {{ define "main" -}}
      <ul>
      {{ range .Data.Pages }}
         <li>
            <a href="{{.RelPermalink}}">{{ .Title }}</a>
         </li>
      {{ end }}
      </ul>
      {{- end }}
   ```
   5. 访问http://xxx/tags/ 
<a id="toc_anchor" name="#12.3-创建header和footer"></a>

### 1.2.3. 创建header 和footer
- 创建header 和 footer 相关模板
<a id="toc_anchor" name="#12.4-自定义边栏"></a>

### 1.2.4. 自定义边栏
- 创建sidebar 模板
<a id="toc_anchor" name="#12.5-列举最新的文章"></a>

### 1.2.5. 列举最新的文章
- 例举最新或者关联的文章
- 創建theme/layout/partial/related.html 模板
```
{{ $related := .Site.RegularPages.Related . | first 5 }}
{{ with $related }}
<h4>相關頁面</h4>
<ul>
	{{ range . }}
	<li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
	{{ end }}
</ul>
{{ end }}
```
- 在layout/_default/single.html模板中加入related.html模板的引用

```
{{< highlight go "linenos=inline,hl_lines=13,linenostart=0" >}}
{{ define "main" -}}
<div class="post">
  <h1>{{ .Title }}</h1>
  <time datetime={{ .Date.Format "2006-01-02T15:04:05Z0700" }} class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
  {{ .Content }}
</div>

{{ if .Site.DisqusShortname -}}
<h2>Comments</h2>
{{ template "_internal/disqus.html" . }}
{{- end }}

{{ partial "related.html" . }}
{{- end }}

{{< / highlight >}}

```
- 這個會讓keyword關聯的頁面顯示出來，但是時間有先後的，晚發的文章裡面有老的文章的連接，老的文章没有新的文章的链接 

<a id="toc_anchor" name="#12.6-创建列表和对应的模板"></a>

### 1.2.6. 创建列表和对应的模板
- posts/_index.md 和 posts/index.md的区别
  1. _index.md默认是列表页面（branch bundle）对应的layout是 list.html,index.md（leaf bundle）对应的layout是 single.html
- 默认对应模板就是layouts/_default/list.html 和layouts/_default/single.html
- 如果自定义模板可以{theme}/layouts/posts/list.html {theme}/layouts/posts/single.html
- 也可以直接在layouts/posts/list.html 和/layouts/posts/single.html 网站的模板优先级别高于主题模板

<a id="toc_anchor" name="#12.7-列表创建摘要"></a>

### 1.2.7. 列表创建摘要


```
   {{ define "main" -}}
      <div class="posts">
      {{ range .Site.RegularPages -}}
      <article class="post">
      <h1 class="post-title">
         <a href="{{ .Permalink }}">{{ .Title }}</a>
      </h1>
      <time datetime="{{ .Date.Format "2006-01-02T15:04:05Z0700" }}" class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
      {{ .Summary }}
      {{ if .Truncated }}
      <div class="read-more-link">
         <a href="{{ .RelPermalink }}">Read More…</a>
      </div>
      {{ end }}
      </article>
      {{- end }}
      </div>
      {{- end }}
```

<a id="toc_anchor" name="#12.8-分頁"></a>

### 1.2.8. 分頁
 1. 默認使用_internal/pagination.html 
 2. 可以自定分頁模板 layouts/partials/pagination.html

   ```
      
      {{ $paginator := .Paginate (where .Pages "Type" "posts") }}
      <ul>
      {{ range $paginator.Pages }}
         <li>
         <span><a href="{{ .Permalink }}">{{ .Title }}</a> <time class="pull-right post-list" datetime="{{ .Date.Format "2006-01-02T15:04:05Z0700" }}">{{ .Date.Format "Mon, Jan 2, 2006" }}</time></span>
      </li>
      {{- end }}

      </ul>

      {{ $pag := $.Paginator }}
      {{ if gt $pag.TotalPages 1 }}
      <ul class="pagination">
         {{ with $pag.First }}
         <li class="page-item">
            <a href="{{ .URL }}" class="page-link" aria-label="First"><span aria-hidden="true">&laquo;&laquo;</span></a>
         </li>
         {{ end }}
         <li class="page-item{{ if not $pag.HasPrev }} disabled{{ end }}">
         <a {{ if $pag.HasPrev }}href="{{ $pag.Prev.URL }}"{{ end }} class="page-link" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a>
         </li>
         {{ $ellipsed := false }}
         {{ $shouldEllipse := false }}
         {{ range $pag.Pagers }}
         {{ $right := sub .TotalPages .PageNumber }}
         {{ $showNumber := or (le .PageNumber 3) (eq $right 0) }}
         {{ $showNumber := or $showNumber (and (gt .PageNumber (sub $pag.PageNumber 2)) (lt .PageNumber (add $pag.PageNumber 2)))  }}
         {{ if $showNumber }}
            {{ $ellipsed = false }}
            {{ $shouldEllipse = false }}
         {{ else }}
            {{ $shouldEllipse = not $ellipsed }}
            {{ $ellipsed = true }}
         {{ end }}
         {{ if $showNumber }}
         <li class="page-item{{ if eq . $pag }} active{{ end }}"><a class="page-link" href="{{ .URL }}">{{ .PageNumber }}</a></li>
         {{ else if $shouldEllipse }}
         <li class="page-item disabled"><span aria-hidden="true">&nbsp;&hellip;&nbsp;</span></li>
         {{ end }}
         {{ end }}
         <li class="page-item{{ if not $pag.HasNext }} disabled{{ end }}">
         <a {{ if $pag.HasNext }}href="{{ $pag.Next.URL }}"{{ end }} class="page-link" aria-label="Next"><span aria-hidden="true">&raquo;</span></a>
         </li>
         {{ with $pag.Last }}
         <li class="page-item">
            <a href="{{ .URL }}" class="page-link" aria-label="Last"><span aria-hidden="true">&raquo;&raquo;</span></a>
         </li>
         {{ end }}
      </ul>
      {{ end }}  
   ```
   3. layouts/_default/list.html

   ```
  {{ define "main" -}}

    {{ partial "pagination.html" .  }}

  {{- end }}
   
   ```
   4. 這樣還是有些問題，按鈕豎排，改一下樣式表 static/css/hyde.css

   ```
      /** vvv Add lines below */
      ul.pagination {
         list-style-type: none;
      }

      ul.pagination > li {
         display: inline;
      }
  
   ```

<a id="toc_anchor" name="#12.9-创建统计"></a>

### 1.2.9. 创建统计
 1. 无论是baidu还是google还是chinaz都有相关的统计代码，只需要把这一段代码加入到我们的页面就可以了，以google来举例
 2. 申请google的analytics，会得到一段代码,把这段代码加入到layouts/partials/analytics.html
  ```
      <!-- Global site tag (gtag.js) - Google Analytics -->
      <script async src="https://www.googletagmanager.com/gtag/js?id=UA-xxxx"></script>
      <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());

      gtag('config', 'UA-152981067-1');
      </script>
 
  ```
 3. 把analytics.html 加入到layouts/_default/baseof.html
  ```
      {{ partial "head.html" . }}
      <body class="{{ .Site.Params.themeColor }} {{if .Site.Params.layoutReverse}}layout-reverse{{end}}">
      {{ partial "sidebar.html" . }}
         <main class="content container">
         {{ block "main" . -}}{{- end }}
         </main>

         {{ partial "google-analytics.html" . }}
      </body>
      </html>

  ```
  4. 访问网站，到google analytics后台就可以看到访问次数和其他的信息了
<a id="toc_anchor" name="#12.10-创建rss"></a>

### 1.2.10. 创建rss
- 什么是rss就是创建一个网站信息的xml文件，别人通过这个xml文件可以把你网站上的信息聚合到别的地方
- 比如你用google rss可以聚合你喜欢的各个体育网站，而不用挨个打开这些网站，让这些网站的信息统一显示在一个页面上.
- 默认hogo使用用内部rss:layouts/_internal/_default/rss.xml
  ```
      {{ printf "<?xml version=\"1.0\" encoding=\"utf-8\" standalone=\"yes\" ?>" | safeHTML }}
      <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
      <channel>
         <title>{{ if eq  .Title  .Site.Title }}{{ .Site.Title }}{{ else }}{{ with .Title }}{{.}} on {{ end }}{{ .Site.Title }}{{ end }}</title>
         <link>{{ .Permalink }}</link>
         <description>Recent content {{ if ne  .Title  .Site.Title }}{{ with .Title }}in {{.}} {{ end }}{{ end }}on {{ .Site.Title }}</description>
         <generator>Hugo -- gohugo.io</generator>{{ with .Site.LanguageCode }}
         <language>{{.}}</language>{{end}}{{ with .Site.Author.email }}
         <managingEditor>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</managingEditor>{{end}}{{ with .Site.Author.email }}
         <webMaster>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</webMaster>{{end}}{{ with .Site.Copyright }}
         <copyright>{{.}}</copyright>{{end}}{{ if not .Date.IsZero }}
         <lastBuildDate>{{ .Date.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</lastBuildDate>{{ end }}
         {{ with .OutputFormats.Get "RSS" }}
            {{ printf "<atom:link href=%q rel=\"self\" type=%q />" .Permalink .MediaType | safeHTML }}
         {{ end }}
         {{ range .Pages }}
         <item>
            <title>{{ .Title }}</title>
            <link>{{ .Permalink }}</link>
            <pubDate>{{ .Date.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</pubDate>
            {{ with .Site.Author.email }}<author>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</author>{{end}}
            <guid>{{ .Permalink }}</guid>
            <description>{{ .Summary | html }}</description>
         </item>
         {{ end }}
      </channel>
      </rss>

      ```