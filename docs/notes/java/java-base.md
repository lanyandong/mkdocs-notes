## Java基础强化——Reflection

1、反射：在程序运行时通过一个类的对象获取这个类的信息的方法。

2、Class类：在java里万事万物皆对象，可以理解为是Class类的对象，Class有一些方法可以由类的对象获取类的信息。获取一个class的Class实例有三种办法，如下：

```java
package reflection;

public class ReflectionDemo {
	
	public static void main(String args[]) {
		// 实例对象如何表示
		ClassDemo classDemo = new ClassDemo();
	
		// ClassDemo是类，其实也是一个实例对象是基于Class的。
		//方法1：（任何一个类都有一个隐含的成员变量class）
		Class c1 = ClassDemo.class; 
		
		//方法2：（通过getClass()方法获取一个对象的类信息）
		Class c2 = classDemo.getClass(); 
		
		// 两种表达式相同的
		System.out.println(c1 == c2);
		
		//方法3：通过Class的forName()方法
		Class c3 = null;
		try {
			c3 = Class.forName("reflection.ClassDemo");
		} catch (Exception e) {
			// TODO: handle exception
		}
		
		// 也是相等的
		System.out.println(c2 == c3);
		
		// 可以通过c1，c2, c3创建该类的对象
		try {
			ClassDemo classDemo2 = (ClassDemo)c1.newInstance(); // 根据实际情况进行强制类型转换
			classDemo2.print();
		} catch (InstantiationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}	
	}
}

// classdemo 类
class ClassDemo{
	
	void print() {
		System.out.println("class demo");
	}	
}
```

3、静态加载和动态加载：静态加载时值程序在编译期的时候就完成了加载编译，比如new对象就是编译时加载的，已经创建好了对象实例。而动态加载时指在运行时需要使用的时候才进行加载，绕过编译且不报错。

4、通过反射API获取类信息：使用Class的提供的方法可以获取一个对象的类的信息，包括成员变量，成员方法，构造方法，甚至包括接口信息，继承关系等。Java的反射API提供的`Field`类封装了成员变量的所有信息，`Method`对象封装了方法的所有信息，`Constructor`对象封装了构造方法的所有信息。

5、动态代理：JDK提供的动态创建接口对象的方式，即没有实现类但是在运行期动态创建了一个接口对象的方式。

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

// 定义一个InvocationHandler实例，它负责实现接口的方法调用；
// 通过Proxy.newProxyInstance()创建interface实例，它需要3个参数：(使用的`ClassLoader`；需要实现的接口数组；用来处理接口方法调用的`InvocationHandler`实例。)
//将返回的`Object`强制转型为接口。 

public class Main {
	
	public static void main(String args[]) {
		
		InvocationHandler handler = new InvocationHandler() {
			@Override
			public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method);
                if (method.getName().equals("morning")) {
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }	
		};
		
		Hello hello = (Hello) Proxy.newProxyInstance(
	            Hello.class.getClassLoader(), // 传入ClassLoader
	            new Class[] { Hello.class },  // 传入要实现的接口
	            handler); // 传入处理调用方法的InvocationHandler
	        hello.morning("Bob");
	}
}

interface Hello {
    void morning(String name);
}
```

6、代理模式和反射机制：动态代理是设计模式当中代理模式的一种（为其他对象提供一种代理以控制这个对象的访问），JDK的动态代理主要是使用的是反射机制。常应用于AOP（面向切面编程）、RPC（远程过程调用），反编译，EventBus 2.x，动态生成类框架等。

优点：运行期类型的判断，动态类加载，动态代理使用反射；

缺点：性能是一个问题，反射相当于一系列解释操作，通知jvm要做的事情，性能比直接的java代码要慢很多。



## Java基础强化——Object类

Object类是java中所有类的父类。 换句话说，它是java的顶级类。

Object类的方法：

| 方法                                   | 描述                                           |
| -------------------------------------- | ---------------------------------------------- |
| `public final Class getClass()`        | 返回此对象的`Class`类对象。                    |
| `protected Object clone() `            | 创建并返回此对象的精确副本(克隆)。             |
| `public boolean equals(Object obj)`    | 判断此对象与给定此对象是否相等（同内存单元）。 |
| `public int hashCode()`                | 返回此对象的哈希码值                           |
| `protected void finalize()`            | 在对象被垃圾收集之前由垃圾收集器调用。         |
| `public String toString()`             | 返回此对象的字符串表示形式。                   |
| `public final void notify()`           | 该方法唤醒在该对象上等待的某个线程。           |
| `public final void notifyAll()`        | 该方法唤醒在该对象上等待的所有线程。           |
| `public final void wait(long timeout)` | 当前线程等待指定的毫秒,直到调用`notify()`      |

补充：wait方法就是使当前线程等待该对象的锁。调用该方法后当前线程进入睡眠状态，直到以下事件发生：

（1）其他线程调用了该对象的notify方法。

（2）其他线程调用了该对象的notifyAll方法。

（3）其他线程调用了interrupt中断该线程。

（4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

