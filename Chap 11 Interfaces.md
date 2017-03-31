 第11章 接口
 ====================
 #### 1. 抽象方法的定义 Eng P219
 只有方法定义，没有方法体的方法。
 
 #### 2. 抽象类的定义 Eng P219
 含有抽象方法的类。如果一个类含有抽象方法，编译器会强迫用户在类前加abstract关键字。
 
 #### 3. 抽象类是否一定需要包含抽象方法？ Eng P220
 不一定。无抽象方法的类也可以定为abstract，这表明用户不希望产生该类的对象。
 
 #### 4. 接口的定义 Eng P222
 接口相当于终极抽象类，在接口中，所有的方法都不被实现。
 
 #### 5. 接口是否都可以加public关键字？ Eng P223
 只有接口存在于同名文件中时可以。如果省略public关键字，表明该接口只能在同一个包里被使用。
 
 #### 6. 接口中的域的自带属性？ Eng P223
 static和final。
 
 #### 7. 接口中的方法的自带属性是什么，需要注意什么？ Eng P223
 public。须注意实现接口中的方法时要显式定义为public方法，否则相当于在继承时削弱了方法的可见性，这是不被Java编译器允许的。
 
 #### 8. 为什么(相对于类而言)接口提高了代码的重用率？ Eng P225
 如果一个方法只接收某个类作为参数，则我们只能向它传递这个类及其子类的对象。如果它接收某个接口，则我们可以更加灵活地重用这个方法，即当我们想将一个类传递给这个方法时，只需将它adapt到实现这个接口即可。
 
 #### 9. 什么叫Strategy design pattern？ Eng P226
 Strategy design pattern即创建一个方法，传给它不同的参数时，该方法会有不同的行为。 此处，这个方法包含一些固定要进行的行为，而Strategy为我们传给它的参数，它包含动态变化的行为。
 
 #### 10. 什么是Adapter design pattern? Eng P229
 需要重用一个接收某接口的方法时，假设我们从某个库里发现了一个类想要把它传给这个方法，此时需要对这个类进行某种包裹而产生一个实现前述接口的类。例如：
 ```Java
 //: interfaces/interfaceprocessor/Apply.java
package interfaces.interfaceprocessor;
import static net.mindview.util.Print.*;

public class Apply {
  public static void process(Processor p, Object s) {
    print("Using Processor " + p.name());
    print(p.process(s));
  }
} 

//: interfaces/interfaceprocessor/FilterProcessor.java
package interfaces.interfaceprocessor;
import interfaces.filters.*;

class FilterAdapter implements Processor {
  Filter filter;
  public FilterAdapter(Filter filter) {
    this.filter = filter;
  }
  public String name() { return filter.name(); }
  public Waveform process(Object input) {
    return filter.process((Waveform)input);
  }
}	
```

上例中的FilterAdapter将Filter这个类包裹成了Apply.process()函数可以接收的接口类对象。

 #### 11. 多重继承的规则是什么？ Eng P230
 只能**extend**（零到）一个类/抽象类；可以**implement**多个interface。
 
 另外，编译器要求如果有extend，extend需要放在implement前面，否则编译器报错。
 
 #### 12. 一个多重继承的类的upcasting是怎样的？ Eng P231
 该类可以被upcast到任意父类。
 
 #### 13. 使用interface的两个核心目的？Eng P231
 1)多重upcast；
 
 2)避免产生该类的对象。
 
 #### 14. 在interface和abstract class之间该如何选择？ Eng P231　（疑惑）
 如果知道要创建一个基类，在不必须定义方法体和域的情况下，相比于abstract class应偏向interface。
 
 #### 15. 如何理解the "diamond problem"？ Eng P231 （https://en.wikipedia.org/wiki/Multiple_inheritance）
 C++中支持多重继承：有一父类A，子类B和C继承了A并分别重写了A中的方法f，D同时继承了B和C，那么D中的方法f对应哪一个?这个问题就是diamond problem。
 
 由于Java不支持类的多重继承而只支持接口的多重继承，将以上A,B,C,D都改为接口时，由于接口不会重写方法，所以不存在此问题。
 
 #### 16. extend关键字后可跟的类/接口个数？ Eng P232
 接上题，Java不支持类的多重继承而只支持接口的多重继承。所以，一个类只能extend一个类；但一个接口可以extend多个接口。
 
 #### 17. 如果一个类实现了多个接口且继承了一个类,这些接口和类中都存在一个同名函数,可能出现哪些情况? Eng P233
 1）这些接口和类中的同名函数签名各不相同：不会出现问题，子类中包含所有函数。
 
 2）一个类和一个接口中的函数签名相同：
 
 	2.1)它们的返回值相同：　不会出现问题，且子类不需再实现接口中的这个函数，因为已被父类实现。
	2.2)它们的返回值不同： 不能通过编译。
	
 3)两个接口中的函数签名相同：
 
 	3.1)它们的返回值相同：　不会出现问题。	
	3.2)它们的返回值不同： 不能通过编译。
	 
 综上所述，应该尽量避免同名函数。
 
 #### 18. 使用interface而非class作为方法参数的好处? Eng P235
 可把任何类adapt成这个方法可以使用的类。
 
#### 19. 如何看待interface中的域与enum类之间的关系? Eng P236
由于interface中的域天生为static final的，它们相当于enum类。如：
```Java
//: interfaces/Months.java
// Using interfaces to create groups of constants.
package interfaces;

public interface Months {
  int
    JANUARY = 1, FEBRUARY = 2, MARCH = 3,
    APRIL = 4, MAY = 5, JUNE = 6, JULY = 7,
    AUGUST = 8, SEPTEMBER = 9, OCTOBER = 10,
    NOVEMBER = 11, DECEMBER = 12;
} ///:~
```
随着Java SE5中enum类的出现，可以不再使用interface中的域，但在一些古早代码中可能出现，会读即可。

 #### 20. interface的域是否属于该interface? Eng P236 （疑惑）
 
 #### 21. 类中的嵌套interface和interface中嵌套interface在定义上有何区别?  Eng P238
 #### 22. 笔记：Notice that when you implement an interface, you are not required to implement any interfaces nested within. Also,  **private** interfaces cannot be implemented outside of their defining classes.
 #### 23. 如何避免过度使用interface使代码太过复杂？Eng P241
 #### 疑惑：
 
 P230. "Multiple inheritance": 
 Because an interface has no implementation at all—that is, there is no storage associated with an interface—there’s nothing to prevent many interfaces from being combined.
 
 P236. "Nesting Interface": 
 ```java
//: interfaces/nesting/NestingInterfaces.java
package interfaces.nesting;

class A {
  interface B {
    void f();
  }
  public class BImp implements B {
    public void f() {}
  }
  private class BImp2 implements B {
    public void f() {}
  }
  public interface C {
    void f();
  }
  class CImp implements C {
    public void f() {}
  }	
  private class CImp2 implements C {
    public void f() {}
  }
  private interface D {
    void f();
  }
  private class DImp implements D {
    public void f() {}
  }
  public class DImp2 implements D {
    public void f() {}
  }
  public D getD() { return new DImp2(); }
  private D dRef;
  public void receiveD(D d) {
    dRef = d;
    dRef.f();
  }
}	

interface E {
  interface G {
    void f();
  }
  // Redundant "public":
  public interface H {
    void f();
  }
  void g();
  // Cannot be private within an interface:
  //! private interface I {}
}	

public class NestingInterfaces {
  public class BImp implements A.B {
    public void f() {}
  }
  class CImp implements A.C {
    public void f() {}
  }
  // Cannot implement a private interface except
  // within that interface's defining class:
  //! class DImp implements A.D {
  //!  public void f() {}
  //! }
  class EImp implements E {
    public void g() {}
  }
  class EGImp implements E.G {
    public void f() {}
  }
  class EImp2 implements E {
    public void g() {}
    class EG implements E.G {
      public void f() {}
    }
  }	
  public static void main(String[] args) {
    A a = new A();
    // Can't access A.D:
    //! A.D ad = a.getD();
    // Doesn't return anything but A.D:
    //! A.DImp2 di2 = a.getD();
	a.receiveD(a.new DImp2());
    // Cannot access a member of the interface:
    //! a.getD().f();
    // Only another A can do anything with getD():
    A a2 = new A();
    a2.receiveD(a.getD());
  }
} ///:~
```

...**A.DImp2** can only be used as itself. You are not allowed to mention the fact that it implements the **private** interface D, so implementing a **private** interface is a way to force the definition of the methods in that interface without adding any type information (that is, without allowing any upcasting).

 (疑惑点:以上一段英文表示class中虽然可以定义private interface，但可以有一个嵌套的public class来实现该interface。private interface的意义在于隐藏了D <- DImp2这层继承关系，所以DImp2不能被upcast到D。然而以上代码`a.receiveD(a.new DImp2());`可以通过编译，这证明compilor是了解D <- DImp2这层继承关系的。目前尚不清楚这个情景的应用意义和特殊之处。）
 
 P240.Interfaces and Factories: 
 ...Without the Factory Method, your code would somewhere have to specify the exact type of **Service** being created, so that it could call the appropriate constructor.
 
Why would you want to add this extra level of indirection? One common reason is to create a framework.
