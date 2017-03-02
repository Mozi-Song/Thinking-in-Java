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

#### 7.简述“引用计数”(reference counting)垃圾处理器的原理和缺点。 P123

#### 8. 


#### 疑惑：
English P123 (GC: 前面讲Java中堆对象分配与其他语言中的栈分配差不多快——因为Java内存有如一个传送带。然后：)
You might observe that the heap isn’t in fact a conveyor belt, and if you treat it that way, you’ll start paging memory—moving it on and off disk, so that you can appear to have more memory than you actually do. Paging significantly impacts performance. Eventually, after you create enough objects, you’ll run out of memory. The trick is that the garbage collector steps in, and while it collects the garbage it compacts all the objects in the heap so that you’ve effectively moved the “heap pointer” closer to the beginning of the conveyor belt and farther away from a page fault.
