---
title: LaTeX common symbols and use
---
[//]: # (  <span style="color: #589bd5; font-weight: bold;">LaTeX symbols and Use</span>)

<h1 style="text-align: center;">
  <span style="color: #1B365D; font-weight: bold;">LaTeX symbols and Use</span>
</h1>

---

## **Preamble**

LaTeX 是一种强大的数学排版工具，在 **Markdown、Jupyter Notebook、Obsidian、Typora** 等支持 LaTeX 的环境中，我们可以使用 LaTeX 语法来编写数学公式。

[//]: # (LaTeX 是一种强大的数学排版工具，它不仅可以为学术论文、报告以及其他专业文档提供高质量的排版支持，也为我们展示数学公式和符号提供了极为精确的控制。无论是在 Markdown、Jupyter Notebook、Obsidian、Typora 等支持 LaTeX 的环境中，还是在专业的学术出版物中，LaTeX 都发挥着不可或缺的作用。)
这份笔记是我在学习和整理 LaTeX 过程中所收集和归纳的常用符号、命令以及排版技巧。
<span style="color: hsl(9, 100%, 60%);">请随意使用以下笔记内容，它不仅仅是我个人的整理，也是希望能够帮助更多的 LaTeX 使用者在日常工作和学习中，快速高效地解决数学公式排版的需求。
</span>
<span style="color: #589bd5;  font-weight: bold;">
<a href="https://hinmouc.github.io/">
More in @hinmouc
</a>
</span>

* * *

## 1 Basci

### 1.1 Inline & Display Mode

LaTeX 提供了 **两种模式** 来书写数学公式：

1. **行内公式（Inline Mode）**：使用 **`$...$`** 语法
2. **独立公式（Display Mode）**：使用 **`\[...\]`** 或 **`$$...$$`** 语法

#### Inline Formula

行内公式用于**正文中插入数学表达式**，不会单独占据一行。 

**Example**

```markdown
文本文本...... $ a^2 + b^2 = c^2 $ ......文本文本
```

**Rendered Output**： 文本文本...... $ a^2 + b^2 = c^2 $ ......文本文本

* * *

#### Display Formula

独立公式会**单独占一行**，并居中显示。  

**Example**

```markdown
\[
a^2 + b^2 = c^2
\]
```

or：

```markdown
$$
a^2 + b^2 = c^2
$$
```

**Rendered Output**：

$$a^2 + b^2 = c^2$$

* * *

### 1.2 Superscript & Subscript

在 LaTeX 公式中，我们使用 **`^`（上标）** 和 **`_`（下标）** 来表示 **指数、角标等**。

#### Superscript

使用 `^` 表示 **上标（指数）**：

```markdown
$ a^2, x^{10}, e^{x+y} $
```

**Rendered Output**： $a^2, x^{10}, e^{x+y}$

* * *

#### Subscript

使用 `_` 表示 **下标（角标）**：

```markdown
$ a_1, x_{i+1}, H_{2}O $
```

**Rendered Output**： $a_1, x_{i+1}, H_{2}O$

* * *

#### **--Note--**

✅ `^` 和 `_` 仅对其后**紧邻的一个字符生效**，如果包含多个字符，需 **使用大括号 `{}` 包裹**：

```markdown
$ x^10 $ （正确 ✅）
$ x^{10} $（正确 ✅）
$ x^10_2 $ （正确 ✅）
$ x^10_2 + x_3^{n+1} $（正确 ✅）

$ x^10_2n $ （错误 ❌，因为 `_2n` 没有 `{}` 包裹）
```

**Rendered Output**： $x^{10}, x_2^{10}, x_{3}^{n+1}$

* * *

### 1.3 Equation Environments

在 LaTeX 中，数学公式可以放在不同的 **数学环境** 里，以获得更好的格式化效果。常见的数学环境包括：`equation`、`align`、`cases`、`multline` 和 `split`。

| Environment | Description |                                      Example                                       |
|:-----------:|:---:|:----------------------------------------------------------------------------------:|
|  equation   | 单行公式，自动编号 |                     $\begin{equation} E = mc^2 \end{equation}$                     |
|    align    | 多行公式，支持对齐，使用 `&` 对齐点 |                  $\begin{align} E &= mc^2 \\ F &= ma \end{align}$                  |
|    cases    | 适用于分段函数的定义 |  $\begin{cases} x^2, & \text{if } x \geq 0 \\ -x, & \text{if } x < 0 \end{cases}$  |
|  multline   | 多行公式，自动换行，长公式分段显示 |                 $\begin{multline} x = y + z \\ + w \end{multline}$                 |
|    split    | 在 `equation` 环境中拆分长公式，适合长公式的排版 | $\begin{equation} \begin{split} x &= y + z \\ &= w + t \end{split} \end{equation}$ |

#### 1. `equation` Environment

* `equation` 环境会自动为**公式编号**，适用于 **长篇文档**。

```markdown
\[
\begin{equation}
E = mc^2
\end{equation}
\]
```

**Rendered Output**：

$$\begin{equation}  
E = mc^2  
\end{equation}$$

* * *

#### 2. `align` Environment

* `align` 用于**对齐多行公式**，`&` 代表对齐点。

```markdown
\[
\begin{aligned}
x &= y + 2z \\
  &= 3y - 4
\end{aligned}
\]
```

**Rendered Output**：

$$\begin{aligned}  
x &= y + 2z \\  
&= 3y - 4  
\end{aligned}$$

* * *

#### 3. `cases` Environment

* `cases` 适用于**分段定义的数学函数**。

```markdown
\[
f(x) =
\begin{cases}
x^2, & \text{if } x \geq 0 \\
-x, & \text{if } x < 0
\end{cases}
\]
```

**Rendered Output**：

$$f(x) =  
\begin{cases}  
x^2, & \text{if } x \geq 0 \\  
-x, & \text{if } x < 0  
\end{cases}$$

* * *

#### 4. `multline` Environment

* `multline` 用于**多行公式**，并将公式分行显示，第一个可用位置作为换行点。

```markdown
\[ 
\begin{multline} 
x = y + z + w + t \\ 
+ a + b + c 
\end{multline} 
\] 
```

**Rendered Output**：

$$\begin{multline} 
x = y + z + w + t \\ + a + b + c 
\end{multline} 
$$

* * *

#### 5.  `split` Environment

* `split` 用于将公式**拆分成多行**，并与 `equation` 环境结合，适用于拆分长公式。

```markdown
\[
\begin{equation} 
\begin{split} 
x &= y + z \\ &= w + t 
\end{split} 
\end{equation}
\]
```

**Rendered Output**：

$$\begin{equation} 
\begin{split} 
x &= y + z \\ &= w + t 
\end{split} 
\end{equation}$$

* * *


### 1.4 Spacing in Math Mode

LaTeX 提供了一些**空格控制命令**，用于调整符号之间的间距。

|        Symbol         | Command | Symbol | Command | Symbol | Command |
|:---------------------:|:---:|:---:|:---:|:---:|:---:|
|      $a \quad b$      | a \quad b | $a \, b$ | a \, b | $a \! b$ | a \! b |
| $x = \text{constant}$ | x = \text{constant} | $a \; b$ | a \; b | $a \: b$ | a \: b |

**Example:**

```latex
\[
    a \quad b, \quad a \, b, \quad a \! b
\]
```

**Rendered Output**:

$$ a \quad b, \quad a \, b, \quad a \! b$$

* * *

#### **Inline Text in Math Mode**

在数学模式中，使用 `\text{}` 来插入普通文本，以保持文本格式正确。

**Example:**

```latex
\[
    x = 5, \quad \text{where } x \text{ is the unknown variable.}
\]
```

**Rendered Output**:

$$ x = 5, \quad \text{where } x \text{ is the unknown variable.}$$

* * *


### 1.5 Using LaTeX in Markdown

在不同的 Markdown 编辑器中，LaTeX 的支持情况如下：

| **编辑器** | **支持情况** | **备注** |
| --- | --- | --- |
| Jupyter Notebook | ✅ 完全支持 | 需安装 `MathJax` |
| Typora | ✅ 完全支持 | 需开启 `Markdown 数学` |
| Obsidian | ✅ 完全支持 | 内置 `MathJax` |
| VS Code + Markdown Preview Enhanced | ✅ 完全支持 | 需安装插件 |
| GitHub Markdown | ⚠ 部分支持 | 仅支持 `$...$`，不支持 `\[\]` |

* * *

### 1.6 Test

可以尝试以下 LaTeX 代码，并查看渲染效果：

```markdown
1. $\frac{a}{b}$
2. $\sqrt{x^2 + y^2}$
3. $\sum_{i=1}^{n} i^2$
4. $\int_{0}^{\infty} e^{-x}dx$
5. $f(x) = \begin{cases} x^2, & x \geq 0 \\ -x, & x < 0 \end{cases}$
```

* * *



## 2 Greek Letters.
Greek letters are widely used in mathematics, physics, and engineering. The following table provides the common lowercase and uppercase Greek letters in LaTeX.

### Lowercase Greek Letters.

|   Symbol    |  Command  |   Symbol   | Command  |    Symbol     |   Command   |
| :---------: | :-------: | :--------: | :------: | :-----------: | :---------: |
|  $\alpha$   |  \alpha   |  $\beta$   |  \beta   |   $\gamma$    |   \gamma    |
|  $\delta$   |  \delta   | $\epsilon$ | \epsilon | $\varepsilon$ | \varepsilon |
|   $\zeta$   |   \zeta   |   $\eta$   |   \eta   |   $\theta$    |   \theta    |
| $\vartheta$ | \vartheta |  $\iota$   |  \iota   |   $\kappa$    |   \kappa    |
|  $\lambda$  |  \lambda  |   $\mu$    |   \mu    |     $\nu$     |     \nu     |
|    $\xi$    |    \xi    |   $\pi$    |   \pi    |   $\varpi$    |   \varpi    |
|   $\rho$    |   \rho    | $\varrho$  | \varrho  |   $\sigma$    |   \sigma    |
| $\varsigma$ | \varsigma |   $\tau$   |   \tau   |  $\upsilon$   |  \upsilon   |
|   $\phi$    |   \phi    | $\varphi$  | \varphi  |    $\chi$     |    \chi     |
|   $\psi$    |   \psi    |  $\omega$  |  \omega  |               |             |

### Uppercase Greek Letters.

|  Symbol   | Command |   Symbol   | Command  |  Symbol  | Command |
| :-------: | :-----: | :--------: | :------: | :------: | :-----: |
| $\Gamma$  | \Gamma  |  $\Delta$  |  \Delta  | $\Theta$ | \Theta  |
| $\Lambda$ | \Lambda |   $\Xi$    |   \Xi    |  $\Pi$   |   \Pi   |
| $\Sigma$  | \Sigma  | $\Upsilon$ | \Upsilon |  $\Phi$  |  \Phi   |
|  $\Psi$   |  \Psi   |  $\Omega$  |  \Omega  | $\aleph$ | \aleph  |
|  $\beth$  |  \beth  | $\daleth$  | \daleth  | $\gimel$ | \gimel  |


有代码的大写希腊字母，直接敲获得正体，使用`\var`前缀转化为斜体

如：`\Gamma` $\Gamma$（正） `\varGamma` $\varGamma$（斜）

没有代码的大写希腊字母，直接敲得斜体，使用`\text `命令转化为正体

如：`T` $T$直接敲（斜） `\text T` $\text T$（正）

（也可以使用`\rm`将下一个单词变正，`\text T`的作用范围只是下一个字母；可以尝试加`{}`）



***


## 3 Math Mode Accents
在 LaTeX 数学模式中，我们可以使用 **重音符号** 来表示：

* **导数（Newton 记号）**
* **单位向量**
* **逼近（傅里叶变换）**
* **复数共轭**
* **上下划线用于变量强调**

**Summary Table**

| Symbol | Command | Symbol | Command | Symbol | Command |
|:---:|:---:|:---:|:---:|:---:|:---:|
| $\hat{a}$ | \hat{a} | $\check{a}$ | \check{a} | $\dot{a}$ | \dot{a} |
| $\breve{a}$ | \breve{a} | $\acute{a}$ | \acute{a} | $\ddot{a}$ | \ddot{a} |
| $\grave{a}$ | \grave{a} | $\tilde{a}$ | \tilde{a} | $\mathring{a}$ | \mathring{a} |
| $\bar{a}$ | \bar{a} | $\vec{a}$ | \vec{a} | $\overrightarrow{AB}$ | \overrightarrow{AB} |
| $\overleftarrow{CD}$ | \overleftarrow{CD} | $\overline{xyz}$ | \overline{xyz} | $\underline{abc}$ | \underline{abc} |
| $\widehat{A}$ | \widehat{A} | $\widetilde{A}$ | \widetilde{A} | $\overbrace{a + b + c}$ | \overbrace{a + b + c} |
| $\underbrace{1 + 2 + \dots + n}$ | \underbrace{1 + 2 + \dots + n} |  |  |  |  |

* * *

### 3.1 Superscripts

用于：

* **单位向量**（$\hat{x}$）
* **近似表示**（$\tilde{x}$）
* **一阶/二阶导数**（$\dot{x}$, $\ddot{x}$）

| Symbol |     Command     | Description |
|:---:|:---------------:|:---:|
| $\hat{x}$ |     \hat{x}     | 单位向量 |
| $\widehat{xy}$ |  \widehat{xy}   | 大范围的帽子符号 |
| $\tilde{x}$ |    \tilde{x}    | 近似表示（如傅里叶变换） |
| $\widetilde{abc}$ | \widetilde{abc} | 大范围的波浪符号 |
| $\dot{x}$ |     \dot{x}     | 一阶导数（微分） |
| $\ddot{x}$ |    \ddot{x}     | 二阶导数（加速度） |

**Example**

```markdown
$\hat{x}, \widehat{xy}, \tilde{x}, \widetilde{abc}, \dot{x}, \ddot{x}$
```

**Rendered Output**

$$\hat{x}, \widehat{xy}, \tilde{x}, \widetilde{abc}, \dot{x}, \ddot{x}$$

* * *

### 3.2 Vector Symbols

用于：

* **物理和工程中的向量**（$\vec{v}$）
* **几何中的方向向量**（$\overrightarrow{AB}$）

| Symbol |       Command       | Description |
|:---:|:-------------------:|:---:|
| $\vec{v}$ |       \vec{v}       | 标准向量符号 |
| $\overrightarrow{AB}$ | \overrightarrow{AB} | 带方向的向量 |
| $\overleftarrow{CD}$ | \overleftarrow{CD}  | 反向向量 |

**Example**

```markdown
$\vec{v}, \overrightarrow{AB}, \overleftarrow{CD}$
```

**Rendered Output**

$$\vec{v}, \overrightarrow{AB}, \overleftarrow{CD}$$

* * *

### 3.3 Overline & Underline

用于：

* **复共轭**（$\overline{z}$）
* **变量强调**（$\underline{x}$）

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\overline{x+y}$ | \overline{x+y} | 复共轭，均值等 |
| $\underline{abc}$ | \underline{abc} | 变量下划线 |
| $\overbrace{a + b + c}^{\text{Sum}}$ | \overbrace{a + b + c}^{\text{Sum}} | 括号上标注 |
| $\underbrace{1 + 2 + \dots + n}_{\text{n-term sum}}$ | \underbrace{1 + 2 + \dots + n}_{\text{n-term sum}} | 括号下标注 |

**Example**

```markdown
$\overline{x+y}, \underline{abc}, \overbrace{a + b + c}^{\text{Sum}}, \underbrace{1 + 2 + \dots + n}_{\text{n-term sum}}$
```

**Rendered Output**

$$\overline{x+y}, \underline{abc}, \overbrace{a + b + c}^{\text{Sum}}, \underbrace{1 + 2 + \dots + n}_{\text{n-term sum}}$$


***



## 4 Math Constructs
在 LaTeX 数学模式中，可以使用各种数学结构来表示 **指数、下标、分数、根号、求和、积分、极限、对数、三角函数** 等。

* * *

### 4.1 Exponents and Subscripts

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $a^2$ | a^2 | **上标符号**，表示指数运算。 |
| $x_i$ | x_i | **下标符号**，常用于表示索引或元素的位置。 |
| $y^{m+n}$ | y^{m+n} | 上标符号，可以表示多项式中的指数。 |
| $z_{i,j}$ | z_{i,j} | 下标符号，常用于表示矩阵或多维数组中的元素。 |

**Example**

```markdown
$a^2, x_i, y^{m+n}, z_{i,j}$
```

**Rendered Output**

$$a^2, x_i, y^{m+n}, z_{i,j}$$

* * *

### 4.2 Fractions

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\frac{a}{b}$ | \frac{a}{b} | 普通分数格式，适用于行内公式。 |
| $\dfrac{a}{b}$ | \dfrac{a}{b} | 显示模式下的分数，显示更大，适合单独一行的公式。 |
| $\tfrac{a}{b}$ | \tfrac{a}{b} | 小尺寸的分数，适用于行内公式。 |

**Example**

```markdown
$\frac{a}{b}, \dfrac{a}{b}, \tfrac{a}{b}$
```

**Rendered Output**

$$\frac{a}{b}, \dfrac{a}{b}, \tfrac{a}{b}$$

* * *

### 4.3 Radicals

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\sqrt{2}$ | \sqrt{2} | **平方根**，表示 $2$ 的平方根。 |
| $\sqrt[3]{x}$ | \sqrt[3]{x} | **立方根**，表示 $x$ 的三次根。 |

**Example**

```markdown
$\sqrt{2}, \sqrt[3]{x}$
```

**Rendered Output**

$$\sqrt{2}, \sqrt[3]{x}$$

* * *

### 4.4 Summation and Integration

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\sum_{i=1}^{n} i^2$ | \sum_{i=1}^{n} i^2 | **求和符号**，表示累加运算。 |
| $\prod_{i=1}^{n} i$ | \prod_{i=1}^{n} i | **积符号**，表示累乘运算。 |
| $\int_{0}^{\infty} e^{-x}dx$ | \int_{0}^{\infty} e^{-x}dx | **积分符号**，表示对函数的积分，通常用于计算连续量。 |
| $\iint_D f(x,y)dxdy$ | \iint_D f(x,y)dxdy | **双重积分**，用于二维空间的积分。 |
| $\iiint_V f(x,y,z)dxdydz$ | \iiint_V f(x,y,z)dxdydz | **三重积分**，用于三维空间的积分。 |


**Example**

```markdown
$\sum_{i=1}^{n} i^2, \prod_{i=1}^{n} i, \int_{0}^{\infty} e^{-x}dx$
```

**Rendered Output**

$$\sum_{i=1}^{n} i^2, \prod_{i=1}^{n} i, \int_{0}^{\infty} e^{-x}dx$$

* * *

### 4.5 Limits, Logarithms, and Trigonometric Functions

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\arccos$ | \arccos | **反余弦函数**，表示角度的反函数。 |
| $\arcsin$ | \arcsin | **反正弦函数**，表示角度的反函数。 |
| $\arctan$ | \arctan | **反正切函数**，表示角度的反函数。 |
| $\cos$ | \cos | **余弦函数**，常用于三角形计算和周期性现象。 |
| $\cosh$ | \cosh | **双曲余弦函数**，常用于描述双曲线的性质。 |
| $\cot$ | \cot | **余切函数**，是正切函数的倒数。 |
| $\csc$ | \csc | **余割函数**，是正弦函数的倒数。 |
| $\deg$ | \deg | **度数符号**，表示角度单位。 |
| $\det$ | \det | **行列式**，表示矩阵的行列式值。 |
| $\exp$ | \exp | **指数函数**，表示以自然常数 $e$ 为底的指数函数。 |
| $\gcd$ | \gcd | **最大公约数**，用于表示两个数的最大公约数。 |
| $\hom$ | \hom | **同态**，用于代数结构中的映射。 |
| $\ker$ | \ker | **核**，表示线性变换的核空间。 |
| $\lim$ | \lim | **极限符号**，表示一个函数在某点的极限值。 |
| $\lg$ | \lg | **常用对数**，以 $10$ 为底的对数。 |
| $\limsup$ | \limsup | **上极限**，表示一列数的上极限。 |
| $\ln$ | \ln | **自然对数**，以 $e$ 为底的对数。 |
| $\log$ | \log | **对数**，一般情况下指任意底数的对数。 |
| $\min$ | \min | **最小值**，表示函数的最小值。 |
| $\Pr$ | \Pr | **概率**，表示事件发生的概率。 |
| $\sup$ | \sup | **上确界**，表示函数的上界。 |
| $\sinh$ | \sinh | **双曲正弦函数**，常用于描述双曲线的性质。 |
| $\sin$ | \sin | **正弦函数**，常用于三角形计算和周期性现象。 |
| $\tan$ | \tan | **正切函数**，常用于角度计算。 |
| $\sec$ | \sec | **正割函数**，是余弦函数的倒数。 |
| $\tanh$ | \tanh | **双曲正切函数**，常用于描述双曲线的性质。 |
| $\inf$ | \inf | **下确界**，表示函数的下界。 |
| $\max$ | \max | **最大值**，表示函数的最大值。 |
| $\arg$ | \arg | **辐角**，表示复数的相位角。 |
| $\liminf$ | \liminf | **下极限**，表示一列数的下极限。 |


**Example**

```markdown
$\lim_{x \to \infty} f(x), \log x, \ln x, \sin x, \cos x, \tan x$
```

**Rendered Output**

$$\lim_{x \to \infty} f(x), \log x, \ln x, \sin x, \cos x, \tan x$$

* * *


## 5 Delimiters

在 LaTeX 数学模式中，分隔符用于 **包围数学表达式**，例如 **括号、绝对值、范数** 等。使用 `\left` 和 `\right` 使括号大小自适应内容。

* * *

### 5.1 Standard Delimiters

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $(a+b)$ | (a+b) | 普通小括号 |
| $[a+b]$ | [a+b] | 普通方括号 |
| ${a+b}$ | {a+b} | 花括号，用于集合表示等 |
| $\langle a, b \rangle$ | \langle a, b \rangle | 尖括号，常用于内积或向量表示 |

**Example**

```markdown
$(a+b), [a+b], \{a+b\}, \langle a, b \rangle$
```

**Rendered Output**

$$(a+b), [a+b], \{a+b\}, \langle a, b \rangle$$

* * *

### 5.2 Resizable Delimiters

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\left( x+y \right)$ | \left( x+y \right) | 自动调节大小的小括号 |
| $\left[ x+y \right]$ | \left[ x+y \right] | 自动调节大小的方括号 |
| $\{ x+y \}$ | \left{ x+y \right} | 自动调节大小的花括号 |
| $\left\langle x+y \right\rangle$ | \left\langle x+y \right\rangle | 自动调节大小的尖括号 |

**Example**

```markdown
\left( x+y \right), \left[ x+y \right], \left\{ x+y \right\}, \left\langle x+y \right\rangle
```

**Rendered Output**

$$\left( x+y \right), \left[ x+y \right], \left\{ x+y \right\}, \left\langle x+y \right\rangle$$

* * *

### 5.3 Absolute Value and Norm

**Example**

```markdown
|x|, \left| x+y \right|, \|x\|, \left\| x+y \right\|
```

**Rendered Output**

$$|x|, \left| x+y \right|, \|x\|, \left\| x+y \right\|$$

* * *

### 5.4 Floor and Ceiling Functions

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\lfloor x \rfloor$ | \lfloor x \rfloor | 向下取整符号 |
| $\left\lfloor x \right\rfloor$ | \left\lfloor x \right\rfloor | 自适应大小的向下取整 |
| $\lceil x \rceil$ | \lceil x \rceil | 向上取整符号 |
| $\left\lceil x \right\rceil$ | \left\lceil x \right\rceil | 自适应大小的向上取整 |

**Example**

```markdown
\lfloor x \rfloor, \left\lfloor x \right\rfloor, \lceil x \rceil, \left\lceil x \right\rceil
```

**Rendered Output**

$$\lfloor x \rfloor, \left\lfloor x \right\rfloor, \lceil x \rceil, \left\lceil x \right\rceil$$

* * *

### 5.5 Additional Delimiters

|    Symbol     |   Command   |    Symbol     |   Command   |    Symbol    | Command    |
| :-----------: | :---------: | :-----------: | :---------: | :----------: | ---------- |
|   $\lgroup$   |   \lgroup   |   $\rgroup$   |   \rgroup   | $\arrowvert$ | \arrowvert |
| $\Arrowvert$  | \Arrowvert  |   $\lbrace$   |   \lbrace   |  $\rbrace$   | \rbrace    |
| $\lmoustache$ | \lmoustache | $\rmoustache$ | \rmoustache | $\bracevert$ | \bracevert |

* * *


## 6 Variable-sized Symbols
在 LaTeX 中，某些数学符号会根据公式模式的不同自动调整大小。特别是在独立公式模式下，这些符号通常会变大，以增强可读性和表现力。

| Symbol | Command | Description |
|:---:|:---:|:---:|
| $\sum$ | \sum | 求和符号 |
| $\prod$ | \prod | 积符号 |
| $\coprod$ | \coprod | 共积符号 |
| $\int$ | \int | 积分符号 |
| $\oint$ | \oint | 曲线积分符号 |
| $\biguplus$ | \biguplus | 并积符号 |
| $\bigcap$ | \bigcap | 交集符号 |
| $\bigcup$ | \bigcup | 并集符号 |
| $\bigotimes$ | \bigotimes | 张量积符号 |
| $\bigvee$ | \bigvee | 并（大写）符号 |
| $\bigwedge$ | \bigwedge | 交（大写）符号 |
| $\bigodot$ | \bigodot | 点积符号 |
| $\bigsqcup$ | \bigsqcup | 并集（带上标） |

* * *

```markdown
\sum, \prod, \coprod, \int, \oint, \biguplus, \bigcap, \bigcup, \bigotimes, \bigvee, \bigwedge, \bigodot, \bigsqcup
```

**渲染后的效果：**

$$\sum, \prod, \coprod, \int, \oint, \biguplus, \bigcap, \bigcup, \bigotimes, \bigvee, \bigwedge, \bigodot, \bigsqcup$$

* * *

---

## 7 Binary Operation/Relation Symbols

### Operators Symbols

|       Symbol       |     Command      |     Symbol      |    Command    |      Symbol      |    Command     |
| :----------------: | :--------------: | :-------------: | :-----------: | :--------------: | :------------: |
|       $\pm$        |       \pm        |      $\mp$      |      \mp      |     $\times$     |     \times     |
|       $\div$       |       \div       |     $\cdot$     |     \cdot     |      $\ast$      |      \ast      |
|      $\star$       |      \star       |    $\dagger$    |    \dagger    |    $\ddagger$    |    \ddagger    |
|      $\amalg$      |      \amalg      |     $\cap$      |     \cap      |      $\cup$      |      \cup      |
|      $\uplus$      |      \uplus      |    $\sqcap$     |    \sqcap     |     $\sqcup$     |     \sqcup     |
|       $\vee$       |       \vee       |    $\wedge$     |    \wedge     |     $\oplus$     |     \oplus     |
|     $\ominus$      |     \ominus      |    $\otimes$    |    \otimes    |     $\circ$      |     \circ      |
|     $\bullet$      |     \bullet      |   $\diamond$    |   \diamond    |      $\lhd$      |      \lhd      |
|       $\rhd$       |       \rhd       |    $\unlhd$     |    \unlhd     |     $\unrhd$     |     \unrhd     |
|     $\oslash$      |     \oslash      |     $\odot$     |     \odot     |    $\bigcirc$    |    \bigcirc    |
|  $\triangleleft$   |  \triangleleft   |   $\Diamond$    |   \Diamond    | $\bigtriangleup$ | \bigtriangleup |
| $\bigtriangledown$ | \bigtriangledown |     $\Box$      |     \Box      | $\triangleright$ | \triangleright |
|    $\setminus$     |    \setminus     |      $\wr$      |      \wr      |    $\sqrt{x}$    |    \sqrt{x}    |
|    $x^{\circ}$     |    x^{\circ}     | $\triangledown$ | \triangledown |  $\sqrt[n]{x}$   |  \sqrt[n]{x}   |
|       $a^x$        |       a^x        |    $a^{xyz}$    |    a^{xyz}    |      $a_x$       |      a_x       |

### Relations Symbols

|    Symbol     |   Command   |   Symbol    |  Command  |    Symbol     |   Command   |
| :-----------: | :---------: | :---------: | :-------: | :-----------: | :---------: |
|     $\le$     |     \le     |    $\ge$    |    \ge    |    $\neq$     |    \neq     |
|    $\sim$     |    \sim     |    $\ll$    |    \ll    |     $\gg$     |     \gg     |
|   $\doteq$    |   \doteq    |  $\simeq$   |  \simeq   |   $\subset$   |   \subset   |
|   $\supset$   |   \supset   |  $\approx$  |  \approx  |   $\asymp$    |   \asymp    |
|  $\subseteq$  |  \subseteq  | $\supseteq$ | \supseteq |    $\cong$    |    \cong    |
|   $\smile$    |   \smile    | $\sqsubset$ | \sqsubset |  $\sqsupset$  |  \sqsupset  |
|   $\equiv$    |   \equiv    |  $\frown$   |  \frown   | $\sqsubseteq$ | \sqsubseteq |
| $\sqsupseteq$ | \sqsupseteq |  $\propto$  |  \propto  |   $\bowtie$   |   \bowtie   |
|     $\in$     |     \in     |    $\ni$    |    \ni    |    $\prec$    |    \prec    |
|    $\succ$    |    \succ    |  $\vdash$   |  \vdash   |   $\dashv$    |   \dashv    |
|   $\preceq$   |   \preceq   |  $\succeq$  |  \succeq  |   $\models$   |   \models   |
|    $\perp$    |    \perp    | $\parallel$ | \parallel |    $\mid$     |    \mid     |
|   $\bumpeq$   |   \bumpeq   |             |           |               |             |

### Negated Relations Symbols

[//]: # (Negations of many of these relations can be formed by just putting \not before the symbol, or by slipping an "n" between the \ and the word. Here are a couple examples, plus many other negations; it works for many of the many others as well.)

|     Symbol      |    Command    |   Symbol    |  Command  |    Symbol    |     Command      |
| :-------------: | :-----------: | :---------: | :-------: | :----------: | :--------------: |
|     $\nmid$     |     \nmid     |   $\nleq$   |   \nleq   |   $\ngeq$    |      \ngeq       |
|     $\nsim$     |     \nsim     |  $\ncong$   |  \ncong   | $\nparallel$ |    \nparallel    |
|     $\not<$     |     \not<     |   $\not>$   |   \not>   |   $\not=$    | \not=, \neq, \ne |
|    $\not\le$    |    \not\le    |  $\not\ge$  |  \not\ge  |  $\not\sim$  |     \not\sim     |
|  $\not\approx$  |  \not\approx  | $\not\cong$ | \not\cong | $\not\equiv$ |    \not\equiv    |
| $\not\parallel$ | \not\parallel |  $\nless$   |  \nless   |   $\ngtr$    |      \ngtr       |
|     $\lneq$     |     \lneq     |   $\gneq$   |   \gneq   |   $\lnsim$   |      \lnsim      |
|    $\lneqq$     |    \lneqq     |  $\gneqq$   |  \gneqq   |              |                  |

***

## 8 Arrow symbols

### Standard Arrows

|        Symbol         |       Command       |        Symbol         |       Command       |          Symbol           |         Command         |
| :-------------------: | :-----------------: | :-------------------: | :-----------------: | :-----------------------: | :---------------------: |
|     $\leftarrow$      |     \leftarrow      |   $\longleftarrow$    |   \longleftarrow    |        $\uparrow$         |        \uparrow         |
|     $\Leftarrow$      |     \Leftarrow      |   $\Longleftarrow$    |   \Longleftarrow    |        $\Uparrow$         |        \Uparrow         |
|     $\rightarrow$     |     \rightarrow     |   $\longrightarrow$   |   \longrightarrow   |       $\downarrow$        |       \downarrow        |
|     $\Rightarrow$     |     \Rightarrow     |   $\Longrightarrow$   |   \Longrightarrow   |       $\Downarrow$        |       \Downarrow        |
|   $\leftrightarrow$   |   \leftrightarrow   | $\longleftrightarrow$ | \longleftrightarrow |      $\updownarrow$       |      \updownarrow       |
|   $\Leftrightarrow$   |   \Leftrightarrow   | $\Longleftrightarrow$ | \Longleftrightarrow |      $\Updownarrow$       |      \Updownarrow       |
| $\overrightarrow{AB}$ | \overrightarrow{AB} | $\overleftarrow{AB}$  | \overleftarrow{AB}  | $\overleftrightarrow{AB}$ | \overleftrightarrow{AB} |

### Mapping and Hook Arrows

|        Symbol        |      Command       |       Symbol        |      Command      |   Symbol   | Command  |
| :------------------: | :----------------: | :-----------------: | :---------------: | :--------: | :------: |
|      $\mapsto$       |      \mapsto       |    $\longmapsto$    |    \longmapsto    | $\nearrow$ | \nearrow |
|   $\hookleftarrow$   |   \hookleftarrow   |  $\hookrightarrow$  |  \hookrightarrow  | $\searrow$ | \searrow |
|   $\leftharpoonup$   |   \leftharpoonup   |  $\rightharpoonup$  |  \rightharpoonup  | $\swarrow$ | \swarrow |
|  $\leftharpoondown$  |  \leftharpoondown  | $\rightharpoondown$ | \rightharpoondown | $\nwarrow$ | \nwarrow |
| $\rightleftharpoons$ | \rightleftharpoons |     $\leadsto$      |     \leadsto      |            |          |

###  Extended Arrows

|        Symbol        |      Command       |         Symbol         |       Command        |        Symbol        |      Command       |
| :------------------: | :----------------: | :--------------------: | :------------------: | :------------------: | :----------------: |
|  $\dashrightarrow$   |  \dashrightarrow   |    $\dashleftarrow$    |    \dashleftarrow    |  $\leftleftarrows$   |  \leftleftarrows   |
|  $\leftrightarrows$  |  \leftrightarrows  |      $\Leftarrow$      |      \Leftarrow      | $\twoheadleftarrow$  | \twoheadleftarrow  |
|   $\leftarrowtail$   |   \leftarrowtail   |    $\looparrowleft$    |    \looparrowleft    | $\leftrightharpoons$ | \leftrightharpoons |
|  $\curvearrowleft$   |  \curvearrowleft   |   $\circlearrowleft$   |   \circlearrowleft   |        $\Lsh$        |        \Lsh        |
|    $\upuparrows$     |    \upuparrows     |    $\upharpoonleft$    |    \upharpoonleft    |  $\downharpoonleft$  |  \downharpoonleft  |
|     $\multimap$      |     \multimap      | $\leftrightsquigarrow$ | \leftrightsquigarrow | $\rightrightarrows$  | \rightrightarrows  |
|  $\rightleftarrows$  |  \rightleftarrows  |  $\rightrightarrows$   |  \rightrightarrows   |  $\leftrightarrows$  |  \leftrightarrows  |
| $\twoheadrightarrow$ | \twoheadrightarrow |   $\rightarrowtail$    |   \rightarrowtail    |  $\looparrowright$   |  \looparrowright   |
| $\rightleftharpoons$ | \rightleftharpoons |   $\curvearrowright$   |   \curvearrowright   | $\circlearrowright$  | \circlearrowright  |
|        $\Rsh$        |        \Rsh        |   $\downdownarrows$    |   \downdownarrows    |  $\upharpoonright$   |  \upharpoonright   |
| $\downharpoonright$  | \downharpoonright  |   $\rightsquigarrow$   |   \rightsquigarrow   |                      |                    |

### Negated Arrows

|     Symbol     |   Command    |     Symbol     |   Command    |       Symbol       |     Command      |
| :------------: | :----------: | :------------: | :----------: | :----------------: | :--------------: |
| $\nleftarrow$  | \nleftarrow  | $\nrightarrow$ | \nrightarrow | $\nleftrightarrow$ | \nleftrightarrow |
| $\nRightarrow$ | \nRightarrow | $\nLeftarrow$  | \nLeftarrow  | $\nLeftrightarrow$ | \nLeftrightarrow |

***

## 9 Miscellaneous symbols

|      Symbol      |    Command     |        Symbol        |      Command       |      Symbol       |     Command     |
| :--------------: | :------------: | :------------------: | :----------------: | :---------------: | :-------------: |
|     $\infty$     |     \infty     |       $\nabla$       |       \nabla       |    $\partial$     |    \partial     |
|      $\eth$      |      \eth      |     $\clubsuit$      |     \clubsuit      |  $\diamondsuit$   |  \diamondsuit   |
|   $\heartsuit$   |   \heartsuit   |     $\spadesuit$     |     \spadesuit     |     $\cdots$      |     \cdots      |
|     $\vdots$     |     \vdots     |       $\ldots$       |       \ldots       |     $\ddots$      |     \ddots      |
|      $\Im$       |      \Im       |        $\Re$         |        \Re         |     $\forall$     |     \forall     |
|    $\exists$     |    \exists     |      $\nexists$      |      \nexists      |    $\emptyset$    |    \emptyset    |
|  $\varnothing$   |  \varnothing   |       $\imath$       |       \imath       |     $\jmath$      |     \jmath      |
|      $\ell$      |      \ell      |       $\iiint$       |       \iiint       |      $\iint$      |      \iint      |
|     $\sharp$     |     \sharp     |       $\flat$        |       \flat        |    $\natural$     |    \natural     |
|     $\Bbbk$      |     \Bbbk      |      $\bigstar$      |      \bigstar      |    $\diagdown$    |    \diagdown    |
|    $\diagup$     |    \diagup     |      $\Diamond$      |      \Diamond      |      $\Finv$      |      \Finv      |
|     $\Game$      |     \Game      |       $\hbar$        |       \hbar        |     $\hslash$     |     \hslash     |
|    $\lozenge$    |    \lozenge    |        $\mho$        |        \mho        |     $\prime$      |     \prime      |
|    $\square$     |    \square     |       $\surd$        |       \surd        |       $\wp$       |       \wp       |
|     $\angle$     |     \angle     |   $\measuredangle$   |   \measuredangle   | $\sphericalangle$ | \sphericalangle |
|  $\complement$   |  \complement   |   $\triangledown$    |   \triangledown    |    $\triangle$    |    \triangle    |
|  $\vartriangle$  |  \vartriangle  |   $\blacklozenge$    |   \blacklozenge    |  $\blacksquare$   |  \blacksquare   |
| $\blacktriangle$ | \blacktriangle | $\blacktriangledown$ | \blacktriangledown |   $\backprime$    |   \backprime    |
|   $\circledS$    |   \circledS    |         $\S$         |         \S         |     $\LaTeX$      |     \LaTeX      |


***

## 10 Other Styles (math mode only)

| Style | Symbol | Command |
|:---:|:---:|:---:|
| **Caligraphic letters** | $\mathcal{A}, \mathcal{B}, \mathcal{C}, \mathcal{D}$ etc. | \mathcal{A}, \mathcal{B}, \mathcal{C}, \mathcal{D} etc. |
| **Mathbb letters** | $\mathbb{A}, \mathbb{B}, \mathbb{C}, \mathbb{D}$ etc. | \mathbb{A}, \mathbb{B}, \mathbb{C}, \mathbb{D} etc. |
| **Mathfrak letters** | $\mathfrak{A}, \mathfrak{B}, \mathfrak{C}, \mathfrak{D}$ etc. | \mathfrak{A}, \mathfrak{B}, \mathfrak{C}, \mathfrak{D} etc. |
| **Math Sans serif letters** | $\mathsf{A}, \mathsf{B}, \mathsf{C}, \mathsf{D}$ etc. | \mathsf{A}, \mathsf{B}, \mathsf{C}, \mathsf{D} etc. |
| **Math bold letters** | $\mathbf{A}, \mathbf{B}, \mathbf{C}, \mathbf{D}$ etc. | \mathbf{A}, \mathbf{B}, \mathbf{C}, \mathbf{D} etc. |
| **Math roman letters** | $\mathrm{A}, \mathrm{B}, \mathrm{C}, \mathrm{D}$ etc. | \mathrm{A}, \mathrm{B}, \mathrm{C}, \mathrm{D} etc. |
| **Math italic letters** | $\mathit{A}, \mathit{B}, \mathit{C}, \mathit{D}$ etc. | \mathit{A}, \mathit{B}, \mathit{C}, \mathit{D} etc. |
| **Math scr letters** | $\mathscr{A}, \mathscr{B}, \mathscr{C}, \mathscr{D}$ etc. | \mathscr{A}, \mathscr{B}, \mathscr{C}, \mathscr{D} etc. |

***

## 11 Font sizes

### Math Mode

| Symbol | Command |
|:---:|:---:|
| $\int f^{-1}(x - x_a) , dx$ | \int f^{-1}(x - x_a) , dx |
| $\displaystyle \int f^{-1}(x - x_a) , dx$ | \displaystyle \int f^{-1}(x - x_a) , dx |
| $\textstyle \int f^{-1}(x - x_a) , dx$ | \textstyle \int f^{-1}(x - x_a) , dx |
| $\scriptstyle \int f^{-1}(x - x_a) , dx$ | \scriptstyle \int f^{-1}(x - x_a) , dx |
| $\scriptscriptstyle \int f^{-1}(x - x_a) , dx$ | \scriptscriptstyle \int f^{-1}(x - x_a) , dx |


### Text Mode

| Symbol | Command |
|:---:|:---:|
| $\tiny{hmc}$ | \tiny{hmc} |
| $\scriptsize{hmc}$ | \scriptsize{hmc} |
| $\small{hmc}$ | \small{hmc} |
| $\normalsize{hmc}$ | \normalsize{hmc} |
| $\large{hmc}$ | \large{hmc} |
| $\Large{hmc}$ | \Large{hmc} |
| $\LARGE{hmc}$ | \LARGE{hmc} |
| $\huge{hmc}$ | \huge{hmc} |
| $\Huge{hmc}$ | \Huge{hmc} |

***

## 12 Math Commands

### Subscripts and Superscripts

|   Symbol    |  Command  |   Symbol    |  Command  |
| :---------: | :-------: | :---------: | :-------: |
|    $3^2$    |    3^2    |    $b_i$    |    b_i    |
|  $3^{23}$   |  3^{23}   |  $m_{i-1}$  |  m_{i-1}  |
| $d^{i+1}_3$ | d^{i+1}_3 |   $y^2_3$   |  y^{2}_3  |
|  $2^{ai}$   |  2^{ai}   | $2^{(a_i)}$ | 2^{(a_i)} |

### Fractions

|                        Symbol                         |                       Command                       |
| :---------------------------------------------------: | :-------------------------------------------------: |
|                     $\frac{1}{2}$                     |                     \frac{1}{2}                     |
|                    $\frac{2}{x+2}$                    |                    \frac{2}{x+2}                    |
|           $\frac{1 + \frac{1}{x}}{3x + 2}$            |           \frac{1 + \frac{1}{x}}{3x + 2}            |
| $\cfrac{2}{1+\cfrac{2}{1+\cfrac{2}{1+\cfrac{2}{1}}}}$ | \cfrac{2}{1+\cfrac{2}{1+\cfrac{2}{1+\cfrac{2}{1}}}} |

|                            Symbol                            |                           Command                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                      $(\frac{a}{x} )^2$                      |                       (\frac{a}{x} )^2                       |
|                $\left(\frac{a}{x} \right)^2$                 |                 \left(\frac{a}{x} \right)^2                  |
|            $\left\lceil \frac{x}{y} \right\rceil$            |             \left\lceil \frac{x}{y} \right\rceil             |
|           $\left\lfloor \frac{x}{y} \right\rfloor$           |            \left\lfloor \frac{x}{y} \right\rfloor            |
|       $\underbrace{a_0 + a_1 + a_2 + \cdots + a_n}_x$        |        \underbrace{a_0 + a_1 + a_2 + \cdots + a_n}_x         |
|        $\overbrace{a_0 + a_1 + a_2 + \cdots + a_n}^x$        |         \overbrace{a_0 + a_1 + a_2 + \cdots + a_n}^x         |
| $\arg \underset{1\leq k \leq n} {max} \frac{\lambda_k}{\lambda_{k+1}}$ | \arg \underset{1\leq k \leq n} {max} \frac{\lambda_k}{\lambda_{k+1}} |

### Radicals

|          Symbol          |        Command         |
| :----------------------: | :--------------------: |
|        $\sqrt{3}$        |        \sqrt{3}        |
|      $\sqrt{x + y}$      |      \sqrt{x + y}      |
| $\sqrt{x + \frac{1}{2}}$ | \sqrt{x + \frac{1}{2}} |
|      $\sqrt[3]{3}$       |      \sqrt[3]{3}       |
|      $\sqrt[n]{x}$       |      \sqrt[n]{x}       |

### Sums, Products, Limits and Logarithms

|              Symbol               |             Command             |
| :-------------------------------: | :-----------------------------: |
| $\sum_{i=1}^{\infty} \frac{1}{i}$ | \sum_{i=1}^{\infty} \frac{1}{i} |
|   $\prod_{n=1}^5 \frac{n}{n-1}$   |   \prod_{n=1}^5 \frac{n}{n-1}   |
| $\lim_{x \to \infty} \frac{1}{x}$ | \lim_{x \to \infty} \frac{1}{x} |
| $\lim_{x \to \infty} \frac{1}{x}$ | \lim_{x \to \infty} \frac{1}{x} |
|           $\log_n n^2$            |           \log_n n^2            |

Some of these are prettier in display mode:

|                     Symbol                     |                   Command                    |
| :--------------------------------------------: | :------------------------------------------: |
| $\displaystyle\sum_{i=1}^{\infty} \frac{1}{i}$ | \displaystyle\sum_{i=1}^{\infty} \frac{1}{i} |
|   $\displaystyle\prod_{n=1}^5 \frac{n}{n-1}$   |   \displaystyle\prod_{n=1}^5 \frac{n}{n-1}   |
| $\displaystyle\lim_{x \to \infty} \frac{1}{x}$ | \displaystyle\lim_{x \to \infty} \frac{1}{x} |

### Mods

|        Symbol         |       Command       |
| :-------------------: | :-----------------: |
|  $\sum \frac{1}{i}$   |  \sum \frac{1}{i}   |
| $\prod \frac{n}{n-1}$ | \prod \frac{n}{n-1} |
|      $\log n^2$       |      \log n^2       |
|        $\ln e$        |        \ln e        |

### Trigonometric Functions

|          Symbol          |        Command         |
| :----------------------: | :--------------------: |
| $\cos^2 x +\sin^2 x = 1$ | \cos^2 x +\sin^2 x = 1 |
|   $\\cos 90^\circ = 0$   |   \\cos 90^\circ = 0   |

### Calculus

|                            Symbol                            |                           Command                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|            $\frac{d}{dx} \left( x^2 \right) = 2x$            |             \frac{d}{dx} \left( x^2 \right) = 2x             |
|                  $\int 2x \, dx = x^2 + C$                   |                    \int 2x , dx = x^2 + C                    |
|                   $\int_1^5 2x \, dx = 24$                   |                    \int_1^5 2x , dx = 24                     |
| $\frac{\partial^2 U}{\partial x^2} + \frac{\partial^2 U}{\partial y^2}$ | \frac{\partial^2 U}{\partial x^2} + \frac{\partial^2 U}{\partial y^2} |
| $\frac{1}{4\pi}\oint_\Sigma\frac{1}{r}\frac{\partial U}{\partial n} ds$ | \frac{1}{4\pi}\oint_\Sigma\frac{1}{r}\frac{\partial U}{\partial n} ds |


### Array environment,examples

|                            Symbol                            |                           Command                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| $\begin{pmatrix} 2\tau & 7\phi-\frac{5}{12} \\ 3\psi & \frac{\pi}{8} \end{pmatrix}$ | \begin{pmatrix} 2\tau & 7\phi-\frac{5}{12} \\ 3\psi & \frac{\pi}{8} \end{pmatrix} |
|            $\begin{pmatrix} x \\ y \end{pmatrix}$            |             \begin{pmatrix} x \\ y \end{pmatrix}             |
|   $\begin{bmatrix} 3 & 4 & 5 \\ 1 & 3 & 729 \end{bmatrix}$   |    \begin{bmatrix} 3 & 4 & 5 \\ 1 & 3 & 729 \end{bmatrix}    |
| $\begin{pmatrix}2\tau & 7\phi-\frac{5}{12} \\3\psi & \frac{\pi}{8}\end{pmatrix}\begin{pmatrix}x \\y\end{pmatrix}\mathrm{and}\begin{bmatrix}3 & 4 & 5 \\1 & 3 & 729\end{bmatrix}$ | \begin{pmatrix}2\tau & 7\phi-\frac{5}{12} \\3\psi & \frac{\pi}{8}\end{pmatrix}\begin{pmatrix}x \\y\end{pmatrix}\mathrm{and}\begin{bmatrix}3 & 4 & 5 \\1 & 3 & 729\end{bmatrix} |


***

### Matrices and Arrays

```markdown
   A= 
   \begin{pmatrix} 
   	a_{11} & a_{12} \\ 
   	a_{21} & a_{22} 
   \end{pmatrix}
```

$$
   A=
   \begin{pmatrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{pmatrix}
$$

```markdown
   A=
   \begin{bmatrix}
       a_{11} & a_{12} \\
       a_{21} & a_{22}
   \end{bmatrix}
```

$$
   A=
   \begin{bmatrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{bmatrix}
$$

```markdown
   \begin{bmatrix}
   	1 & 2 & 3 \\
   	4 & 5 & 6 \\
   \end{bmatrix}
```

$$
   \begin{bmatrix}1 & 2 & 3 \\4 & 5 & 6 \\\end{bmatrix}
$$

```markdown
   A=
   \begin{Bmatrix}
       a_{11} & a_{12} \\
       a_{21} & a_{22}
   \end{Bmatrix}
```

$$
   A=
   \begin{Bmatrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{Bmatrix}
$$

```markdown
   A=
   \begin{vmatrix}
       a_{11} & a_{12} \\
       a_{21} & a_{22}
   \end{vmatrix}
```

$$
   A=
   \begin{vmatrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{vmatrix}
$$

```markdown
   A=
   \begin{Vmatrix}
       a_{11} & a_{12} \\
       a_{21} & a_{22}
   \end{Vmatrix}
```

$$
   A=
   \begin{Vmatrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{Vmatrix}
$$

```markdown
   A=
   \begin{matrix}
       a_{11} & a_{12} \\
       a_{21} & a_{22}
   \end{matrix}
```

$$
   A=
   \begin{matrix}
   a_{11} & a_{12} \\
   a_{21} & a_{22}
   \end{matrix}
$$

```markdown
   \begin{array}{ccc} 
       a & b & c \\ 
       d & e & f \\ 
       g & h & i  
   \end{array}
```


$$
   \begin{array}{ccc} a & b & c \\ d & e & f \\ g & h & i  \end{array}
$$


```markdown
    \mathbf{X} = 
        \left(
            \begin{array}{cccc}
                x_{11} & x_{12} & \ldots & x_{1n}\\
                x_{21} & x_{22} & \ldots & x_{2n}\\
                \vdots & \vdots & \ddots & \vdots\\
                x_{n1} & x_{n2} & \ldots & x_{nn}\\
            \end{array}
        \right) 
```

$$
\mathbf{X} = \left(
\begin{array}{cccc}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{array}
\right) 
$$


```markdown
    \begin{matrix}
        1 & 2 \\\\ 3 & 4
    \end{matrix} \qquad
    \begin{bmatrix}
        x_{11} & x_{12} & \ldots & x_{1n}\\
        x_{21} & x_{22} & \ldots & x_{2n}\\
        \vdots & \vdots & \ddots & \vdots\\
        x_{n1} & x_{n2} & \ldots & x_{nn}\\
    \end{bmatrix}
```

$$
\begin{matrix}
1 & 2 \\\\ 3 & 4
\end{matrix} \qquad
\begin{bmatrix}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{bmatrix}
$$


#### Multi-line formula alignment

```markdown
\begin{split}
L(\theta)
&=	\arg\max_{\theta}\ln(P_{All})\\
&=	\arg\max_{\theta}\ln\prod_{i=1}^{n}
    \left[
        (h_{\theta}(\mathbf{x}^{(i)}))^{\mathbf{y}^{(i)}}\cdot
        (1-h_{\theta}(\mathbf{x}^{(i)}))^{1-\mathbf{y}^{(i)}}
    \right]\\
&=	\arg\max_{\theta}\sum_{i=1}^{n}
	\left[
		\mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
		(1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
	\right]\\
&=	\arg\min_{\theta}
	\left[
        -\sum_{i=1}^{n}
        \left[
            \mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
            (1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
        \right]
	\right]\\
&=	\arg\min_{\theta}\mathscr{l}(\theta)
\end{split}

```

$$
\begin{split}
L(\theta)
&=	\arg\max_{\theta}\ln(P_{All})\\
&=	\arg\max_{\theta}\ln\prod_{i=1}^{n}
    \left[
        (h_{\theta}(\mathbf{x}^{(i)}))^{\mathbf{y}^{(i)}}\cdot
        (1-h_{\theta}(\mathbf{x}^{(i)}))^{1-\mathbf{y}^{(i)}}
    \right]\\
&=	\arg\max_{\theta}\sum_{i=1}^{n}
	\left[
		\mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
		(1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
	\right]\\
&=	\arg\min_{\theta}
	\left[
        -\sum_{i=1}^{n}
        \left[
            \mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
            (1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
        \right]
	\right]\\
&=	\arg\min_{\theta}\mathscr{l}(\theta)
\end{split}
$$

```markdown
\begin{split}
L(\theta)
=	\arg\max_{\theta}\ln(P_{All})\\
=	\arg\max_{\theta}\ln\prod_{i=1}^{n}
    \left[
        (h_{\theta}(\mathbf{x}^{(i)}))^{\mathbf{y}^{(i)}}\cdot
        (1-h_{\theta}(\mathbf{x}^{(i)}))^{1-\mathbf{y}^{(i)}}
    \right]\\
=	\arg\max_{\theta}\sum_{i=1}^{n}
	\left[
		\mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
		(1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
	\right]\\
=	\arg\min_{\theta}
	\left[
        -\sum_{i=1}^{n}
        \left[
            \mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
            (1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
        \right]
	\right]\\
=	\arg\min_{\theta}\mathscr{l}(\theta)
\end{split}
```

$$
\begin{split}
L(\theta)
=	\arg\max_{\theta}\ln(P_{All})\\
=	\arg\max_{\theta}\ln\prod_{i=1}^{n}
    \left[
        (h_{\theta}(\mathbf{x}^{(i)}))^{\mathbf{y}^{(i)}}\cdot
        (1-h_{\theta}(\mathbf{x}^{(i)}))^{1-\mathbf{y}^{(i)}}
    \right]\\
=	\arg\max_{\theta}\sum_{i=1}^{n}
	\left[
		\mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
		(1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
	\right]\\
=	\arg\min_{\theta}
	\left[
        -\sum_{i=1}^{n}
        \left[
            \mathbf{y}^{(i)}\ln(h_{\theta}(\mathbf{x}^{(i)})) +
            (1-\mathbf{y}^{(i)})\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
        \right]
	\right]\\
=	\arg\min_{\theta}\mathscr{l}(\theta)
\end{split}
$$

```markdown
\begin{split}
&\ln h_{\theta}(\mathbf{x}^{(i)})
=	\ln\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}}
= 	-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})\\
&\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
=	\ln(1-\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}})
= 	-\theta^T \mathbf{x}^{(i)}-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})
\end{split}
```

$$
\begin{split}
&\ln h_{\theta}(\mathbf{x}^{(i)})
=	\ln\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}}
= 	-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})\\
&\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
=	\ln(1-\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}})
= 	-\theta^T \mathbf{x}^{(i)}-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})
\end{split}
$$

```markdown
\begin{align}
&\ln h_{\theta}(\mathbf{x}^{(i)})
=	\ln\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}}
	= -\ln(1+e^{\theta^T \mathbf{x}^{(i)}})\\
&\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
=	\ln(1-\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}})
	= -\theta^T \mathbf{x}^{(i)}-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})
\end{align}
```

$$
\begin{align}
&\ln h_{\theta}(\mathbf{x}^{(i)})
=	\ln\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}}
	= -\ln(1+e^{\theta^T \mathbf{x}^{(i)}})\\
&\ln(1-h_{\theta}(\mathbf{x}^{(i)}))
=	\ln(1-\frac{1}{1+e^{-\theta^T \mathbf{x}^{(i)}}})
	= -\theta^T \mathbf{x}^{(i)}-\ln(1+e^{\theta^T \mathbf{x}^{(i)}})
\end{align}
$$



#### Groups of equations

```markdown
\begin{cases}
    \begin{split}
        p &= P(y=1|\mathbf{x})=
        	\frac{1}{1+e^{-\theta^T\mathbf{X}}}\\
        1-p &= P(y=0|\mathbf{x})=1-P(y=1|\mathbf{x})=
        	\frac{1}{1+e^{\theta^T\mathbf{X}}}
    \end{split}
\end{cases}
```

$$
\begin{cases}
    \begin{split}
        p &= P(y=1|\mathbf{x})=
        	\frac{1}{1+e^{-\theta^T\mathbf{X}}}\\
        1-p &= P(y=0|\mathbf{x})=1-P(y=1|\mathbf{x})=
        	\frac{1}{1+e^{\theta^T\mathbf{X}}}
    \end{split}
\end{cases}
$$

```markdown
\text{Decision Boundary}=
\begin{cases}
    1\quad \text{if }\ \hat{y}>0.5\\
    0\quad \text{otherwise}
\end{cases}
```

$$
\text{Decision Boundary}=
\begin{cases}
    1\quad \text{if }\ \hat{y}>0.5\\
    0\quad \text{otherwise}
\end{cases}
$$

## LOADING

***

## Reference
---

[LaTeX:symbol](https://artofproblemsolving.com/wiki/index.php/LaTeX:Symbols)

[LaTeX:command](https://artofproblemsolving.com/wiki/index.php/LaTeX:Commands)

[lshort-zh-cn](http://texdoc.net/texmf-dist/doc/latex/lshort-chinese/lshort-zh-cn.pdf)

[CSDN:Latex数学公式符号大全（超详细）](https://blog.csdn.net/Yushan_Ji/article/details/134322574)

[CSDN:LaTex符号大全（LaTeX_Symbols）](https://blog.csdn.net/yen_csdn/article/details/79966985)


---

## Last
若您发现文中存在任何不准确或错误的地方，欢迎提出宝贵意见，笔者将会尽快修正并改进。同时，若本文中的任何内容无意中侵犯了某些个人或机构的版权或知识产权，笔者深感抱歉，并愿意及时处理相关事宜，删除或修改相关部分。

本笔记内容仅供学习和交流使用，任何商业用途或其他非法使用行为，请务必遵守相关法律法规，并获得原作者授权。