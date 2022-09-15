### snnu-paper-temples-2022 陕西师范大学硕士学位论文latex模板
    > 由overleaf上的陕师大学位论文模板修改制作而成
    > 创建日期：2022/3/31
    > 模板维护：Chalie
    > 根据2022年3月新出的论文格式进行修改

#### 文件结构
    - bib  存放参考文献bibtex
    - ref.bib

    - configuration 存放字体
    - simhei.ttf
    - simkai.ttf
    - simsun.ttf

    - csl
    - 存放输出 doc时的参考文献格式csl

    - figure
    - 论文封面

    - tex
    - 论文内容

####  使用要求：
    - 编译器：Texlive2019或以上
    - 编辑器：
      - 1. Texstudio 
      - 2. vscode 安装插件 Latex Workshop, 使用latex(xelatex)编译

####  使用方法：
    - 编辑/tex目录下的各个tex文件的文件内容
    - 最后编译main.tex
    - 警告：需要修改配置就编辑main.tex的内容，编辑内容去/tex目录下的各个文件
#### 输出 docx查重： 
- 参考链接： https://zhuanlan.zhihu.com/p/455713759
- 软件：
  -下载安装：pandoc-2.17.1.1 [pandoc-2.17.1.1-windows-x86_64.msi](https://github.com/jgm/pandoc/releases/download/2.17.1.1/pandoc-2.17.1.1-windows-x86_64.msi)
  - 载解压，复制到pandoc安装目录 [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref/releases/tag/v0.3.12.2a)


- 在项目主目录下运行如下命令
**g7714-2005**
```bash
pandoc main.tex  --filter pandoc-crossref --citeproc --csl csl/chinese-gb7714-2005-numeric --bibliography=bib/ref.bib -M reference-section-title=Reference  -M autoEqnLabels -M tableEqns  -t docx+native_numbering --number-sections -o output.docx

```
**g7714-2015**
```bash
pandoc main.tex  --filter pandoc-crossref --citeproc --csl csl/china-national-standard-gb-t-7714-2015-numeric --bibliography=bib/ref.bib -M reference-section-title=Reference  -M autoEqnLabels -M tableEqns  -t docx+native_numbering --number-sections -o output.docx
```

> 注意： Word 无法显示矢量图，如需发送完整版word给导师修改，提前将文中使用的矢量图转为 .PNG 格式。

> tips：查重不用放图，因此，可以将所有的图都画成矢量的
