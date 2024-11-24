# 程序的控制流

## 知识点回顾

!!! warning "警告"

    此处仅对一些易忽视的、容易出错的知识点和常用的知识点进行回顾，**不能**代替课本和课件的学习！同时，我们在这里会提及的东西可能会超出标题所框定的范围，但尽可能限制在课程范围之内，如果有超出范围的内容，将放在蓝色方框中以示提醒.

所谓程序的控制流（control flow），就是程序按照何种方式和顺序执行. Python 的执行是“一行一行”完成的. 也就是说，当你写下代码时，应当也能够想象出程序按照怎么样的方法进行执行. 在计算机的世界中没有魔法，所以程序的执行是可靠的、可以解释的，这是我们希望读者记住的一点.

!!! note "表达式、语句、子句"

    在这里，为了描述的清晰性，我们会引入几个术语. 表达式（expression）是我们通常说的一个“算式”，它能够给出一个值. 语句（statement）是程序执行过程中会遇到的每一行内容，它会一行一行地被执行. 子句（substatement）是一类在某个语句内部的语句，它们往往依赖于最外层的语句存在，如果独立存在则没有明确的语义. 我们会提到 `#!python else` “子句”和 `#!python if` “语句”，就是出于这样的考量.

!!! note "空语句"

    在 Python 中，有一个表示“什么也不做”的语句，叫做 `#!python pass`. 当我们直接写下这个语句时，什么也不会发生. 它只是一个占位符，用来保证在应该有语句的地方确实有语句存在而已.

### 分支语句

有一个非常经典的笑话：

> - 甲：“回来的时候帮我带四个包子，如果看到了卖西瓜的，就买一个。”
> - 乙：“好的。”
> - ……
> - 甲：“为什么你只买了一个包子？”
> - 乙：“因为我看到了卖西瓜的。”

这时甲和乙对这几句话的理解出现了分歧. 但无论分歧是什么，这里都出现了一个判断：是否出现卖西瓜的人会影响买回来的东西是什么. 在程序编写的过程中，我们也会遇到类似的问题. 而 `#!python if` 语句就是用来查看是否遇到这种条件的：

```python
if a == 0:
    print("Zero!")
```

在出现 `#!python if` 语句的地方，我们注意到后面出现了一个冒号，后面的语句则带有一些缩进. 这是语句块（block）的指示：同一级缩进中的语句会被理解成一整块语句，而冒号指示一个语句块的开始. 我们可以将 `#!python if` 语句的执行拆解成以下几个部分：

```python hl_lines="2"
<statement1>
if <cond_expr>:
    <statement2>
<statement3>
```

其中的 `<statement>` 表示任意语句序列. 这个程序会按顺序执行，在执行完 `<statement1>` 之后，因为出现了 `#!python if` 语句，它会计算 `<cond_expr>` 的值，并且将其理解成布尔值，如果它的值为真，则执行 `<statement2>`，否则将其视作已经完成. 最后，它会回归原本线性的控制流，进入 `<statement3>` 执行.

注意到，`<statement2>` 的执行是根据 `<cond_expr>` 的值的真假性决定的. 在这里，根据它的真假性，程序的执行流发生了分叉，这就是为什么我们称之为分支语句. 而实际上，如果 `<cond_expr>` 的值为假，我们也可以使用 `#!python else` 子句来使得程序做一些事情：

```python hl_lines="4"
<statement1>
if <cond_expr>:
    <statement2>
else:
    <statement3>
<statement4>
```

在执行完 `<statement1>` 之后，程序进入 `#!python if` 语句的执行. 如果 `<cond_expr>` 为真，那么它会执行 `<statement2>`，这与我们前面的叙述是一致的；而如果 `<cond_expr>` 为假，那么它会执行 `<statement3>`，这是我们新增的 `#!python else` 子句表达的. 然后，在执行了 `<statement2>` **或者** `<statement3>` 之后，它会回归正常的控制流，执行 `<statement4>`.

敏锐的读者可能会注意到，只用 `#!python if` 语句就完全可以实现上面的语句的效果，我们可以这样来改写上面的程序：

```python
<statement1>
if <cond_expr>:
    <statement2>
if not <cond_expr>:
    <statement3>
<statement4>
```

这样的程序在实际执行的时候结果与上面的代码等价. 但是，两者的控制流并不相同——如果画出其执行流程图的话，会发现它进行了两次对 `<cond_expr>` 的计算和分支，而非像上面的代码那样只执行了一次计算和分支. 类似地，我们也可以用 `#!python if ... else ...` 来改写最开始的 `#!python if` 语句：

```python hl_lines="4-5"
<statement1>
if <cond_expr>:
    <statement2>
else:
    pass
<statement3>
```

也就是说，默认地，没有 `else` 语句就意味着在这种情况下什么也不做. 那么，如果有多个条件呢？我们通常会需要在 `#!python else` 子句当中嵌套地使用 `#!python if ... else ...` 语句，比方说：

```python hl_lines="5-8"
<statement1>
if <cond_expr1>:
    <statement2>
else:
    if <cond_expr2>:
        <statement3>
    else:
        <statement4>
<statement5>
```

这样的嵌套当然也是可以的. 但是，对于以缩进分隔语句块的方式来说，这样的写法也是并不美观的. 因此，Python 的设计者发明了 `#!python elif`，它是 else if 的缩写：

```python hl_lines="5-8"
<statement1>
if <cond_expr1>:
    <statement2>
elif <cond_expr2>:
    <statement3>
else:
    <statement4>
<statement5>
```

当然，同样可以省略掉 `#!python else` 语句，这和将 `<statement4>` 替换成 `#!python pass` 是完全等价的. 这个语句会首先判断 `<cond_expr1>` 是否为真，如果为真，则执行 `<statement2>`. 然后，在 `<cond_expr1>` 为假的情况下，它会对 `<cond_expr2>` 进行判断. 如果为真，则执行 `<statement3>`，否则执行 `<statement4>`. 在完成这一切操作之后，它会回归 `<statement5>` 的执行.

!!! note "一点说明"

    此后我们将省略上面代码中的 `<statement1>` 和 `<statement5>`，将 `<statement1>` 执行完毕称作**进入语句的执行**，将进入 `<statement5>` 执行称作**完成语句的执行**或者**终止语句的执行**.

!!! note "`#!python match` 语句"

    `#!python match` 语句是 Python 3.10 版本新增的一种语句，它的功能被称为“模式匹配”（pattern matching）. 实际上，它也可以被视作是一种分支语句，而且是一种更紧凑，更强大的分支语句. 它的语法结构如下：

    ```python
    <statement1>
    match <subject_expr>:
        case <patterns> [if <cond_expr>]:
            <statement2>
        case ...
    <statement3>
    ```

    其中中括号表示这一部分是可选的. 在一个 `#!python match` 语句中，可以插入多个 `#!python case` 子句. 它会根据 `<subject_expr>` 的值找到第一个满足 `<patterns>` 且 `<cond_expr>` 为真的 `#!python case` 子句并执行其下的语句块. 而 `<pattern>` 则有许多种，其中可以实现赋值操作等操作. 有兴趣的读者可以阅读[官方描述](https://docs.python.org/3/reference/compound_stmts.html#patterns)以获得进一步的理解.

`#!python if` 语句的另一种用法是用在一个表达式中. 一个表达式可以形如：

```python
<expr1> if <cond_expr> else <expr2>
```

这个表达式的值在 `<cond_expr>` 为真时会被计算成 `<expr1>`，反之则会被计算成 `<expr2>`.

### 循环语句

循环语句有两种类型，`#!python for` 语句和 `#!python while` 语句. `#!python while` 语句对于编程新手来说比较好理解，它的基本语法如下：

```python
while <cond_expr>:
    <statements>
```

当 `<cond_expr>` 为真时，它会执行 `<statements>`. 在执行结束之后，它会再次判断 `<cond_expr>` 是否为真，如果为真，则再次执行 `<statements>`，直到 `<cond_expr>` 为假为止.

另外，我们还有两个作用在控制流上的执行，称作 `#!python break` 语句和 `#!python continue` 语句. 这两个语句分别会终止循环语句的执行和终止当前 `<statements>` 中的剩余部分，直接进入下一次 `<cond_expr>` 的判断流程. 实际上，可以使用 `#!python break` 语句和 `#!python if` 语句的组合来实现循环，这往往对新手来说是比较清晰的：

```python
while True:
    <statement1>    # 我们需要循环执行的是哪些东西？
    if <cond_expr>: # 何时应该终止循环？
        break
    <statement2>    # 如果还没有终止循环，那么还需要进行哪些处理？
```

不够熟悉的读者可以尝试用这种形式和想法先尝试写一些代码，这往往是比较牢靠的.

!!! warning "循环变量的初始化"

    通常，我们需要在一个循环中记录某些东西. 这些东西有时应该在循环开始时清空，有时则不应该清空，这是一个非常常见的问题，需要仔细思考.

另外，和 `#!python if` 语句类似，可以在 `#!python while` 语句后边加上 `#!python else` 子句：

```python
while <cond_expr>:
    <statement1>
else:
    <statement2>
```

如果**循环正常结束**，则 `<statement2>` 会在循环结束后被执行一次. 所谓的循环正常结束，指的就是 `<cond_expr>` 为假结束循环的情况. 通过 `#!python break` 语句跳出循环时不会执行 `<statement2>`，而 `#!python continue` 不会直接终止循环，自然不会引发循环的非正常结束.

!!! notes "对 C 语言基础者的提示"

    学过 C 语言的读者可能会想，在 Python 中是否存在 `#!c do ... while ...` 语句这样的东西呢. 答案是否定的. 但所幸，使用上面 `#!python while True` 的范式完全可以实现一个自己的 `#!c do ... while ...` 形式的循环，读者可以自行思考其实现.

`#!python for` 语句的功能相较 `#!python while` 语句更为强大，我们接下来解释它的含义. 它的基本语法如下：

```python
for <varname> in <iter_obj>:
    <statements>
```

同样注意这里的缩进. 在这里，我们会碰到一个新的概念，称作迭代（iteration）. 上边的 `<iter_obj>` 就是一个可迭代的对象. 

!!! notes "关于迭代器"

    所谓迭代，顾名思义，就是“反复代入”. 而可迭代也就是说，我们可以“从中”拿到一些东西：

    ```python
    >>> iter([1, 2, 3])
    <list_iterator object at 0x0000018840F12F20>
    ```

    `at` 后边的东西是内存地址，每次执行都会有差异，无需在意. 需要注意的是，我们得到了一个 `list_iterator` 类的对象. 所谓的迭代器（iterator）是 Python 用来遍历一组数据的工具. 我们可以在上面调用 `#!python next` 函数来访问其中的下一个对象：

    ```python hl_lines="2"
    >>> a = [1, 2, 3]
    >>> a_iter = iter(a)
    >>> next(a_iter)
    1
    >>> next(a_iter)
    2
    ```

    注意其中的第二行，我们为这个列表创建了一个迭代器. 在迭代器中记录了两个信息，一是迭代目前所在的位置，而是所需迭代的所有内容的位置. 在两次调用 `#!python next` 函数时，调用的结果不同，这是因为迭代器内部记录的信息在 `#!python next` 函数的调用过程中发生了改变.

那么，一个问题是，哪些东西是可迭代的，哪些东西是不可迭代的？就我们已经学过和将要学到的类型而言，`#!python str`，`#!python list`，`#!python tuple`，`#!python set`，`#!python dict` 是所有我们需要掌握的可迭代的类型. 这些类型从形式上看自然就有“可迭代”的特征：里边有元素，可以一个一个取出来访问. 另一种特殊的可迭代类型是 `#!python range` 类型：

```python
>>> type(range(10))
<class 'range'>
```

它最多可以有三个参数：

- 一个参数的情况，`#!python range(n)` 会遍历从 `0` 到 `n - 1` 的整数；
- 两个参数的情况，`#!python range(m, n)` 会遍历从 `m` 到 `n - 1` 的整数；
- 三个参数的情况，`#!python range(m, n, k)` 会以 `k` 为步长遍历 `m` 到最大的**小于** `n` 的整数.

例如，`range(0, 12, 2)` 会返回六个元素，分别为 `0, 2, 4, 6, 8, 10`.

!!! notes "解包"

    `range` 类型的对象和迭代器也可以被 `*` 解包，例如：

    ```python
    >>> print(*range(0, 12, 2))
    0 2 4 6 8 10
    >>> print(*iter(range(0, 12, 2)))
    0 2 4 6 8 10
    >>> print(*iter([1, 2, 3]))
    1 2 3
    ```

在有了上面这些关于迭代器的介绍之后，`#!python for` 循环的语义就不难理解了，它首先会在 `<iter_obj>` 上调用 `#!python iter` 函数，得到一个对应的迭代器；然后，将 `<var_name>` 赋值成在这个迭代器上调用 `#!python next` 函数的结果，执行 `<statements>`. 在执行完之后，如果迭代器中还有剩余的内容，那么它继续调用 `#!python next` 函数，直到执行结束.

!!! notes "关于迭代器的更多补充"

    在一个迭代器上调用 `#!python iter` 函数仍然会得到一个迭代器类型的对象：

    ```python
    >>> iter(iter([1, 2, 3]))
    <list_iterator object at 0x0000018840F110F0>
    >>> iter(iter(iter([1, 2, 3])))
    <list_iterator object at 0x0000018840F12080>
    ```

    在迭代器中不再有更多元素的时候，它会抛出一个 `#!python StopIteration` 类型的异常（或者用通俗的说法，就是报错），告诉程序现在已经没有东西可迭代了. 关于异常的更多信息，可以参见关于错误处理和恢复的补充材料（暂无）.

    ```python
    >>> l = iter([])
    >>> next(l)
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    StopIteration
    ```

    因此，`for` 循环实际上相当于执行下面的代码：

    ```python hl_lines="3 5 7"
    _iterator = iter(<iter_obj>)
    while _flag:
        try:
            <var_name> = next(_iterator)
        except StopIteration:
            _flag = False
            continue
        <statements>
    ```

    其中第 3 和第 5 行是检查是否存在异常，读者可以忽略. 第 7 行之所以使用 `#!python continue` 来结束循环而非使用 `#!python break` 直接终止循环是为了使得 `#!python while ... else ...` 语句的语义和 `#!python for ... else ...` 的语义一致，读者可以自己思考原因.

同样的，在 `#!python for` 语句中，也可以使用 `#!python break` 和 `#!python continue` 语句来终止循环或者进入下一个迭代. `#!python for ... else ...` 语句也将在循环正常终止的时候执行 `#!python else` 后的语句，循环正常终止的条件和 `#!python while ... else ...` 语句一致，就是迭代器抵达终点而非以 `#!python break` 终止.

!!! notes "一点编程提示"

    读者可能已经发现了，在所有的表述中，所有的执行都可以用 `#!python while True` 的范式实现，除了 `#!python else` 子句会出现语义问题之外. 那么，为什么要设计这么多种不同的循环语句呢？而且，在许多别的语言中，并没有 `#!python else` 子句出现在循环语句之后的语法，实际上这通常并非必须的. 但是，对于程序员来说，虽然新手会认为 `#!python while True` 的范式心智负担最小，但当经验逐渐丰富之后，对于迭代将会有一些不同的、更为自然的认知. 实际上，`#!python for` 语句适用于访问有固定数目的迭代对象的情况下更加自然.

最后，我们介绍一点比较有用，但是课堂上或许没提的小知识. `enumerate` 函数可以从任意可迭代对象（或迭代器）构造一个 `enumerate` 类型的对象，它也是一个迭代器：

```python
>>> l = enumerate([1, 2, 3])
>>> l
<enumerate object at 0x0000018840EE7560>
>>> next(l)
(0, 1)
```

它迭代返回的对象是 `(index, value)` 类型的元组. 也就是说，上边的迭代器中有 `(0, 1), (1, 2), (2, 3)` 三个元素. 这使得我们有一个非常方便的手段来完成一些操作：

```python
>>> s = "python"
>>> for idx, i in enumerate(s):
...     print(idx, i)
...
0 p
1 y
2 t
3 h
4 o
5 n
```

!!! notes "在交互式环境下输入语句块"

    在交互式环境下，如果输入的语句后边带有冒号，那么它就会提示 `...`，这是告诉你应该继续输入的意思. 自己调整缩进并完成整个语句块的书写之后，通过一个空行退出输入，就可以执行一整个语句块.

同时，这个例子也表明，在 `#!python for` 语句中就可以完成解包操作.

### 函数定义及其调用

### 变量的作用域

### 函数也是一种变量

### 迭代器和惰性求值

## 常见问题

