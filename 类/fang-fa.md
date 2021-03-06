# 方法

方法用于实现可由类或对象执行的计算或操作。

## 方法形参

没有默认值的形参称为必选形参；具有默认值的形参称为可选形参；必选形参必须在可选形参之前。

params修饰符用于声明一个形参数组，只能是一维数组类型。

有四种类型的形参：

1. 值形参
2. 引用形参
3. 输出形参
4. 形参数组

值形参：

声明时**不带修饰符**的形参是值形参，一个值形参对应于一个局部变量，初始值来自该方法调用所提供的实参。

值形参值的改变不会影响方法调用方调用时给出的实参值。

引用形参：

使用**ref**修饰符声明的形参是引用形参。引用形参不会创建新的存储位置，引用形参实际的存储位置是方法调用时调用方传递的实参变量的存储位置。

方法调用时对应实参必须使用修饰符ref；一个变量在可以作为引用形参传递之前，必须明确赋值。

声明为迭代器的方法不能有引用形参；

输出形参：

使用**out**修饰符声明的形参是输出形参。输出形参不创建新的存储位置，输出形参表示的存储位置是方法调用中作为实参传递的那个变量所表示的存储位置。

方法调用时对应实参必须使用修饰符out；一个变量在可以作为输出形参传递之前不一定需要明确赋值，但在将变量作为输出形参传递的调用之后，该变量必须明确赋值。

声明为分部方法或迭代器的方法不能有输出参数。

形参数组：

使用**params**修饰符声明的形参是形参数组。形参数组必须位于形参表的最后并且必须是一维数组类型。

## 静态方法和实例方法

方法的声明中包含**static**修饰符时，该方法为静态方法；不包含static修饰符的方法为实例方法。

## 虚方法

一个实例方法的声明中包含**virtual**修饰符时，该方法为虚方法；不包含virtual修饰符的方法为非虚方法。

实现：

在声明的实例上和在派生类中都可以实现。

重写：

在派生类中实现虚方法，这个过程称为重写。

调用：

类的实例的运行时类型决定了要被调用的实现是哪一个。

虚方法在方法隐藏中的使用：

因为方法可以隐藏继承来的方法，因此一个类中可以包含若干个具有相同签名的虚方法。因为除了派生程度最大的那个方法，其他的方法都被隐藏起来了。

```
using System;
class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}
class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}
class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}
class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}
class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();    //B.F
        b.F();    //B.F
        c.F();    //D.F，在C类中隐藏了从A类继承的方法F，引入了新的虚方法，运行时类型为D类型的实例，因此输出D.F
        d.F();    //D.F
    }
}
```

## 重写方法

一个实例方法的声明中包含**override**修饰符，该方法为重写方法，重写方法使用相同的签名重写继承来的虚方法。只有在使用了override修饰符时，一个方法才能重写另一个方法，没有使用override修饰符时，具有相同签名的派生类方法会隐藏基类方法，并产生一条编译时警报。

重写方法遵循以下规则：

* 已重写了的基方法是一个虚的、抽象或者重写方法，不能是静态、密封或非虚方法；
* 重写方法和已重写了的基方法具有相同的返回类型；
* 重写方法和已重写了的基方法具有相同的可访问性；

基访问：

重写方法可以通过基访问（base-access）的方式访问已重写了的基方法。

```
class A
{
    int x;
    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}
class B: A
{
    int y;
    public override void PrintFields() {
        base.PrintFields();                    //通过基访问方式，访问重写了的基方法
        Console.WriteLine("y = {0}", y);
    }
}
```

## 密封方法

一个实例方法声明中包含**sealed**修饰符时，该方法为密封方法，且声明时也必须包含override修饰符。密封方法的作用是防止派生类进一步重写虚方法。

```
using System;
class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
    public virtual void G() {
        Console.WriteLine("A.G");
    }
}
class B: A
{
    sealed override public void F()     //防止C类进一步重写该方法
    {
        Console.WriteLine("B.F");
    } 
    override public void G() {
        Console.WriteLine("B.G");
    } 
}
class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```

## 抽象方法

一个实例方法声明时包含**abstract**修饰符时，该方法是抽象方法。抽象方法同时隐含为虚方法，但不能使用virtual修饰符，也不提供实现。

**只允许在抽象类中声明抽象方法**。

抽象方法适用于只提供抽象概念，所有默认实现都没有意义的场合，因为默认实现没有意义，派生类必须根据自己的意义来实现。

一个抽象方法的什么可以重写虚方法，这使在抽象类的派生类中该方法的原始实现不可再陪被访问。

```
using System;
class A
{
    public virtual void F()     //虚方法的原始实现
    {
        Console.WriteLine("A.F");
    }
}
abstract class B: A
{
    public abstract override void F();    //抽象方法重写了虚方法
}
class C: B
{
    public override void F()             //派生类必须有自己的实现，且不可访问原始实现（base.F()）
    {
        Console.WriteLine("C.F");
    }
}
```

## 外部方法

方法的声明中包含**extern**修饰符时，该方法是外部方法。外部方法是在外部实现的，编程语言通常是C\#以外的语言。

外部方法通常与DllImport属性一起使用，从而使外部方法可以由dll实现。

当包含DllImport属性时，方法的声明必须包含**static修饰符**。

```
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;
class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);
    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);
    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);
    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

## 分部方法

方法的声明中包含**partial**修饰符时，该方法是分部方法。

只能在分部类型中声明分部方法，而且要受多种约束。

## 扩展方法

方法的声明中，第一个形参包含**this**修饰符时，该方法为扩展方法。且第一个形参不能使用除了this之外的修饰符，且形参类型不能是指针类型。

只能在非泛型、非嵌套的静态类中声明扩展方法。

## 方法体

由一个block块或一个分号构成；

返回类型：

如果返回类型为void，或者方法是异步方法并且返回类型为System.Threading.Tasks.Task，则方法的返回类型为void；

## 方法重载



