第6章 复用类
==============================
#### 1.简述派生类中对基类的一般初始化处理。 Eng 170-171
#### 2.简述派生类中对成员对象的一般cleanup顺序。(疑惑) P174
#### 3.简述final关键字应用于数据域的作用。它用于原始类型数据和用于对象有何不同效果？如何判断它是编译时间常数（编译时初始化，计算可在编译时间执行）还是运行时间常数（运行时进行初始化，初始化后便不会改变）？ 如何保证一个域为编译时间常数？P180
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
