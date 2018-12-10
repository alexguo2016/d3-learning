资料:
http://www.ourd3js.com/wordpress/100/


### 引用D3, test1
1. script文件引用
2. 其他

#### 更改页面文字
````js
d3.select("body").selectAll("p").text("更改后的文字")
````
所以说, d3其实和jquery一样, 是一个比较方便的库

#### d3里面有一个概念, 选择集
````js
//选择<body>中所有的<p>，其文本内容为 www.ourd3js.com，选择集保存在变量 p 中
var p = d3.select("body")
          .selectAll("p")
          .text("www.ourd3js.com");
 
//修改段落的颜色和字体大小
p.style("color","red")
 .style("font-size","72px"); // 注意, 这里不像其他库, 用的是原生的css语法,例如font-size, 不需要驼峰处理
````
这里, 每一步, 例如d3.select, 都返回一个d3选择集对象, 类似于jquery的jquery对象
和jquery一样, 可以链式调用

#### d3中, 如何选择元素
1. d3.select(), 选择指定元素中的第一个, querySelector
2. d3.selectAll() --> querySelectorAll
经过测试, 可以使用.className, #id, 选择特定的元素

### 如何绑定数据, test2
d3的独特功能, 将数据绑定到DOM上, 这个是什么意思呢?
d3是通过以下两种函数来绑定数据的
1. datum() 绑定一个数据到选择集上
````js
var str = "China";

var body = d3.select("body");
var p = body.selectAll("p");

p.datum(str);
// 这个时候, 只是绑定, 没有更改值

p.text(function(d, i){
    // 第一个参数是value, 第二个是index
    // 直到这个匿名函数被执行, 选择集的数据才会改变
    return "第 "+ i + " 个元素绑定的数据是 " + d;
});
// 注意, 这里用和一个匿名函数, 返回的数据作为所有p的str值
````

2. data() 绑定一个数组到选择集上, 数组的各项值分别与选择集的各元素绑定
一般来说, 这个用得比较多
数组和选择集元素一般来说, 一一对应, 需要测试不对应的情况
````js
var dataset = ["apple", "pear", "chocolate"];

var body = d3.select("body");
var p = body.selectAll("p");

p.data(dataset).text(function(d, i) {
    return d
})
````
经过测试, 数据按照少的一方为准


### 选择, 插入, 删除元素, test3
#### 选择
可以使用class, id选择器

#### 插入元素
1. append() 在选择集末尾插入元素
````js
body.append("p").text("this is appended p element")
// 需要注意的是, append的是元素, 例如p, div, span等等

body.append("p", "#id").text("with class and id")
// 但是, 这里插入的p元素并没有#id, 只是一个普通的p元素
````
2. insert() 在选择集前面插入元素
和append类似

#### 删除元素
选择之后, 直接remove即可, 例如
````js
var p = d3.select("p").remove()
````

### 做一个简单的图表, test4
这里需要想想了, d3.js可能主要使用的是svg, 但是, 现在canvas似乎发展得更好?

#### 添加画布
````js
var svg = d3.select("body").append("svg").attr("width", 400).attr("height", 200).style("background-color", "red")
````

#### 绘制矩形
在svg中, 矩形的标签是rect, 有4个属性, x, y, width, height, 和canvas的方法有点类似, 一个个画出来
需要注意这几行代码
````js
svg.selectAll("rect")   //选择svg内所有的矩形
    .data(dataset)  //绑定数组
    .enter()        //指定选择集的enter部分
    .append("rect") //添加足够数量的矩形元素
````

### 比例尺的使用, test5
由于是svg, 放大或者缩小, 并不会影响图像的准确性
一般来说, 有两种比较常用的比例尺
1. 线性比例尺
    将dataset中最小的映射成min, 最大的为max
    ````js
    
    ````
2. 序数比例尺
