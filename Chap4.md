第4章 初始化和清除 
========
#### 1.构造函数与普通函数的最大区别是什么? P106
构造函数没有返回值。这一概念与返回void是不同的，因返回void是用户自己选择的，而对于构造函数，程序员别无选择。
#### 2.原始类型变量重载的“升级”和“降级”分别指什么? P109-113
调用一个接收原始类型变量的函数时，假如函数定义的参数类型范围窄于用户调用的参数（如存在函数`f1(double d)`，用户调用`int x = 1; f1(x);`)，此时用户调用的参数会被“升级”至比较窄的范围（int -> double）。例外是char会被升级为int。反过来，若函数为`f2(int x)`，用户定义`double d = 0.0;`，则必须对`d`进行显式“降级”才能调用函数，即`f2((int)d);`，否则不予通过编译。原因是降级时容易造成信息丢失，所以编译器要确定降级是程序员的主观意愿。
#### 3.编译器在何时会自动创建无参数的构造函数? P113-114
当用户未定义任何构造函数时。如果用户定义了构造函数，则编译器不会自动创建默认构造函数。即使此时用户定义的是几个有参数的构造函数，编译器也不会自动增加无参数的构造函数，因为编译器认为构造函数已经存在。
#### 4.使用this关键字调用构造函数的规则是什么?如何理解? P116-117
用this关键字调用构造函数必须在构造函数的第一句。即使用this关键字调用构造函数的语句不能存在于非构造函数中，也不能在构造函数中用this调用两个构造函数。
#### 5.finalize()方法的使用规则是什么?通常在哪里进行使用? English P120
理论上，finalize()会在垃圾处理之前被调用。但要注意，垃圾处理并不是一定会做，有时程序运行一直未达到内存的边界，那么垃圾处理在程序运行过程中可能不会被执行。（这是好事，因为程序运行完毕后，所有消耗的内存会被还给操作系统，而垃圾处理所需的开销相当于被节省了）在这种情况下，如果依赖于finalize()函数去做一些扫尾工作，会出现问题，因为不能保证finalize()会被调用。

由于垃圾处理只与内存的使用有关，而finalize()的调用是与垃圾处理相联结的，因此**finalize()必须只与内存的释放有关**。考虑到内存中的那些对象会被gc处理，而Java中的一切都是对象，推断finalize()需要处理的可能是一些*类C*的内存分配场景。如java底层的原生方法中，如果调用了`malloc()`，就要在相应的finalize()中调用`free()`。
#### 6.为什么在Java中为堆对象分配存储空间可达到其他语言在堆栈中创建存储空间差不多的速度? P123
Java的垃圾处理机制使得它的内存对程序来说有如一条传送带，创建堆中的存储空间只涉及堆指针的移动，因此速度接近于其他语言在栈上分配存储空间的速度。
#### 7.简述“引用计数”(reference counting)垃圾处理器的原理和缺点。 P123
每个对象会维护一个“引用计数器”，记录指向它的指针的个数，这个计数在程序运行期间会随着指针的增加或减少而变动。垃圾收集器会遍历整个对象列表，找到引用计数为0的对象把它删掉。 缺点：对于对象之间循环引用的情况需要另外处理，耗费时间。
#### 8. 简述JVM的自适应垃圾处理机制? Eng P123-124

====to: Eng P124 middle（4.3-垃圾回收需重看。大神讲：垃圾回收先跳过，先熟悉语法和API，先广后深）

#### 9. 如果将一个类成员在构造函数中进行初始化，实际运行时，构造函数被调用之前，该成员还会被默认初始化吗? P128
会。在调用构造函数前，Java会将每个类成员做默认初始化（原始类型初始化到0，对象初始化到null）。初始化的顺序由成员在类中的定义决定。

#### 10.简述Java创建一个对象时发生的事情。 Eng P130
  1. 当用户创建某类的对象或使用该类的静态变量/方法时，Java解释器会在classpath中寻找.class文件并加载。
  2. 进行所有的静态初始化。即某一个类的静态初始化只进行一次。
  3. 在内存堆中按需分配该对象的内存。
  4. 初始化该类的所有非静态变量至0或null。
  5. 按照类中变量的定义对变量进行初始化。
  6. 调用构造函数。  
  
#### 11. 数组定义的格式是什么? P134
`int[] a1;`

或： `int a1[];`
#### 12. 若想定义一个int数组，该数组的长度由某函数/某变量在运行时间决定，应该怎样定义? P136

#### 13. 如何初始化一个非基类对象的数组?以Integer类为例。 P136-137

#### 14. 如何初始化一个“变参表”，如一个Object数组，但其中的元素为Object的不同子类对象? P138

#### 疑惑：
English P123 (GC: 前面讲Java中堆对象分配与其他语言中的栈分配差不多快——因为Java内存有如一个传送带。然后：)
You might observe that the heap isn’t in fact a conveyor belt, and if you treat it that way, you’ll start paging memory—moving it on and off disk, so that you can appear to have more memory than you actually do. Paging significantly impacts performance. Eventually, after you create enough objects, you’ll run out of memory. The trick is that the garbage collector steps in, and while it collects the garbage it compacts all the objects in the heap so that you’ve effectively moved the “heap pointer” closer to the beginning of the conveyor belt and farther away from a page fault.

...any non-dead object must ultimately be traceable back to a reference that lives either on the stack or in static storage.
```
(from: http://stackoverflow.com/questions/19623563/where-does-java-reference-variable-stored)
//On the stack, primitives(int, double, boolean, etc) and object *references* pointing to the heap are stored.
[ STACK ]                          [ HEAP ] 
int a: 10;                     ->  MyWrapperObject@21f03b70====||
double b: 10.4;                |   ||     int someField: 11    ||
MyWrapperObject@21f03b70 ------|   ||     String@10112222  ---------- 
......                             ||==========================||    |
                                                                     |
                                                                     |
                                    String@10112222============||<----
                                    || ...                     ||
                                    || ...                     ||
                                    }}=========================||                                  
```
//大神讲：Java所有类都会被线程加载，线程在栈上，所以对对象的引用在栈上；静态内存中也可能有引用，场景是静态方法中在堆上创建对象，则对该对象的引用存在静态内存中。
P124
当然，对象从一个地方挪到另一个地方的时候，对该对象的所有引用都必须改变。其中从堆或静态存储区域到对象的引用马上就可以改变，但可能还有指向该对象的另一些引用，它们要在以后“遍历”的时候才会遇到。那些引用要等到发现的时候才会修改（想象一张表吧，它将老地址对应成新地址）。
