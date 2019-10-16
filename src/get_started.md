# 开始（Getting Started）



读完此篇，你将能使用 Hugo 搭建一个 Hugo 页面。



[TOC]

## 安装Hugo（Install Hugo）



🌊 原文档介绍了不同平台安装 Hugo 的各种方法，所以没什么好多说的，看原文档安装好了就行。原文档还说了一下如何更新 Hugo，这里就不污辱大家智商了。（[原文](https://gohugo.io/getting-started/installing/)、[Hugo Releases](https://github.com/gohugoio/hugo/releases)）



## 快速开始（Quick Start）



🔰 学习这节使用 Ananke 主题快速的创建一个 Hugo 网站。



> 此节入门示例使用的是 macOS 系统。有关如何在其它操作系统上安装 Hugo 的说明，请参阅 [install](https://gohugo.io/getting-started/installing)。 
>
> 建议安装 Git 来运行本教程。 [Git installed](https://git-scm.com/downloads)



### 安装 Hugo



> `Homebrew` 是一个 macOS 的软件包管理器，可以从 [brew.sh](https://brew.sh/) 安装。如果你运行的是其它操作系统，请参阅安装  [install](https://gohugo.io/getting-started/installing) 。



```shell
# 使用 Homebrew 进行安装
brew install hugo

# 验证是否安装成功安装：
hugo version
```



### 创建一个新的页面



```shell
# 下面的命令将在名为 quickstart 的文件夹中创建一个新的 Hugo 网站。
hugo new site quickstart
```



### 添加一个主题



有关要主题列表，请参见 [themes.gohugo.io](https://themes.gohugo.io/)。这里我们使用精美的 Ananke 主题。



```shell
cd quickstart

# 下载主题（这是两个简单的 git 命令，如果不懂请谷歌、百度）
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke


# 编辑 config.toml 配置文件，将主题设置为 ananke
echo 'theme = "ananke"' >> config.toml
```



### 添加内容



你可以手动的创建内容文件（内容的目录结构 `content/<CATEGORY>/<FILE>.<FORMAT>`）并在其中提供元数据。你也可以使用 `new` 命令为来快速创建（它会自动完成一些事情，例如添加标题和日期） ：



```shell
hugo new posts/my-first-post.md
```



如果需要，请编辑新创建的内容文件（就是上面命令创建的文件），它将从以下内容开始：



```markdown
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```



### 启动 Hugo 服务器



现在，启动 Hugo 服务器并启动 [草稿可用 ](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content)。



```shell
# -D 的意思就是启动草稿可用
hugo server -D
```



然后在浏览器中打开 http://localhost:1313/，进行查看。

你可以随意编辑或添加新内容，只需在浏览器中刷新即可快速查看更改（您可能需要在 Web 浏览器中强制刷新，通常使用 Ctrl+R、F12 之类的键）。



### 自定义主题



👍 你的新站点看起来已经不错了，但是在将其发布给公众之前，您需要对其进行一些调整。

打开 `config.toml` 配置文件



```toml
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```



将上面的 `title` 替换为个人的标题。另外，如果你已经准备好域名，请设置 `baseURL`。注意，运行本地开发服务器时不需要此值。

有关 ananke 主题特定的配置选项，请参见 [主题站点](https://github.com/budparr/gohugo-theme-ananke)。

有关进一步的主题自定义，请参阅自定义主题。[Customize a Theme](https://gohugo.io/themes/customizing/)



> 🔔 提示：在运行 Hugo 服务器时，对站点配置或站点中的任何其它文件进行更改，你会立即在浏览器中看到更改，即使你可能需要清除缓存。



### 构建页面



很简单，运行下面的命令



```shell
hugo
```



默认情况下，输出将位于 `./public/` 目录中（ `-d /-destination` 标志可对其进行更改，或在配置文件中设置`publishdir`）。



## 基本用法（Basic Usage）



💭 这节介绍了 Hugo 的基本用法，以及 Hugo 命令行的简单使用。（运行命令后的效果就不一一展示了，请自行运行命令并观察）



Hugo 的 CLI（Command Line Interface） 功能全面、易于使用，对命令行使用经验有限的人也一样。以下是在开发 Hugo 项目时将使用的最常用命令的描述。请阅读 [命令行参考](https://gohugo.io/commands/) 以全面了解 Hugo 的 CLI。



### 安装测试



你可以通过 `help` 命令测试是否已正确安装 Hugo：（它还会列出一些有用的使用帮助）



```shell
hugo help
```



### hugo 命令



尽管你可以通过更改 `publishDir` 字段在网站配置文件中自定义输出目录，但默认情况下你的网站将生成到`public/`目录。`hugo` 命令将站点渲染到 `public/`目录中，并准备将其部署到 Web 服务器。



```shell
hugo
```



### 草稿，未来内容和过期内容



Draft, Future, and Expired Content 



Hugo 允许你在内容的开头设置草稿，发布日期，甚至是到期日期。默认情况下，Hugo 不会发布。

1. 有 `publishdate` 值的内容
2. 有 `draft: true `状态的内容
3. 有 `expirydate` 值的内容

4. 以上三个都可以分别向 `hugo` 和 `hugo server` 添加以下标志在本地开发和部署期间被覆盖，或者在配置中分别更改它们的布尔值。（配置中没有 `--`）
   1. `--buildFuture`
   2. `--buildDrafts`
   3. `--buildExpired`



### 实时加载（LiveReload）



Hugo 内置了 [LiveReload](https://github.com/livereload/livereload-js)，不需要安装的其它软件包。开发时使用 Hugo 的一种常见方法使用 `hugo server` 命令运行服务器并观察变化。



```shell
hugo server
```



这将运行一个功能齐全的 Web 服务器，同时像下面一样的 [项目组织目录](https://gohugo.io/getting-started/directory-structure/) 中监视文件系统中的添加，删除或更改：

- `/static/*`
- `/content/*`
- `/data/*`
- `/i18n/*`
- `/layouts/*`
- `/themes/<CURRENT-THEME>/*`
- `config`



每当进行更改时，Hugo 都会同时重建站点并继续提供内容。构建完成后，LiveReload 会让浏览器以静默方式重新加载页面。大多数情况 Hugo 都构建的非常快，除非你是直接在浏览器中查看的，否则你可能不会注意到变化。这意味你无需离开文本编辑器即可查看网站的最新版本。



> Hugo 在 `</body>` 标签前将 LiveReload `<script>` 注入模板中，因此如果不存在此标签，将无法使用。



### 关闭实时加载



LiveReload 通过将 JavaScript 注入到 Hugo 生成的页面中来工作。该脚本创建了从浏览器的 Web 套接字客户端到 Hugo Web 套接字服务器的连接。它可能对开发者来说很棒。但是，一些 Hugo 用户可能会在生产中使用 hugo 服务器来立即显示更新的内容。以下方法可以轻松禁用   LiveReload：



```shell
hugo server --watch=false

# 或者
hugo server --disableLiveReload

# 或者修改配置文件 config.toml
disableLiveReload = true
```



### 部署网站



在本地开发之后，你需要执行最终的 `hugo` 命令来构建将要发布站点。然后，你将 `public/`目录复制到 Web 服务器来部署站点。由于 Hugo 会生成一个静态网站，因此你的网站可以使用任何 Web 服务器托管在任何地方。有关由 Hugo 社区提供的用于托管和自动化部署的方法，请参阅 [托管和部署](https://gohugo.io/hosting-and-deployment/)。



> ⚠️ 运行 `hugo` 命令不会删除之前生成的文件，这意味着你应该 删除`public/` 目录（或者通过标志或配置文件来指定发布目录）。如果不删除这些文件，则可能会在生成的站点中留下错误的文件（例如草稿或将来的帖子）。



### 开发与部署



Hugo 不会在生成之前删除生成的文件，一个简单的解决方法是使用不同的目录进行开发和生产。如：启动构建草稿内容的服务器（有助于编辑），可以指定其它目录，例如 `dev/` 目录：



```shell
hugo server -wDs ~/Code/hugo/docs -d dev

# 内容准备好发布后，请使用默认的 public/目录：
# 这样可以防止草稿内容意外变为可用。
hugo -s ~/Code/hugo/docs
```



## 目录结构（Directory Structure）



🌵 这节介绍使用 Hugo CLI 搭建的 Hugo 项目的目录结构，以及每一个结构的作用。



Hugo CLI 搭建了项目目录结构，然后采用该目录，并将其用作创建完整网站的输入。`hugo new site` 命令行将将创建具有以下元素的目录结构：



```text
.
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```



以下是每个目录的高级概述，并提供了指向 Hugo 文档中各个部分的链接。



### [`archetypes`](https://gohugo.io/content-management/archetypes/)（原型）



你使用 `hugo new` 创建了一个新的内容文件。默认情况下，Hugo 将创建至少具有日期、标题和 `draft= true` 的新内容文件。这样可以节省时间并为使用多种内容类型的网站提供一致性。你还可以创造属于自己具有自定义 front matter 的原型  [archetypes](https://gohugo.io/content-management/archetypes/)。



> ❗️ 意思就是默认生成的日期、标题和 `draft= true` 就叫 front matters，它是提前在 archetype 中定义好的。

 

### [`assets`](https://gohugo.io/hugo-pipes/introduction/#asset-directory)（资源）



存储所有需要由 [Hugo Pipes](https://gohugo.io/hugo-pipes/) 处理的文件。仅将使用 `.Permalink`、`.RelPermalink` 的文件发布到 `public` 目录。注意：默认情况下不会自动创建 `assets` 目录。



### [`config`](https://gohugo.io/getting-started/configuration/)（配置）



Hugo 附带了大量的 [配置指令](https://gohugo.io/getting-started/configuration/#all-variables-yaml)。这些指令以 JSON，YAML 或 TOML 文件的形式存储在 [config目录](https://gohugo.io/getting-started/configuration/#configuration-directory) 中。每个根设置对象都可以作为配置项并根据环境进行结构化。简单的项目可以使用根目录下的 `config.toml` 文件。

许多站点可能几乎不需要配置，但是 Hugo 附带了大量的配置指令，以更详尽地指导你如何构建 Hugo 网站。注意：默认情况不会自动创建创建 `config` 目录。



### [`content`](https://gohugo.io/content-management/organization/)（内容）



你网站的所有内容都将位于此目录中。目录中的每个顶级文件夹都被视为一个内容部分（[content section](https://gohugo.io/content-management/sections/)）。举例来说，如果你的网站主要有三个 sections - `blog`，`articles `和 `tutorials` 你将有三个目录`content/blog`，`content/articles`和`content/tutorials`。Hugo 使用 sections 来分配默认 [内容类型](https://gohugo.io/content-management/types/)。



### [`data`](https://gohugo.io/templates/data-templates/)（数据）



该目录用于存储生成网站时 Hugo 可以使用的配置文件。您可以用 YAML，JSON 或 TOML 格式编写这些文件。除了添加到此文件夹中的文件之外，你还可以创建从动态内容提取的 [数据模板](https://gohugo.io/templates/data-templates/)。



### [`layouts`](https://gohugo.io/templates/)（排版）



以 `.html` 文件形式存储模板，这些模板指定如何将你的内容视图呈现到静态网站中。模板包括 [list pages](https://gohugo.io/templates/list/)、[homepage](https://gohugo.io/templates/homepage/)、[taxonomy templates](https://gohugo.io/templates/taxonomy-templates/)、[partials](https://gohugo.io/templates/partials/)、 [single page templates](https://gohugo.io/templates/single-page-templates/) 等。



### [`static`](https://gohugo.io/content-management/static-files/)（静态资源）



存储所有静态内容：图像，CSS，JavaScript 等。当 Hugo 构建网站时，静态目录内的所有资源都按原样复制。使用该 `static` 文件夹的一个很好的例子是 [在Google Search Console 上验证网站所有权](https://support.google.com/analytics/answer/1142414?hl=en)，你希望 Hugo 在其中复制完整的HTML文件而不修改其内容。




> 从 Hugo 0.31 起，你可以拥有多个静态目录。

