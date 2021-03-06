# 成员变量与局部变量
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Variable_Scope/bl.png)<br>

## 要点
* 就近原则
* 变量的分类
  * 成员变量：类变量、实例变量
  * 局部变量
* 非静态代码块的执行：每次创建实例对象都会执行
* 方法的调用规则：调用一次执行一次

## 题目分析
首先要分清哪些是成员变量，哪些是局部变量<br>
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Variable_Scope/bl2.png)<br>

* 当main方法开始执行后,首先加载初始化Exam5类,此时s被初始化为0放入内存方法区
* Exam5 obj1 = new Exam5();
  * 实例化Exam5(obj1)，先初始化成员变量，i = 0, j = 0
  * 执行非静态代码块
    * 初始化局部变量i=1放入栈中,然后局部变量i+1
    * obj1成员变量j+1=1
    * 类成员变量s+1=1
    * 离开非静态代码块，此时局部变量i失效
* Exam5 obj2 = new Exam5();
  * 实例化Exam5(obj2), 先初始化成员变量，i = 0, j = 0
  * 执行非静态代码块
    * 初始化局部变量i=1放入栈中,然后局部变量i+1
    * obj2成员变量j+1=1
    * 类成员变量s+1=2
    * 离开非静态代码块，此时局部变量i失效
* obj1.test(10);
  * 形参j为局部变量，下面的j++根据就近原则作用在局部变量j上
  * i++显然作用在成员变量i上，obj1.i+1=1
  * 类成员变量s+1=3
  * 离开test(int j)方法后,局部变量j失效
* obj1.test(20);
  * 形参j为局部变量，下面的j++根据就近原则作用在局部变量j上
  * i++显然作用在成员变量i上，obj1.i+1=2
  * 类成员变量s+1=4
  * 离开test(int j)方法后,局部变量j失效
* obj2.test(30);
  * 形参j为局部变量，下面的j++根据就近原则作用在局部变量j上
  * obj2.i+1=1
  * 类成员变量s+1=5
  * 离开test(int j)方法后,局部变量j失效

所以最后显示的结果为
```
2 1 5
1 1 5
```
实际上，通读代码，熟悉的话到底哪部分代码能够对成员变量和类变量产生影响是能够很容易看出的。
<br>

## 补充：如何避开就近原则的影响
* 局部变量与实例变量重名，在成员变量前面加 "this."
* 局部变量与类变量重名，在类变量前面加 "类名."
