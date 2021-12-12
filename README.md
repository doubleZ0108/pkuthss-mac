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
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
    ],
    "editor.wordWrap": "on"
   ```

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

   <img src="https://upload-images.jianshu.io/upload_images/12014150-56a1881f24e17624.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="image.png" width="50%;" />

   如果顺利看到pdf结果则证明latex环境安装顺利完成

<br/>

## 📄编译pkuthss-mac

1. clone该项目到本地
    ```
    git clone https://github.com/doubleZ0108/pkuthss-mac.git
    ```
2. 在TEX标签下点击`Recipe: xelatex`进行编译
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

6. 在vscode中编译也类似的，直接`Recipe: xelatex`即可

   > 填上面的坑并挖下面的坑，如果用`xelatex -> bibtex -> xelatex*2`，就总是有一堆报错，也没搞很明白，留下问题的关键词有需要的朋友自己检索吧`I found no \\citation commands---while reading file`

</details>
