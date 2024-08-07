# 比较操作 - 变量 指令说明

从TIA V13SP1 开始，S7-1200
V4.0开始，支持以下处理Variant类型的变量的指令，如图1-2所示。

![](images/03-01.jpg){width="507" height="516"}

图1 LAD中Variant类型的变量比较操作指令

![](images/03-02.jpg){width="422" height="216"}

图2 SCL中Variant类型的变量比较操作指令

注：EQ_TypeOfDB、NE_TypeOfDB、TypeOfDB指令参见[DB_ANY](../../02-basic/01-Data_Type/08-DB_ANY.md#3-与指定数据类型比较)。

## EQ_Type、NE_Type、TypeOf

表1 LAD指令详情

| LAD指令  | 操作数1    | 操作数2           | 说明            |
|------------|---------|----------------|----------------|
|  操作数1<br>      ┫EQ_Type┣<br>操作数2 | Variant | 除Variant以外所有类型 | 比较操作数1对应的实参与操作数2的数据类型是否相等，相等则该指令返回逻辑运算结果 (RLO)&ldquo;1&rdquo;。如果不相等则该指令返回 RLO&ldquo;0&rdquo;。<br>   操作数1是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。  |
| 操作数1<br>┫NE_Type┣<br>操作数2        | Variant | 除Variant以外所有类型 | 比较操作数1对应的实参与操作数2的数据类型是否不相等，不相等则该指令返回逻辑运算结果 (RLO)&ldquo;1&rdquo;。如果相等则该指令返回 RLO&ldquo;0&rdquo;。<br>  操作数1是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。 |

SCL指令：TypeOf（操作数），操作数是FC/FB的Input/Output/InOut/Temp中定义为Variant类型的参数，该语句输出是数据类型，在程序中只能用在IF与CASE进行比较。

=== "用法1"

    IF指令，操作数对应的实参的类型与一个变量类型的比较，例如：

    ```c

    IF (TypeOf(操作数1) = 变量类型（例如Byte）)
    ...
    END_IF;

    ```

=== "用法2"

    IF指令，两个操作数对应的实参的类型比较，例如：

    ```c
    IF (TypeOf(操作数1) = TypeOf(操作数2))
    ...
    END_IF;

    ```

=== "用法3"

    CASE OF指令，操作数对应的实参的类型与多个变量类型的比较，例如：

    ```c
    CASE (TypeOf(操作数)) OF
    Byte:
    ...
    Int:
    ...
    ELSE
    ...
    END_CASE;

    ```

!!! info "使用举例"

    编写FC，检查输入Variant变量类型，Byte则输出True，其它则输出False.

如图3所示

![](images/03-03.jpg){width="330" height="173"}

图3 FC6参数定义

![](images/03-04.jpg){width="556" height="108"}

图4 程序详情

SCL的版本程序，如图5所示。

![](images/03-05.jpg){width="220" height="99"}

图5 SCL版本的程序

OB1多次调用该FC6，可以看到结果，DB16.Static_1是Byte类型，DB16.Static_3不是Byte类型。

![](images/03-06.jpg){width="556" height="281"}

图6 OB1多次调用FC6



## EQ_ElemType、NE_ElemType、TypeOfElements

表2 LAD指令说明


| LAD指令         | 操作数1         | 操作数2         | 说明            |
|-----------------|-----------------|-----------------|-----------------|
| 操作数1 <br>  ┫EQ_ElemType┣  <br> 操作数2  | Variant   | 除Variant以外所有类型  | 如果操作数1对应的实参是数组类型，则比较其数组元素与操作数2的数据类型是否相等，相等则该指令返回逻辑运算结果 (RLO)“1”，如果不相等则该指令返回 RLO“0”；<br> 如果操作数1对应的实参不是数组类型，并且操作数1对应的实参与操作数2的数据类型相等，则该指令返回 RLO“1”，其余情况，该指令返回 RLO“0”。<br> 操作数1是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。比较之前，通常先使用IS_ARRAY检查操作数1对应的实参是否是数组类型。|
| 操作数1 <br>  ┫NE_ElemType┣  <br> 操作数2  | Variant   | 除Variant以外所有类型  | 如果操作数1对应的实参是数组类型，则比较其数组元素与操作数2的数据类型是否相等，不相等则该指令返回逻辑运算结果 (RLO)“1”， <br> 如果相等则该指令返回 RLO“0”。如果操作数1对应的实参不是数组类型，则该指令返回 RLO“1”。 <br> 操作数1是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。比较之前，通常先使用IS_ARRAY检查操作数1对应的实参是否是数组类型。|


SCL指令：TypeOfElements（操作数），操作数是FC/FB的Input/Output/InOut/Temp中定义为Variant类型的参数，该语句输出是数据类型，在程序中只能用在IF与CASE进行比较。

比较之前，通常先使用IS_ARRAY检查操作数对应的实参是否是数组类型。

=== "用法1"

    IF指令，操作数对应的实参为数组类型，对该数组元素的类型与一个变量类型的比较，例如：

    ```c
    IF (TypeOfElements(操作数1) = 变量类型（例如Byte）)

    ...

    END_IF;

    ```

=== "用法2"

    IF指令，两个操作数对应的实参均为数组类型的类型，比较它们数组元素的类型，例如：

    ```c

    IF (TypeOfElements(操作数1) = TypeOfElements(操作数2))
    ...
    END_IF;

    ```

=== "用法3"

    CASE OF指令，操作数对应的实参为数组类型，对该数组元素的类型与多个变量类型的比较，例如：

    ```c
    CASE (TypeOfElements(操作数)) OF
    Byte:
    ...
    Int:
    ...
    ELSE
    ...
    END_CASE;

    ```

!!! note "注意"

    - 1.如果上述三种用法操作数不是数组类型，但是数据类型和比较对象的数据类型相同，也会当做该数据类型的数组进行处理，相当于执行的TypeOf指令。
    - 2.用法2，也可以是这样的：

    ```c

    IF (TypeOfElements(操作数1) = TypeOf(操作数2))
    ...
    END_IF;

    \\或者

    IF (TypeOf(操作数1) = TypeOfElements(操作数2))
    ...
    END_IF;

    ```

    即一边是数组，一边不是数组的比较。

使用举例：

编写FC，检查输入Variant变量类型，数组元素如果是Byte则输出1为True，输出2为False，数组元素如果是Int则输出1为False，输出2为True，其余情况输出1为False，输出2为False，如图7-10所示。

![](images/03-07.jpg){width="330" height="212"}

图7 FC7参数定义

![](images/03-08.jpg){width="556" height="587"}

图8 程序详情

SCL的版本程序，如图9所示。

![](images/03-09.jpg){width="282" height="286"}

图9 SCL版本的程序

OB1多次调用该FC7，可以看到结果，DB17.Static_1是Byte数组，DB17.Static_4是Int数组，DB17.Static_7不是以上两种类型。

![](images/03-10.jpg){width="556" height="478"}

图10 OB1多次调用FC7

## IS_NULL、NOT_NULL

表3 LAD指令说明

|LAD指令| 操作数| 说明 |
|---------------|----------|------------------------------|
|操作数 <br> ┫IS_NULL┣ |Variant |如果操作数对应的实参有指向变量，该指令返回逻辑运算结果 (RLO)“0”，否则该指令返回 RLO“1”。操作数是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。|
|操作数 <br> ┫NOT_NULL┣ | Variant |如果操作数对应的实参有指向变量，该指令返回逻辑运算结果 (RLO)“1”，否则该指令返回 RLO“1”。操作数是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。 |


对于SCL，虽然没有相对应指令，但是可以在IF指令中，将Variant变量与NULL比较

```c

IF (操作数 = NULL)
...
END_IF;

```

理论上来说，对于每个参数出现了Variant的FC/FB，都应该检查该Variant变量是否指向了空指针，此处的空指针不一定是形参填写NULL，也有可能填写没有初始化的Temp中的Variant。

对于最新的S7-1200 V4.2版本，只有一种情况可以初始化Temp中的Variant，就是指令DB_ANY_TO_VARIANT（参见[DB_ANY](../../02-basic/01-Data_Type/08-DB_ANY.md#2-指向-udt-或-sdt-建立的-db-块的-db_any)，同时运行没有错误，否则Temp中的Variant就相当于NULL。

!!! info "使用举例"

    程序架构：OB1调用FC9，FC9调用FC8，FC8中检查3个输入是否是NULL，是则输出True，不是则输出False，FC9的3个Temp变量作为FC8的3个输入，Temp_1是不赋值的Int变量，Temp_2和Temp_3是通过DB_ANY_TO_VARIANT初始化的Variant变量，其中为Temp_2初始化的DB1是不满足DB_ANY_TO_VARIANT条件的DB块，为Temp_3初始化的DB19是不满足DB_ANY_TO_VARIANT条件的DB块，最终将FC8的3个输出关联FC9的3个输出至OB1中显示，

如图11-13所示。

![](images/03-11.jpg){width="558" height="440"}

图11 FC8程序详情

![](images/03-12.jpg){width="594" height="523"}

图12 FC9程序详情

OB1调用FC9

![](images/03-13.jpg){width="556" height="202"}

图13 OB1调用FC9

从图13中可知，不满足DB_ANY_TO_VARIANT条件的DB1初始化的Temp_2相当于NULL，其余两个都可以视作有明确指向。

## IS_ARRAY

表4 LAD指令说明


| LAD                   | 操作数                | 说明                  |
|-----------------------|-----------------------|-----------------------|
| 操作数   <br>  <br> ┫IS_ARRAY┣  <br>  | Variant  | 如果操作数对应的实参为数组或者P#指针格式，该指令返回逻辑运算结果 (RLO)"1"，否则该指令返回 RLO"0"。<br> 操作数是FC/FB的Input/Output/InOut/Temp以及OB的Temp中定义为Variant类型的参数。 |


SCL指令：

IS_ARRAY(操作数)，操作数是FC/FB的 **Input/Output/InOut/Temp** 中定义为Variant类型的参数，当操作数对应的实参为数组或者P#指针格式，IS_ARRAY(操作数) 为True，否则IS_ARRAY(操作数) 为False。

使用方法：

```c

IF IS_ARRAY(操作数) THEN
...
END_IF;

```

使用举例参见[CountOfElements](04-Move/05-MOVE_Variant.md#countcountofelements)。
