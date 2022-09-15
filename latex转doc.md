使用 Latex 编写的文档有时需要转化成 _.docx_ 格式，现有的 Latex2Word 软件大多需要收取一定费用才能实现。 Pandoc 是一款免费且开源的文档格式转化工具，支持众多文本类型之间的转化，而且转化效果相当优秀。本文将简单介绍使用 Pandoc 将 _.tex_ 转化成 _.docx_ 文件的流程及常见问题的处理方法。本人只在 Windows 操作系统下进行过测试， 因此以下教程在其他操作系统下不一定适用。

Pandoc 安装比较简单，可以在 [Pandoc 官网](https://link.zhihu.com/?target=https%3A//pandoc.org/installing.html) 下载和安装最新版本， 或者在 [Github Releases](https://link.zhihu.com/?target=https%3A//github.com/jgm/pandoc/releases/) 下载需要的版本。 本文使用 pandoc-2.17.1.1 版本。

## 3\. 基本使用

在 Windows 下打开 DOS 命令窗口并进入到要转化的 _.tex_ 目标文件的目录。之后使用命令：

```
pandoc input.tex -o output.docx
```

即可将 input.tex 转化成 output.docx。 input 是输入的文件名，_.tex_ 文件； output 是输出的文件名，注意必须是 _.docx_ 后缀。转化的 _.docx_ 文件不需要和 _.tex_ 文件同名。

## 4\. 进阶使用

使用基本命令转化的效果不足以满足一些复杂的情况，例如用 Latex 写的学术论文需要转换成 Word。 在进阶使用中将会看到利用一些工具和命令配置满足更多转换需求的方法。

### 4.1 交叉引用

Pandoc 不能直接生成表格，公式或者图片在正文的交叉引用编号。pandoc-crossref 这个工具可以帮助 Pandoc 达到这个目的。 对于 Windows 系统，需要从 [GitHub Repo](https://link.zhihu.com/?target=https%3A//github.com/lierdakil/pandoc-crossref/releases) 下载 _pandoc-crossref-Windows.7z_，解压后将 _pandoc-crossref.exe_ 粘贴到 Pandoc 的安装目录中。**注意： pandoc-crossref 的版本必须与 pandoc 的版本匹配**。

使用以下配置命令启用交叉引用：

#### 4.1.1 公式编号

通过添加以下配置命令自动生成公式的编号并对齐：

```
-M autoEqnLabels 
-M tableEqns
```

#### 4.1.2 标注的编号

Latex 中表格和图片的 `\caption` 的编号通过以下配置命令自动转化：

#### 4.1.3 章节编号

通过添加以下配置命令生成各个章节的编号：

### 4.2 参考文献

参考文献是论文写作中必不可少的部分，自动从 _.tex_ 生成参考文献可节约大量格式转化的时间。 通过添加配置命令生成参考文献。

```
--bibliography=reference.bib
```

#### 4.2.1 指定参考文献格式

一些情况下需要生成指定格式的参考文献，例如需要 IEEE 或 Springer 的格式。 在 Pandoc 中，参考文献的格式是通过 _.csl_ 文件指定。 在 [Zotero Style Repository](https://link.zhihu.com/?target=https%3A//www.zotero.org/styles) 可以下载到所需的 _.csl_ 文件，如springer-basic-note.csl。将下载的 _.csl_ 文件放置到与\*.tex\* 文件的同级目录下。使用命令指定格式：

```
--csl springer-basic-note.csl 
```

此外，pandoc 生成指定格式的参考文献需要使用执行器，网络上很多教程推荐使用 `--filter pandoc-citeproc`这个命令。**但新版本的 Pandoc 已经弃用了这个命令**，而改为直接使用：

由此完整的生成指定格式的参考文献命令为：

```
--citeproc --csl springer-basic-note.csl
```

#### 4.2.2 制定章节名称

上面命名只能生成参考文献列表，但不能生成参考文献的章节名。使用如下命令可以自定义参考文献的章节名：

```
-M reference-section-title=Referensi
```

其中 _Reference_ 为自定义的章节名。

## 5\. 完整命令

学术论文中最常用的 _.tex_ 文件转 _.docx_ 命令为:

```
pandoc input.tex  --filter pandoc-crossref --citeproc --csl springer-basic-note.csl  --bibliography=reference.bib -M reference-section-title=Reference  -M autoEqnLabels -M tableEqns  -t docx+native_numbering --number-sections -o output.docx
```

配置的详细解释请在第 4 节查阅。 使用上述命令转化 `Springer Lecture Notes in Computer Science (LNCS)` 的示例可以在 [Latex2wordExample](https://link.zhihu.com/?target=https%3A//github.com/xhan97/Latex2WordExample) 中查看。

## 6\. 踩过的坑

### 6.1. Pandoc 版本不符合预期

安装了与 Pandoc 版本匹配的 pandoc-crossref 但使用 pandoc-crossref 时出现版本不匹配的问题，报告如下错误：

```
WARNING: pandoc-crossref was compiled with pandoc 2.11.0.4 but is being run through 1.19.2.1. This is not supported. Strange things may (and likely will) happen silently.
pandoc-crossref: Error in $: Incompatible API versions: encoded with [1,17,0,4]
but attempted to decode with [1,22].
```

同时使用

发现使用的 pandoc 与在步骤 1 安装的版本不符。 例如安装的版本为 Pandoc-2.11.0.4, 但发现使用的是 Pandoc-1.19.2.1。

解决办法：

1.  Python 包管理器 Anaconda 提供了 Pandoc 的运行脚本，如果安装了 Anaconda 并设置了环境变量，系统很大可能会调用Anaconda 中的 Pandoc 可执行文件，从而导致运行版本与预期不符。**这时可以暂时把 ~\\ProgramData\\Anaconda3\\Scripts 下的 pandoc.exe 移除**，转化完之后再重新放回。
2.  检查是否安装过旧版本的 _Pandoc_ ，如果是，将新旧版本全部卸载后重新安装新版本。

### 6.2. 参考文献无法生成

在确保命令使用正确下从以下方面检查：

1.  _reference.bib_ 是否和要转换的 _.tex_ 文件在同一目录下。
2.  `--filter pandoc-crossref` 是否紧跟在 `pandoc input.tex` 后面。

### 6.3. _docx_ 文件不更新

_.tex_ 文件更新，但生成的\*.docx\* 文件并没有更新。

解决方法：

1.  _tex_ 文件更新后，需要正确编译后才能使用 Pandoc 成功转化为 _.docx_.

## 7\. 总结

本文主要介绍使用开源软件 Pandoc 将 _.tex_ 转为 _.docx_ 文件的方法。首先介绍了 pandoc 的安装及基本使用， 进而介绍了针对特殊的转化需求的配置和使用方法。之后给出了针对论文转化常用的命令。最后总结了本人在使用和配置过程遇到的坑及解决方法。

> 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布