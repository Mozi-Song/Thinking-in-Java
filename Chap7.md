第七章 多态
==============
#### 1.binding的定义；late binding和early binding的定义 Eng P196
binding: 将方法的调用与方法体进行连接。

late binding: binding在runtime基于对象的类型发生

early binding: binding在compile time发生
#### 2.late binding大概是如何实现的？ Eng P196
某机制需要知道运行时间对象的类型，也需要会调用那个类型的方法。可认为对象中包含与运行时间类型有关的信息。
#### 3.Java何时采用late binding？ Eng P196
Java中的方法，除非有static或final关键字，否则一律自动被late bound。（注意private方法自动为final）
#### 4.如何看待将一个方法设为final对performance的影响？ Eng P196
由于final关键字使一个方法不能late bound，省去了late binding所需要的开销，会将performance提高一点点，但这一点不太会对整体performance造成太大影响，因此不应被作为使用final关键字的依据。是否使用final关键字还是要考虑设计上的需要。
#### 5.举例解释upcasting的意义  Eng P197
 `Parent p = new Child()`该赋值将等式右边的Child指针赋给了Parent指针，由于Child继承了Parent，这个语句编译器是允许的。这个过程就是upcasting。
#### 6.为何多态思想提高了代码的可扩展性？  Eng P199
多态思想使得在已有代码中可以添加任何子类并重写子类中的方法，而并不会影响（已有的）基类指针对应的方法调用。
#### 7.详述合成派生类构造函数的调用顺序。  Eng P205 
1)父类构造函数（按2）-3）的顺序一直向上直到根父类）

2）对象域的初始化

3）子类构造函数
#### 8.对于合成类中的对象域，为什么要尽可能地在定义的时候将他们初始化？ Eng P206
因为构造函数的执行很可能基于“这些对象域已经有效存在”的假设。
#### 9.对于合成类的cleanup顺序,需要遵循哪两个原则? Eng P2081#### 
1）子类先于父类进行cleanup（因为子类的cleanup方法有可能依赖于父类域/方法的存在）；
2重写polymorphism/ReferenceCounting.java Eng P208-209
