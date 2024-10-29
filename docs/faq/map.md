# `#!python map` 函数与列表推导式

!!! note "关于 `#!python map`"

    `#!python map` 函数是 Python 提供的一种非常强大的内置函数——它和 Python 语言的其他特性，例如 `#!python filter` 函数、lambda 表达式等，共同赋予了 Python 进行基本的**函数式编程**的能力. 尽管我们不会涉及到关于函数式编程的内容，不过应该知道，`#!python map` 要比我们在课堂中学到或用到的能力要强大得多.

## `#!python map` 函数

目前我们在课堂上学到或用到的 `#!python map` 函数的唯一用途是将一行中的多个输入转换为数字：

```python
a, b, c, d = map(int, input().split())
```

那么我们就从这里开始. 在阅读了[类型与值](typing.md)一节后，我们应当知道，对于一个复杂的 Python 语句，我们总是可以借助中间变量，将它拆分为多个简单的语句以便理解. `#!python input().split()` 的作用已经不必多说，它产生输入字符串按连续空白符分割的列表. 那么，要研究这个 `#!python map` 调用，我们仍然可以通过研究下面这样等价的片段来理解：

```python
L = ["6", "28", "496", "8128"]
m = map(int, L)
a, b, c, d = m
```

当然，执行完这三行后，变量 `a` `b` `c` `d` 就分别成为整数 `#!python 6` `#!python 28` `#!python 496` `#!python 8128`，这应当是我们已经知道的结果，但也许还不知道发生了什么. 最重要的第一步是搞清楚 `L` 是什么？它是一个包含 4 个字符串的列表；用官话来说，就是一个长度为 4，包含 4 个 `#!python str` 的  `#!python list`.

现在我们可以这么粗略地说：`#!python map(f, L)` 函数返回的是一个 **`#!python map` 对象**，当我们使用这个对象时，它产生的结果是 `f` **应用**在 `L` 中的每一个元素的结果集合.

稍微等等，这里还是很难理解，并且突然多出好几个概念！暂时不用去理解上面这段在说什么，我们可以启动我们最趁手的武器——交互式环境：

```python
>>> L = ["6", "28", "496", "8128"]
>>> m = map(int, L)
>>> m
<map object at 0x153267220>
>>> list(m)
[6, 28, 496, 8128]
```

首先来试试将这个结果套用到上面的说法上吧：

- `#!python map(int, L)` 返回了一个 `#!python map` 对象，我们在交互式环境中看到它被显示为 `<map object at 0x153267220>`. 最后的十六进制串的含义是这个对象在计算机中所处的内存地址，所以每次运行都是不一样的，这不重要.
- 我们用 `#!python list(m)` 来将 `m` 转换为了列表，其结果是将 `#!python int` 应用在列表 `L` 中的每个元素上，它们是 `#!python "6"` `#!python "28"` `#!python "496"` `#!python "8128"`. 在这里“应用”就是说，`#!python map` 函数做了这些调用：
    - `#!python int("6")`
    - `#!python int("28")`
    - `#!python int("496")`
    - `#!python int("8128")`

    所以我们可以说，这在效果上就是将 4 个字符串依次用 `#!python int` 来将其转换为了整数. 我们称 `#!python list(m)` 这一步**使用**了 `m` 这个 `#!python map` 对象.

上面的交互式环境中没有演示的就是最后的 `a, b, c, d = m` 了；这是我们在[类型与值](typing.md)中提到过的**解包**.

我们再举一个例子：这次要用到我们自己写的函数了！

```python
def my_fun(var):
    print(f"I see you: {var!r}")
    return 3 * var
```

然后我们有一个列表，有 4 个元素，分别为 `#!python int` `#!python 1`、`#!python str` `#!python "abc"`、`#!python list` `#!python [-8]`、`#!python float` `#!python 9.5`：

```python
L = [1, "abc", [-8], 9.5]
```

我们接下来研究的是下面的结果：

```python
>>> m = map(my_fun, L)
>>> m
<map object at 0x153587cd0>
>>> L2 = list(m)
I see you: 1
I see you: 'abc'
I see you: [-8]
I see you: 9.5
>>> L2
[3, 'abcabcabc', [-8, -8, -8], 28.5]
```

注意到了吗！这里实际发生了这些调用：

- `#!python my_fun(3)`
- `#!python my_fun("abc")`
- `#!python my_fun([-8])`
- `#!python my_fun(9.5)`

并且 `#!python list(m)` 的结果就是上面这些调用的结果组成的列表. 很神奇吧？

从上面我们可以观察到 `#!python map` 另一个非常有意思的特性. 发现了吗？当我们创建 `#!python map` 对象时，并没有调用 `my_fun`；真正的转换过程是在我们**使用** `m` 时才发生的. 我们也可以通过使用内置的 `#!python next` 函数，来逐个进行转换：

```python hl_lines="1"
>>> m = map(my_fun, L)
>>> a = next(m)
I see you: 1
>>> b = next(m)
I see you: 'abc'
>>> c = next(m)
I see you: [-8]
>>> d = next(m)
I see you: 9.5
>>> e = next(m)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> a, b, c, d
(3, 'abcabcabc', [-8, -8, -8], 28.5)
```

注意到在这里，每次调用 `#!python next` 都会转换一个值，并且由于只有 4 个值，在尝试进行第 5 次转换时，Python 会抛出一个 `#!python StopIteration`，这表示已经没有更多值可以转换了. 这个特性叫做**惰性求值**：顾名思义，只有在需要用到值的时候，才做转换. 也因此，`#!python map` 对象是不能被索引的：如果想要访问结果中的任意一个值，就必须首先把 `#!python map` 对象中的全部结果收集在列表中.

在这里，我们也可以换个视角，用 `#!python for` 结构来研究它同样是可以的：

```python hl_lines="1"
>>> m = map(my_fun, L)
>>> for item in m:
...     print(f'for-loop sees a {item!r}')
...
I see you: 1
for-loop sees a 3
I see you: 'abc'
for-loop sees a 'abcabcabc'
I see you: [-8]
for-loop sees a [-8, -8, -8]
I see you: 9.5
for-loop sees a 28.5
```

每次循环中，`item` 都表示 `m` 中的一个转换结果. 并且在所有的值都转换完后，`#!python for` 也会自然结束.

最后要说明的一点是，每个 `#!python map` 对象仅能被**使用**一次：

```python3
>>> m = map(print, range(0, 10, 3))
>>> next(m)
0
None
>>> list(m)
3
6
9
[None, None, None]
>>> list(m)
[]
>>> for item in m:
...     print(f'uwu? {item!r}')
...
```

在这里要分清 `#!python 0` `#!python 3` `#!python 6` `#!python 9` 是 `#!python print` 的输出，而那个 `None` 和 `#!python [None, None, None]` 分别是 `#!python next(m)` 和 `#!python list(m)` 的结果，因为我们已经知道 `#!python print` 就是返回 `None` 的. 在第二次调用 `#!python list(m)` 时，并没有调用 `#!python print` 输出任何东西，并且结果是一个空列表. 在用尽的 `#!python map` 对象上使用 `#!python for` 循环，也不会有任何结果. 注意到在上面的几个例子中，我们都在第 1 行重新创建了 `m`，就是这个原因.

!!! note "迭代器"

    这背后涉及更通用的概念：**迭代器**. `#!python map` 实际上能同时操作任意数量的迭代器：

    ```python
    >>> list(map(print, range(8), 'abcd', [4, ["w", "q"] , -8]))
    0 a 4
    1 b ['w', 'q']
    2 c -8
    [None, None, None]
    ```

## 列表推导式

列表推导式（list comprehension）是一种能够快速生成特定值的列表的语法：

```python
[expression for item in iterable]
```

其中 `expression` 是要生成的项的表达式，`iterable` 是任何可以迭代的对象，例如 `#!python map` 对象、列表、字符串等等，任何能放在 `#!python for ... in ...` 结构中的东西. 它和下面的写法在效果上是等同的，其中 `L` 就是结果的列表：

```python
L = []
for item in iterable:
    L.append(expression)
```

用 `#!python map` 的例子来看看吧. 我们之前做过这种事：

```python
>>> L = ["6", "28", "496", "8128"]
>>> list(map(int, L))
[6, 28, 496, 8128]
```

这也可以用列表推导式来写，就是这样：

```python
>>> [int(v) for v in L]
[6, 28, 496, 8128]
```

如果这个语法还是看起来很怪的话，可以这么理解：

- 生成一个新列表：对于 `L` 中的每一项 `v`...
- ...计算 `#!python int(v)`，将结果添加到新列表中.

那么 `#!python list(map(my_fun, L))` 对应的列表推导式写法就是下面这样：“对于 `L` 中的每一项 `u`，计算 `#!python my_fun(u)`”：

```python
>>> L = [1, "abc", [-8], 9.5]
>>> [my_fun(u) for u in L]
I see you: 1
I see you: 'abc'
I see you: [-8]
I see you: 9.5
[3, 'abcabcabc', [-8, -8, -8], 28.5]
```

而我们的 `#!python map(int, input().split())` 也就可以写成：“对于 `#!python input().split()` 中的每一项 `b`，计算 `#!python int(b)`”：

```python
[int(b) for b in input().split()]
```

总体来说，列表推导式的表现力要比 `#!python map` 更强，而要比明确地写 `#!python for` 循环更简洁. 例如下面的这四种写法都生成同样的列表 `L`：

```python
# 列表推导式写法
L = [i ** 2 for i in range(9, 25, 3)]

# map 写法
def my_square_fun(x):
    return x ** 2
L = list(map(my_square_fun, range(9, 25, 3)))

# map + lambda 写法
L = list(map(lambda y: y ** 2, range(9, 25, 3)))

# for 循环写法
L = []
for c in range(9, 25, 3):
    L.append(c ** 2)
```

!!! note "生成器表达式"

    写列表推导式的时候，用方括号 `#!python []` 是必须的，这样才会生成一个列表. 在 Python 中还有一种叫做**生成器表达式**（generator expression）的东西，它的语法和列表推导式相同，除了把方括号 `#!python []` 换成圆括号 `#!python ()`：

    ```python
    >>> L = [i ** 2 for i in range(9, 25, 3)]
    >>> L
    [81, 144, 225, 324, 441, 576]
    >>> g = (i ** 2 for i in range(9, 25, 3))
    >>> g
    <generator object <genexpr> at 0x1024a18a0>
    >>> list(g)
    [81, 144, 225, 324, 441, 576]
    ```

    生成器与 `#!python map` 对象实际上表现得几乎一模一样；最重要的一点是，它们都是惰性求值的. 而列表推导式不是惰性求值的；在我们回车的那一刻，所有的值就全部被计算出来了.

列表推导式还有其他写法；可以有任意数量的 `#!python for` 语句：

```python
[expression for item1 in iterable1 for item2 in iterable2]
# 和下面一样
L = []
for item1 in iterable1:
    for item2 in iterable2:
        L.append(expression)
```

也可以在其后进行筛选操作：

```python
[expression for item in iterable if condition]
# 和下面一样
L = []
for item in iterable:
    if condition:
        L.append(expression)
```

- 生成一个新列表：对于 `iterable` 中的每一项 `item`...
- ...如果 `condition` 是真值，则计算 `expression`，将结果添加到新列表中；...
- ...否则，跳过这个元素.

例如在上面 `#!python i ** 2` 的例子中，如果我们只想筛选其平方后个位为 4 的结果：

```python
>>> [i ** 2 for i in range(9, 25, 3) if (i ** 2) % 10 == 4]
[144, 324]
```

或者是，例如对于一个列表的字符串，筛掉空串：

```python
>>> L = "/2024/10//01/".split("/")
>>> L
['', '2024', '10', '', '01', '']
>>> [t for t in L if t]
['2024', '10', '01']
```

再或者，生成 `#!python "123"` 和 `#!python "xyz"` 中每个字符的所有组合：

```python
>>> [a + b for a in "123" for b in "xyz"]
['1x', '1y', '1z', '2x', '2y', '2z', '3x', '3y', '3z']
```

### 一个实际的例子

!!! note "提示"

    这部分不需要掌握，只是作为一个例子来演示在 Python 中，我们可以以很优雅的方式用很少的几行代码就实现相当复杂的操作. 从中可以看到一些函数式编程思想的影子.

我们来研究一个比较实际的例子：有一个文件，其中包含了很多同学的姓名、学号和成绩数据. 为了方便起见，我们直接把这个文件作为字符串来提供：

```python
lstr = """
张三, 3240102001, 91
李空, 3230100721, 76.8
王五, 3240300112, 59
小明, 3210106042, 83.3
...
""".strip()
```

!!! tip "多行字符串"

    三对引号 `#!python """..."""` 是 Python 中的**多行字符串**的写法：在其中可以任意换行，这些全都算作字符串的内容.

我们想做的事情有两件：

1. 计算所有同学成绩的平均值；
2. 计算有多少同学没有达到及格线（60 分）.

那么首先，观察到每一行都是一位同学的数据，我们可以在字符串上使用 `#!python splitlines()` 方法将字符串从换行的地方分割开来，变成包含字符串的列表.

```python
lines = lstr.splitlines()
```

然后，注意到每一行都包含由逗号分隔的 3 项；我们在这里只需要第 3 项，即成绩.

```python
split_items = [l.split(",") for l in lines]
scores_str = [l[2] for l in split_items]
scores = [float(l) for l in scores_str]
```

至此，`scores` 列表已经保存了所有同学的成绩项，现在只需求平均值和统计不及格人数：

```python
average_score = sum(scores) / len(scores)
fail_count = sum([i < 60 for i in scores])
```

就是这样！注意在计算不及格人数时我们的做法：首先将 `scores` 转换为一个表示对应成绩是否及格的布尔值列表. 通过利用 `True` 和 `False` 参与运算时分别有 `#!python 1` 和 `#!python 0` 的值这一特性，我们可以很容易地用求和操作实现数量的统计.

综合起来，我们可以写成下面这样：

```python
lstr = """
张三, 3240102001, 91
李空, 3230100721, 76.8
王五, 3240300112, 59
小明, 3210106042, 83.3
...
""".strip()

# 这一行应该这样读：
# - 对于 lstr.splitlines() 结果中的每一个值 l...
# - 计算 float(l.split(',')[2]) 的值：
#   - 首先，进行 l.split(',')，将一行分割为 3 项...
#   - 然后，取 3 项中的第 3 项...
#   - 最后，将其转换为 float
scores = [float(l.split(',')[2]) for l in lstr.splitlines()]

average_score = sum(scores) / len(scores)
fail_count = sum([i < 60 for i in scores])
```

想象一下，如果不用列表推导式来写的话，我们要写成什么样啊！
