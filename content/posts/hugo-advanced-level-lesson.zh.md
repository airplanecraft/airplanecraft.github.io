+++
Categories = ["Technology"]
Description = "Hogo sample websites and themes"
tags = ["hugo", "themes"]
date = "2019-11-26T12:40:37+08:00"
menu = "posts"
title = "手把手使用Hugo搭建网站-高级篇"
toc = true
+++


<a id="toc_anchor" name="#13-手把手使用hugo搭建网站高级篇"></a>

## 1.3. 手把手使用hugo搭建网站高级篇

<a id="toc_anchor" name="#13.1-創建页面的目錄"></a>

### 1.3.1. 創建页面的目錄

- hugo提供了目錄的支持table of content

  1. 創建目錄首先你的文章必須有標題
  2. 在layouts/_default/single.html 添加代碼 {{.TableOfContents}}

  ```
            {{ define "main" -}}
            <div class="post">
            <h1>{{ .Title }}</h1>
            <time datetime={{ .Date.Format "2006-01-02T15:04:05Z0700" }} class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
            {{.TableOfContents}}

            {{ .Content }}
            </div>

            {{ if .Site.DisqusShortname -}}
            <h2>Comments</h2>
            {{ template "_internal/disqus.html" . }}
            {{- end }}
            {{ partial "related.html" . }}
            {{- end }}

   ```

<a id="toc_anchor" name="#13.2-创建搜索"></a>

### 1.3.2. 创建搜索

- 伪站内搜索 使用google cse
   1. 网站注册然后把生成的代码放入到layout/partial/search.html
     ```
     <script async src="https://cse.google.com/cse.js?cx=xxx:xxx"></script>
     <div class="gcse-search"></div>

     ```
   2. 把search.html 加入到baseof  {{ partial "search.html" . }} .
   3. 搜索的结果必须是google收录的网页
- 站内搜索hugo-lunr
   1. hugo自从0.20.0版本已经可以支持output format了,config.toml
    ```
     [outputs]
         section = [ "HTML", "JSON"]
    ```
   2. layouts/post/list.json
    ```
    [
    {{ range $index, $value := where .Site.Pages "Type" "posts" }}
    {{ if $index }}, {{ end }}
    {
        "url": "{{ .RelPermalink }}",
        "title": "{{ .Title }}",
        "content": {{ .Content | plainify | jsonify }}
    }
    {{ end }}
    ]
    ```
    3. 上面两步配置完成后,执行hugo命令将会在posts目录下生成index.json

    4. 制作搜索页面
     ```
       
      <script src="https://unpkg.com/lunr/lunr.js"></script>
      <script type="text/javascript">

      // define globale variables
      var idx, searchInput, searchResults = null
      var documents = []

      function renderSearchResults(results){

         if (results.length > 0) {

            // show max 10 results
            if (results.length > 9){
                  results = results.slice(0,10)
            }

            // reset search results
            searchResults.innerHTML = ''

            // append results
            results.forEach(result => {
            
                  // create result item
                  var article = document.createElement('article')
                  article.innerHTML = `
                  <a href="${result.ref}"><h3 class="title">${documents[result.ref].title}</h3></a>
                  <p><a href="${result.ref}">${documents[result.ref].title}</a></p>
                  <br>
                  `
                  searchResults.appendChild(article)
            })

         // if results are empty
         } else {
            searchResults.innerHTML = '<p>No results found.</p>'
         }
      }

      function registerSearchHandler() {

         // register on input event
         searchInput.oninput = function(event) {

            // remove search results if the user empties the search input field
            if (searchInput.value == '') {
                  
                  searchResults.innerHTML = ''
            } else {
                  
                  // get input value
                  var query = event.target.value

                  // run fuzzy search
                  var results = idx.search(query + '*')

                  // render results
                  renderSearchResults(results)
            }
         }

         // set focus on search input and remove loading placeholder
         searchInput.focus()
         searchInput.placeholder = ''
      }

      window.onload = function() {

         // get dom elements
         searchInput = document.getElementById('search-input')
         searchResults = document.getElementById('search-results')

         // request and index documents
         fetch('/posts/index.json', {
            method: 'get'
         }).then(
            res => res.json()
         ).then(
            res => {

                  // index document
                  idx = lunr(function() {
                     this.ref('url')
                     this.field('title')
                     this.field('content')

                     res.forEach(function(doc) {
                        this.add(doc)
                        documents[doc.url] = {
                              'title': doc.title,
                              'content': doc.content,
                        }
                     }, this)
                  })

                  // data is loaded, next register handler
                  registerSearchHandler()
            }
         ).catch(
            err => {
                  searchResults.innerHTML = `<p>${err}</p>`
            }
         )
      }
      </script>

      <input id="search-input" type="text" placeholder="Loading..." name="search">

      <section id="search-results" class="search"></section> 
    
     ```
    5. 然后打开页面输入搜索框就可以搜多到内容


<a id="toc_anchor" name="#13.3-创建多语言支持"></a>

### 1.3.3. 创建多语言支持
- hugo支持多語言和多語言的生成
   1. 配置默認語言和menu ：config.toml
     ```
         baseURL = "http://example.org/"
         title = ""
         theme = "hyde"
         defaultContentLanguage = "en"

         [params]
         description = ""
         homeMetaContent = "clouda3 personal blog"
         footer = "clouda3的个人网站"
         date = "2019-04-10 14:05:50"
         codePenUser = "someUser"
         [languages]
         [language.en]
            languageName = "English"
            title = "English"

         [[languages.en.menu.main]]
            identifier = "home"
            title = "my home"
            name = "Home"
            url = "/"
            weight = 1

         [[languages.en.menu.main]]

            identifier = "blog"
            title = "my blog"
            name = "Blog"
            url = "/posts/"
            weight = 2

         [language.cn]
            languageName = "cn" 
            title = "中文"

         [[languages.cn.menu.main]]
            identifier = "home"
            title = "主頁"
            name = "Home"
            url = "/cn"
            weight = 7

            [[languages.cn.menu.main]]
            identifier = "blog"
            title = "博客"
            name = "博客"
            url = "/cn/posts/"
            weight = 8
     ```
     2. 創建頁面post/first.cn.md first.en.md

     3. 創建可以切換中英文的按鈕,layouts/partials/lang.html

     ```
         <nav class="LangNav">

            {{ range $.Site.Home.AllTranslations }}
                  <a href="{{ .Page.Permalink }}">{{ .Language.Lang  }}</a> 
            {{ end }}

         </nav>
     ```
     4. 修改layouts/partials/sidebar.html
     ```
          {{ partial "lang.html" . }}
     ```
     5. 生成靜態html 目錄結構,cn是在一個目錄en是more的上一層目錄
      ```
       public
          ---cn
              ---post
          ---post

      ```
      6. 模板文字的多语言问题，创建i18n/en.toml cn.toml,在
       ```
        cn.toml
       [home_title]
         other = "主頁"
       ```

      ```
        layouts/partial/related.html
      
         {{ $related := .Site.RegularPages.RelatedIndices . "tags" | first 6 }}
         {{ with $related }}
         <div class="related-content">
            <h2>Related content</h2>
            <ul class="article-gallery">
               {{ range . }}
               <span><a href="{{ .Permalink }}">{{ .Title }}</a> <time class="pull-right post-list" datetime="{{ .Date.Format "2006-01-02T15:04:05Z0700" }}">{{ .Date.Format "Mon, Jan 2, 2006" }}</time></span>
               {{ end }}
            </ul>
         {{ else }}
         <ul>{{ i18n  "nothing_related"}}</ul>
         </div>
         {{ end }}
    
       ```

<a id="toc_anchor" name="#13.4-语法高亮highlight"></a>

### 1.3.4. 语法高亮 highlight
  ```
{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
// ... code
{{< / highlight >}}

  ```
<a id="toc_anchor" name="#13.5-显示相关内容"></a>

### 1.3.5. 显示相关内容
- 默认的相关内容是keywords相关
   1. 创建layouts/partial/related.html
  ```
   {{ $related := .Site.RegularPages.Related . | first 6 }}
   {{ with $related }}
   <div class="related-content">
      <h2>Related content</h2>
      <ul class="article-gallery">
         {{ range . }}
         <span><a href="{{ .Permalink }}">{{ .Title }}</a> <time class="pull-right post-list" datetime="{{ .Date.Format "2006-01-02T15:04:05Z0700" }}">{{ .Date.Format "Mon, Jan 2, 2006" }}</time></span>
         {{ end }}
      </ul>
   {{ else }}
   <ul>. Nothing related</ul>
   </div>
   {{ end }}
  ```
   2. single.html
    ```
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
    ```
  3. 新的post里面一定要包含keywords,默认对应keywords
    ```
         ---
      title: "Second"
      date: 2019-11-01T16:14:58+08:00
      draft: false
      keywords: ["peter"]
      ---
    ```
   4. 如果你的关键字name不是keywords比如是tags，那么需要调整related.html

   ```
      {{ $related := .Site.RegularPages.RelatedIndices . "tags" | first 6 }}
      {{ with $related }}
      <div class="related-content">
         <h2>Related content</h2>
         <ul class="article-gallery">
            {{ range . }}
            <span><a href="{{ .Permalink }}">{{ .Title }}</a> <time class="pull-right post-list" datetime="{{ .Date.Format "2006-01-02T15:04:05Z0700" }}">{{ .Date.Format "Mon, Jan 2, 2006" }}</time></span>
            {{ end }}
         </ul>
      {{ else }}
      <ul>. Nothing related</ul>
      </div>
      {{ end }}
   ```
<a id="toc_anchor" name="#13.6-显示当前页面所有的tag"></a>

### 1.3.6. 显示当前页面所有的tag
 1. 创建layouts/partial/tags.html
   ```
          {{ range .Params.tags }}
            <a href="/tags/{{ . }}">
             #{{ . }} 
            </a>
        {{ end }}  
   ```
 2. 把tags.html加入single.html
   ```
         {{ define "main" -}}

         <div class="post">
         <h1>{{ .Title }}</h1>
         
         <time datetime={{ .Date.Format "2006-01-02T15:04:05Z0700" }} class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
         {{ partial "tags.html" . }}
         {{ .Content }}
         </div>

         {{ if .Site.DisqusShortname -}}
         <h2>Comments</h2>
         {{ template "_internal/disqus.html" . }}
         {{- end }}
         {{ partial "related.html" . }}

         {{- end }}

   ```
<a id="toc_anchor" name="#13.7-社交联系方式"></a>

### 1.3.7. 社交联系方式

  - 网站大多会留下社交联系方式，比如fb，twitter,大部分网站是一排图标，这些图标都是svg格式的
  1. sidebar.html
      ```
	  	  	{{- with .Site.Params.social }}
			<div id="home-social">
				{{ partialCached "social-icons.html" . }}
			</div>
		{{ end }}

      ```
   2. social-icons.html

      ```
         {{ range . -}}
         <a href="{{ .url }}" target="_blank" rel="noopener" title="{{ .name | humanize }}">{{ partial "svg.html" . }}</a>
         {{- end -}}

   
      ```
   3. config.toml

      ```
         [[params.social]]
         name = "twitter"
         url = "https://twitter.com/xxx"

      [[params.social]]
         name = "instagram"
         url = "https://instagram.com/xxx"

      [[params.social]]
         name = "github"
         url = "https://github.com/xxx"
      
      [[params.social]]
         name = "email"
         url = "https://marcusnunes.me/xxx"

      [[params.social]]
         name = "linkedin"
         url = "http://www.linkedin.com/in/xxx"  
      [[params.social]]
         name = "weibo"
         url = "http://www.weibo.com/xxx" 
     
      ```  
   4. svg.html

      ```
            {{- if (eq .name "codepen") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-codepen"><polygon points="12 2 22 8.5 22 15.5 12 22 2 15.5 2 8.5 12 2"></polygon><line x1="12" y1="22" x2="12" y2="15.5"></line><polyline points="22 8.5 12 15.5 2 8.5"></polyline><polyline points="2 15.5 12 8.5 22 15.5"></polyline><line x1="12" y1="2" x2="12" y2="8.5"></line></svg>
            {{- else if (eq .name "facebook") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-facebook"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"></path></svg>
            {{- else if (eq .name "github") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-github"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path></svg>
            {{- else if (eq .name "gitlab") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-gitlab"><path d="M22.65 14.39L12 22.13 1.35 14.39a.84.84 0 0 1-.3-.94l1.22-3.78 2.44-7.51A.42.42 0 0 1 4.82 2a.43.43 0 0 1 .58 0 .42.42 0 0 1 .11.18l2.44 7.49h8.1l2.44-7.51A.42.42 0 0 1 18.6 2a.43.43 0 0 1 .58 0 .42.42 0 0 1 .11.18l2.44 7.51L23 13.45a.84.84 0 0 1-.35.94z"></path></svg>
            {{- else if (eq .name "instagram") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-instagram"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path><line x1="17.5" y1="6.5" x2="17.5" y2="6.5"></line></svg>
            {{- else if (eq .name "linkedin") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-linkedin"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
            {{- else if (eq .name "slack") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-slack"><path d="M22.08 9C19.81 1.41 16.54-.35 9 1.92S-.35 7.46 1.92 15 7.46 24.35 15 22.08 24.35 16.54 22.08 9z"></path><line x1="12.57" y1="5.99" x2="16.15" y2="16.39"></line><line x1="7.85" y1="7.61" x2="11.43" y2="18.01"></line><line x1="16.39" y1="7.85" x2="5.99" y2="11.43"></line><line x1="18.01" y1="12.57" x2="7.61" y2="16.15"></line></svg>
            {{- else if (eq .name "telegram") -}}
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" aria-hidden="true" class="feather"><path d="m 22.05,1.577 c -0.393,-0.016 -0.784,0.08 -1.117,0.235 -0.484,0.186 -4.92,1.902 -9.41,3.64 C 9.263,6.325 7.005,7.198 5.267,7.867 3.53,8.537 2.222,9.035 2.153,9.059 c -0.46,0.16 -1.082,0.362 -1.61,0.984 -0.79581202,1.058365 0.21077405,1.964825 1.004,2.499 1.76,0.564 3.58,1.102 5.087,1.608 0.556,1.96 1.09,3.927 1.618,5.89 0.174,0.394 0.553,0.54 0.944,0.544 l -0.002,0.02 c 0,0 0.307,0.03 0.606,-0.042 0.3,-0.07 0.677,-0.244 1.02,-0.565 0.377,-0.354 1.4,-1.36 1.98,-1.928 l 4.37,3.226 0.035,0.02 c 0,0 0.484,0.34 1.192,0.388 0.354,0.024 0.82,-0.044 1.22,-0.337 0.403,-0.294 0.67,-0.767 0.795,-1.307 0.374,-1.63 2.853,-13.427 3.276,-15.38 L 23.676,4.725 C 23.972,3.625 23.863,2.617 23.18,2.02 22.838,1.723 22.444,1.593 22.05,1.576 Z"></path></svg>
            {{- else if (eq .name "twitter") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-twitter"><path d="M23 3a10.9 10.9 0 0 1-3.14 1.53 4.48 4.48 0 0 0-7.86 3v1A10.66 10.66 0 0 1 3 4s-4 9 5 13a11.64 11.64 0 0 1-7 2c9 5 20 0 20-11.5a4.5 4.5 0 0 0-.08-.83A7.72 7.72 0 0 0 23 3z"></path></svg>
            {{- else if (eq .name "youtube") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-youtube"><path d="M22.54 6.42a2.78 2.78 0 0 0-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 0 0-1.94 2A29 29 0 0 0 1 11.75a29 29 0 0 0 .46 5.33A2.78 2.78 0 0 0 3.4 19c1.72.46 8.6.46 8.6.46s6.88 0 8.6-.46a2.78 2.78 0 0 0 1.94-2 29 29 0 0 0 .46-5.25 29 29 0 0 0-.46-5.33z"></path><polygon points="9.75 15.02 15.5 11.75 9.75 8.48 9.75 15.02"></polygon></svg>
            {{- else if (eq .name "email") -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-mail"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
            {{- else -}}
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg>
            {{- end -}}

      ``` 

<a id="toc_anchor" name="#13.8-社交分享"></a>

### 1.3.8. 社交分享

   - 社交分享就是把这篇文章分享到社交网站上去,实际上很多网站表现出来的形式是一排社交网站的浮动按钮，这个浮动按钮不用自己做，有很多的第三方软件提供，比如国外addthis，国内又bshare

   - 以addthis为例,注册addthis,选择社交网站比如wechat，fb，twitter，Qzone，申请完毕后得到一行代码

   ```

         <script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-5dc9230b5e0b9007"></script>

   ```
   - 把这行代码添加到模板layouts/_default/single.html

   ```
         {{ define "main" -}}
         <div class="post">
         <h1>{{ .Title }}</h1>
         <time datetime={{ .Date.Format "2006-01-02T15:04:05Z0700" }} class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
         {{ .Content }}
         </div>
         
         <script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-xxxxxxxx"></script>

   ```

<a id="toc_anchor" name="#13.9-社交评论"></a>

### 1.3.9. 社交评论

   1. 社交评论就是利用个人的社交账号对目前所发的文章进行评论，这里也有很多第三方的公司提供组件，比如官方默认的Disqus 
   2. 要先在Disqus注册然后 install on site -> 要绑定好自己的域名
   3.  得到一段代码加入到自己的layouts/partials/disqus.html

   ```
         <div id="disqus_thread"></div>
         <script>

         /**
         *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
         *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
         /*
         var disqus_config = function () {
         this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
         this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
         };
         */
         (function() { // DON'T EDIT BELOW THIS LINE
         var d = document, s = d.createElement('script');
         s.src = 'https://www-gohugo-me.disqus.com/embed.js';
         s.setAttribute('data-timestamp', +new Date());
         (d.head || d.body).appendChild(s);
         })();
         </script>
         <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>    
     
   ```

   4. 将layouts/partials/disqus.html加入到layouts/_defaults/single.html

   ```
         {{ define "main" -}}
         <div class="post">
         <h1>{{ .Title }}</h1>
         <time datetime={{ .Date.Format "2006-01-02T15:04:05Z0700" }} class="post-date">{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
         {{ .Content }}
         </div>
         
         <script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-5dc9230b5e0b9007"></script>
         {{ partial "related.html" . }}
         {{ if .Site.DisqusShortname -}}

         <br/><br/>
         <h2>##Comments</h2>

         {{ partial "disqus.html" . }}
         {{- end }}
         {{- end }}
 
   ```

    5.  config.toml 配置 
    disqusShortname = "disqus_xxxxx"

<a id="toc_anchor" name="#13.10-gitalk评论"></a>

### 1.3.10. gitalk 评论

- Gitalk 是一个基于 Github Issue 和 Preact 开发的评论插件,巧妙的利用github issue的功能开发出来的插件，需要用户有github账户
  1. 首先要用github账户创建一个仓库(repository)
  2. 在repository的setting里面开通issues功能
  3. 在github在用户的setting里面，developer settings 创建Oauth apps 创建app得到
     需要填写homepage url 和authorization callback URL *
  ```
      Client ID
         194e36d3bbcd14e7def9
      Client Secret
         2c99c40d21596f4dc7b9f31a76467ec19c72e653
  ```
  4. 注册完毕后将下列代码加入layouts/partials/gitalk.html

  ```
   <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
      <script src="https://unpkg.com/gitalk@latest/dist/gitalk.min.js"></script> 

      <div id="gitalk-container"></div>     
      <script type="text/javascript">
         var gitalk = new Gitalk({
         // gitalk的主要参数
            clientID: `xxxx`,   //上面获取到的值
            clientSecret: `xxxxx`,//上面获取到的值
            repo: `clouda3`,  //您刚才建立仓库的名字
            owner: 'clouda3',   //你的GitHub用户名字
            admin: ['clouda3'],  //你的GitHub用户的名字
            id: 'indow.location.pathname', //id不能重复，如果重复就会把其他页面的评论引进来
            });
            gitalk.render('gitalk-container');
      </script> 
  ```
  5. 把gitalk.html 加入single.html

  ```
  	{{ partial "gitalk" . }}
  ```

