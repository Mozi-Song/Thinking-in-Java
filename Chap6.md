第6章 复用类
==============================
#### 1.简述派生类中对基类的一般初始化处理。 Eng 170-171
#### 2.简述派生类中对成员对象的一般cleanup顺序。(疑惑) P174
#### 3.简述final关键字应用于数据域的作用。它用于原始类型数据和用于对象有何不同效果？如何判断它是编译时间常数（编译时初始化，计算可在编译时间执行）还是运行时间常数（运行时进行初始化，初始化后便不会改变）？ 如何保证一个域为编译时间常数？P180
#### 4.Java语法是如何保证final域在使用之前一定被初始化？ Eng P185
#### 5.将方法的参数设为final会起到什么效果？ Eng P185
#### 疑惑：
Eng P182
One of the clearest ways to determine whether you should use composition or inheritance is to ask whether you’ll ever need to upcast from your new class to the base class. If you must upcast, then inheritance is necessary, but if you don’t need to upcast, then you should look closely at whether you need inheritance. The Polymorphism chapter provides one of the most compelling reasons for upcasting, but if you remember to ask “Do I need to upcast?” you’ll have a good tool for deciding between composition and inheritance.
