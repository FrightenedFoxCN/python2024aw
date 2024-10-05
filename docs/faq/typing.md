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
    >>> str.split
    <method 'split' of 'str' objects>
    >>> str.split("abacadabra", "a")
    ['', 'b', 'c', 'd', 'br', '']
    ```

    这和 `#!python "abacadabra".split("a")` 是完全等价的.

我们以一道题目结束这一节：

!!! quote "判断题"

    当输入是：`45,8` 时，下面程序的输出结果是 37。

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

!!! note "提示"

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

最后，我们以一个有趣的例子结束本讲：

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

    因为 `#!python input` 函数是一个 `#!python builtin_function_or_method` 类型的值，而不是一个字符串. 它没有 `#!python split` 方法. 只有调用它，我们才能得到一个 `#!python str` 类型的值.