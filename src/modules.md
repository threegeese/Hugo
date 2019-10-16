# Hugo 模块

:package: 此篇学习 Hugo 模块。配置模块、使用模块、主题模块。



[TOC]

## 大纲



Hugo 模块是雨果的核心构建块。 一个模块可以是您的主项目，也可以是一个较小的模块，它提供 Hugo 中定义的7 种组件类型中的一种或多种：静态、内容、布局、数据、资产、i18n 和原型。 （ **static**, **content**, **layouts**, **data**, **assets**, **i18n**, and **archetypes** ）



您可以按自己喜欢的任何方式组合模块，甚至可以从非 Hugo 项目中挂载目录，从而形成一个大型的虚拟联合文件系统。



Hugo 模块由 Go 模块驱动。有关 Go 模块的更多信息，请参见：

- https://github.com/golang/go/wiki/Modules
- https://blog.golang.org/using-go-modules



全新的几个示例项目：

* https://github.com/bep/docuapi   是一个已移植到 Hugo 模块的主题。这是将非 Hugo 项目安装到 Hugo 文件夹结构中的一个很好的例子。它甚至显示了常规 Go 模板中的 JS Bundler 的实现。 
*  https://github.com/bep/my-modular-site  一个非常简单的用于测试的网站。 



##  配置模块



:desktop_computer:  这节介绍模块的配置选项 。



###  模块配置：顶级 



```yml
module:
  noProxy: none
  private: '*.*'
  proxy: direct
```



`proxy`：定义用于下载远程模块的代理服务器。默认值为direct，表示“ git clone”和类似名称。 

`noProxy`：逗号分隔的全局列表匹配路径，不应使用配置的代理。

`private`：逗号分隔的全局列表匹配路径应视为私有路径。



> :waning_gibbous_moon: 请注意，以上术语直接映射到Go模块中的相应术语。这些设置中的一部分可能很自然地设置为OS环境变量。例如，要设置要使用的代理服务器：



```go
env HUGO_MODULE_PROXY=https://proxy.example.org hugo
```



> Hugo 模块的大多数命令都需要安装更高版本的 Go（请参阅 https://golang.org/dl/ ）和相关的 VCS 客户端（例如 Git，请参阅 https://git-scm.com/downloads）。如果您在 Netlify 上运行一个“较旧”的站点，则可能必须在“环境”设置中将`GO_VERSION` 设置为 1.12。
>
> 
>
> 有关 Go 模块的更多信息，请参见：
>
> - https://github.com/golang/go/wiki/Modules
> - https://blog.golang.org/using-go-modules



### 模块配置：hugo 版本



如果您的模块需要特定版本的 Hugo才能正常工作，则可以在 `modules` 部分中指出，如果使用的版本太旧/太新，则会警告用户。（ 以下任何内容均可省略。 ）



```yaml
module:
  hugoVersion:
    extended: false
    max: ""
    min: ""
```



`min`：支持的最低 Hugo 版本，例如 0.55.0。

`max`： 支持的最高 Hugo 版本，例如 0.55.0。

`extended`：是否需要扩展版本的 Hugo。



### 模块配置：导入（imports）



```yaml
module:
  imports:
  - disable: false
    ignoreConfig: false
    path: github.com/gohugoio/hugoTestModules1_linux/modh1_2_1v
  - path: my-shortcodes
```



`path`：可以是有效的 Go Module 模块路径，例如 `github.com/gohugoio/myShortcodes` 或存储在主题文件夹中的模块目录名称。

`ignoreConfig`：如果启用，则为任何模块配置文件，例如 `config.toml`，将不会加载。注意，这还将停止加载任何可传递模块依赖项。

 `disable`：设置 `true` 以禁用模块，同时将所有版本信息保留在 `go.*` 文件中。 



> Hugo 模块的大多数命令都需要安装更高版本的 Go（请参阅 https://golang.org/dl/ ）和相关的 VCS 客户端（例如 Git，请参阅 https://git-scm.com/downloads）。如果您在 Netlify 上运行一个“较旧”的站点，则可能必须在“环境”设置中将`GO_VERSION` 设置为 1.12。
>
> 
>
> 有关 Go 模块的更多信息，请参见：
>
> - https://github.com/golang/go/wiki/Modules
> - https://blog.golang.org/using-go-modules



### 模块配置：挂载（mounts）



> 当 Hugo 0.56.0 中引入了 `mounts` 配置时，我们小心地保留了现有的 `staticDir` 和类似的配置，以确保所有现有站点都能继续工作。
>
> 但是，您不应同时拥有两者。因此，如果添加 `mounts` 部分，则应使其完整并删除旧的 `staticDir` 等设置。



```yaml
module:
  mounts:
  - source: content
    target: content
  - source: static
    target: static
  - source: layouts
    target: layouts
  - source: data
    target: data
  - source: assets
    target: assets
  - source: i18n
    target: i18n
  - source: archetypes
    target: archetypes
```



`source`：挂载的源目录。对于主项目，这可以是项目相对的或绝对的，甚至是符号链接。对于其他模块，它必须是相对于项目的。 

 `target`：应该将其挂载到 Hugo 的虚拟文件系统中的位置。它必须以 Hugo 的其中一个组件文件夹开头： `static`、`content`、`layouts`、`data`、`assets`、`i18n`、`archetypes`。如 `content/blog`。

 `lang`：语言代码，例如 “en”。仅与 `content` 挂载以及在多主机模式下的 `static` 挂载有关。 



## 使用 Hugo 模块



:m: 这节学习如何使用 Hugo Modules 构建和管理网站。



### 初始化新模块



使用 `hugo mod init` 初始化新的 Hugo 模块。如果无法确定模块路径，则必须将其作为参数提供，例如：



```shell
hugo mod init github.com/gohugoio/myShortcodes
```



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



### 更新模块



将模块作为导入添加到配置时，将先下载并添加模块，请参阅模块导入。  [Module Imports](https://gohugo.io/hugo-modules/configuration/#module-config-imports) 



要更新或管理版本，您可以使用 `hugo mod get`。一些例子：



```shell
# 更新所有模块
hugo mod get -u

# 更新一个模块
hugo mod get -u github.com/gohugoio/myShortcodes

# 获取特定版本
hugo mod get github.com/gohugoio/myShortcodes@v1.0.7
```



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



### 在模块中进行和测试更改



对导入到项目中的模块进行本地开发的一种方法是在 go.mod 中将替换指令添加到本地目录中：



```shell
replace github.com/bep/hugotestmods/mypartials => /Users/bep/hugotestmods/mypartials

```



如果正在运行 hugo 服务器，则将重新加载配置，并将 `/Users/bep/hugotestmods/mypartials` 放入监视列表。



### 打印依赖图



使用相关模块目录中的 `hugo mod graph`，它将打印依赖关系图，包括供应商，模块更换或禁用状态。如：



```shell
hugo mod graph

github.com/bep/my-modular-site github.com/bep/hugotestmods/mymounts@v1.2.0
github.com/bep/my-modular-site github.com/bep/hugotestmods/mypartials@v1.0.7
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myassets@v1.0.4
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myv2@v1.0.0
DISABLED github.com/bep/my-modular-site github.com/spf13/hyde@v0.0.0-20190427180251-e36f5799b396
github.com/bep/my-modular-site github.com/bep/hugo-fresh@v1.0.1
github.com/bep/my-modular-site in-themesdir
```



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



### 发布模块 （Vendor）



`hugo mod vendor` 会将所有模块依赖关系写入  `_vendor` 文件夹，该文件夹随后将用于所有后续构建。



注意： 

* 您可以在模块树的任何级别上运行 `hugo mod vendor`。
*  供应商将不会存储存储在您的 `themes` 文件夹中的模块。 
* 大多数命令接受 `--ignoreVendor` 标志，然后它将忽视存在的 `_vendor` 文件夹运行。



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



### 整理 go.mod、go.sum



运行 `hugo mod tidy` 来删除 go.mod 和 go.sum 中未使用的条目。



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



### 清理模块缓存



运行 `hugo mod clean` 删除整个模块缓存。

请注意，您还可以使用 `maxAge` 配置模块缓存（ `modules` cache ），请参见文件缓存。 [File Caches](https://gohugo.io/configuration/#configure-file-caches) 



另请参阅 CLI 文档。  [The CLI Doc](https://gohugo.io/commands/hugo_mod_init/)



## 主题组件



Hugo 提供了主题组件的高级主题支持。



> :warning: 本节包含可能已过时且正在被重写的信息。



从 Hugo 0.42 开始，一个项目可以将主题配置为所需的许多主题组件的组合：



```yaml
theme:
- my-shortcodes
- base-theme
- hyde
```



您甚至可以嵌套它，并使主题组件本身在其自己的 `config.toml`（主题继承）中包含主题组件。

上面 `config.toml` 中的主题定义示例创建了一个具有 3 个主题组件的主题，其优先级从左到右。

对于任何给定的文件，数据输入等，Hugo 都将首先在项目中查找，然后在 `my-shortcode`、`base-theme` 和最后的 `hyde` 中查找。



Hugo 根据文件类型使用两种不同的算法来合并文件系统：

* 对于 `i18n` 和 `data` 文件，Hugo 使用文件内的翻译 ID 和数据键进行了深度合并。 

* 对于 `static`, `layouts`（templates）、 `archetypes` 文件，这些文件在文件级别上合并。因此，将选择最左边的文件。



上面的主题定义中使用的名称必须与 `/your-site/themes` 中的文件夹匹配，如 `/your-site/themes/my-shortcodes` 。有计划对此进行改进并获得 URL 方案，以便可以自动解决此问题。



还请注意，作为主题一部分的组件可以具有自己的配置文件，例如 `config.toml`。当前对主题组件可以配置的内容有一些限制：

- `params` (global and per language)
- `menu` (global and per language)
- `outputformats` and `mediatypes`



相同的规则适用于此：具有相同 ID 的最左侧参数/菜单等将获胜。上面提供了一些隐藏的和试验性的名称空间支持，我们将在以后的工作中进行改进，但是建议主题作者创建自己的名称空间以避免命名冲突。



> :fist_oncoming: 对于在 Hugo Themes Showcase 上托管的主题，需要将 git 子模块添加为指向目录 `exampleSite/themes` 的子模块。