## 简介 / Introduction

一个可以完整提取道客巴巴预览文档（非截图）的工具。

A tool to extract and convert doc88 documents (non-screenshot).

> **本项目基于 [cmy2008/doc88_extractor](https://github.com/cmy2008/doc88_extractor) 修改，保留原作者完整提交历史。感谢原作者的开源贡献。**
>
> This project is modified from [cmy2008/doc88_extractor](https://github.com/cmy2008/doc88_extractor), preserving the original author's full commit history. Thanks to the original author for the open-source contribution.

## 特点 / Features



* 利用 [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler) (以下简称 ffdec) 工具，几乎完美转换文档，保留原始文本、形状与图片。

  Powered by [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler), this tool preserves original text, shapes, and images—almost identical to the source.

* 适用文档范围：几乎所有

  It's available for almost all documents.

## 安装 / Installation

### Python



* 需要 Python 3.10 或更高版本。

  Requires Python 3.10 or newer.

安装依赖：



```
pip3 install retrying requests curl\_cffi xmltodict
```

### Java



* 需要安装 Java 才能进行文档转换（推荐 Java 17）:

  Requires Java (recommended: version 17):

  [Microsoft Build of OpenJDK 17 for Windows x64](https://aka.ms/download-jdk/microsoft-jdk-17.0.14-windows-x64.msi)

### SVG 转换 / SVG Converting



* 若启用 swf2svg，程序将自动下载 swf2svg 以实现 SVG 到 PDF 的转换。若安装失败，可尝试从 [typst/svg2pdf](https://github.com/typst/svg2pdf) 编译。

  If swf2svg is enabled, the tool will download swf2svg automatically to perform SVG-to-PDF conversion. if installation fails, try building it from [typst/svg2pdf](https://github.com/typst/svg2pdf).

* 支持平台 /support platform:

  Windows (x86\_64) / Linux (x86\_64/arm64) / MacOS (x86\_64/arm64) / Android (arm64)

## 如何使用 / How to Use

在程序目录下运行：



```
python3 main.py
```



* 控制台输入以下任意一种内容并回车：

  Enter any of the following into the console and press Enter:

1. doc88 链接 / Doc88 URL

2. 文档 ID (链接中 `p-` 后面的数字) / Document ID (the number after `p-` in URL)

3. 含有 ebt 文件的文件夹路径 (需要原始文件名) / Folder path containing ebt files (raw filenames required)

4. `m_main` 数据 (base64 变种格式) / `m_main` data

* 获取 `m_main` 数据的方法： / How to get the `m_main` data:

1. 浏览器打开文档网页 / Open the document page in browser

2. 打开 `开发者工具`，转到 `控制台` 选项卡 / 	Open `DevTools` and switch to the `Console` tab.

3. 手动输入`允许粘贴`并回车，启用粘贴功能 / Type `allow pasting` manually then enter. This will allow you to paste code.

4. 执行以下代码，即可一键复制 `m_main` 数据 / Run the code below to copy the `m_main` data in a single click.



```
(match = document.documentElement.outerHTML.match(/m\_main\\.init\\("(\[^"]\*)"\\);/)) ? (copy(match\[1]), console.log('Success.')) : console.log('Not found.')
```



* 首次运行会生成配置文件，检测更新并下载 ffdec 和 presse（用于 PDF 合并）。

  On first run, there will be a configuration file `config.json`, then check the updates and download the ffdec and presse(uses for pdf merging).

## 配置 / Configuration

### 说明 / Description

默认情况下配置在 `config.json` 文件中，主要说明如下：



| 键名 / Key           | 说明                                          | Description                                                                   |
| ------------------ | ------------------------------------------- | ----------------------------------------------------------------------------- |
| `proxy_url`        | Github 代理服务的 URL                            | The URL of Github's proxy service.                                            |
| `check_update`     | 是否在启动时检查更新                                  | Always check updates on startup.                                              |
| `swf2svg`          | 是否先转换到 SVG 再转到 PDF                          | Convert swf files to svg first.                                               |
| `svgfontface`      | （仅 swf2pdf 为 false 时有效）在 SVG 转换中是否转换字体来呈现文本 | Only works when swf2pdf is false; using font to show texts in SVG converting. |
| `clean`            | 是否保留中间文件                                    | Keep intermediate files.                                                      |
| `get_more`         | 是否始终通过扫描获取页面                                | Always via scanning to get pages.                                             |
| `path_replace`     | 是否在 Windows 下替换过长路径                         | Replace long paths on Windows.                                                |
| `download_workers` | 下载文件的线程数                                    | Number of threads for downloading files.                                      |
| `convert_workers`  | 转换文件的线程数                                    | Number of threads for converting files.                                       |
| `pdf_scale`        | 转换为 PDF 的缩放大小                               | Scale of PDF  converting.                                                     |

### 注意事项 / Attention



* 使用`proxy_url`选项，可解决国内无法正常下载文件的问题，内容应为 Github 的加速代理服务的 URL 前缀（一般以 [https://aaa.bbb.ccc/](https://aaa.bbb.ccc/) 为标准格式，注意不要漏最后的`/`）

* 使用 `pdf_scale` 选项，例如修改为 `0.5`（建议不要小于这个值，第三方软件可能无法自动处理过小的缩放），可以减小文档文件大小并加快转换速度，但是转换出来的文档也会相应缩小

* 使用 `swf2svg` 选项，也许会解决部分文档的字体问题或形状问题

* 使用 `swf2svg` 选项，而不使用 `svgfontface` 选项，由于省去了文本转换过程，可以大大加快转换速度

* 若启用 `svgfontface` 选项，由于 [typst/svg2pdf](https://github.com/typst/svg2pdf) 的缺陷，将无法转换字体，会自动替换为默认字体

* 若启用 `svgfontface` 选项，由于 [ffdec](https://github.com/jindrapetrik/jpexs-decompiler) 的缺陷，某些形状或文本会出现转换错误


## Rust 引擎 / Rust Engine

本项目支持使用纯 Rust 实现的 [swf2pdf-rs](https://github.com/yanlearn2/swf2pdf-rs) 作为 SWF→PDF 转换引擎，**完全不需要 Java 环境**。

This project supports using the pure Rust [swf2pdf-rs](https://github.com/yanlearn2/swf2pdf-rs) as the SWF→PDF conversion engine, **requiring no Java environment at all**.

### 启用方法 / How to Enable

1. 从 [swf2pdf-rs Releases](https://github.com/yanlearn2/swf2pdf-rs/releases) 下载对应平台的二进制文件（Windows: `swf2pdf.exe`）

2. 将二进制文件放到程序目录下，或在 `config.json` 中设置 `swf2pdf_bin` 为其绝对路径

3. 在 `config.json` 中将 `use_rust` 设为 `true`：

```json
{
    "use_rust": true,
    "swf2pdf_bin": "swf2pdf.exe"
}
```

### 对比 / Comparison

| 维度 | ffdec (Java) | swf2pdf-rs (Rust) |
|------|-------------|-------------------|
| 运行时依赖 | 需要 JRE 8+ | 无，单二进制 |
| 启动速度 | 慢（JVM 启动） | 快（原生） |
| 分发体积 | JRE ~200MB | 二进制 ~3MB |
| 矢量完整度 | 高（含渐变/蒙版） | 中（纯色为主，渐变待实现） |
| 适用场景 | 通用，复杂文档 | 标准文档（黑白文字+表格） |

### 注意事项 / Notes

- Rust 模式下会自动跳过 Java/ffdec/presse 的检查和下载
- 适用于道客巴巴标准文档（黑白文字+表格+简单矢量图），可覆盖 80%+ 场景
- 如遇复杂文档（含渐变、位图填充、透明度等），可切换回 ffdec 模式（`use_rust: false`）

## 致谢 / Acknowledgements

- **原项目 / Original Project**: [cmy2008/doc88_extractor](https://github.com/cmy2008/doc88_extractor) — 核心逻辑与实现
- **ffdec**: [jindrapetrik/jpexs-decompiler](https://github.com/jindrapetrik/jpexs-decompiler) — SWF 反编译与转换
- **svg2pdf**: [typst/svg2pdf](https://github.com/typst/svg2pdf) — SVG 转 PDF

## 版权声明 / Copyright Notice

本项目基于 [cmy2008/doc88_extractor](https://github.com/cmy2008/doc88_extractor) 修改，原项目未附带开源许可证（LICENSE）。根据著作权法，原作者保留所有权利。

This project is modified from [cmy2008/doc88_extractor](https://github.com/cmy2008/doc88_extractor). The original project does not include an open-source license (LICENSE). Under copyright law, the original author retains all rights.

- 本仓库仅用于个人学习与研究，不用于商业用途。 / This repository is for personal learning and research only, not for commercial use.
- 保留原作者完整提交历史与署名。 / The original author's full commit history and attribution are preserved.
- 如原作者认为本仓库侵犯其权益，请联系删除。 / If the original author believes this repository infringes their rights, please contact us for removal.
