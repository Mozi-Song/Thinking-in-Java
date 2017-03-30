 第11章 接口
 ====================
 #### 1. 抽象方法的定义 Eng P219
 #### 2. 抽象类的定义 Eng P219
 #### 3. 抽象类是否一定需要包含抽象方法？ Eng P220
 #### 4. 接口的定义 Eng P222
 #### 5. 接口是否都可以加public关键字？ Eng P219Eng P223
 #### 6. 接口中的域的自带属性？ Eng P223
 #### 7. 接口中的方法的自带属性是什么，需要注意什么？ Eng P223
 #### 8. 为什么接口提高了代码的重用率？ Eng P225
 #### 9. 什么叫Strategy design pattern？ Eng P226
 #### 10. 什么是Adapter design pattern? Eng P227
 #### 11. 多重继承的规则是什么？ Eng P228
 #### 12. 一个多重继承的类的upcasting是怎样的？ Eng P231
 #### 13. 使用interface的两个核心目的？Eng P231
 #### 14. 在interface和abstract class之间该如何选择？ Eng P231
 #### 15. 如何理解the "diamond problem"？ Eng P231 （https://en.wikipedia.org/wiki/Multiple_inheritance）
 #### 16. extend关键字后可跟的类/接口个数？ Eng P232
 #### 17. 如果一个类实现了多个接口且继承了一个类,这些借口和类中都存在一个同名函数,可能出现哪些情况? Eng P233
 #### 18. 使用interface而非class作为方法参数的好处? Eng P235
 #### 19. 如何看待interface中的域与enum类之间的关系? Eng P236
 #### 20. interface的域是否属于该interface? Eng P236
 #### 21. 类中的嵌套interface和interface中嵌套interface在定义上有何区别?  Eng P238
 #### 22. 笔记：Notice that when you implement an interface, you are not required to implement any interfaces nested within. Also,  **private** interfaces cannot be implemented outside of their defining classes.
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
