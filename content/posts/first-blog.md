---
title: 純文字講解如何開個blog
date: 2025-01-22T16:22:23+08:00
draft: false
---

> 前言 
> 為何要用HUGO：
> 因為我先學這個，用HEXO之類的也都可以 
> 用wordpress不好嗎：
> 服務器搞起來更麻煩，免費的GITHUB更方便

⚠️ Warning 
本教程給電腦小白、windows用戶，若有錯處那是因為本人也是個小白，程式一點都不懂

## 下載

萬事開頭都要先下載 
[HUGO官網](https://gohugo.io/installation/windows/) 有給如何下載的詳細步驟

### 命令行的程式下載器-chocolatey

首先我們先下載 [chocolatey](https://chocolatey.org/) 
用系統管理員權限打開PowerShell 

> PS：在命令介面直接點右鍵就能貼上命令行

```gdscript3
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 用 Chocolatey 安裝 HUGO 和 GIT

用系統管理員權限打開CMD 

```fallback
# 下載HUGO 和 GIT命令行
choco install hugo -y
choco install git -y
```

基本上你現在打開CMD 
打 `hugo` `git` 就能有命令行的反應了 
如果沒有你要去環境變數把他們加進去 

## HUGO建站

1. 找一個你想要寫部落格的資料夾上方地址位置打開CMD
2. 建立你部落格名稱的資料夾
	```fallback
	hugo new site mysite
	# mysite是我資料夾的名稱
	```
	這樣你就能看到一個叫mysite、裡面一堆配置文件的資料夾產生了
3. 選一個主題，我這裡選 [archie](https://github.com/athul/archie) 
	進入網站目錄
	```fallback
	cd mysite
	```
	安裝主題
	```fallback
	git clone https://github.com/athul/archie.git themes/archie
	```
4. 現在別用命令行了，直接去你mysite底下的資料夾裡面，會有一個叫 `config.toml` 的文件 用記事本或什麼其他的編輯器打開來，你會看到裡面幾行而已
	```fallback
	baseURL = 'https://example.org/'
	languageCode = 'en-us'
	title = 'My New Hugo Site'
	```
	你可以直接去主題的readme或是你下載的theme路徑裡 `mysite\themes\archie\theme.toml` 看看他有沒有推薦的配置選項，直接用他的預設也可以 ex：
	```fallback
	baseURL = "https://athul.github.io/archie/"
	 languageCode = "zh-tw"
	 title = "我的網站"
	 theme="archie"
	 copyright = "© Athul"
	 # Code Highlight
	 pygmentsstyle = "monokai"
	 pygmentscodefences = true
	 pygmentscodefencesguesssyntax = true
	 disqusShortname = "yourDisqusShortname"
	 paginate=3 # articles per page
	 [params]
	   mode="auto" # color-mode → light,dark,toggle or auto
	   useCDN=false # don't use CDNs for fonts and icons, instead serve them locally.
	   subtitle = "Minimal and Clean [blog theme for Hugo](https://github.com/athul/archie)"
	   mathjax = true # enable MathJax support
	   katex = true # enable KaTeX support
	 # Social Tags
	 [[params.social]]
	 name = "GitHub"
	 icon = "github"
	 url = "https://github.com/athul/archie"
	 [[params.social]]
	 name = "Twitter"
	 icon = "twitter"
	 url = "https://twitter.com/athulcajay/"
	 [[params.social]]
	 name = "GitLab"
	 icon = "gitlab"
	 url = "https://gitlab.com/athul/"
	 # Main menu Items
	 [[menu.main]]
	 name = "Home"
	 url = "/"
	 weight = 1
	 [[menu.main]]
	 name = "All posts"
	 url = "/posts"
	 weight = 2
	 [[menu.main]]
	 name = "About"
	 url = "/about"
	 weight = 3
	 [[menu.main]]
	 name = "Tags"
	 url = "/tags"
	 weight = 4
	```
5. 新增一個文章 首先你能在 `\mysite\archetypes\default.md` 裡面看到下面的樣式
	```fallback
	+++
	date = "{{ .Date }}"
	draft = true
	title = "{{ replace .File.ContentBaseName '-' ' ' | title }}"
	+++
	```
	這是之後在你markdown裡面會自動生成的表頭，格式是toml， 它會記錄你這文件建立的時間、標題、要不要發布、還有它的其他種種狀態 我會建議你換成下面的yaml格式
	```fallback
	---
	date: '{{ .Date }}'
	draft: false
	title: '{{ replace .File.ContentBaseName "-" " " | title }}'
	---
	現在你可以去CMD新建一個文章了
	```
	> first-post可以自訂義名稱
	```fallback
	hugo new posts/first-post.md
	```
6. 看看你的網站如何
	```fallback
	hugo server
	```
	去 [http://localhost:1313/](http://localhost:1313/) 看看

## 上傳到Github

### 新建一個Repositories

創建一個公開的Repositories，我叫它blog 
然後他就會給你一個空的倉庫，記住這個倉庫的地址大概如下： `https://github.com/@username/blog.git`

> @username是你自己的帳號名

現在回到你的mysite資料夾，打開CMD準備用GIT上傳 
這時候你的GIT會要你登入，為求偷懶我是用瀏覽器登入 
但未來你要方便登入還是要配置ssh來方便登入

```fallback
git init
git remote add origin https://github.com/@username/blog.git
```

然後 
重點來了

git不能直接處理hugo的檔案變成網站，你要做的事是把 `mysite\public\` 這個資料夾裡用 `hugo server` 生成的html上傳到github上面

```fallback
git checkout --orphan gh-pages # 創建一个空的 gh-pages 分支
git --work-tree=public add --all
git --work-tree=public commit -m "上傳到 GitHub Pages"
git push -u origin gh-pages
```