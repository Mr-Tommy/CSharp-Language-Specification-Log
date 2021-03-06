```
类成员
```

一个类的成员由两部分组成：类内部定义的成员，和从它的直接基类继承的成员。

类成员包含以下几种类别：

1. 常量：与该类相关联的常量值；
2. 字段：该类的变量；
3. 方法：由该类执行的计算和操作；
4. 属性：定义一些命名特征以及与读取和写入这些特征相关的操作；
5. 事件：由该类生成的通知；
6. 索引器：使该类的实例可以按照与数组相同的方式（语法）进行索引；
7. 运算符：定义表达式运算符，对该类的实例进行运算；
8. 实例构造函数：初始化该类的实例所需的操作；
9. 析构函数：在永久的放弃该类的一个实例之前所需的操作。
10. 静态构造函数：初始化该类的自身所需的操作。
11. 嵌套类型：该类的局部类型。

适用于类成员声明的一些规则：

* 实例构造函数、静态构造函数和析构函数必须具有与包容它们的类相同的名称，其他的成员的名称不能与类同名；
* 常量、字段、属性、事件或嵌套类的名称必须不能与其他类成员同名（这些成员没有重载）；

## 实例类型

？

## 构造类型的成员

[**构造类型**](/类型/gou-zao-lei-xing.md)的非继承成员是通过将成员声明中的每个类型形参（type-parameter）替换为构造类型的对应类型实参（type-argument）来获得的。替换过程基于类型声明的语义含义，并不只是文本替换。

```
泛型声明如下：
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}

构造类型Gen<int [], IComparable<string>>成员如下：
    public int[,][] a;
    public void G(int i, int[] t, Gen<IComparable<string>, int[]> gt) {...}
    public IComparable<string> Prop { get {...} set {...} }
    public int H(double d) {...}
```

## 继承

继承的一些重要性质：

* 继承是可传递的。
* 派生类扩展它的直接基类。
* 实例构造函数、析构函数和静态构造函数不可继承，其他所有成员都可继承，无论可访问性如何，但是根据基类成员的可访问性，有些成员在派生类中时无法访问到的。
* 派生类可声明具有相同名称或签名的新成员来隐藏基类成员，但是并不移除该成员，只是使其不可访问。
* 类的实例含有该类中以及它的所有基类中声明的所有实例字段集，并且存在从派生类到任意基类的隐式转换。
* 类可以声明虚方法、虚属性、虚索引器，派生类可重新这些函数成员的实现，使类展示出“多态”的特征（意思是说，调用同一个函数所执行的操作可能是不同的，这取决于用来调用该函数成员的实例的运行时类型）。

## new修饰符

派生类中声明一个与基类中相同名称或签名的成员时会隐藏基类成员，隐藏继承的成员不算是错误，但编译器会发出警高，若要取消此警告，派生类成员声明时可使用一个new修饰符，表示派生成员是有意隐藏基类成员的。

如果在不隐藏基类成员的派生类成员上使用new修饰符会导致编译器发出警告。

## 静态成员和实例成员

静态成员属于类类型，实例成员属于对象。

静态成员的特征：

* 静态字段只有一个副本，标识一个所有实例共享的存储位置。
* 静态函数不能作用于实例，在静态函数中引用this会导致编译时错误。

实例成员的特征：

* 类的每个实例包含一组该类的实例成员。
* 实例成员作用于类的实例，该实例可用this访问。

## 嵌套类型

在类或结构内部声明的类型称为嵌套类型。

可访问性：

* 在类中声明的嵌套类型：可以具有5种可访问性，默认为private。
* 在结构中声明的嵌套类型：可以具有3种（public、internal、private）可访问性，默认为private。

隐藏基类成员：

使用new操作符声明的嵌套类型可以显示隐藏基类成员。

```
using System;
class Base
{
    public static void M() 
    {
        Console.WriteLine("Base.M");
    }
}
class Derived: Base 
{
    new public class M             //显示隐藏基类方法M
    {
        public static void F() 
        {    
            Console.WriteLine("Derived.M.F");
        }
    }
}
```

嵌套类中访问包含它的类：

在嵌套类内部，不能通过this引用包含它的类型的实例成员，可通过在包含类中将包含类的实例this作为参数传递给嵌套类的构造函数来添加对包含类的引用。

```
using System;
class C
{
    int i = 123;
    public void F() {
        Nested n = new Nested(this);    //创建嵌套类，调用构造函数并传递this参数（this是类型C的实例）
        n.G();
    }
    public class Nested                //嵌套类声明
    {
        C this_c;                      //保留对包含类的引用
        public Nested(C c)             
        {
            this_c = c;                //保留对包含类的引用
        }
        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();                        //输出 123
    }
}
```

对包含类型的私有成员的访问：

嵌套类型可以访问包含它的类型可以访问的所有成员。

```
using System;
class C 
{
    private static void F() 
    {
        Console.WriteLine("C.F");
    }
    public class Nested 
    {
        public static void G() 
        {
            F();            //访问包含类型的私有静态方法F
        }
    }
}
class Test 
{
    static void Main() 
    {
        C.Nested.G();
    }
}
```

对包含类型的基类的受保护成员的访问：

```
using System;
class Base 
{
    protected void F() {                    //基类定义的受保护的成员
        Console.WriteLine("Base.F");
    }
}
class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();                        // ok，可以访问
        }
    }
}
class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```

## 泛型类中的嵌套类型

泛型类中可以声明嵌套类型。包含类的类型形参可以在嵌套类型中使用；另外，包含类中声明的嵌套类型可以定义仅用于嵌套类型的附加类型形参。

在编写对泛型类中定义的嵌套类型的引用时，必须指定其构造类型（指定类型实参）。可在包含类中不加限制的使用嵌套类型。

在构造嵌套类型时可以隐式的使用包含类的实例。

```
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }
    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");        //前两条语句效果相同
        Inner<string>.F(t, "abc");
        Outer<int>.Inner<string>.F(3, "abc");      //这个语句指定了外层包含类的形参类型
        Outer.Inner<string>.F(t, "abc");           //错误，没有指定外层包含类的形参类型
    }
}
```

## 保留成员的名称

为便于C\#基础运行时的实现，对于每个属性、事件、索引器的源成员声明，实现都会根据源成员的种类（属性、事件...）、名称和类型保留两个方法签名。如果存在与这些签名相同的成员，则引发编译时错误。

保留名称不会引入声明，因此不会参与成员查找。但它们会参与继承，可通过new修饰符隐藏起来。

保留这些名称有3个目的：

为属性保留的成员：

```
对于类型为T的属性P，保留的如下签名：
T get_P();
void set_P(T value);
```

为事件保留的成员：

```
对于委托类型为T的事件E，保留了如下签名：
void add_E(T handler);
void remove_E(T handler);
```

为索引器保留的成员：

```
对于类型为T且具有形参列表L的索引器，保留了如下签名：
T get_Item(L);
void set_Item(L, T value);
此外，保留成员名称Item。
```

为析构函数保留的成员：

```
对于包含析构函数的类，保留了如下签名：
void Finalize();
```



