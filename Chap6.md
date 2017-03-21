第6章 复用类
==============================
#### 1.简述派生类中对基类的一般初始化处理。 Eng 170-171
如果基类存在默认构造函数，Java会自动在派生类的构造函数中调用基类的构造函数。若基类没有默认构造函数，Java则会抱怨未找到基类的默认构造函数，此时程序员需要用super关键字显式指定调用基类的哪一个构造函数，且这句话必须是派生类构造函数中的第一句话。

#### 2.简述派生类中对成员对象的一般cleanup顺序。(疑惑) P174
如果需要在程序执行完毕后对一些对象进行手动清除，不应该依赖垃圾处理（即用finanlize()函数），因为垃圾处理的行为不是确定的（不知何时执行；不知是否执行）。此时应该为自己的类添加自己的清除方法。

在一个派生类的清除方法中，清除顺序一般如下：先清除派生类中的对象，按创建它们的顺序的相反顺序（通常这要求基类元素依然可用（?））；然后调用基类中的清除方法。
#### 3.简述final关键字应用于数据域的作用。它用于原始类型数据和用于对象有何不同效果？如何判断它是编译时间常数（编译时初始化，计算可在编译时间执行）还是运行时间常数（运行时进行初始化，初始化后便不会改变）？ 如何保证一个域为编译时间常数？P180
final关键字告诉compiler当前数据域保持不变。用于原始类型数据时，final会将它变成一个常数；用于对象时，仅仅会将当前的引用保持不变，即永远不能使它指向另一个对象。但是对象的值是可以改变的。这一限制也适用于数组，因为它们也是对象。
如果定义时赋给该对象的值为定值，则为编译时间常数；若赋的是运行时间值（如`final int i = (int)(Math.random()*20)`）则该常数的值是运行时间决定的。
要保证一个域为编译时间常数，该域必须为原始数据类型，必须用final关键字修饰，且必须在定义时进行赋值。
#### 4.Java语法是如何保证final域在使用之前一定被初始化？ Eng P185

#### 5.将方法的参数设为final会起到什么效果？ Eng P185
#### 6.将一个方法设为final会起到什么效果？ 将一个private方法设为final会起到什么效果？Eng P186
#### 7.在父类中定义一个private方法，在子类中试图将该方法覆盖为public方法，然后使用upcasting去调用该子类的该方法，会如何？为什么？ Eng P187
#### 8.将一个类设为final会起到什么效果; final类中的域和方法是否必须为final？ Eng P187-188
#### 9.Java中的类一般是何时被加载的？static对象和static代码块又是何时被加载的？ Eng P189
#### 10.简述创建一个子类对象时发生的事情。 Eng P189-190
#### 疑惑：
Eng P182
One of the clearest ways to determine whether you should use composition or inheritance is to ask whether you’ll ever need to upcast from your new class to the base class. If you must upcast, then inheritance is necessary, but if you don’t need to upcast, then you should look closely at whether you need inheritance. The Polymorphism chapter provides one of the most compelling reasons for upcasting, but if you remember to ask “Do I need to upcast?” you’ll have a good tool for deciding between composition and inheritance.
