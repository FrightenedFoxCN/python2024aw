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

### 函数定义及其调用

### 变量的作用域

### 函数也是一种变量

### 迭代器和惰性求值

## 常见问题

