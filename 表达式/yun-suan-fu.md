# 运算符

有3类运算符：

* 一元运算符：只有一个操作数；
* 二元运算符：有两个操作数；
* 三元运算符：有三个操作数；

## 运算符的优先级和顺序关联性

运算符的优先级控制各运算符的计算顺序。运算符的优先级由运算符的关联语法产生式的定义确定。

下表按照从高到底的**优先级**列出了所有的运算符：

| 类别 | 运算符 |
| :--- | :--- |
| 基本 | e.M、f\(x\)、a\[x\]、x++、x--、new、typeof、default、checked、unchecked、deledate |
| 一元 | +、-、!、~、++x、--x、\(T\)x、await x |
| 乘法 | \*、/、% |
| 加减 | +、- |
| 位移 | &lt;&lt;、&gt;&gt; |
| 关系和类型检测 | &lt;、&gt;、&lt;=、&gt;=、is、as |
| 相等 | ==、!= |
| 逻辑AND | & |
| 逻辑XOR（异或） | ^ |
| 逻辑OR | \| |
| 条件AND | && |
| 条件OR | \|\| |
| null合并 | ?? |
| 条件 | ?: |
| 赋值和lambda表达式 | =、\*=、/=、%=、+=、-=、&lt;&lt;=、&gt;&gt;=、&=、\|=、^=、=&gt; |

运算符的顺序关联性：

* 除赋值运算符和null合并运算符外，所有二元运算符均为左结合，从左向右执行运算。
* 赋值运算符、null合并运算符和条件运算符为右结合，从右向左执行运算。

优先级和顺序关联性都可以用括号控制。

## 运算符的重载

通过在类或结构中报考operator声明来引入用户定义的运算符。

用户定义的运算符实现的优先级始终高于预定义运算符实现的优先级。

可重载的运算符：

| 类别 | 可重载的运算符 |
| :--- | :--- |
| 一元运算符 | +、-、!、~、++、--、true、false（true和false在布尔表达式、涉及条件运算符和条件逻辑运算符的表达式中被调用，因此也被视为运算符） |
| 二元运算符 | +、-、\*、/、%、&、\|、^、&lt;&lt;、&gt;&gt;、==、!=、&gt;、&lt;、&gt;=、&lt;=（当重载一个二元运算符是，也会隐式重载相应的赋值运算符，例如运算符\*的重载也是运算符\*=的重载） |
| 强制转换运算符 | \(T\)x（通过提供用户定义的转换来重载） |
| 元素访问 | a\[x\]（可通过索引器支持用户定义的索引） |

在表达式中，使用运算符表示法来引用运算符；在声明中，使用函数表示法来引用运算符：

| 运算符表示法 | 函数表示法 |
| :--- | :--- |
| op x | operator op\(x\) |
| x op | operator op\(x\) |
| x op y | operator op\(x, y\) |

用户定义的运算符不能修改运算符的语法、优先级或顺序关联性。

一元运算符重载决策：

* 
二元运算符重载决策：

* 
数值提升



提升运算符

* 




