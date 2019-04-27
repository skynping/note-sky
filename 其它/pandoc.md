[TOC]
<h1><center>pandoc使用教程</center></h1>

### 1.pandoc
```markdown
	Pandoc是一个用于从一种标记格式转换为另一种的Haskell库，还是一个使用该库的命令行工具。它可以读取 markdown格式和Textile格式（的子集）、reStructuredText格式、HTML格式、以及LaTeX格式；而且它可以写成纯文本、markdown格式、reStructuredText格式、HTML格式、LaTeX格式、ConTeXt格式、RTF格式、DocBook XML格式、OpenDocument XML格式、ODT格式、GNU Texinfo格式、MediaWiki markup格式、EPUB格式、Textile格式、groff man页面、Emacs Org-Mode格式、以及Slidy格式或S5格式的HTML幻灯片显示。
	Pandoc的markdown增强版包括的语法有：脚注、表格、灵活有序列表、定义列表、分隔的代码块、上标、下标、删除线、标题块、自动目录、嵌入式LaTeX数学符号、引用、以及将HTML标记内的块元素转化为markdown格式。（在下面的Pandoc的markdown格式小节下描述了这些增强语法，还可以使用--strict选项将其禁用。）
	同大多数用于从markdown格式转换为HTML格式的现有工具不同的是，那些工具都使用了正则替换，而Pandoc具有模块化设计：它由一系列读出器和一系列编写器组成的，读出器用于以给定格式分析文本并生成一份此文档的本地表示，编写器则用于将这份本地表示转换为目标格式。因此，增加某种输入或输出格式只需要增加一个读出器或编写器就可以了。
```

### 2.使用
```markdown
# 打开终端窗口，windows下打开cmd
# 小试牛刀，将input.txt文件转换为output.html文件。-o参数表示输出文件
pandoc -o output.html input.txt

-f: 指定输入格式，比如docx、epub、md、html等
-t: 指定输出格式，比如docx、epub、md、html等
-o: 输出到file文件
--verbost: 显示详细调试信息
--log： 指定输出日志信息

--list-input-formats：列出支持的输入格式。
--list-output-formats：列出支持的输出格式。
--list-extensions：列表支持Markdown扩展，后面跟一个+或者-说明是否在pandoc的Markdown中默认启用。
--list-highlight-languages:列出语法突出显示支持的语言。
--list-highlight-styles:列出支持语法高亮的样式。。
-v: 打印版本信息。
-h：显示语法帮助
```

### 3.范例
```markdown
# 一些例子
# 获取网页内容，并将其转换为markdown格式
pandoc -f html -t markdown http://www.fsf.org

# 将input.txt文件作为markdown输入，转换为latex
pandoc -f markdown -t latex input.txt

# 如果未指定-f、-t，pandoc则会根据输入文件输出文件的后缀来转换
pandoc input.txt -o output.pdf

# pandoc要求输入输出使用utf8编码，可以使用iconv命令进行编码转换
iconv -t utf-8 input.txt | pandoc | iconv -f utf-8

# 如果需要使用tex生成pdf文件，则需要下载latex编译器  
```


