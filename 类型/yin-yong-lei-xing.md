# 引用类型

引用类型包含类类型、接口类型、数组类型和委托类型。

引用类型的值是对该类型的某个实例的一个引用，类型的某个实例称为对象；null值兼容于所有引用类型，用来表示“没有被引用的实例”。

## 类类型

类类型支持继承，**继承**是派生类用来扩展和专门化基类的一种机制。

具有特殊含义的预定义类类型：

| 类类型 | 说明 |
| :--- | :--- |
| System.Object | C\#中所有类型的最终基类 |
| System.String | C\#语言的字符串类型 |
| System.ValueType | 所有值类型的基类 |
| System.Enum | 所有枚举类型的基类 |
| System.Array | 所有数组类型的基类 |
| System.Delegate | 所有委托类型的基类 |
| System.Exception | 所有异常类型的基类 |

## dynamic（动态）类型

dynamic类型与object类型一样，可以引用任何对象。在将运算符应用于dynamic类型的表达式时，可以将解析推迟到程序运行时进行。

## string类型

直接从object继承的**密封类**类型，string的实例表示Unicode字符串。

## 接口类型

接口定义协定，实现某接口的类必须遵守该接口定义的协定。

## 数组类型

数组是一种**数据结构**，数组的元素具有相同的类型，数组元素可通过索引访问。

## 委托类型

委托是引用一个或多个方法的**数据结构**。C\#中的委托既可以引用静态方法，还可以引用实例方法。在引用实例方法时，还存储了对实例对象的引用。



