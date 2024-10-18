# 类型与值

!!! warning "警告"

    此处仅对一些易忽视的、容易出错的知识点和常用的知识点进行回顾，**不能**代替课本和课件的学习！同时，我们在这里会提及的东西可能会超出标题所框定的范围，但尽可能限制在课程范围之内，如果有超出范围的内容，将放在蓝色方框中以示提醒.

## 变量

读者应该已经非常熟悉 Python 中的变量了：

```python
a = 1234
```

与 C 等语言不同，Python 是一种**动态强类型**语言. 读者不必现在理解这个概念，但是应当已经知道，对于一些不同的值，我们能够进行不同的操作：例如，对整数能够进行加减乘除，对字符串能够进行拼接、重复等等. 尝试将整数和字符串做加法会产生错误，这也许意味着有一些操作是不被允许的.

```python
>>> 1 + 2
3
>>> "1" + "2"
'12'
>>> 1 + "2"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

让我们首先回到**类型**的概念上. 我们知道，`#!python 1234` 是一个整数，其类型是 `#!python int`；`#!python "1234"` 是一个字符串，其类型是 `#!python str`. **类型是和值绑定的**，也就是说，每个值都有其固定的类型. 我们不能将一个整数作为字符串来处理，反之亦然.

那么变量呢？变量只是一个名字，它可以**容纳**任何类型的值. 我们可以将一个字符串赋给一个变量，然后再将一个整数赋给这个变量，这是完全合法的：

```python
a = '1234'  # 在这里，a 容纳字符串 "1234"
a = 1234    # 现在 a 容纳整数 1234
```

`#!python "1234"` 和 `#!python 1234` 是两个截然不同的值，前者是字符串，后者是整数. 这非常重要.

## `#!python int(input())` 做了什么？

在编程题中，我们经常会使用这样的代码. 也许读者仍然不太清楚这段代码的作用，或说，到底发生了什么：

```python
a = int(input())
```

为方便起见，我们将这段代码拆分成两部分：

```python
s = input()
a = int(s)
```

我们应当已经非常熟悉 `#!python input()` 函数，它会从标准输入中读取一行字符串，在这里“标准输入”的意思就是你的键盘.

```python
>>> s = input()
42
>>> s
'42'
```

我们称 `#!python input()` **返回**一个字符串. 通过使用 `#!python input()` 函数，我们得到了一个字符串 `s`，它的值是 `#!python "42"`. 然而，我们绝不可将字符串作为整数来处理. 我们首先需要将字符串**转换**为整数：

```python
>>> int(s)
42
>>> a = int(s)
>>> a
42
```

`#!python int()` 函数将字符串转换为整数. 请注意，`#!python int()` 函数**不会**改变原来的字符串，而是返回一个新的整数. 有同学写下过类似如下的代码：

```python
b = input()
int(b)
```

这段代码是逻辑上错误的，因为 `#!python int(b)` 的返回值没有被保存（即，我们并没有将这个结果赋给任何变量），所以第二行实际上什么也没做，`b` 仍然是一个字符串. 读者应当注意这一点. 如果我们想要将 `b` 转换为整数，应当这样写：

```python
b = input()
b = int(b)
```

或者，更简洁的写法即：

```python
b = int(input())
```

同样的，我们可以用 `#!python float()` 函数将字符串转换为浮点数，或是用 `#!python str()` 函数将任意类型的值转换为字符串.

```python
>>> float("3.14")
3.14
>>> str(6.28)
'6.28'
>>> int(3.14)
3
>>> int("3.14")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '3.14'
```

我们无法将一个**表示浮点数的字符串**转换为整数，因为 `#!python int()` 函数只能解析表示整数的字符串. 在涉及到浮点数的题目中，就应当使用 `#!python float()` 函数来解析. 不过我们可以将一个**浮点数**转换为整数，这会将小数部分直接截断，注意这与向上取整、向下取整或四舍五入均不同.

## 操作

### 以 `#!python split()` 为例

对于不同的类型，我们能够进行不同的操作. 我们来看一个稍复杂一些的例子：

```python
a, b, c = input().split()
```

或者说，仍然拆作两部分：

```python
s = input()
a, b, c = s.split()
```

在此，我们需要再次强调：**类型和值是绑定的**. 我们已经知道 `#!python input()` 函数返回一个字符串，那么第一行代码执行结束后，`s` 无疑是一个字符串. 那么，从类型的角度来看，研究上面的代码和研究下面的代码是一样的：

```python
s = "1 2 3"
a, b, c = s.split()
```

别忘了，我们永远可以在交互式环境中查看一个操作的结果：

```python
>>> "1 2 3".split()
['1', '2', '3']
```

术语上来讲，`#!python split()` 是 `#!python str` 类型的一个**方法**，不过就目前来说，我们认为这就是我们对字符串能进行的一种**操作**. 再次强调，**类型和值是绑定的**. `#!python split()` 是属于字符串的一个操作，那么，对于任何字符串，我们都可以使用 `#!python split()` 操作. 在上面的例子中，对于 `#!python "1 2 3"` 这个字符串，在其上使用 `#!python split()` 操作，我们得到了一个列表 `#!python ['1', '2', '3']`. 列表也是 Python 中的一种类型，我们在后面的课程中会学习到. 目前我们只需要知道列表可以容纳任意多个值. 那么 `#!python ['1', '2', '3']` 即是容纳了 3 个字符串 `#!python "1"`、`#!python "2"` 和 `#!python "3"` 的列表. `#!python split()` 方法的作用即是将字符串按照空白符分割，它返回一个列表，列表中的元素是原字符串从左到右按照空格分割后的结果，它们也都是字符串.

有一点非常重要：`#!python split()` 是**属于字符串的**方法，我们需要在一个字符串上调用它. 并没有一个叫做 `#!python split()` 的函数.

!!! warning "提示"

    `.` 和 `()` 都是很重要的部分，不可省略或更改. 有同学写下过类似如下的代码：

    ```python
    a, b = input.split()
    # 或者
    a, b = input().split
    # 或者
    a, b = input(),split()  # 注意这里用的是逗号
    ```

    这些代码都是错误的. `#!python input` 是一个函数，我们需要调用它，所以应当写成 `#!python input()`；`split` 是一个方法，我们需要在一个字符串上实施这个操作，所以应当写成 `#!python input().split()`. 至于第三种，逗号 `,` 并不能用于调用方法，也并没有 `split()` 这个函数.


剩下的操作就与下面的代码无异了：

```python
a, b, c = ['1', '2', '3']
```

这会将列表中的 3 个字符串分别赋给变量 `a`、`b` 和 `c`，即 `#!python a = "1"`、`#!python b = "2"` 和 `#!python c = "3"`.

!!! note "提示"

    实际上，这背后涉及到更复杂的知识：**解包**. 我们在这里不会详细讨论什么是解包. 不过读者在编程时，可能会遇到类似的报错：

    ```python hl_lines="5"
    >>> a, b, c = input().split()
    1 2 3 4
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: too many values to unpack (expected 3)
    ```

    这是因为 `#!python input().split()` 的结果列表中有 4 个元素，而左边的变量只有 3 个. Python 无法将 4 个值赋给 3 个变量，所以会报错. 注意到 `too many values to unpack` 这个错误信息，“unpack” 就是解包的意思. 在进行解包时，左边的变量个数必须和右边的值的个数相同.

我们也可以给 `#!python split()` 方法传递一个参数，指定应该在哪个字符处分割字符串：

```python
>>> "/2024/10//01/".split("//")
['/2024/10', '01/']
>>> "/2024/10//01/".split("/")
['', '2024', '10', '', '01', '']
```

朴素的想法告诉我们，如果一个字符串中有 \( n \) 个分隔符，那么 `#!python split()` 方法返回的列表中应当会有 \( n+1 \) 个元素. 事实也的确如此：在上面的例子中，`#!python "/2024/10//01/"` 中有 5 个 `/`，所以 `#!python split('/')` 返回了 6 个元素，每个元素都是原字符串中两个 `/` 之间的内容，或是字符串开头和 `/` 之间的内容，或是 `/` 和字符串结尾之间的内容. 那么，为什么 `#!python 'A'.split('A')` 返回 `#!python ['', '']`，即含有两个空字符串的列表，也就不难理解了.

最后还要注意，不带参数的 `#!python split()` 方法和带空格参数的 `#!python split()` 方法并不一样：

```python
>>> "1  2   3".split()
['1', '2', '3']
>>> "1  2   3".split(" ")
['1', '', '2', '', '', '3']
```

注意这里的 `1` `2` `3` 之间分别有 2 个和 3 个空格. 若不带参数，`#!python split()` 方法会将连续的空白符都视作一个空白符处理，这里的空白符可以是空格、tab、换行符等等. 而 `#!python split(" ")` 方法则只是严格地将**一个空格**作为分隔符. 这在某些题目中是会考察的，例如第 3 周作业题：

!!! quote "7-6 计算立方体水箱的水重量"

    计算立方体水箱的水重量.<br>
    某一水箱的外表是典型的立方体形状，请设计程序计算此水箱装满水后，其中水的重量是多少，要求：长，高，宽的单位是厘米，水的计量单位是吨.

    **输入格式：**<br>
    在一行中依次输入水箱的长，高，宽，各数据之间**至少**用一个空格隔开. 注意：长，高，宽的单位是厘米，可以是小数.

    **输出格式：**<br>
    对每一组输入，在一行中输出此水箱所装的水的重量. 注意：水的计量单位是吨，要保留 3 位小数（提示：用 `#!python format` 函数）.

在此我们只需关注输入格式，即“各数据之间**至少**用一个空格隔开”. 这意味着输入的数据之间可能有多个空格，所以使用 `#!python split(" ")` 一定会出错，必须使用不带任何参数的 `#!python split()`.

最后，请读者自行尝试将空字符串 `#!python ""` 作为参数传递给 `#!python split()` 方法，看看会发生什么.

!!! note "提示"

    实际上我们还有一些有意思的写法：

    ```python
    >>> str.split("abacadabra", "a")
    ['', 'b', 'c', 'd', 'br', '']
    ```

    这和 `#!python "abacadabra".split("a")` 是完全等价的.

我们以一道题目结束这一节：

!!! quote "判断题"

    当输入是：`45,8` 时，下面程序的输出结果是 37.

    ```python
    a, b = input().split(',')
    b = int(b)
    c = int('a', b)
    print(c)
    ```

这是错误的. 读者应当注意到，这里的陷阱在于第 3 行，`#!python int('a', b)` 的意思是将字符串 `#!python 'a'` 转换为整数，这与变量 `a` 毫无关系. 第三行会因为 `#!python 'a'` 不能被解析为一个合法的八进制数而报错（回忆一下八进制数可以包含的内容）. 正确的代码应当是 `#!python c = int(a, b)`，这会正确地将变量 `a` 所容纳的字符串 `#!python "45"` 解析为一个以 8 为基数的整数，即 37.

### `#!python str` 的其他常用方法

`#!python str` 类型还有很多其他的方法，例如 `#!python strip()`、`#!python find()`、`#!python replace()` 等等. 读者可以查阅 Python 官方文档中[字符串的方法](https://docs.python.org/zh-cn/3/library/stdtypes.html#string-methods)一节，搭配交互式环境进行实验.

`#!python find()` 方法接收一个、两个或三个参数：

```python
>>> "programming".find("r")
1
>>> "programming".find("r", 2)
4
>>> "programming".find("r", 5)
-1
>>> "programming".find("r", 2, 5)
4
>>> "programming".find("r", 2, 4)
-1
```

`#!python find()` 方法在字符串中寻找**第一个**出现的子串，并返回其索引，注意索引是从 0 开始的. 如果没有找到，返回 `-1`. 第二个参数是可选的，表示从哪个索引开始查找. 第三个参数也是可选的，表示到哪个索引结束查找. 这个范围是左闭右开的. 例如对于字符串 `#!python "programming"`，每个字符的索引如下：

```
p  r  o  g  r  a  m  m  i  n  g
0  1  2  3  4  5  6  7  8  9  10
```

所以对于上面的例子，`#!python "programming".find("r", 2, 4)` 返回 `-1`，因为 `#!python "r"` 在索引 2 和 4 之间是不存在的（左闭右开！位于索引 4 的 `r` 并不在查找范围内）. `#!python find()` 方法也有一个**反向**的版本 `#!python rfind()`，它会从字符串的末尾开始查找.

`#!python strip()` 方法会去掉字符串两端的空白符：

```python
>>> "  1,    5   ".strip()
'1,    5'
```

与 `#!python split()` 方法类似，`#!python strip()` 方法也可以接收一个参数，表示去掉哪些字符，并且无参数的 `#!python strip()` 方法和带空格参数的 `#!python strip()` 方法是不一样的：

```python
>>> "abacadabra".strip("a")
'bacadabr'
>>> "  \t  abacadabra  \t  ".strip()
'abacadabra'
>>> "  \t  abacadabra  \t  ".strip(" ")
'\t  abacadabra  \t'
```

!!! quote "7-1 产生每位数字相同的 n 位数"

    读入 2 个正整数 A 和 B，1<=A<=9，1<=B<=10，产生数字 AA...A，一共 B 个 A.

    **输入样例 1：**
    ```
      1,  5
    ```
    **输出样例 1：**
    ```
    11111
    ```
    **输入样例 2：**
    ```
            3  ,4
    ```
    **输出样例 2：**
    ```
    3333
    ```

一种（错误的）解是：

```python
a, b = input().split(',')
b = int(b)
print(a * b)
```

请读者思考为什么这段代码是错误的？应当如何修改？提示：注意输入中可能有多余的空格.

!!! note "提示"

    我们可以用 `#!python dir()` 函数查看一个类型或对象的所有方法：

    ```python
    >>> dir(str)
    ['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'removeprefix', 'removesuffix', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
    ```

    `#!python str` 类型有很多方法！当然，这里的绝大多数方法我们都是不会用到的. 如果感兴趣，你可以查看上面给出的官方文档链接，了解每个方法的具体作用.

## 再回到类型

我们可以使用 `#!python type()` 函数查看一个值的类型：

```python
>>> type(1234)
<class 'int'>
>>> type("1234")
<class 'str'>
>>> type(3.14159)
<class 'float'>
```

从上面的输出粗略来看，`#!python type()` 函数告诉我们一个值的类型是什么. 有趣的是，如果我们直接查看 `#!python int`、`#!python str`、`#!python float` 等类型：

```python
>>> int
<class 'int'>
>>> str
<class 'str'>
>>> float
<class 'float'>
```

这是否说明 `#!python type` 函数返回的东西和 `#!python int`、`#!python str`、`#!python float` 这些类型是一样的呢？实际上，是的，我们可以用 `#!python ==` 运算符来比较它们：

```python
>>> type(1234) == int
True
>>> type("1234") == str
True
>>> type(3.14159) == float
True
>>> type(1234) == float
False
```

!!! note "关于 `#!python type`"

    思考一下这一行代码是如何运作的！

    ```python
    >>> type(1)("1234")
    1234
    ```

    ??? info "答案"

        `#!python type(1)` 的结果是 `#!python int`，所以 `#!python type(1)("1234")` 实际上就是 `#!python int("1234")`.

从现在开始，我们要改口了！我们不再使用 `#!python int` 函数、`#!python str` 函数等说法；现在我们称之为 `#!python int` 类型、`#!python str` 类型，等等.

`#!python "1234"` 是一个字符串，它的类型是 `#!python str`，这应当毫无疑问. 我们称 `#!python "1234"` 是一个**对象**，它的**类型**是 `#!python str`——这照应了我们在第一节中提到的说法：Python 是一种**面向对象**的语言. 当我们使用下面的代码：

```python
a = "1234"
b = int(a)
```

我们实际上在做的事情，是使用 `#!python "1234"` 这个 `#!python str` 对象，创建了一个新的 `#!python int` 对象 `#!python 1234`，并将其赋给了变量 `#!python b`. 这就是**类型转换**的过程，在这里，我们将一个 `#!python str` 转换为了一个 `#!python int`. 读者应当注意，`#!python "1234"` 这个 `#!python str` 对象并没有被改变，它仍然是一个 `#!python str`. 我们只是使用它创建了一个新的 `#!python int` 对象 `#!python 1234`. 即使无法完全理解这个过程也无所谓，只需知道这个过程是存在的.

### 任何值都有类型，而类型也是值

我们以一个有趣的例子结束本节：

```python
>>> type(print)
<class 'builtin_function_or_method'>
>>> type(input)
<class 'builtin_function_or_method'>
>>> type(int)
<class 'type'>
>>> type(type)
<class 'type'>
>>> type(type) == type
True
```

无需理解这些东西. 重要的是，通过使用 `#!python type()`，我们可以查看**任何值**的类型，包括函数、类，甚至是 `#!python type` 类型本身. 类型本身也是一个值，这正是 Python 中**一切皆对象**的体现.

我们可以这样：

```python
>>> int()
0
>>> str()
''
>>> float()
0.0
```

当我们像这样使用 `#!python int`、`#!python str`、`#!python float` 等类型时，我们实际上就是在创建一个新的 `#!python int`、`#!python str`、`#!python float` 对象. 创建一个新的数值的默认值是 0，创建一个新的字符串的默认值是空字符串.

当然，并不是所有的类型都可以这样使用：

```python
>>> type()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: type() takes 1 or 3 arguments
```

这已经远远超出了我们的课程范围.

!!! tip "提示"

    现在我们应当可以理解下面的代码为什么是错误的：

    ```python
    a = input.split()
    ```

    因为 `#!python input` 函数是一个 `#!python builtin_function_or_method` 类型的值，而不是一个字符串. 它没有 `#!python split` 方法. 只有使用 `#!python input()`，我们才能得到一个 `#!python str` 类型的值，并在其上调用 `#!python split` 方法.

## `#!python bool` 类型与逻辑运算

在 Python 中，`#!python bool` 表示**布尔值**. `#!python bool` 类型只有两个值：`True` 和 `False`. 同 `#!python int`、`#!python str` 等类型一样，`#!python bool` 也是 Python 中的一个**内置类型**. 它被广泛地运用在各种控制结构中，例如 `#!python if` 语句、`#!python while` 循环等等.

```python
>>> type(bool)
<class 'type'>
>>> type(True)
<class 'bool'>
>>> type(False)
<class 'bool'>
```

而提到布尔值，我们就不得不提到另一对极其重要的概念：**真值**（truthy）和**假值**（falsy），我们现在可以理解为，能使得控制结构中的条件成立的值是真值，反之则是假值. 在这里非常非常非常重要的一点是，真值和假值只是一个**概念**，它们并不指代一个具体的值.

!!! quote "[逻辑值检测](https://docs.python.org/zh-cn/3/library/stdtypes.html#truth-value-testing)"

    ...以下基本完整地列出了具有假值的内置对象：

    - 被定义为假值的常量: `None` 和 `False`
    - 任何数值类型的零: `#!python 0`, `#!python 0.0`, `#!python 0j`, `#!python Decimal(0)`, `#!python Fraction(0, 1)`
    - 空的序列和多项集: `#!python ''`, `#!python ()`, `#!python []`, `#!python {}`, `#!python set()`, `#!python range(0)`

```python
if 0:  # 把这里的 0 换成上面的任何一个假值，效果都是一样的
    print("这一句永远不会打印")
```

如果将任何假值转换为 `#!python bool` 类型，就会得到一个 `False`：

```python
>>> bool(0)
False
>>> bool("")
False
>>> bool([])
False
>>> bool(False)
False
```

而真值则是除了假值之外的所有值. 任何非零的数，非空的字符串、列表、元组、字典、集合，甚至是函数、类型等等，都是真值. 例如：

```python
>>> bool(-1)
True
>>> bool(1e-324)  # 10 的 -324 次方
True
>>> bool(" ")     # 一个空格
True
>>> bool([0])
True
>>> bool(bool)
True
>>> bool(type)
True
>>> bool(print)
True
```

同样的，这些对变量也是适用的：

```python
>>> a = [0]
>>> bool(a)
True
```

!!! quote "判断题"

    在 Python 中，`#!python if` 语句的条件必须是一个布尔值.

当然，我们知道这是错误的. 条件可以是任何值，只要它能被转换为布尔值即可.

!!! quote "判断题"

    `#!python bool(FALSE)` 的返回值是 `True`.

这是一道错题，即题目本身是错误的. 它既不是 `#!python bool(False)`（注意大小写！），也不是 `#!python bool("FALSE")`. 在这里 `FALSE` 只会被作为一个变量名来解释，所以结果完全取决于 `FALSE` 这个变量的值：

```python
>>> FALSE = 0
>>> bool(FALSE)
False
>>> FALSE = 1
>>> bool(FALSE)
True
```

当然，如果 `FALSE` 没有被定义，那么会产生一个 `#!python NameError` 错误.

---

我们举个例子：如何将字符串 `#!python "True"` 和 `#!python "False"` 转换为布尔值？直接使用 `#!python bool` 作转换是行不通的：

```python
>>> bool("True")
True
>>> bool("False")
True
```

这是因为任何非空的字符串都是真值，当然包括 `#!python "False"` 这个字符串. 对于这种情况，简单地使用 `#!python if` 语句来判断即可：

```python
>>> s = "True"
>>> if s == "True":
...     b = True
... elif s == "False":
...     b = False
... else:
...     print("Invalid input")
...
>>> b
True
```

### `#!python 0` 不是 `False`

这个小节标题称作 `#!python int` 不是 `#!python bool` 也许会更恰当一些. 我们已经知道 `#!python 0` 是一个假值，但它**不是** `#!python False`：

```python
>>> 0 == False
True
>>> 0 is False
False
```

`#!python ==` 运算符是**值比较**，它会比较两个值是否相等. 而 `#!python is` 运算符是**身份比较**，它会比较两个值是否是同一个对象. 当然，我们无需理解 `#!python is` 运算符的具体细节，只需知道，在这里我们强调的是：`#!python 0` 是一个 `#!python int` 类型的值，而 `#!python False` 是一个 `#!python bool` 类型的值. 它们是不同的类型. 我们很快会看到这意味着什么.

### `#!python bool` 作为数字，以及逻辑运算

在 Python 中，`#!python bool` 类型的值也可以被**当作**数字来使用. `True` 被当作 `#!python 1`，`False` 被当作 `#!python 0`：

```python
>>> True + True
2
>>> True + False
1
>>> False + False
0
>>> True * 3
3
>>> True * False
0
```

!!! note "关于 `#!python bool`"

    实际上 `#!python bool` 类型的值是 `#!python int` 类型的子类，但这并不是我们需要关心的事情.

    ```python
    >>> issubclass(bool, int)
    True
    ```

逻辑运算符 `#!python not`、`#!python and`、`#!python or` 是 Python 中表示逻辑运算的关键字，优先级按这个顺序递减.

`#!python not` 计算一个值的**逻辑非**：

```python
>>> not True
False
>>> not False
True
>>> not -1
False
```

它会将一个真值转换为 `False`，将一个假值转换为 `True`，所以 `#!python not` 运算符的结果永远是一个 `#!python bool` 类型的值.

当然，这没什么意思. 真正有意思的是 `#!python and` 和 `#!python or` 运算符，它们分别计算**逻辑与**和**逻辑或**. 我们应当已经知道逻辑与和逻辑或是如何工作的，不过也许，实际上，它们真正的工作方式并不是我们想象的那样——不妨来看看 Python 文档中的[布尔运算](https://docs.python.org/zh-cn/3/library/stdtypes.html#boolean-operations-and-or-not)一节：

!!! quote "[布尔运算 --- `and`, `or`, `not`](https://docs.python.org/zh-cn/3/library/stdtypes.html#boolean-operations-and-or-not)"

    这些属于布尔运算，按优先级升序排列：

    | **运算** | **结果：** |
    |--------|---------|
    | `#!python x or y` | 如果 *x* 为真值，则 *x*，否则 *y* |
    | `#!python x and y` | 如果 *x* 为假值，则 *x*，否则 *y* |
    | `#!python not x` | 如果 *x* 为假值，则 `True`，否则 `False` |

<!-- 同时，就在这一节的下面，[比较运算](https://docs.python.org/zh-cn/3/library/stdtypes.html#comparisons)一节中，我们可以看到：

!!! quote "[比较运算](https://docs.python.org/zh-cn/3/library/stdtypes.html#comparisons)"

    在 Python 中有八种比较运算符。 它们的优先级相同（比布尔运算的优先级高）。 比较运算可以任意串连；例如，`#!python x < y <= z` 等价于 `#!python x < y and y <= z`，前者的不同之处在于 *y* 只被求值一次（但在两种情况下当 `#!python x < y` 结果为假值时 *z* 都不会被求值）。
-->

我们来看一个例子：

!!! quote "判断题"

    表达式 `#!python 3 and 0 and "hello"` 的值是 `False`.

我们来分析这个表达式：

1. 逻辑与运算是从左到右进行的，所以上面的表达式等价于 `#!python (3 and 0) and "hello"`.
2. 首先来看 `#!python 3 and 0`. 由于 `#!python 3` 是真值，所以根据上面的表格，它的结果是 `#!python 0`.
3. 然后再看 `#!python 0 and "hello"`. 由于 `#!python 0` 是假值，所以根据上面的表格，它的结果是 `#!python 0`.
4. 最后，表达式的值是 `#!python 0`，而不是 `False`. 所以这道题是错误的.

注意到这里体现了 `#!python 0` 不是 `False` 的概念. 在这里最为重要的一点是，尽管这些值在许多方面都可能表现得很相似，但归根结底，它们仍然是不同的类型，不可混为一谈. 这也是我们在本节中反复强调的：**类型和值是绑定的**.

## `None`

`None` 是 Python 中的一个特殊值，表示**空值**. 它是 `NoneType` 类型的唯一一个值. `None` 通常用于表示一个函数没有返回值，或者一个变量存在但没有被赋值. 例如：

```python hl_lines="2"
>>> a = None
>>> a
>>> print(a)
None
>>> type(a)
<class 'NoneType'>
```

注意这里的第二行，交互式环境中没有任何输出. 这是因为交互式环境不会**显示** `#!python None` 的值. 但这里又没有报错——表明 `a` 这个变量是确实存在的. `#!python print()` 函数会正常**输出** `#!python None`.

同时，`#!python print()` 函数的返回值就是 `#!python None`：

```python
>>> a = print("Hello, world!")
Hello, world!
>>> print(a)
None
```

或者，一个更有意思的写法是：

```python
>>> print(print(None))
None
None
```

读者应该可以理解这段代码为什么会输出两个 `#!python None` 了.
