# 北京大学学位论文Latex模板(for Mac)

本项目核心基于[pkuthss](https://github.com/CasperVector/pkuthss) 1.9.1版本，主要进行Mac的适配修改和个人最佳实践说明

## 🖋Mac+VSCode配置Latex环境

1. 下载并无脑安装[MacTex](https://www.tug.org/mactex/mactex-download.html)（将近5个G而且在外网）

2. vscode安装Latex Workshop插件

   <img src="https://upload-images.jianshu.io/upload_images/12014150-6efe6221bdba92fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" width="50%;" />

3. 在设置中打开setting.json配置文件，然后添加这段配置代码

   <details>
   <summary>配置代码</summary>

   ```json
   "latex-workshop.latex.tools": [
     {
       "name": "xelatex",
       "command": "xelatex",
       "args": [
         "-synctex=1",
         "-interaction=nonstopmode",
         "-file-line-error",
         "-pdf",
         "%DOC%"
       ]
     },
     {
       "name": "latexmk",
       "command": "latexmk",
       "args": [
         "-synctex=1",
         "-interaction=nonstopmode",
         "-file-line-error",
         "-pdf",
         "%DOC%"
       ]
     },
     {
       "name": "pdflatex",
       "command": "pdflatex",
       "args": [
         "-synctex=1",
         "-interaction=nonstopmode",
         "-file-line-error",
         "%DOC%"
       ]
     },
     {
       "name": "bibtex",
       "command": "bibtex",
       "args": [
         "%DOCFILE%"
       ]
     },
     {
       "name": "biber",
       "command": "biber",
       "args": [
         "%DOCFILE%"
       ]
     }
   ],
   "latex-workshop.latex.recipes": [
     {
       "name": "xelatex -> biber -> xelatex*2",
       "tools": [
         "xelatex",
         "biber",
         "xelatex",
         "xelatex"
       ]
     },
     {
       "name": "xelatex -> bibtex -> xelatex*2",
       "tools": [
         "xelatex",
         "bibtex",
         "xelatex",
         "xelatex"
       ]
     },
     {
       "name": "xelatex",
       "tools": [
         "xelatex"
       ]
     },
   ],
   "editor.wordWrap": "on",
   "latex-workshop.view.pdf.viewer": "tab"
   ```

   > 注意这里有三种Recipes方式，第三种方式是最简单的直接使用xelatex编译，第一种第二种方式是针对不同项目中有参考文献的编译方法，总体上都是先编译第一次 -> 编译参考文献 -> 再编译两次
   >
   > 在pkuthss项目中我们使用biber(第一种方式)进行编译

   </details>

4. 创建一个tex文件写点东西试一试

   ```latex
   \documentclass{article}
   \usepackage[UTF8, scheme = plain]{ctex}
   \begin{document}
   测试中文
   \end{document}
   ```

5. 依次进行编译和预览

   <img src="https://upload-images.jianshu.io/upload_images/12014150-5994f5458670af83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" width="50%;" />

   如果顺利看到pdf结果则证明latex环境安装顺利完成

<br/>

## 📄编译pkuthss-mac

1. clone该项目到本地
    ```
    git clone https://github.com/doubleZ0108/pkuthss-mac.git
    ```
2. 在TEX标签下点击`Recipe: xelatex->biber->xelatex*2`进行编译
3. 最终就得到了Mac+VSCode+pkuthss优雅写出来的论文啦🎉

    <img src="https://upload-images.jianshu.io/upload_images/12014150-f8a6db887c3b636e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" width="25%;" />


<br/>

<details>

<summary>🧱从原pkuthss项目开始</summary>


## 编译pkuthss

1. 直接下载最新[pkuthss release版本](https://github.com/CasperVector/pkuthss/releases)即可

2. 下载文件夹里`pkuthss/doc/example/` 是之后自己写文章的部分，把这个文件夹单独拷贝到自己的目录下

3. 把下载文件夹里`pkuthss/tex/` 里的3个`.def`文件和1个`.cls`文件拷贝到上一步的`example/`里（别问我这步找问题找了多长时间）

4. 修改`example/ctex-fontset-pkuthss.def`文件，这个文件的问题是原作者主要是在windows平台进行开发的，而由于windows和mac下字体名称不一样，导致编译时会报错The font xxx cannot be found.

   【解决办法】

   首先明确下两平台下的字体对应关系

   - win：simhei（黑体），simkai（楷体）, simsun（宋体）， simfang（仿宋）
   - mac：STHeiti （华文黑体），STKaiti （华文楷体），STSong（华文宋体） ，STFangsong（华文仿宋）

   以楷体的替换为例，在字体册中搜索楷体，这个STKaiti即是mac中对应simkai的那个字体

   <img src="https://upload-images.jianshu.io/upload_images/12014150-bf74eb66c3fea92e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" width="50%;" />

   在这个文件里全局进行几种字体的替换即可

   > 还有网友说要修改`/usr/local/texlive/[year]/texmf-dist/tex/latex/ctex/fontset/ctex-xecjk-winfonts.def` 文件，但这样应该是直接修改了texlive的配置，大家也可以尝试

5. 最终在命令行打开`example/`，然后通过`latexmk`命令进行编译，如果没有其他问题即可得到最终的pdf文件

6. 在vscode中编译也类似的，直接`Recipe: xelatex -> biber -> xelatex*2`即可

   > 注意 如果没进行引用的话，在编译过程中会报错`I found no \\citation commands---while reading file`，添加`\cite{}`即可

</details>

<br/>

## 🔧pkuthss中的一些设置问题

1. 关于参考文献及引用

   <details>

   <summary>🚪一扇任意门</summary>

   在这里踩的坑最多，先说最简单最原始的方法，直接将需要引用的文献BibTex放到`thesis.bib`中即可，然后在需要引用的部分`\cite{name_of_citation}`即可，在example中作者还给出了三种不同的引用样式：

   - \cite未格式化的  1
   - \parencite带方括号的  [2]
   - \supercite上标且带方括号的 <sup>[1,2]</sup>

   使用中还发现几个问题，第一是参考文献的样式不太对，按照要求把`thesis.tex`25行的biblatex改成如下即可，其中第二个参数是用来忽略出版社和出版地信息的，如果在参考文献中出现[S.l.]: [s.n.]则说明你需要添加这个配置

   ```latex
   \usepackage[backend = biber, gbpub=false, style = gb7714-2015]{biblatex}
   ```

   我个人在摸索的过程中需要实现的效果是每部分单独列出参考文献，并且希望连续编号，大致样式为

   ```
   This is chapter 1[1]
   
   参考文献
   [1] reference1
   
   -----
   
   This is chapter 2[2]
   
   参考文献
   [2] reference2
   ```

   在经过了两天漫长的踩坑和严谨的搜索后，终于找到了解决方案：

   ```latex
   \newrefsegment
   This is chapter 1\cite{reference1}
   \printbibliography[segment=\therefsegment]
   
   \newrefsegment
   This is chapter 2\cite{reference2}
   \printbibliography[segment=\therefsegment]
   ```

   如果想把『参考文献』四个字换成自定义的『MyName』，需要在`\begin{document}`前添加一段

   ```latex
   \defbibheading{myname}{
     \section*{MyName\ref{refsegment:\therefsection\therefsegment}}
   }
   ```

   然后在调用时`\printbibliography[segment=\therefsegment, heading=myname]`指定heading参数

   原项目通过`\printbibliography`一次性打印所有参考文献

   此外，在检索过程中还发现一些关键词可能对你有帮助：`\begin{refsegment}`, `\bibbysegment`

   > 其实有一种很简洁的平替方案
   >
   > ```latex
   > \begin{refsection}
   > \cite{reference1}
   > \printbibliography
   > \end{refsection}
   > 
   > \begin{refsection}
   > \cite{reference2}
   > \printbibliography
   > \end{refsection}
   > ```
   >
   > 但它的问题是不能接续编号，每个section都是从1开始编号，就为了这一点花了我太多时间踩坑了...

   </details>

2. 关于空白页面

   <details>
   <summary>🚪又一扇任意门</summary>

   直接在模板里修改会发现几乎每一章/部分后面都有一个空白页面，对于不是要做毕设只是想用模板的同学来说非常抓狂

   一个简单的做法是在开头第一行加上openany参数 `\documentclass[UTF8]{pkuthss}`，但仔细观察会发现奇数页的页眉是这个chapter的名字，但偶数页是一个固定的名称，大概是“北京大学硕士学位论文”类似的

   

   ~~这个问题暂时没有找到优雅的解决方案，我个人应急的方法如下：

   比如要去掉`test.tex`后的空白页，则在`\input{test.tex}`前加入这行

   ```latex
   \let\cleardoublepage\clearpage
   ```

   然后在每个章节tex文件`\chapter`的下方加入两行，这两行是指定这个页面的页眉信息

   ```latex
   \thispagestyle{fancy}
   \fancyhead[C]{Your Personal Title}
   ```

   这样可以解决不同页面上方页眉内容不一致的问题，但测试下来还有一个bug，如果这章超过了一页，则第二页的页眉页脚都会消失，暂时还没找到解决方案

   或者可以修改`pkuthss.cls`中`\fancypagestyle{plain}`这部分代码，删掉其中关于twoside的部分，变为

   ```latex
   \fancypagestyle{plain}{
   	\fancyhf{}\renewcommand*{\headrulewidth}{0.75bp}
   	\fancyfoot[C]{\zihao{5}\normalfont{\thepage}}
   	\fancyhead[C]{\zihao{5}\normalfont\thss@int@setcase{\leftmark}}
   }
   \pagestyle{plain}
   ```

   关于页眉页脚的更详细设置可以搜索`\pagestyle`相关内容

   </details>

3. 有待发现和补充💭
