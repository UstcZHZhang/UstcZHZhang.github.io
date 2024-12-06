---
title:	"Texlive和Tex studio的安装及使用"
layout: post
categories: Config_Log
---

### 1. Latex介绍

LaTeX基于TeX，主要目的是为了方便排版。在学术界的论文，尤其是数学、计算机等学科论文都是由 LaTeX编写, 因为用它写数学公式非常漂亮。

在稍微了解一点LaTeX后，你会发现LaTeX的工作方式类似web page，都是由源文件（.tex or .html）经由引擎（TeX or browser）渲染产生最终效果（得到PDF文件或者生成页面）。两者极其神似，包括语法规则与工作方式。所以呢，与 HTML一样，入门其实很简单。

![image-20230429160238382](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429160238382.png)

一般的规范写法中都是在HTML文件中写入web page的结构与内容，再由css控制页面生成的样式。当然你也可以选择在HTML中直接写入样式内容，不过这并不提倡。同样，在LaTeX有着同样的情况，你可以在tex源文件中同时写入内容和样式，也可以内容与样式分离，以网络上流传广泛的清华大学LaTeX模板为例，以.cls(class)结尾的thuthesis.cls便可看作是与css起到同样作用的样式文件。

LaTeX有所谓宏包的概念，`\usepackage{foo}` 即可使用宏包foo中定义的内容。所谓宏包就是一些写好的内容打包出来以便大家使用而已。这跟C语言的include是一致的，将文件加载进来进行使用。利用宏包，我们可以使用很多现成的好用的样式。当然了，如果要编写一个自己的个性化的宏包也是可以的，不过需要学习成本。

初期的话，我们可以选择一个LaTeX模板进行改造。不过第一次见到一些模板，可能会对其中很多文件的作用一头雾水. 下面是简单的介绍。

| LaTeX模板常见文件类型 | 功能简要介绍                                               |
| --------------------- | ---------------------------------------------------------- |
| .dtx                  | Documented LaTeX sources，宏包重要部分                     |
| .ins                  | installation，控制TeX从.dtx文件里释放宏包文件              |
| .cfg                  | config， 配置文件，可由上面两个文件生成                    |
| .sty                  | style files，使用`\usepackage{…}`命令进行加载              |
| .cls                  | classes files，类文件，使用`\documentclass{…}`命令进行加载 |
| .aux                  | auxiliary， 辅助文件，不影响正常使用                       |
| .bst                  | BibTeX style file，用来控制参考文献样式                    |

LaTex使用安装，主要要安装两样东西

1. 根据平台选择一个**TeX发行版** 进行安装，建议选择最全功能最多的版本。

   TeX发行版的概念相当于Linux及其发行版，Linux内核虽然只有一个，但是有很多基于内核的不同特色的Linux发行版，Ubuntu，Fedora等等不胜枚举。

2. 选择一个合适的**LaTex编辑器**

   在安装好LaTeX环境以后，通常都会有一个自带的编辑器，比如CTex的WinEdt，MacTeX的TeXShop，不过功能并不强大，好比Windows记事本，只有一些基本的文本编辑功能。

我们这里采用Texlive（Tex发行版）+Tex studio（Latex编辑器）。

---

### 2. Texlive的下载和安装

#### 2.1 下载

推荐使用离线安装包进行安装的方式。

进入[TeX Live官方下载网址](https://tug.org/texlive/)，按以下操作进行：

![image-20230429162033649](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429162033649-1682756442262-2.png)

![image-20230429162201648](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429162201648.png)

![image-20230429162312864](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429162312864.png)

![image-20230429162524487](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429162524487.png)

官方下载可能比较慢，也可以利用[清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)进行下载：

![image-20230429162753269](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429162753269.png)

#### 2.2 安装

打开iso文件，按以下操作进行：

![image-20230429164154439](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429164154439.png)

![image-20230429164414447](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429164414447.png)

![image-20230429164623648](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429164623648.png)

接下来要装比较久的时间，耐性等待。

安装结束后，可以在终端中输入`tex -version`，出现版本号即安装成功。

![image-20230429172611307](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429172611307.png)

---

### 3. Texstudio的安装以及简单使用

#### 3.1 安装

去[TeXstudio官网](https://www.texstudio.org/)下载安装程序，按以下操作进行：

![image-20230429172944367](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429172944367.png)

![image-20230429173304102](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429173304102.png)

#### 3.2 设置中文界面

安装结束后，一开始的打开界面是英文的，这里我们可以切换成中文。

依次点击：`Options`，`Configure Texstudio`，`General`，`Language`，`zh_CN`

![image-20230429174928953](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429174928953.png)

#### 3.3 添加行号

添加段落行号，这样可以很方便查看段落的某句话所在的位置，尤其是在运行报错时，有行号就非常方便查看错误的位置了。

依次点击：`选项`，`设置Texstudio`，`编辑器`

![image-20230429175529802](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429175529802.png)

#### 3.4 设置编译器与编码

为了正常的输出中文，我们需要把编译器改成xelatex，utf-8编码
如果是为了编写英文论文的，那就下面第一张图不要改成“xelatex”，英文论文要用“pdflatex”

![image-20230429175714981](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429175714981.png)

![image-20230429175725411](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429175725411.png)

#### 3.5 第一个简单程序

新建一个 .tex 文件后，将以下代码复制粘贴进去，然后，点击下图中的构建并查看

```latex
% 导言区
\documentclass{article} 

% 导入中文宏
\usepackage{ctex}

% 构建命令,取别名，使用degree 代替 ^ circ
\newcommand\degree{^\circ}

\title{\heiti 浅谈勾股定理}
\author{\kaishu Lossfun}
\date{\today}
% 正文区
\begin{document}
    \maketitle
    hello world!
    
    勾股定理可以用现代的语言描述如下：
    
    直角三角形斜边的平方等于两腰的平法和。
    
    可以用符号语言描述为：设直角三角形 
    $\angle C=90\degree $则有：
    $$ 
    	AB^2 = BC^2 + AC^2 
    $$
    这就是勾股定理
\end{document}
```

![image-20230429175935507](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429175935507.png)

![image-20230429175946689](/images/ConfigLog/2023-04-29-Texlive和Tex studio的安装及使用_images/image-20230429175946689.png)

主要参考：

1. [ LaTeX学习：Texlive 2019和TeX studio的安装及使用_Mikchy的博客-CSDN博客](https://blog.csdn.net/Mikchy/article/details/94448707)





