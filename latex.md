# 作者

## [作者署名](https://tex.stackexchange.com/questions/156523/multiple-authors-with-common-affiliations-in-ieeetran-conference-template)

https://tex.stackexchange.com/questions/107739/authors-with-multiple-affiliations

/、https://tex.stackexchange.com/questions/268/whats-the-best-way-to-write-e-mail-addresses

https://tex.stackexchange.com/questions/156523/multiple-authors-with-common-affiliations-in-ieeetran-conference-template

https://tex.stackexchange.com/questions/303678/custom-positions-of-author-names-affiliations-and-emails-in-latex-article



# 图表

## [Latex给表格加脚注](https://blog.csdn.net/ShuqiaoS/article/details/86230367)

```
        \hline
        \multicolumn{7}{l}{$^{\mathrm{*}}$Linux Foundation.}
    \end{tabular}
\end{table*}
```

## [Latex table with wrapped text in cells](https://stackoverflow.com/questions/33432787/latex-table-with-wrapped-text-in-cells)

https://tex.stackexchange.com/questions/284369/text-too-long-for-the-cell

https://stackoverflow.com/questions/790932/how-to-wrap-text-in-latex-tables

https://blog.csdn.net/weixin_43971980/article/details/96513197

https://blog.csdn.net/virhuiai/article/details/7886265

## 

## [How to influence the position of float environments like figure and table in LaTeX?](https://tex.stackexchange.com/questions/39017/how-to-influence-the-position-of-float-environments-like-figure-and-table-in-lat)

# 引用



## [添加bibTex参考文献](https://zhuanlan.zhihu.com/p/114733612)

## [参考文献图片的相互跳转](https://zhuanlan.zhihu.com/p/114733612)

`\usepackage[backref]{hyperref}`





## [参考文献合并](https://blog.csdn.net/qq_44773018/article/details/105792425)







## [IEEE模板引用同名作者破折号省略](https://blog.csdn.net/luolang_103/article/details/81288603)



1. 找到参考文献格式文件位置，我的是“C:\Program Files\texlive\2019\texmf-dist\bibtex\bst\IEEEtran”；
2. 打开IEEEtran.bst，我用Texstudio打开的，当然别的文本编辑工具也可以；
3. 找到以下代码：

```
% #0 turns off the "dashification" of repeated (i.e., identical to those
% of the previous entry) names. The IEEE normally does this.
% #1 enables
FUNCTION {default.is.dash.repeated.names} { #1 }
```
可以看到这里有对重名作者的默认设置，“1”表示设置重名参考文献用破折号（dash）代替，这也是IEEE默认的。“0”表示关掉默认设置，将原代码修改并保存：
```
% #0 turns off the "dashification" of repeated (i.e., identical to those
% of the previous entry) names. The IEEE normally does this.
% #1 enables
FUNCTION {default.is.dash.repeated.names} { #0 }
```
多编译几次，能看到原先缺省的作者已经正常显示出来。

## [引用插入网址和换行问题](https://blog.csdn.net/qq_38558554/article/details/105496028)

首先加入宏定义
`\usepackage{url}`
然后在.bib文件中按如下格式写入

```tex
@Misc{biohit,
author = {BioHit},
title = {
   {TongueImageDataset}},
howpublished={\url{https://github.com/BioHit/TongeImageDataset}},
year={2014},
}
```
内容替换为自己需要的内容即可但是这样做之后会在参考文献出现间距过大的问题
![img](https://img-blog.csdnimg.cn/20200413195240801.png)

这时在你的宏定义下边加入一段又丑又长的内容即可

```latex
\def\UrlBreaks{\do\A\do\B\do\C\do\D\do\E\do\F\do\G\do\H\do\I\do\J
\do\K\do\L\do\M\do\N\do\O\do\P\do\Q\do\R\do\S\do\T\do\U\do\V
\do\W\do\X\do\Y\do\Z\do\[\do\\\do\]\do\^\do\_\do\`\do\a\do\b
\do\c\do\d\do\e\do\f\do\g\do\h\do\i\do\j\do\k\do\l\do\m\do\n
\do\o\do\p\do\q\do\r\do\s\do\t\do\u\do\v\do\w\do\x\do\y\do\z
\do\.\do\@\do\\\do\/\do\!\do\_\do\|\do\;\do\>\do\]\do\)\do\,
\do\?\do\'\do+\do\=\do\#}
```
宏定义加URL断行如下
![img](https://img-blog.csdnimg.cn/20200413195522320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NTU4NTU0,size_16,color_FFFFFF,t_70)最后正常的引用结果：

![img](https://img-blog.csdnimg.cn/20200413195633160.png)

## [bibtex to cite a web page](https://tex.stackexchange.com/questions/3587/how-can-i-use-bibtex-to-cite-a-web-page)

A simple way of doing it in BibTeX is with a `@misc` entry:

```tex
@misc{WinNT,
  title = {{MS Windows NT} Kernel Description},
  howpublished = {\url{http://www.808multimedia.com/winnt/kernel.htm}},
  note = {Accessed: 2010-09-30}
}
```

You should also perhaps include an author if you know it. And remember to load a package such as `hyperref` or `url`.

------

If you are [using BibLaTeX](https://tex.stackexchange.com/questions/5091/what-to-do-to-switch-to-biblatex) there is an `@online` entry type:

```tex
@online{WinNT,
  author = {MultiMedia LLC},
  title = {{MS Windows NT} Kernel Description},
  year = 1999,
  url = {http://web.archive.org/web/20080207010024/http://www.808multimedia.com/winnt/kernel.htm},
  urldate = {2010-09-30}
}
```

**Maybe the [bibalias package](http://absatzen.de/#YmliYWxpYXM=) will help. I had a similar problem merging several databases, of course you can never remove a key or your old documents will break.**

## [Bibtex crossref field](https://tex.stackexchange.com/questions/401138/what-is-the-bibtex-crossref-field-used-for)

```tex
\documentclass{article}
\usepackage[mincrossrefs=99]{biblatex}

\usepackage{filecontents}
\begin{filecontents*}{\jobname.bib}
@inproceedings{duck2015,
    author = {Duck, D.},
    title = {Duck tales},
    crossref = {ICRC2015},
}

@inproceedings{mouse2015,
    author = {Mouse, M.},
    title = {Mouse stories},
    crossref = {ICRC2015},
}

@proceedings{ICRC2015,
    title = "{Proceedings of the 34\textsuperscript{th} International Cosmic Ray Conference}",
    year = "2015",
    month = aug,
}
\end{filecontents*}
\addbibresource{\jobname.bib}

\begin{document}
\cite{duck2015}
\cite{mouse2015}

\printbibliography
\end{document}
```

## [several reference name?](https://tex.stackexchange.com/questions/5248/can-a-bibtex-entry-be-given-more-than-one-reference-name)

Could you use crossref?

```tex
 @ARTICLE{Smith2006:AGreatTitle,
    crossref = {Smith:AGreatTitle}
 }
 @ARTICLE{Smith:AGreatTitle,
     title = {A Great Title},
     author = {Smith, John P.},
     journal = {The Coolest Ever},
     year = {2006},
     volume = {15},
     pages = {31--65}
 }
```

That seems to work decently well with some .bst's (though I haven't tried them all). However, you'll need to make sure that you don't use both `\cite{Smith:AGreatTitle}` and `\cite{Smith2006:AGreatTitle}` in the same document, though, or else you'll get duplicate entires in the Bibliography.





# 模板

https://github.com/qinnian/LaTeX





# 安装

[从零入门](https://www.zhihu.com/question/62943097/answer/203670095)

- https://liam.page/texlive/

## 关于 CTeX

### CTeX 套装

CTeX 套装是科学院吴凌云研究员的个人作品。在 CTeX 套装刚刚问世之时，因其解决了繁琐的中文字体安装工作，所以广受欢迎。但是，一方面 CTeX 套装已经很久不更新，内里的宏包、工具陈旧；另一方面，随着 XeLaTeX 的发展，以及 xeCJK 等技术的成熟，上述这些繁琐的工作已经没有必要而失去意义；因此，现在不推荐使用 CTeX 套装。

> 不要安装和使用 CTeX 套装！

### CTeX 宏集

虽然它的名字也是「CTeX」，但是 CTeX 宏集和 CTeX 套装是两个不同的东西。CTeX 宏集是集成了中文支持、操作系统判定、字体选择、版式预设为一体的一组宏包和文档类的合集。我们推荐在任何情况下，优先使用 CTeX 宏集处理中文。

> 请在任何情况下优先使用 CTeX 宏集在 LaTeX 中处理中文！

## 关于 TeX Live

TeX Live 是 TUG (TeX User Group) 维护和发布的 TeX 系统，可说是「官方」的 TeX 系统。我们推荐任何阶段的 TeX 用户，都尽可能使用 TeX Live，以保持在跨操作系统平台、跨用户的一致性。TeX Live 的官方站点是 https://tug.org/texlive/。

### Mac 用户

TeX Live 在 macOS/OS X 上的名字是 MacTeX，它的官方站点是 https://tug.org/mactex。

## REFERENCES

1. https://liam.page/texlive/
2. https://liam.page/2014/09/08/latex-introduction/
