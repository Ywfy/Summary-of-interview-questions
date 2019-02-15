# JAVA传值机制
![图片无法加载](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Passing_Param/%E4%BC%A0%E5%80%BC.png)<br>
## 要点
* 形参是基本数据类型
  * 传递数据值
* 实参是引用数据类型
  * 传递地址值
  * 特殊的类型:String、包装类等对象不可变性

## 题目分析
下图为实参值存储图<br>
![图片无法加载](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Passing_Param/tz1.png)<br>
int型数据直接存放值，其他都是存放引用(地址),指向其他地方。<br>
注意：对于Integer，若值在-128到127，则会存到静态缓存空间内<br>
例如对于以下代码
```
Integer i1 = 100;
Integer i2 = 100;

Integer i3 = 200;
Integer i4 = 200;
System.err.println(i1 == i2);
System.err.println(i3 == i4);
```
运行结果为
```
true
false
```
### 形参存储
```
int j ===> 基本数据类型，直接传递储存值
String str -> string s 引用数据类型，传递地址值
Integer num -> Integer n  引用数据类型，传递地址值
int[] arr -> int[] a 引用数据类型，传递地址值
MyData my -> MyData m  引用数据类型，传递地址值
```

### 修改操作
* j += 1; 这操作的是形参存储空间j的值，i没有影响
* s += "world";
  * 这涉及到String、包装类等对象不可变性
  * 该语句会在常量池内拼接"hello"+"world"生成"helloworld"字符串，并让s重新指向"helloworld"字符串
  * <strong>但是str并没有受到影响，str仍然指向"hello"字符串</strong>
* n += 1;
  * 与上同，只是让n指向了新的地址，而num并没有受到影响
* a[0] += 1;
  * 到堆中，修改[0]下标的空间内容,使其+1，结果为2
  * 此时arr仍然指向这个内存空间，所以a[0]同样为2
* m.a += 1;
  * 到堆中，修改a的值，使其+1，结果为11
  * 此时my仍然指向这个地址，所以my.a结果同样为11

最终结果
```
i = 1
str = hello
num = 200
arr = [2,2,3,4,5]
my.a = 11
```
