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
Java会强制程序员在定义该域时或在构造函数中对域进行初始化。
#### 5.将方法的参数设为final会起到什么效果？ Eng P185
设为final的参数在方法中不能再进行赋值。如`f(final int i){i++;}`非法；及`f(Car c){c = new Car();} `也非法， 不能通过编译。
#### 6.将一个方法设为final会起到什么效果？ 将一个private方法设为final会起到什么效果？Eng P186
final方法不能被子类重写。在早版本的Java中，final方法会被编译器设为inline方法，即不需要在栈上加载和清除方法参数，而是将方法体直接复制到调用方法的地方。这样会减少方法调用的开销，所以许多人用final做效率提升。然而，如果该方法本身很大，则它带来的臃肿抵消了效率的提升。高版本Java中作了一些改进，不提倡用户用final修饰方法来提高效率，在使用final关键字时只需考虑是否愿意该方法被重写即可。

private方法自动是final的，因为无法access，也就无法继承。private方法可以加上final关键字，但加不加效果相同。
#### 7.在父类中定义一个private方法，在子类中试图将该方法覆盖为public方法，然后使用upcasting去调用该子类的该方法，会如何？为什么？ Eng P187
会无法通过编译。既然父类中该方法为private，说明父类向子类提供的接口中并不存在这个方法。子类中重新声明一个名字相同的public方法，其实并不是重写，而是生成了一个新方法。由于父类给出的接口中无此方法，使用upcasting时调用此方法是非法的。
#### 8.将一个类设为final会起到什么效果; final类中的域和方法是否必须为final？ Eng P187-188
final类不能被继承。由此，final类中的方法也自动是final的，不管有没有final关键字效果都相同。但final类中的域不一定要设为final，视用户是否希望该域保持恒定而定。
#### 9.Java中的类一般是何时被加载的？static对象和static代码块又是何时被加载的？ Eng P189
Java中的所有东西都是对象。每个类经过编译后存在于一个单独的文件中，该文件在第一次被用到时会被加载。“第一次被用到”指产生该类的第一个对象，或某静态域/静态方法被用到时。
static对象和代码块是在上述文件第一次被加载时按照定义次序一次性被加载的。
#### 10.简述创建一个子类对象时发生的事情。 Eng P189-190
首先java加载器会找到这个子类的编译文件，当它发现该子类有父类时，会加载其父类直至根父类；然后从根父类开始进行static初始化，然后进行第二根父类的static初始化，一直到子类的初始化；随后，子类中的所有域会被初始化为0或null；然后加载父类的构造函数，然后子类的域按定义次序进行初始化，最后执行子类的构造函数#### 疑惑：
Eng P182
One of the clearest ways to determine whether you should use composition or inheritance is to ask whether you’ll ever need to upcast from your new class to the base class. If you must upcast, then inheritance is necessary, but if you don’t need to upcast, then you should look closely at whether you need inheritance. The Polymorphism chapter provides one of the most compelling reasons for upcasting, but if you remember to ask “Do I need to upcast?” you’ll have a good tool for deciding between composition and inheritance.。
