# 循环

循环是任何编程语言中控制流语法的重要组成部分，它允许我们重复执行一段代码，直到满足某个条件为止。在Shell脚本中，我们可以使用以下几种循环结构：

- for-in 循环: 依次循环某个数组中的每个元素。
- for 循环：带有初始化、判断条件、迭代语句的循环。
- while循环: 在某个条件为真时，重复执行一段代码。
- until循环: 在某个条件为假时，重复执行一段代码。

接下来就让我们逐一讲解这些循环结构。

## 数组？

数组是Shell脚本中的一种数据结构，它可以存储多个值，并且可以通过索引来访问这些值。在Shell脚本中，我们可以使用以下语法来定义一个数组：

```shell
array=("value1" "value2" "value3")
```

数组中的元素都拥有一个唯一的索引，索引从0开始。我们可以使用以下语法来访问数组中的元素：

```shell
array[0]
```

这将会返回array数组中的第一个元素，即"value1"。

## for-in 循环

for-in循环是Shell脚本中最常用的循环结构之一，它允许我们依次循环某个数组中的每个元素。以下是一个简单的for-in循环示例：

```shell
for variable in array
do
    loop_body
done
```

在这个示例中，variable是循环变量，array是要循环的数组。在每次循环中，variable都会被赋值为array中的一个元素，然后执行loop_body。当array中的所有元素都被循环完毕后，循环结束。

## for 循环

for循环是Shell脚本中另一种常用的循环结构，它允许我们使用初始化、判断条件、迭代语句来控制循环的执行。以下是一个简单的for循环示例：

```shell
for ((initialization; condition; iteration))
do
    loop_body
done
```

在这个示例中，initialization是循环的初始化语句，condition是循环的判断条件，iteration是循环的迭代语句。在每次循环中，condition都会被判断，如果为真，则执行loop_body，然后执行iteration。当condition为假时，循环结束。

## while循环

while循环是Shell脚本中另一种常用的循环结构，它允许我们在某个条件为真时，重复执行一段代码。以下是一个简单的while循环示例：

```shell
while condition
do
    loop_body
done
```

在这个示例中，condition是循环的判断条件，loop_body是循环的主体。在每次循环中，condition都会被判断，如果为真，则执行loop_body。当condition为假时，循环结束。

## until循环

until循环刚好与while循环相反，它允许我们在某个条件为假时，重复执行一段代码。以下是一个简单的until循环示例：

```shell
until condition
do
    loop_body
done
```

在这个示例中，condition是循环的判断条件，loop_body是循环的主体。在每次循环中，condition都会被判断，如果为假，则执行loop_body。当condition为真时，循环结束。

until循环常常被用于多次尝试的场景下，例如尝试连接到一个远程服务器，直到连接成功为止。

## 课后作业

- 使用for循环制作一个程序，程序读取一个正整数N，并输出 2^N 的值。
- 使用while循环制作一个猜数字游戏，程序随机生成一个1到100之间的整数，玩家需要猜出这个数字是多少，程序会根据玩家的猜测给出相应的提示，直到玩家猜对为止。
- 使用for-in循环制作一个程序，该程序读取一个列表，并输出它的所有元素。
