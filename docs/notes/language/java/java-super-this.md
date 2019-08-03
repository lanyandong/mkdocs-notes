### Java中this和super的用法区别

**this**：this是自身的一个对象，代表对象本身，用法大致分为以下3类：

1. 普通直接引用当前对象本身
2. 形参和成员名重名，用this来区分
3. 引用构造方法 ，this(参数) ，应该为构造函数中的第一条语句，调用的事1本类中另外一种形式的构造方法。

**super**: super可以理解为是指向自己超（父）类对象,这个超类指的是离自己最近的一个父类。也大致分为3中中用法:

1. 普通的直接引用，与this类似，只不过它是父类对象，可以通过它调用父类成员。
2. 子类中的成员变量或方法与父类中的成员变量或方法同名，可以使用super区分。
3. 引用构造方法，super（参数）：调用父类中的某一个构造方法（应该为构造方法中的第一条语句）。



```java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
    //构造方法(1) 
    Person() { 
       prt("父类·无参数构造方法： "+"A Person"); 
    }
    //构造方法(2) 
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类无参数构造方法" + "A chinese coder"); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3）-->调用(2)
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       cn = new Chinese("people"); 
       cn = new Chinese("people", 18); 
    } 
}
```

**总结**

1. super()和this()类似,区别是，super()从子类中调用父类的构造方法，this()在同一类内调用其它方法。
2. super()和this()均需放在构造方法内第一行。
3. 尽管可以用this调用一个构造器，但却不能调用两个。
4. this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
5. this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。
6. 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。