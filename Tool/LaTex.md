### LaTex 常用语法

行内公式： $ 1+1=2 $

行间公式：

$$ 2-2=0 $$

乘号： $ 3 \times 3=9 $

除号： $ 9 \div 3=3 $

带余除法： $ 9 \div 2=4 \cdots\cdots 1 $

分数： $ \frac 2 3 , \frac{2+2}{3 \times 8} $

比较： $ a \gt b $

下标：$ x_1, x_2 $

幂： $ m^2, a^3b^4 $

开方： $ \sqrt 9 \sqrt[3]{999} $

方程组：$ \left\{ \begin{array}{lr}
3x + 2y = 11 \\
x - y = 1
\end{array} \right. $

求和：$ \sum\_{i=1}^{10} t_i \displaystyle\sum_0^n $

区间：$ x \in [-1,1) $

矢量：$ \vec a $ $ \overrightarrow{AB} $

特殊符号：$ \Box \triangle \angle \Diamond \heartsuit \to $

插入图片：$\includegraphics[width=20px]{http://qiniu.lookingedu.com/WechatIMG106.jpeg1525852401589}$
![](http://qiniu.lookingedu.com/WechatIMG106.jpeg1525852401589)

\begin{figure}
\centering
\begin{subfigure}
\includegraphics[width=20%]{http://qiniu.lookingedu.com/emhpbW9fcXJjb2RlLnBu1527139636292}
\label{图(1)}
\end{subfigure}
\begin{subfigure}
\includegraphics[width=20%]{http://qiniu.lookingedu.com/emhpbW9fcXJjb2RlLnBu1527139636292}
\label{图(2)}
\end{subfigure}
\end{figure}

竖式：

$$
\begin{array}
{}&3&3\\
\times &{}&3\\
\hline
{}&9&9
\end{array}
$$

短除法：

$$
\def\sdiv#1#2{#1\ \raisebox{0.1em}| \!\underline{#2}\\}
\begin{array}{r}
\sdiv{2}{\ 12 \quad 18}
\sdiv{3}{\ 6 \quad \ \ 9}
2 \quad \ \ 3
\end{array}
$$

表格：
$$\begin{array}{|c|c|c|c|c|c|}\hline年龄\left(岁\right)&12&13&14&15&16\\\hline人数&1&4&3&7&5\\\hline\end{array}$$

矩阵：

$$
\begin{matrix}
   a & b \\
   c & d
\end{matrix}
$$

条件判断

$$
\begin{cases}
   a &\text{if } b \\
   c &\text{if } d
\end{cases}
$$

$$
\left\{\begin{array}{l}
   a b c\\
   d e \\
   f
\end{array}\right.
$$

$$
\left.\begin{array}{r}
   a b c\\
   d e \\
   f
\end{array}\right\}
$$

[Supported Functions](https://katex.org/docs/supported.html)
