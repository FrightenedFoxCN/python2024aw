# 基于 Monte-Carlo 算法实现积分计算

!!! failure "警告"

    本实验未正式发布，为试行版本.

## 实验目的

1. 理解 random 库的用法以及 Monte-Carlo 算法的基本思路。
2. 使用 Matplotlib 完成初步的可视化。
3. \* 对随机和伪随机的概念有初步的理解，

## 实验环境

- Python 3.x 环境
- MatplotLib 库

## 实验步骤

### 环境配置和安装

参考[环境配置指南](http://127.0.0.1:8000/bonus/00env/)中的指导安装 Matplotlib 库. 

### 利用圆面积估计 $\pi$ 值

我们都已经熟知，积分本质上就是面积的计算. 我们首先考虑计算简单图形的面积：圆面积. 一个圆是由 $x^2 + y^2 <= 1$ 所界定的一块面积. 而根据高中学过的关于几何概型的知识，一个图形的面积与总面积的比值就是在总面积上投点落入图形当中的概率. 也就是说，我们可以考虑以下图形：

<center>
<svg
   width="48"
   height="48"
   viewBox="0 0 48 48"
   version="1.1"
   id="svg1"
   inkscape:version="1.3.2 (091e20e, 2023-11-25, custom)"
   sodipodi:docname="绘图.svg"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:svg="http://www.w3.org/2000/svg">
  <sodipodi:namedview
     id="namedview1"
     pagecolor="#ffffff"
     bordercolor="#000000"
     borderopacity="0.25"
     inkscape:showpageshadow="2"
     inkscape:pageopacity="0.0"
     inkscape:pagecheckerboard="0"
     inkscape:deskcolor="#d1d1d1"
     inkscape:document-units="px"
     inkscape:zoom="16.958333"
     inkscape:cx="23.970516"
     inkscape:cy="24"
     inkscape:window-width="1920"
     inkscape:window-height="1010"
     inkscape:window-x="-6"
     inkscape:window-y="-6"
     inkscape:window-maximized="1"
     inkscape:current-layer="layer1" />
  <defs
     id="defs1" />
  <g
     inkscape:label="图层 1"
     inkscape:groupmode="layer"
     id="layer1">
    <circle
       style="fill:#d7d7d7;fill-opacity:0.74011302;stroke:#38e8d9;stroke-width:0.298137;stroke-dasharray:none;stroke-opacity:1"
       id="path1"
       cx="23.849998"
       cy="23.85"
       r="23.850931" />
    <rect
       style="fill:#9e9e9e;fill-opacity:0.0169492;stroke:#38e8d9;stroke-width:0.298137;stroke-dasharray:none;stroke-opacity:1"
       id="rect2"
       width="47.701862"
       height="47.701862"
       x="0.14906833"
       y="0.14906833" />
    <rect
       style="fill:#e92b58;fill-opacity:0;stroke:#e56fcb;stroke-width:0.296296;stroke-dasharray:none;stroke-opacity:1"
       id="rect3"
       width="23.703705"
       height="23.703705"
       x="24.148148"
       y="0.14814816" />
  </g>
</svg>
</center>

上面的图形中，只要取在阴影部分，我们就假定它在圆内. 因此，采用一个随机数生成器的手段，根据点落在圆内的频率估计总的概率，就能给出圆面积的估计. 为了方便起见，我们考虑第一象限的圆，即上图中粉色方框给出的部分. 

首先，我们需要引入随机数库并完成基本参数的设置：

```python
import random

inside_circle = 0
num_samples = 10000000
```

其中 `inside_circle` 为在圆圈内的取样点的个数，`num_samples` 为总的取样个数，下面我们将取样操作进行 `num_samples` 次，这使用 `#!python for` 循环来实现：

```python
for _ in range(num_samples):
    x = random.random()
    y = random.random()
```

!!! note

    其中的 `_` 指的是循环变量可以被抛弃，这是一个通用的“垃圾桶”. 也就是说，你甚至可以写出这样的解包操作：


    ```python
    a, _, b = [1, 2, 3]
    ```

    这样就令 `a` 为 `1`，`b` 为 `3`，而中间的值则被永久抛弃了.

`random` 库中的 `random` 函数在 `(0, 1]` 区间内取到一个随机浮点数. 接下来判断它是否在圆内，对于每一次循环，都需要进行一次下面的判断：

```python
if x ** 2 + y ** 2 <= 1:
    inside_circle += 1
```

最后我们就能根据面积进行对 $\pi$ 的估算：

```python
pi_estimate = 4 * (inside_circle / num_samples)
```

思考，如何用这个方法计算更多的积分？你可以尝试计算一些简单的值，例如 $y = x^2$ 之类的函数在 `(0, 1)` 区间中曲线下的面积，这如我们所知就是其在它在 `(0, 1)` 上的积分.

### 利用函数改写现在的代码

现在的代码如下：

```python
import random

inside_circle = 0
num_samples = 10000000
for _ in range(num_samples):
    x = random.random()
    y = random.random()
    
    if x ** 2 + y ** 2 <= 1:
        inside_circle += 1
        
pi_estimate = 4 * (inside_circle / num_samples)
```

思考，这样的过程是一个怎样的流程？首先，我们初始化了一堆变量；然后，我们频繁进行采样，判断采样的点是否满足要求；最后，根据采样得到的频率给出计算结果. 为了让这样的程序更加清晰，我们定义几个函数：

```python
def sampling(i):
    """
    @param       i: 第 i 次采样；
    @return (x, y): 返回采样的结果.
    """
    x = random.random()
    y = random.random()

    return (x, y)
```

!!! note

    TODO: 关于函数和函数文档注释的补充说明.


这个函数主要做的事情是完成对结果的采样. 我们在后面会发现，这样的采样并非是真正的随机，而是具备自己的算法内核的. 这也就是为什么 Python 官方将 `random` 库描述为**伪**随机数生成器而非真正意义上的随机数生成器. 而第二个函数主要做的是返回点是否满足要求：

```python
def check(x, y):
    """
    @param (x, y): 采样所得的点
    @return     b: True 或者 False，是否满足要求
    """
    return x ** 2 + y ** 2 <= 1
```

复盘整理一下，现在的主程序是

```python
inside_circle = 0
num_samples = 10000000
for _ in range(num_samples):
    x, y = sampling(_)
    
    if check(x, y):
        inside_circle += 1
        
pi_estimate = 4 * (inside_circle / num_samples)
```

!!! note
    关于 `_` 作为传入参数的说明.

函数的定义放在主函数的前面以使得其可以更好地被利用. 这使得我们：
1. 可以更清楚地看到主程序执行的过程；
2. 可以更容易地修改需要修改的部分，例如，只要修改 `check` 函数就能计算其它区域的积分（当然，最后计算的结果需要稍微做出一点修改）.

这就是应用函数来厘清控制流的一个例证.

### 利用 matplotlib 可视化采样的过程

### *修改随机数生成器：一个练习

## 参考资料

- [Python `random` 库的官方文档](https://docs.python.org/3/library/random.html)
- [Matplotlib 的官网](https://matplotlib.org/)
- [Physically Based Rendering 一书中关于 Halton 序列的叙述](https://www.pbr-book.org/3ed-2018/Sampling_and_Reconstruction/The_Halton_Sampler)