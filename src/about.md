# 关于 Hugo



读完此篇，你将对 Hugo 有一个基础的了解。



[TOC]

## Hugo 和 GDPR（Hugo 和通用数据保护条例）



这节主要介绍了 Hugo 为了 GDPR 的相关内容，从 0.41 版本开始 Hugo 提供了一个涵盖相关内置模板的隐私配置（Privacy Config）以及隐私配置的一些详细解释。



> [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation)：（General Data Protection Regulation）通用数据保护条例是欧盟法律中针对欧盟和欧洲经济区内所有个人的数据保护和隐私权的条例。



* 以下是所有隐私设置及其默认值（默认不可用），放在网站配置文件 `config.toml` 中。

* 配置文件可以有三种格式：`.yaml`、`.json`、 `.toml`。

* 现在不须要急着了解每一个配置项的具体作用 ✔️ 。（马上了解 [The Privacy Settings Explained](https://gohugo.io/about/hugo-and-gdpr/#the-privacy-settings-explained) ❌）

  

```yaml
privacy:
  disqus:
    disable: false
  googleAnalytics:
    anonymizeIP: false
    disable: false
    respectDoNotTrack: false
    useSessionStorage: false
  instagram:
    disable: false
    simple: false
  twitter:
    disable: false
    enableDNT: false
    simple: false
  vimeo:
    disable: false
    simple: false
  youtube:
    disable: false
    privacyEnhanced: false
```



## Hugo 0.32 HOWTO（Hugo 0.32 指南）



这一节介绍了关于页面打包，图像处理等。



> 这一节内容本来是属于其它章节的，但它被提前了，因此重点看一下 “组织内容”，其它稍微过一遍即可。
>
> 这一节提供了一个 [demo project](https://github.com/bep/hugotest) 。（[预览](https://temp.bep.is/hugotest/)）



### 页面资源 - 组织内容



* 下面是一个具有图像资源的页面

  

![](https://d33wubrfki0l68.cloudfront.net/4c06428897df426b60d300c8f6de175b37d7fdde/637cb/images/hugo-content-bundles.png)



上方的 content 文件夹展示了内容页面（md，即 markdown 文件）和图像资源的混合体。



> 你可以使用任何文件类型作为内容资源，只要它是 Hugo 可以识别的 MIME 类型（例如，json 文件就可以正常工作）。你也可以定义[自己的媒体类型](https://gohugo.io/templates/output-formats/#media-types)。



* 从上到下解释用红色标记的三个页面捆绑包：
  1. 有一个图像资源的主页（`1-logo.png`）
  2. 有两个图像资源，两个页面资源（`content1.md`、`content2.md`）的部分（section）。注意 `_index.md` 表示此部分（section）的 URL。
  3. 一篇文章（hugo-is-cool），其中包含一个包含一些图像和一个内容资源的文件夹（cats-info.md）。注意，`index.md` 表示本文的 URL。

* `blog/posts` 下的内容只是常规的独立页面。



> 这里你可能有疑问，你可以按照上面的目录构建页面来查看效果。
>
> `index.md` 与 `_index.md ` 有什么作用？你可以在目录中加入这两个文件来查看它们有什么不同。



> 注意，对 `content` 文件夹中任何资源的更改将在监视（又名服务器或实时重载模式）下运行时触发重新加载，甚至可以与--navigateToChanged一起使用。



* 排序

  * 页面根据标准的 Hugo 页面排序规则进行排序。
  * 图片和其他资源按字典顺序排序。

  

### 页面资源 - 处理模板中的页面资源 



* 列出所有资源

  

```html
{{ range .Resources }}
  <li><a href="{{ .RelPermalink }}">{{ .ResourceType | title }}</a></li>
  <li><a href="{{ .Permalink }}">{{ .ResourceType | title }}</a></li>
{{ end }}

<!-- 下面是在页面上的实际渲染结果 -->
<li><a href="/dir1/dir2/dog.jpg">Image</a></li>
<li><a href="http://localhost:1313/dir1/dir2/dog.jpg">Image</a></li>
```



对于绝对 URL，请使用 `.Permalink`

注意：permalink（永久链接）将相对于内容页面，并遵守 permalink 链接设置。此外，包含的页面资源将没有`RelPermalink` 值。（这里结合实际的效果来理解就好了）



* 按类型列出所有资源



```html
{{ with .Resources.ByType "image" }}
{{ end }}
```



这里的类型是页面类型，同时也是 MIME 类型中的主要的类型，主要的 MIME 类型还有 image，json 等。



* 获取特定资源



```html
{{ $logo := .Resources.GetByPrefix "logo" }}
{{ with $logo }}
{{ end }}
```



* 包括页面资源内容



```
{{ with .Resources.ByType "page" }}
  {{ range . }}
    <h3>{{ .Title }}</h3>
    {{ .Content }}
  {{ end }}
{{ end }}
```



### 图像处理



图像资源实现了 `Resize`、`Fit` 和 `Fill`:” 方法：（调整、适合和填充）

* **Resize**：调整为给定尺寸，`{{$ logo.Resize“ 200x”}}` 将调整为200像素宽，并保留宽高比。使用 `{{$ logo.Resize“ 200x100”}}` 来控制高度和宽度。
* **Fit**：缩小图像以适合给定尺寸，例如 `{{$ logo.Fit“ 200x100”}}` 将使图像适合200像素宽和100像素高的盒子。
* **Fill**：给定尺寸调整图像并裁剪图像，例如 `{{$ logo.Fill“ 200x100”}}` 将调整大小并裁剪为宽度200像素和高度100像素。



> 由于 Go 的图片包（[image package](https://github.com/golang/go/search?q=exif&type=Issues&utf8=✓)）不支持 EXIF 数据，因此 Hugo 中的图片操作目前无法保存 EXIF 数据。将来会对此进行改进。



> 👉 原文档给出了一个图像处理的示例，点击查看 [Image Processing Examples](https://gohugo.io/about/new-in-032/#image-processing-examples)



### 图像处理 - 图像处理选项



除了可以省略高度或宽度的尺寸（例如200x100）外，Hugo 还支持一组其他图像选项：

- **锚（Anchor）**：仅与 `Fill` 有关 。这对于在哪生成缩略图很有用，例如，左上角。有效值 `Center`、`TopLeft`、`Top`、`TopRight`、`Left`、`Right`、`BottomLeft`、`Bottom`、`BottomRight`。例如：`{{ $logo.Fill "200x100 BottomLeft" }}`

- **JPEG 画质（JPEG Quality）**：仅与 JPEG 图像相关，值为 1 到 100，越高越好，默认值为75。`{{ $logo.Resize "200x q50" }}`

- **旋转（Rotate）**：将图像逆时针旋转给定角度。它将先旋转以获得正确的尺寸。`{{ $logo.Resize "200x r90" }}`。其主要用途是能够手动校正 JPEG 图像的 [EXIF方向](https://github.com/golang/go/issues/4341)。

- **重采样过滤器（Resample Filter）**：调整大小时使用的过滤器。默认值为 `Box`，这是一个适合缩小比例的简单快速的重采样滤波器。有关更多信息，请参见 https://github.com/disintegration/imaging。如果您想以质量换取更快的处理速度，可以选择进行测试。

  

### 图像处理 - 性能



处理后的图像存储在 `<project-dir>/resources` 下（可以使用 `resourceDir` 配置设置进行设置）。这个是文件夹故意放置在项目中，因为它们被建议作为项目的一部分来检查源代码管理。这些图像不是 “雨果快速” 生成，但是一旦生成，便可以重复使用。



如果你更改了图像设置（例如尺寸），删除或重命名了图像等，最终将导致未使用的图像占用空间并使项目混乱。你可一运行下面的命令：



```shell
hugo -gc
```



> GC 是 Garbage Collection (垃圾收集）的缩写。



### 配置



默认图像处理配置：

您可以使用默认的图像处理选项在 `config.toml` 中配置成像部分：



```toml
[imaging]
# Default resample filter used for resizing. Default is Box,
# a simple and fast averaging filter appropriate for downscaling.
# See https://github.com/disintegration/imaging
resampleFilter = "box"

# Default JPEG quality setting. Default is 75.
quality = 68
```



## What is Hugo（什么是雨果）



这一节介绍了 Hugo 是什么、做什么以及谁应该使用雨果。（了解即可）



Hugo 是一个用 Go 语言编写的快速、现代化的静态网站生成器，旨在让网站创建再次变得有趣。Hugo 站点可以托管在任何地方，包括 [Netlify](https://netlify.com/)，[Heroku](https://www.heroku.com/)，[GoDaddy](https://www.godaddy.com/)，[DreamHost](https://www.dreamhost.com/)，[GitHub Pages](https://pages.github.com/)，[GitLab Pages](https://about.gitlab.com/features/pages/)，[Surge](https://surge.sh/)，[Aerobatic](https://www.aerobatic.com/)，[Firebase](https://firebase.google.com/docs/hosting/)，[Google Cloud Storage](https://cloud.google.com/storage/)，[Amazon S3](https://aws.amazon.com/s3/)，[Rackspace](https://www.rackspace.com/cloud/files)，[Azure](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)和[CloudFront，](https://aws.amazon.com/cloudfront/)并且可以与 CDN 很好地协作。



用技术术语来说，Hugo 获取文件和模板的源目录，并将其用作输入来创建完整的网站。



## Hugo Features（Hugo 特色）



🐮 这一节主要介绍了 Hugo 的牛掰之处，如强大的内容管理和模板语言。



> 📌 注：下面有一些陌生的名词，如 taxonomies、front matter、shortcodes 等，将在后面的解释，本节内容大致过一遍就好。



- [极快的](https://github.com/bep/hugo-benchmark)构建时间（每页<1毫秒）⚡️
- 跨平台，可在 macOS，Linux，Windows 等平台上[轻松安装](https://gohugo.io/getting-started/installing/)
- 在开发过程中使用 [LiveReload](https://gohugo.io/getting-started/usage/) 即时渲染更改
- [强大的主题](https://gohugo.io/themes/)
- [在任何地方托管您的网站](https://gohugo.io/hosting-and-deployment/)



* 简单明了的[项目组织](https://gohugo.io/getting-started/directory-structure/)，包括页面部分
* 可自定义的 [URLs](https://gohugo.io/content-management/urls/)
* 支持可配置的分类系统（ [taxonomies](https://gohugo.io/content-management/taxonomies/)），包括类别分类和标签分类
* 通过强大的模板 [函数](https://gohugo.io/functions/) 对 [内容](https://gohugo.io/templates/) 进行排序
* 自动生成 [目录表](https://gohugo.io/content-management/toc/)
* [动态菜单 ](https://gohugo.io/templates/menus/)创建
* [漂亮的URL ](https://gohugo.io/content-management/urls/)支持
* [永久链接 ](https://gohugo.io/content-management/urls/#permalinks)模式支持
* 通过 [别名](https://gohugo.io/content-management/urls/#aliases) 重定向



- 本地 Markdown 和 Emacs Org-Mode 支持以及通过外部帮助程序提供的其他语言（请参阅 [支持的格式](https://gohugo.io/content-management/formats/)）
- 在 TOML，YAML 和 JSON 元数据支持 [front matter](https://gohugo.io/content-management/front-matter/)
- 可自定义的 [homepage](https://gohugo.io/templates/homepage/)
- 多种 [content types](https://gohugo.io/content-management/types/)
- 自动和用户定义的 [content summaries](https://gohugo.io/content-management/summaries/)
- 在 Markdown 中启用丰富内容的 [Shortcodes](https://gohugo.io/content-management/shortcodes/) 
- [“阅读分钟”](https://gohugo.io/variables/page/) 功能
- [“字数统计” ](https://gohugo.io/variables/page/) 功能



- 集成的 [Disqus](https://disqus.com/) 评论支持
- 集成的 [Google Analytics（分析）](https://google-analytics.com/) 支持
- 自动创建 [RSS](https://gohugo.io/templates/rss/)
- 支持 [Go](https://golang.org/pkg/html/template/)，[Amber ](https://github.com/eknkc/amber) 和 [Ace](https://gohugo.io/templates/alternatives/) HTML模板
- 由 [Chroma ](https://github.com/alecthomas/chroma)支持的 [语法突出显示](https://gohugo.io/tools/syntax-highlighting/)（与 Pygments 部分兼容）



## The Benefits of Static Site Generators（静态站点生成器的好处）



这一节简单介绍了一下静态站点生成器的好处，没有需要特殊了解的点。（下面是拓展知识，与文档无关）



什么是静态网站和动态网站？（下面个人理解）

* 静态网站：你访问一个网站，该网站的服务器返回响应内容，通常是 html。然后浏览器渲染 html，浏览器根据 HTML 的内容加载相应的样式、脚本、图片等文件，并渲染到网页上，这就是一个静态网站，它总是返回、展示相同的内容。
* 动态网站：服务器根据你的请求，实时计算出一个 html 以及一些处理交互的脚本，返回给浏览器，然后浏览器根据它进行相应的渲染。



## License（许可证）



📝 这一节介绍 Hugo 使用的许可证，Hugo v0.15 和更高版本是根据 [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) 许可发布的，早期版本是根据 “[Simple Public License](https://opensource.org/licenses/Simple-2.0)” 发布的。（下面内容与文档无关，可以选择性阅读）



Apache License 2.0 是一个由 Apache 软件基金会发布的自由软件许可证，它允许个人使用、商业使用、复制、分发、修改，作者免责，需要保留作者版权信息，声明更改的地方。下面是简单解释：

- Apache 协议允许使用了本协议开源的代码不必开源。
- 如果要使用基于 Apache 协议的开源代码，则必须声明你修改了哪些，并且保留原作者的信息。
- 你可以在 Apache 协议基础上添加新的协议，但不能该协议要求产生冲突。
- 你使用基于 Apache 协议的开源代码造成的后果，原作者不承担任何责任。



实际开发中如何使用 Apache 2.0 许可证？

1. 准备一份 Apache 2.0 许可证的拷贝，（可以从 [Apache 基金会](https://www.apache.org/licenses/LICENSE-2.0#apply) 拷贝）
2. 修改下面的通告声明，修改之前你需要知道：
   1. `[yyyy]` 应该被替换成年份如 2019，`[name of copyright owner] `替换成作者姓名如 Jack。
   2. 你须要把通告声明用合适的 “注释语法” 写到每一份文件中去（最好是在文件开头）。
   3. 在发布作品的根目录下准备两个文件，一个文件命名成 `LICENSE`，把准备的许可证内容拷贝进去。另一个文件命名成 `NOTICE`，用来放置通告声明的内容，以及一系列项目中用到的第三方类库的名字（最好也包括这些类库的作者）。
   4. 确认项目中的许可证是否与 Apache 2.0 许可证兼容。



```
Copyright [yyyy] [name of copyright owner]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```




