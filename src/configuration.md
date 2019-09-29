# 配置（Configuration）



🌐 此篇详细介绍了 Hugo 的配置项。



[TOC]

## 配置文件



Hugo 使用 `config.toml`、`config.yaml` 或 `config.json`（如果根目录中存在的话）作为默认网站配置文件。

你可以使用命令行 `--config` 选择使用一个或多个站点配置文件来覆盖该默认设置。如：



```shell
hugo --config debugconfig.toml
hugo --config a.toml, b.toml, c.toml
```



> 可以使用多个配置文件用逗号分隔开来。



## 配置目录



除了使用单个站点配置文件外，还可以使用 `configDir`目录（默认为 `config/`）维护组织和环境特定的设置。



```text
config
├── _default
│   ├── config.toml
│   ├── languages.toml
│   ├── menus.en.toml
│   ├── menus.zh.toml
│   └── params.toml
├── staging
│   ├── config.toml
│   └── params.toml
└── production
├── config.toml
└── params.toml
```



- 每个文件代表一个配置根对象，如`Params`，`Menus`，`Languages`等...
- 每个目录包含一组文件，这些文件包含特定的环境设置。
- 可以对文件进行本地化以使其成为特定语言的配置文件。



根据以上结构，在运行 `hugo --environment staging`命令时，Hugo 将使用来自 `config/_default` 的所有设置，并合并 `staging` 之上的这些。



> 默认开发环境使用 `hugo serve` 命令，默认生成环境使用 `hugo` 命令 。



## 所有配置项设置



以下是 Hugo 定义的变量的完整列表，及其默认值。用户可以选择在其站点配置文件中覆盖这些值。



1. **`archetypeDir: archetypes`**

   Hugo 在该目录查找原型文件（内容模板）。从 Hugo 0.56 开始，还有配置此目录的另一种方式，请参考 [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-config-mounts)。

   

2. **`assetDir: assets`**

   Hugo 使用 [Hugo Pipes](https://gohugo.io/hugo-pipes/) 在该目录查找资源文件。从 Hugo 0.56 开始，还有配置此目录的另一种方式，请参考 [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-config-mounts)。

   

3. **`baseURL`**

   根的主机名（和路径），例如 https://bep.is/。

   

4. **`blackfriday`**

   查看 [Configure Blackfriday](https://gohugo.io/getting-started/configuration/#configure-blackfriday)。（在下面）

   

5. **`buildDrafts: false`**

   构建时是否包括草稿。

   

6. **`buildExpired: false`**

   是否包含已过期的内容。

   

7. **`buildFuture: false`**

   包含带有在将来的 `publishdate` 内容。

   

8. **`caches`**

   查看 [Configure File Caches](https://gohugo.io/getting-started/configuration/#configure-file-caches)。（在下面）

   

9. **`canonifyURLs: false`**

   启用将相对 URL 变为绝对 URL。

   

10. **`contentDir: content`**

    Hugo 在该目录查找内容文件。从 Hugo 0.56 开始，还有配置此目录的另一种方式，请参考 [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-config-mounts)。

    

11. **`dataDir: data`**

    Hugo 在该目录查找数据文件。从 Hugo 0.56 开始，还有配置此目录的另一种方式，请参考 [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-config-mounts)。

    

12. **`defaultContentLanguage: en`**

    设置没有语言指示符的内容的默认为该语言。

    

13. **`defaultContentLanguageInSubdir: false`**

    在子目录中呈现内容的默认语言，例如 `content/en/`。站点根目录 `/` 将重定向到 `/en/`。

    

14. **`disableAliases: false`**

    禁止产生别名重定向。请注意，即使设置了`disableAliases`，别名本身还是保留在页面上的。这样做是为了能够使在一个 `.htaccess`，Netlify `_redirects` 文件或使用类似自定义输出格式的文件中生成 301 重定向。

    

15. **`disableHugoGeneratorInject: false`**

    默认情况下，Hugo 只会在首页的 HTML 头中插入一个 generator meta 标签。你可以关掉它，但是不建议这么做。

    

16. **`disableKinds: []`**

    禁用所有指定种类的页面。此列表允许的值：`"page"`，`"home"`，`"section"`，`"taxonomy"`，`"taxonomyTerm"`，`"RSS"`，`"sitemap"`，`"robotsTXT"`，`"404"`。

    

17. **`disableLiveReload: false`**

    禁用自动实时重新加载浏览器窗口。

    

18. **`disablePathToLower: false`**

    请勿将网址/路径转换为小写。

    

19. **`enableEmoji: false`**

    为页面内容启用表情符号支持。 查看表情 [Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet/)。

    

20. **`enableGitInfo: false`**

    为每个页面启用 `.GitInfo` 对象（如果 Hugo 网站使用 Git 进行版本控制）。它将使用该内容文件的最后 git 提交日期来更新每个页面的 `Lastmod` 参数。

    

21. **`enableInlineShortcodes`**

    启用内联  shortcode 支持，查看 [Inline Shortcodes](https://gohugo.io/templates/shortcode-templates/#inline-shortcodes)。

    

22. **`enableMissingTranslationPlaceholders: false`**

    如果缺少翻译，显示一个占位符而不是默认值或空字符串。

    

23. **`enableRobotsTXT: false`**

    启用生成 `robots.txt` 文件。

    

24. **`frontmatter`**

    查看 [Front matter Configuration](https://gohugo.io/getting-started/configuration/#configure-front-matter)。（在下面）

    

25. **`footnoteAnchorPrefix: “”`**

    脚注锚点的前缀。

    

26. **`footnoteReturnLinkContents: “”`**

    用文本显示脚注返回链接。

    

27. **`googleAnalytics: “”`**

    Google Analytics 跟踪 ID。

    

28. **`hasCJKLanguage: false`**

    如果为 `true`，则自动检测内容中的中文/日语/韩语。这将使 `.Summary` 和 `.WordCount` 对于 CJK 语言正确运行。

    

29. **`imaging`**

    查看 [Image Processing Config](https://gohugo.io/content-management/image-processing/#image-processing-config).

    

30. **`languages`**

    查看 [Configure Languages](https://gohugo.io/content-management/multilingual/#configure-languages).

    

31. **`languageCode: “”`**

    网站的语言代码。

    

32. **`languageName: “”`**

    网站的语言名称。（跟上面那个有什么区别？）

    

33. **`disableLanguages`**

    查看 [Disable a Language](https://gohugo.io/content-management/multilingual/#disable-a-language)

    

34. **`layoutDir: “layouts”`**

    Hugo 从中读取布局（模板）的目录。

    

35. **`log: false`**

    启用日志记录。

    

36. **`logFile: “”`**

    日志文件路径（如果设置，将自动启用日志记录）。

    

37. **`menu`**

    查看 [Add Non-content Entries to a Menu](https://gohugo.io/content-management/menus/#add-non-content-entries-to-a-menu)。

    

38. **`module`**

    模块配置。查看 [Module Config](https://gohugo.io/hugo-modules/configuration/)。

    

39. **`newContentEditor: “”`**

    创建新内容时要使用的编辑器。

    

40. **`noChmod: false`**

    不要同步文件的权限模式。

    

41. **`noTimes: false`**

    不要同步文件的修改时间。

    

42. **`paginate: 10`**

    分页中每页的默认元素数。查看 [pagination](https://gohugo.io/templates/pagination/)。

    

43. **`paginatePath: “page”`**

    分页期间使用的 path 元素。例如 (https://example.com/page/2)。

    

44. **`permalinks`**

    查看 [Content Management](https://gohugo.io/content-management/urls/#permalinks)。

    

45. **`pluralizeListTitles: true`**

    将列表中的标题变成多个。

    

46. **`publishDir: “public”`**

    Hugo 将在其中写入最终静态站点的目录（如 HTML 文件等）。

    

47. **`pygmentsCodeFencesGuessSyntax: false`**

    对没有指定语言的代码栏启用语法猜测。

    

48. **`pygmentsStyle: “monokai”`**

    突出显示语法的颜色主题或样式。查看 [Pygments Color Themes](https://help.farbox.com/pygments.html)。

    

49. **`pygmentsUseClasses: false`**

    启用使用外部 CSS 突出显示语法。

    

50. **`related`**

    查看 [Related Content](https://gohugo.io/content-management/related/#configure-related-content)。

    

51. **`relativeURLs: false`**

    启用此选项可使所有相对 URL 均相对于内容的根目录。注意，这不会影响绝对 URL。

    

52. **`refLinksErrorLevel: “ERROR”`**

    当使用 ref 或 relref 解析页面链接并且无法解析链接时，它将以此 logg 级别记录。有效值为 `ERROR`（默认）或`WARNING`。任何错误都会使构建失败（`exit -1`）。

    

53. **`refLinksNotFoundURL`**

    URL 用作占位符当 ref 或 relref 中找不到页面引用时，照常使用。

    

54. **`rssLimit: unlimited`**

    RSS feed 中的最大项目数。

    

55. **`sectionPagesMenu: “”`**

    查看 [“Section Menu for Lazy Bloggers”](https://gohugo.io/templates/menu-templates/#section-menu-for-lazy-bloggers)。

    

56. **`sitemap`**

    默认请查看 [sitemap configuration](https://gohugo.io/templates/sitemap-template/#configure-sitemap-xml)。

    

57. **`staticDir: “static”`**

    Hugo 在该目录或目录列表查找静态文件 [static files](https://gohugo.io/content-management/static-files/)。从 Hugo 0.56 开始，还有配置此目录的另一种方式，请参考 [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-config-mounts)。

    

58. **`summaryLength: 70`**

    在  [`Summary`](https://gohugo.io/content-management/summaries/#hugo-defined-automatic-summary-splitting) 要显示的文字的长度。

    

59. **`taxonomies`**

    查看 [Configure Taxonomies](https://gohugo.io/content-management/taxonomies#configure-taxonomies)。

    

60. **`theme: “”`**

    要使用的主题（默认位于`/themes/THEMENAME/`中）。

    

61. **`themesDir: “themes”`**

    Hugo 从中读取主题的目录。

    

62. **`timeout: 10000`**

    生成页面内容的超时时间（以毫秒为单位, 默认为 10 秒）。注意：这用于避免递归内容生成，如果你的页面生成速度较慢（例如需要大图像处理或依赖于远程内容），则可能需要提高此限制。

    

63. **`title: “”`**

    网站标题。

    

64. **`titleCaseStyle: “AP”`**

    查看 [Configure Title Case](https://gohugo.io/getting-started/configuration/#configure-title-case)

    

65. **`uglyURLs: false`**

    启用后，创建格式为 `/filename.html` 而不是 `/filename/`的 URL。

    

66. **`verbose: false`**

    ​	启用详细输出。

    

67. **`verboseLog: false`**

    ​	启用详细日志记录。

    

68. **`watch: false`**

    ​	监视文件系统是否有更改，并根据需要重新创建。



## 配置标题（titleCaseStyle）



设置 `titleCaseStyle` 以指定使用的标题样式，通过 [标题 ](https://gohugo.io/functions/title/)模板功能和 Hugo 中自动章节标题。默认使用 [AP Stylebook ](https://www.apstylebook.com/)作为标题样式，但你也可以将其设置为 `Chicago` 或 `Go`。（每个单词都以大写字母开头）。



## 配置环境变量



HUGO_NUMWORKERMULTIPLIER 可以增加或减少在 Hugo 中进行并行处理的数量。如果未设置，将使用合理的 CPU 数量。



## 配置查找顺序



与模板查找顺序（[lookup order](https://gohugo.io/templates/lookup-order/)）类似，Hugo 具有一组默认规则，用于在你网站的根目录中搜索配置文件，作为默认行为：

1. `./config.toml`

2. `./config.yaml`

3. `./config.json`

   

在 `config` 文件中，你可以指导 Hugo 关于网站的呈现方式，控制网站的菜单以及任意定义特定于项目的站点范围参数。



## 配置示例



以下是一个配置文件的典型示例。嵌套在 `params` 下的值：将填充 [`.Site.Params`](https://gohugo.io/variables/site/) 变量以在模板中使用：



```toml
baseURL = "https://yoursite.example.com/"
footnoteReturnLinkContents = "↩"
title = "My Hugo Site"

[params]
  AuthorName = "Jon Doe"
  GitHubUser = "spf13"
  ListOfFoo = ["foo1", "foo2"]
  SidebarRecentLimit = 5
  Subtitle = "Hugo is Absurdly Fast!"

[permalinks]
  posts = "/:year/:month/:title/"
```



## 使用环境变量进行配置



除了已经提到的 3 个配置选项外，还可以通过操作系统环境变量来定义配置键值。例如，以下命令将在类似 Unix 的系统上有效地设置网站的标题：



```shell
$ env HUGO_TITLE="Some Title" hugo
```



如果你使用 Netlify 之类的服务来部署站点，这将非常有用。查看Hugo docs [Netlify配置文件](https://github.com/gohugoio/hugoDocs/blob/master/netlify.toml) 作为示例。



> `HUGO_`设置操作系统环境变量时，名称必须带有前缀，并且配置键必须以大写形式设置。
>
> 要设置配置参数，请在名称前加上 `HUGO_PARAMS_`



## 渲染时的忽略文件（ignoreFiles）



`./config.toml ` 里面的以下语句将导致 Hugo 在渲染时忽略以 `.foo` 和结尾的文件 `.boo`：



```
ignoreFiles = [ "\\.foo$", "\\.boo$" ]
```



上面是一个正则表达式列表。请注意，`\` 在此示例中转义了反斜杠字符，以使 TOML 保持满意状态。



## 配置前件（Front Matter）



日期在 Hugo 中很重要，你可以配置 Hugo 如何为内容页面分配日期。你可以通过在 `config.toml` 中添加一个`frontmatter` 部分来实现。默认配置为：



```toml
[frontmatter]
date = ["date", "publishDate", "lastmod"]
lastmod = [":git", "lastmod", "date", "publishDate"]
publishDate = ["publishDate", "date"]
expiryDate = ["expiryDate"]
```



如果你的某些内容中包含非标准日期参数，则可以覆盖日期设置，如：



```toml
[frontmatter]
date = ["myDate", ":default"]
```



`:default` 是默认设置的快捷方式。以上将设置`.Date` 中的日期值为 `myDate`（如果存在），如果没有，我们将查找 `date`、`publishDate`、`lastmod` 并选择第一个有效日期。



以 `:`开头的值是具有特殊含义的日期处理程序（请参见下文）。其它只是在 front matter 配置中的日期参数的名称。（不区分大小写）还要注意的是，Hugo 有一些内置别名：`lastmod` => `modified`, `publishDate` => `pubdate`, `published` 和 `expiryDate` => `unpublishdate`。以此为例，默认情况下，将 `pubDate` 用作日期，将默认分配给 `.PublishDate`。



特殊的日期处理程序有：



- `:fileModTime`：从内容文件的上次修改时间戳中获取日期。

  

  如下面的代码将首先尝试从 `lastmod` 参数开始提取 `.Lastmod` 的值，然后再提取内容文件的修改时间戳记。最后`:default` 不需要在这里，但是 Hugo 最终会在 `:git`，`date` 和 `publishDate`中寻找一个有效的日期。

  

```toml
[frontmatter]
lastmod = ["lastmod", ":fileModTime", ":default"]
```



- `:filename`：从内容文件的文件名中获取日期。例如，`2018-02-22-mypage.md` 将提取日期 `2018-02-22`。另外，如果未设置 `slug`，则将 `mypage` 用作`.Slug` 的值。

  

  如下面的方法首先尝试从文件名中提取 `.Date` 的值，然后再查看 front matter 中的 `date`，`publishDate` 和 `lastmod`参数。

  

```toml
[frontmatter]
date = [":filename", ":default"]
```



- `:git`：这是该内容文件的最新修订版的Git作者日期。仅在设置 `--enableGitInfo` 或 `enableGitInfo = true `时才有效。



## 配置渲染引擎（Blackfriday）



[Blackfriday ](https://github.com/russross/blackfriday)是 Hugo 内置的 Markdown 渲染引擎。

Hugo 通常使用合理的默认值来配置Blackfriday，这些默认值应该相当适合大多数用例。但是，如果你对Markdown 有特殊需求，Hugo 会公开其 Blackfriday 行为选项供你修改。下表列出了这些 Hugo 选项，并与Blackfriday 的源代码（[html.go](https://github.com/russross/blackfriday/blob/master/html.go) 和 [markdown.go](https://github.com/russross/blackfriday/blob/master/markdown.go)）中的相应标志配对。



## Blackfriday 选项



> 🚧 选项后面的为默认值。
>
> *有关所有扩展的信息，请参见 Blackfriday 扩展部分。 [Blackfriday extensions](https://gohugo.io/getting-started/configuration/#blackfriday-extensions)*



- `taskLists: true`

  用途：关闭 GitHub 样式的自动任务/TODO列表生成。



- `smartypants: true`
  标记：**HTML_USE_SMARTYPANTS**
  用途： `false` 禁用智能标点符号替换，包括智能引号，智能破折号，智能分数等。如果为 `true`，则可以使用`angledQuotes`, `fractions`, `smartDashes`, 和`latexDashes`。（请参见下文）



- `smartypantsQuotesNBSP: false`
  标记：**HTML_SMARTYPANTS_QUOTES_NBSP**
  用途： `true` 启用法语风格的 Guillemets，且引号内不间断。



- `angledQuotes: false`
  标记：**HTML_SMARTYPANTS_ANGLED_QUOTES**
  用途：`true` 启用智能的双引号。示例： “Hugo” 渲染为 «Hugo» 而不是 “Hugo”。



- `fractions: true`
  标记：**HTML_SMARTYPANTS_FRACTIONS**
  用途：`false` 禁用智能分数。
  示例：`5/12` 渲染为 ^5^/~12~ (`<sup>5</sup>&frasl;<sub>12</sub>`)。
  警告：即使 `fractions = false`，Blackfriday 仍会分别将 `1/2`, `1/4`, 和`3/4` 转换为 ½ (`&frac12;`), ¼ (`&frac14;`) 和 ¾ (`&frac34;`)但只有这三个。



- `smartDashes: true`
  标记：**HTML_SMARTY_DASHES**
  用途：`false` 禁用智能破折号；即将多个连字符转换为一个破折号或全破折号。如果为 `true`，则可以使用下面的 `latexDashes` 标志修改其行为。

  

- `latexDashes: true`
  标记：**HTML_SMARTYPANTS_LATEX_DASHES**
  用途：`false` 禁用 LaTeX 的智能破折号，并选择常规的智能破折号。假设 `smartDashes` 为 `true`，`--` 转换为 – (`&ndash;`), 而 `---` 转换为 — (`&mdash;`)。但是，两个单词之间的单个连字符会翻译成一个破折号，如 “`12 June - 3 July`” 变成 `12 June &ndash; 3 July`。



- `hrefTargetBlank: false`
  标记：**HTML_HREF_TARGET_BLANK**
  用途： `true` 将在新窗口或选项卡中打开外部链接绝对链接。虽然 `target=“_ blank”` 属性通常用于外部链接，但 Blackfriday 会为所有绝对链接使用该属性（[ref](https://discourse.gohugo.io/t/internal-links-in-same-tab-external-links-in-new-tab/11048/8)）。需要注意这一点，在整个过程中使用绝对链接，对内部链接也一样（例如通过将 `canonifyURLs` 设置为 `true` 或通过 `absURL`)。



- `nofollowLinks: false`
  标记：**HTML_NOFOLLOW_LINKS**
  用途：`true` 创建外部链接，将绝对链接添加为 `nofollow` 的 `rel`属性。因此，建议爬虫不要点击链接。 虽然 `rel=“nofollow”` 属性通常用于外部链接，但 Blackfriday 会为所有绝对链接使用该属性。需要注意这一点，如果它们在整个内部都使用绝对链接，内部链接也一样（例如，通过将 `canonifyURLs` 设置为 `true` 或通过 `absURL`）。



- `noreferrerLinks: false`
  标记：**HTML_NOREFERRER_LINKS**
  用途：`true` 创建外部链接，将绝对链接添加 `noreferrer` 到它们的 `rel` 属性中。因此当跟随链接时，没有引用者信息将被泄漏。虽然该属性通常用于外部链接，但 Blackfriday 会为所有绝对链接使用该属性。需要注意这一点，如果它们在整个内部都使用绝对链接，内部链接也一样（例如，通过将 `canonifyURLs` 设置为 `true` 或通过 `absURL`）。



- `plainIDAnchors: true`
  标记：**FootnoteAnchorPrefix and HeaderIDSuffix**
  用途：`true` 表示渲染不带文档 ID 的任何标题和脚注 ID。
  示例：渲染 `#my-heading` 而不是 `#my-heading:bec3ed8ba720b970`。

  

- `extensions: []`
  用途：启用一个或多个 Blackfriday 的 Markdown 扩展程序（EXTENSION_ *）。
  示例： 在列表中包括 `hardLineBreak` 以启用 Blackfriday 的 `EXTENSION_HARD_LINE_BREAK`。



- `extensionsmask: []`
  用途：停用一个或多个 Blackfriday 的 Markdown 扩展程序（EXTENSION_ *）。
  示例：在列表中将 `autoHeaderIds` 设置为 `false` 可禁用 Blackfriday 的 `EXTENSION_AUTO_HEADER_IDS`。*



- `skipHTML: false`
  标记：**HTML_SKIP_HTML**
  用途： `true` 会导致 Markdown 文件中的所有 HTML 被跳过。



## Blackfriday 扩展



- `noIntraEmphasis`：默认启用。在讨论代码时，“ _” 字符通常在单词内使用，因此让 Markdown 将其解释为强调命令通常是错误的事情。启用后，Blackfriday 允许将所有强调标记出现在单词中时都视为普通字符。

  

- `tables`：默认启用。启用后，可以使用以下语法在输入中绘制表来创建表：示例：



```md
   Name | Age
--------|------
    Bob | 27
  Alice | 23
```



- `fencedCode`：默认启用。启用后，除了用于标记代码块的常规 4 位缩进之外，还可以显式标记它们并提供一种语言（以简化语法突出显示）。你可以使用 3 个或更多的反引号标记该块的开始，并使用相同的数字标记该块的结束。

  

  ~~~md
  ```md
  # Heading Level 1
  Some test
  ## Heading Level 2
  Some more test
  ```
  ~~~

  

- `autolink`：默认启用。启用后，未明确标记为链接的 URL 将转换为链接。

  

- `strikethrough`：默认启用。启用后，将删除用两个波浪号包裹的文本。如 `~~crossed-out~~`。

  

- `laxHtmlBlocks`：默认停用。启用后，放宽 HTML 块解析规则。

  

- `spaceHeaders`：默认启用。启用后，请严格遵守前缀头规则。

  

- `hardLineBreak`：默认停用。启用后，输入中的换行符将转换为输出中的换行符。

  

- `tabSizeEight`：默认停用。启用后，将选项卡扩展到八个空格而不是四个空格。

  

- `footnotes`：默认启用。启用后，将支持 Pandoc 样式的脚注。文本中的脚注标记将成为上标文本；脚注定义将放在文档末尾的脚注列表中。如：

  

  ```
  This is a footnote.[^1]
  
  [^1]: the footnote text.
  ```

  

- `noEmptyLineBeforeBlock`：默认停用。启用后，无需插入空行来启动（代码，引号，有序列表，无序列表）块。

  

- `headerIds`：默认启用。启用后，允许使用 `{#id}` 指定标头  ID。

  

- `titleblock`：默认停用。启用后，支持 Pandoc 样式的标题栏。 [Pandoc-style title blocks](http://pandoc.org/MANUAL.html#extension-pandoc_title_block)。

  

- `autoHeaderIds`：默认启用。启用后，将根据标题文本自动创建标题 ID。

  

- `backslashLineBreak`：默认启用。启用后，将尾部反斜杠转换为换行符。

  

- `definitionLists`：默认启用。启用后，一个简单的定义列表由一个单行术语，一个冒号和该术语的定义组成。术语必须与以前的定义用空白行分隔。

  

  ```
  Cat
  : Fluffy animal everyone likes
  
  Internet
  : Vector of transmission for pictures of cats
  ```

  

- `joinLines`：默认启用。当启用后，删除换行符并加入行。

  

> 1. 从 Hugo v0.15 开始，Blackfriday 标志区分大小写。
> 2. Blackfriday 标志必须分组在 `blackfriday ` 键下，并且可以在网站级别和页面级别上进行设置。页面上的任何设置都将覆盖其各自的站点设置。



```toml
[blackfriday]
  angledQuotes = true
  extensions = ["hardLineBreak"]
  fractions = false
  plainIDAnchors = true
```



## 配置其它输出格式



Hugo v0.20 引入了将内容呈现为多种输出格式（例如，JSON，AMP html 或 CSV）的功能。有关如何将这些值添加到 Hugo 项目的配置文件中的信息，请参见输出格式  [Output Formats](https://gohugo.io/templates/output-formats/)。



## 配置文件缓存



从 Hugo 0.52 开始，你还可以配置 `cacheDir`。下面是一些默认配置，你可以在自己的 `config.toml` 中覆盖所有这些缓存设置。



```toml
[caches]
[caches.getjson]
dir = ":cacheDir/:project"
maxAge = -1
[caches.getcsv]
dir = ":cacheDir/:project"
maxAge = -1
[caches.images]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.assets]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.modules]
dir = ":cacheDir/modules"
maxAge = -1
```



- `:cacheDir`

  如果已设置，则这是 `cacheDir` 配置项的值（也可以通过 OS env 变量 `HUGO_CACHEDIR` 进行设置）。它会退回到Netlify上的 `/opt/build/cache/hugo_cache/`，或其它操作系统的 temp 目录下的 `hugo_cache` 目录。这意味着，如果你在 Netlify 上运行构建，使用 `:cacheDir` 配置的所有缓存将在下一个版本中保存和恢复。对于其它 CI 供应商，请阅读其文档。有关 CircleCI 示例，请参见此 [配置](https://github.com/bep/hugo-sass-test/blob/6c3960a8f4b90e8938228688bc49bdcdd6b2d99e/.circleci/config.yml)。



- `:project`

  当前的Hugo项目的基本目录名称。这意味着，在默认设置下，每个项目都将具有单独的文件缓存，这意味着当执行 `hugo --gc` 时，将不会触摸与同一 PC 上运行的其它 Hugo 项目相关的文件。



- `:resourceDir`

  这是 `resourceDir` 配置项的值。



- `maxAge`

  这是退出缓存条目之前的持续时间，-1 表示永远，0 则表示有效地关闭了该特定的缓存。有效值为 `“10s”`、`“10m”`、`“10h”`（10秒、10分钟、10小时）。



- `dir`

  该缓存文件的存储位置的绝对路径。允许的起始占位符为 `:cacheDir `和 `:resourceDir`（参见上文）。



## 配置格式规格



- [TOML Spec](https://github.com/toml-lang/toml)
- [YAML Spec](https://yaml.org/spec/)
- [JSON Spec](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)

