# 分析JavaScript性能

## JavaScript分析概论

使用谷歌浏览器,打开[V8基准套件](//flamechart02.png)页面。再打开Chrome DevTools,移导航到概要文件面板,并验证“收集JavaScript CPU配置文件”是否被选中。现在,单击**开始**按钮或按 <button>Cmd</button>  +  <button>E</button> 开始记录一个JavaScript CPU配置文件。然后刷新V8基准套件页面。

当页面完成重载时,就会显示基准测试的分数。回到DevTools，点击止按钮或再次按下 <button>Cmd</button>  +  <button>E</button> 停止记录。
<br>
<br>

![](/images/profiling-js-heavy-bottom-up.png)
<br>
<br> 

这种 **Bottom Up** 视图通过对性能的影响程度来排列函数。您还可以检查这些函数的调用路径。

现在，通过单击 Bottom Up / Top Down 按钮选择 **Bottom Up** 视图。然后单击 **函数** 列中左边的小箭头 **(程序)** 。 **Top Down** 视图显示了调用的整体结构,从堆栈的顶部开始调用。

>**注**:您可以点击 **Percentage** 按钮查看绝对时间百分比。

选择**函数**列中的一个函数,然后单击**焦点选定函数**按钮(右边的眼睛图标)。
<br>
<br>

![](/images/profiling-js-focus-selected-function.png)
<br>
<br>

这个可以过滤配置文件，只显示所选函数和其调用者。点击窗口右下角的**刷新**按钮恢复原状。

选择一个函数的**函数**列,然后单击**排除选择函数**按钮(X图标)。根据您所选择的功能,您应当会看到类似这样的:
<br>
<br>

![](/images/profiling-js-tree-top-down.png)
<br>
<br>

**排除选择函数**按钮可以在整个排除函数时间里，从配置文件中删除选中的函数并且管理调用者。单击**刷新**按钮可恢复原状。

您可以记录多个配置文件。点击开始分析按钮,重新加载V8基准页面,然后单击**停止分析**按钮。
<br>
<br>

![](/images/profiling-js-sidebar-profile-history.png)
<br>
<br>

左边的栏列出你的配置文件记录,右边的树视图显示了所选的配置文件的概要信息。

## 使用火焰图表

火焰图表视图提供了一个JavaScript随着时间的推移进行处理的可视化表示,类似时间轴和网络面板里的。在 Details 视图中使用**火焰图表**功能，执行JavaScript和CPU配置文件之后,您可以查看配置文件数据的几个不同方式。

### 可视化执行路径

通过可视化地分析和理解函数调用过程，你可以更好地了解你的应用程序的执行路径。
<br>
<br>

![](/images/profiling-js-flamechart00.png)
<br>
<br>

### 用颜色编码识别异常值

当缩小可以识别的重复的模式,就能进行优化,或者更重要的是,你能够发现异常值或意想不到的使执行更加容易。
<br>
<br>

![](/images/profiling-js-flamechart-outliers.png)
<br>
<br>

### 可视化JavaScript对时间尺度的数据分析器(如时间轴)

其他JavaScript分析报告的数据是随时间推移而产生的,而火焰图表按时间来报告数据。这意味着当你看到事件的发生,你可以通过时间尺度,真正做到对JavaScript执行的透视。例如,看到大的黄色条纹时间表,这是看问题完美方式。
<br>
<br>

![](/images/profiling-js-flamechart02.png)
<br>
<br>

>**注**:水平轴表示时间，垂直轴表示调用堆栈。 Expensive 函数是宽的。Y轴表示调用堆栈,所以高火焰不一定是重要的。密切关注宽条纹,不管他们在调用堆栈的什么位置。

### 如何使用火焰图:

1.开DevTools找到配置文件面板。

2.选择**记录JavaScript CPU配置文件**,然后单击**开始**。

3.当你完成收集数据,点击**停止**。

在概要视图中,选择火焰图可视化，该选择菜单在底部的DevTools中。
<br>
<br>

![](/images/profiling-js-flamechart03.jpg)

<br>
<br>
>**注**:为了增加精度分析时间，可以在配置文件中的DevTools flame-chart里启用**高分辨率CPU分析**。启用之后,您可以放大图，甚至是十分之一毫秒时间间隔也可以。

面板的顶部是一个概观,给出了完整的记录。你可以通过用鼠标单击在概观中放大特定区域，如下所示。你也可以全景左边和右边，通过点击白色区域并且拖动鼠标。在 Details 视图中，时间尺度相应减少。
<br>
<br>

![](/images/profiling-js-flamechart04.png)

<br>
<br>
在 Details 视图中,函数的**调用堆栈**被表示为一个堆栈“块”。在某个块顶部的块通过下层函数块来命名。当鼠标悬停在一个给定的块上时，会显示其函数名和时间数据:
<br>
<br>

![](/images/profiling-js-flamechart05.jpg)
<br>
<br>

- **名称**——函数的名称。
- **自我时间**——花了多长时间完成当前函数的调用,只包括函数自身的声明,不包括它调用的任何函数。
- **总时间**——完成当前函数的调用和调用其他函数的时间和。
- **自我聚合时间**——聚合时间，所有记录中函数的调用所用时间,不包括通过该函数调用的函数所用时间。
- **聚合的总时间**—聚合总时间，对所有函数的调用所用时间,包括通过该函数调用的函数所用时间。

火焰图表中的颜色比较随机,但是通过调用的函数会被标记为相同的颜色。这就允许您看到执行的模式,然后更容易看出异常值。这里与时间轴里使用的颜色没有相关性。
<br>
<br>

![](/images/profiling-js-flamechart06.png)

<br>
<br>
点击函数块中函数定义那一行，可以打开它资源面板中包含的JavaScript文件。

*该内容在[CC-By 3.0版本](http://creativecommons.org/licenses/by/3.0/)下可用*




