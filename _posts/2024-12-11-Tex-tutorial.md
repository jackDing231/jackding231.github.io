---
layout:     post
title:      "浅谈LaTex语法"
subtitle:   "LaTex Tutorial"
date:       2024-12-11
author:     "Jack Ding"
header-style: text
mathjax: true
tags:
    - LaTex
    - tutorial
---

# 什么是LaTex

LaTeX是一种基于ΤΕΧ的排版系统，由美国计算机学家莱斯利·兰伯特（Leslie Lamport）在20世纪80年代初期开发，利用这种格式，即使使用者没有排版和程序设计的知识也可以充分发挥由TeX所提供的强大功能，能在几天、甚至几小时内生成很多具有书籍质量的印刷品。对于生成复杂表格和数学公式，这一点表现得尤为突出。因此它非常适用于生成高印刷质量的科技和数学类文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。

# Hello LaTeX

最简单的LaTex模板如下：

```tex
\documentclass{article}
\begin{document}
Hello LaTeX
\end{document}
```

一般来说，documentclass{article}文章的类型可以设置为article，book等。

# LaTeX框架

## 中文

如果需要使用中文，则需要使用中文包（`\usepackage{ctex}`）。同时，还需要注意，排版时需要使用`XeLaTeX`。

```tex
\documentclass{article}
\usepackage{ctex}
\begin{document}
你好，LaTeX
\end{document}
```

## 空格

`LaTeX`默认是忽略文字之间的空格的（比如’你好 啊！'和’你好啊！'是一样的效果），需要支持空格的话，你有三种方式：

- 你{ }好啊！
- 你\ 好啊！

- \usepackage[space]{ctex}

一般来说，最后一种方式是比较常见的。

## 简单的规则

- 换行：用控制命令"\\\\"，或"\newline"
- 分段：用控制命令"\par"或空出一行
- 换页：用控制命令"\newpage"或"\clearpage"

## 字体

\rm 罗马字体 \it 意大利字体
\bf 黑体 \sl 倾斜体
\sf 等线体 \sc 小体大写字母
\tt 打字机字体 \mit 数学斜体

## 设置A4

这里设置纸张大小，字体大小，一个最简单的LaTeX模板就制作完成了。

```tex
\documentclass[11pt,a4paper]{article}
\usepackage[space]{ctex}
\begin{document}
你好！LaTeX。
\end{document}
```

## 标题、作者以及日期

当然，如果你不想显示日期的话可以使用`\date{}`花括号中留空。注意`maketitle`是将title、author、date等全部显示出来，如果没有这一句，则上面的设置全部不起作用。

```tex
\documentclass[11pt, a4paper]{article}
\usepackage[space]{ctex}
\title{LaTeX快速入门}
\author{qingdujun}
\date{\today}
\begin{document}
\maketitle
你好！LaTeX。
\end{document}
```

## 脚注

比如，这里给LaTeX添加一个脚注（使用`\footnote`指令）。脚注会显示在本页的左下角，并且以横线与正文隔开。

```tex
你好！LaTeX\footnote{LaTeX是一个与Word比肩，甚至更好的工具}。
```

## 标题级别

不同的文章类型标题级别不完全相同（比如，如果为`book`的话还有`chapter`级别），以下为`article`的几种标题级别。

```tex
\part{part标题}
\section{section标题}
\subsection{subsection标题}
\subsubsection{subsubsection标题}
\paragraph{paragraph标题}
\subparagraph{subparagraph标题}
```

## 插入图片

当然，下面这个是最简单的scale是缩放尺寸。

```tex
\includegraphics[scale=0.6]{latex.png}
```

更复杂的浮动设置，如下。

```tex
\begin{figure}[h]
\begin{center}
\includegraphics[scale=0.8]{test_demo.jpg}
\end{center}
\caption{该图显示了一个人的测试示例。 它表明我们的系统跟踪人进入房间时的姿势，甚至当他完全被遮挡在墙后时。}
\label{fig:test_demo}
\end{figure}
```

注意，scale为图片缩放比例，caption为位于图片下面的那行描述，label为引用图片的标签（可以随意设置，一般设置和图片名一致好记）。

```tex
\ref{fig:test_demo}
```

在文中可以这样引用，“人体姿态估计，就是将一幅图像或一段视频中，人的头、手、躯干和腿部关节点位置恢复出来，做出一个由关节点构成的骨架如图\ref {fig:test_demo}所示。”

## 代码片段

前提需要引入宏包`\usepackage{listings}`，注意将language设置成目标语言类型。

```tex
\begin{lstlisting}[language={Python}]
#Layer3 - Convolution
with tf.variable_scope('layer3-conv2'):
        conv2_weights = tf.get_variable('weight',[5,5,6,16],initializer
            =tf.truncated_normal_initializer(stddev=0.1))
        conv2_biases = tf.get_variable('bias',[16],initializer
            =tf.constant_initializer(0.0))
        conv2 = tf.nn.conv2d(pool1,conv2_weights,
            strides=[1,1,1,1],padding='VALID')
        relu2 = tf.nn.relu(tf.nn.bias_add(conv2,conv2_biases))
\end{lstlisting}
```

## 字号

字号从小到大依次为：

\tiny 2. \scriptsize 3. \footnotesize 4. \small 5. \normalsize
\large 7. \Large 8. \LARGE 9. \huge 10. \Huge

```tex
{\color{red}LaTeX}由美国计算机学家{\kaishu{莱斯利·兰伯特}在\underline{20世纪80年代}初期开发。
```

## 无序列表

注意，这里的textbf作用为使字体加粗显示。

```tex
\begin{itemize}
\item \textbf{上图}：由与无线电传感器共同定位的相机拍摄的图像，并在此处显示以供视觉参考。
\item \textbf{中间}：仅从RF信号中提取的关键点置信度图，没有任何视觉输入。
\item \textbf{底部}：从关键点置信度图解析的骨架，表明即使存在完全遮挡，我们也可以使用RF信号来估计人体姿势。
\end{itemize}
```

## 模板（麻雀虽小五脏俱全）

将以上的知识点总结一下，可以整理出以下这个模板。一般排版，交作业基本上也就用到这些知识点了。另外关于数学公式，后文专门介绍。（注意，这里需要使用XeLaTeX生成排版）。

补充：在LaTeX中，%符号后面的文字将被视为注释内容。

```tex
\documentclass[11pt, a4paper]{article}
\usepackage[space]{ctex}
\title{LaTeX快速入门}
\author{qingdujun}
\date{\today}
\begin{document}
\maketitle
你好！LaTeX\footnote{LaTeX是一个与Word比肩，甚至更好的工具}。
%\includegraphics[scale=0.6]{latex.png}
\part{part标题}
\section{section标题}
\subsection{subsection标题}
\subsubsection{subsubsection标题}
\paragraph{paragraph标题}
\subparagraph{subparagraph标题}
\begin{thebibliography}{99}
\bibitem{1} 参考文献1
\bibitem{2} 参考文献2
\end{thebibliography}
\begin{appendix}
\section{附录1}
\section{附录2}
\end{appendix}
\end{document}
```

# 数学公式

框架介绍完毕之后，相信你对LaTeX有了个基本的了解了。接下来介绍LaTeX的强项——对数学公式的排版。

## 常见的公式写法

行内公式一般写法为$我是公式内容$，就是用美元符号夹住。单行公式这些用双美元符号夹住，比如$$我是公式内容$$。

### 行内公式

```tex
大家好，我是$a^2+b^2=c^2$行内公式。
```

大家好，我是$a^2+b^2=c^2$行内公式。

### 单行公式

```tex
强势写一波勾股定理$$3^2+4^2=5^2$$其中，吧啦吧啦！
```

强势写一波勾股定理$$3^2+4^2=5^2$$其中，吧啦吧啦！

## 公式编号

美元符号写法公式不能编号，这里再给出一种写法。

```tex
\begin{equation}
1+2+3+\dots+(n-1)+n = \frac{n(n+1)}{2}
\end{equation}
```

$$
\begin{equation}
1+2+3+\dots+(n-1)+n = \frac{n(n+1)}{2}
\end{equation}
$$

如果不需要编号，也可以写么写——后面带个星号即可。

```tex
\begin{equation*}
1+2+3+\dots+(n-1)+n = \frac{n(n+1)}{2}
\end{equation*}
```

$$
\begin{equation*}
1+2+3+\dots+(n-1)+n = \frac{n(n+1)}{2}
\end{equation*}
$$

## 其他

```tex
a^{2 \atop {3 \substack 4}}+b^2=c^2
```

$$
a^{2 \atop {3 \substack 4}}+b^2=c^2
$$

```tex
\sideset{^a_b}{^c_d} \prod ^e_f
```

$$
\sideset{^a_b}{^c_d} \prod ^e_f
$$

```tex
\underset{e}{\overset{f}{_a^bM_c^d}}
```

$$
\underset{e}{\overset{f}{_a^bM_c^d}}
$$

```tex
A=\overbrace{(a+b)+\underbrace{(c+d)i}_{\text{虚数}}}^{\text{复数}}+(e+f)+\underline{(g+h)}
```

$$
A=\overbrace{(a+b)+\underbrace{(c+d)i}_{\text{虚数}}}^{\text{复数}}+(e+f)+\underline{(g+h)}
$$

```tex
\frac{1}{2+\frac{1}{3+\frac{1}{4+\dots}}}
```

$$
\frac{1}{2+\frac{1}{3+\frac{1}{4+\dots}}}
$$

```tex
\sum_{i=1}^{n}i=\frac{n(n+1)}{2}
```

$$
\sum_{i=1}^{n}i=\frac{n(n+1)}{2}
$$

```tex
\lim_{x\rightarrow{\infty}}(1+\frac{1}{x})^{x}=e
```

$$
\lim_{x\rightarrow{\infty}}(1+\frac{1}{x})^{x}=e
$$

```tex
\int_{a}^{b}f(x)dx=F(b)-F(a)
```

$$
\int_{a}^{b}f(x)dx=F(b)-F(a)
$$

```tex
\frac{\partial f(x)}{\partial x}=x^2
```

$$
\frac{\partial f(x)}{\partial x}=x^2
$$

```tex
\left[
\begin{array}{cc|c}
1&1&1 \\ 
2&2&2 \\  \hline
3&3&3 
\end{array}
\right]
```


$$
\left[
\begin{array}{cc|c}
1&1&1 \\ 
2&2&2 \\  \hline
3&3&3 
\end{array}
\right]
$$

# 参考资料

1. [LaTeX快速入门：一文浅谈TeX排版语法_tex语法-CSDN博客](https://blog.csdn.net/qingdujun/article/details/80805613)

