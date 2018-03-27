##### 不生成 SourceMap
devtool                        | 描述
------------------------------ | -----
(none)                         | 不生成 SourceMap ，不映射文件
eval                           | 不生成 SourceMap ，`eval()` 执行模块字符串映射到 webpack 转换后代码

##### 生成 SourceMap
devtool                        | 描述
------------------------------ | -----
source-map                     | 生成 SourceMap ，映射到源代码
hidden-source-map              | 同 `source-map`，删除打包文件的 `//@ sourceMappingURL` 注释
inline-source-map              | 同 `source-map`，SourceMap 通过 DataUrl 方式添加到打包文件最后
eval-source-map                | 生成 SourceMap 通过 DataUrl 方式添加到模块字符串后面，`eval()` 执行映射到源代码，同时显示 webpack 转换后代码
nosources-source-map           | 生成 SourceMap （不含 `sourcesContent` ），映射到文件结构和文件名，文件没有内容

##### 单行模式（cheap）, 压缩的情况不可用
devtool                        | 描述
------------------------------ | -----
cheap-source-map               | 单行版 `source-map`，映射的是 Loader 编译后的代码
cheap-module-source-map        | 单行源码版 `source-map`，映射的是源代码
inline-cheap-source-map        | 单行版 `inline-source-map`，映射的是 Loader 编译后的代码
inline-cheap-module-source-map | 单行源码版 `inline-source-map`，映射的是源代码
cheap-eval-source-map          | 单行版 `eval-source-map`，映射的是 Loader 编译后的代码，同时显示 webpack 转换后代码
cheap-module-eval-source-map   | 单行源码版 `eval-source-map`，映射的是源代码，同时显示 webpack 转换后代码


## [文档]()
devtool                        | build | rebuild | production | quality
------------------------------ | ----- | ------- | ---------- | -----------------------------
(none)                         | +++   | +++     | yes        | bundled code
eval                           | +++   | +++     | no         | generated code
cheap-eval-source-map          | +     | ++      | no         | transformed code (lines only)
cheap-module-eval-source-map   | o     | ++      | no         | original source (lines only)
eval-source-map                | --    | +       | no         | original source
cheap-source-map               | +     | o       | no         | transformed code (lines only)
cheap-module-source-map        | o     | -       | no         | original source (lines only)
inline-cheap-source-map        | +     | o       | no         | transformed code (lines only)
inline-cheap-module-source-map | o     | -       | no         | original source (lines only)
source-map                     | --    | --      | yes        | original source
inline-source-map              | --    | --      | no         | original source
hidden-source-map              | --    | --      | yes        | original source
nosources-source-map           | --    | --      | yes        | without source content

T> `+++` 超级快, `++` 快, `+` 较快, `o` 一般, `-` 较慢, `--` 慢

### 特性

`bundled code` - 打包代码不能映射到具体文件(不生成 SourceMap 文件)。

`generated code` - 打包代码能映射到具体文件(不生成 SourceMap 文件)，显示的是 webpack 转换后的代码。

`transformed code` - 打包代码能映射到具体文件，显示 Loader 编译后 webpack 转换前的代码。

`original source` - 打包代码能映射到具体文件，显示源代码(依赖 Loader 支持)。

`without source content` - 打包代码能映射到文件，但是 SourceMap 不包含文件内容即不显示源代码。

`(lines only)` - SourceMap 不包含列信息只映射到代码每一行，压缩代码通常只有一行所以不可用。

### 开发环境

开发环境建议选项：

`eval` - 每个模块通过 `eval()` 执行生成，根据 `//@ sourceURL` 确定路径，生成速度超级快，主要缺陷是不能显示对应的源码行号（显示实际执行的代码）。

`eval-source-map` - 每个模块通过 `eval()` 执行生成，其中还包含了 DataUrl 方式保存的 SourceMap，这个选项首次构建很慢，但是重复构建就较快。映射的是源代码，同时还显示 webpack 实际执行的代码。

`cheap-eval-source-map` - 和 `eval-source-map` 类似, 差别在于不包含列信息，映射的是 Loader 编译后的代码。

`cheap-module-eval-source-map` - 和 `cheap-eval-source-map` 类似，不包含列信息，映射的是源代码。


### 生产环境

生产环境建议选项:

`(none)` (忽略 `devtool` 选项) - 没有 SourceMap 生成，适合新手选择。

`source-map` - 生成完整的 SourceMap ，打包文件包含 `//@ sourceMappingURL` 使得开发工具可以映射到源代码。

W> 开发者应该配置服务器禁止普通用户访问 SourceMap 文件。

`hidden-source-map` - 和 `source-map` 类似，只是打包文件不包含 `//@ sourceMappingURL`，可以使 SourceMap 只用于跟踪错误报告，而不在浏览器开发工具上面映射源代码。

W> 开发者不应该把 SourceMap 部署到服务器，只用在错误报告工具。

`nosources-source-map` - 生成不含 `sourcesContent` 的 SourceMap 使得用户端（浏览器）可以跟踪调用堆栈又不会暴露源代码，这个选项可以把 SourceMap 部署到服务器。

W> 解码可以得到文件名和文件结构，但是不会暴露源代码。


### 特殊环境

不建议用于开发和生产环境，供第三方工具使用的特殊选项

`inline-source-map` - SourceMap 通过 DataUrl 方式添加在打包文件之后。

`cheap-source-map` - SourceMap 不包含列信息，映射的是 Loader 编译后的代码。

`inline-cheap-source-map` - 和 `cheap-source-map` 类似，SourceMap 通过 DataUrl 方式添加在打包文件之后。

`cheap-module-source-map` - SourceMap 不包含列信息，映射的是源代码。

`inline-cheap-module-source-map` - 和 `cheap-module-source-map` 类似，SourceMap 通过 DataUrl 方式添加在打包文件之后。


T> 当使用 `uglifyjs-webpack-plugin` 应该设置 `sourceMap: true` 选项，开启 SourceMap 支持。

[原文档](https://webpack.js.org/configuration/devtool/)
