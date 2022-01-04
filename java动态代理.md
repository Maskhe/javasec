想必有过编程经验的同学应该知道什么叫做代理，即使是没有接触过编程，也一定在生活中听过各种各样的代理，举个例子，我们生活中经常到火车站去买车票，但是人一多的话，就会非常拥挤，于是就有了代售点，我们能从代售点买车票了

我们不仅仅能够在代售网点买车票，还可以从中获得一些额外的服务，比如一些代售网点可以帮我们规划行程、订酒店等等

所以，代理模式不仅仅是提供了一个中间代理来控制、访问原目标，它还增强了原目标的功能以及简化了访问方式

那具体到java语言中，我们应该怎么应用代理模式、怎么给一个对象创建代理呢？

接下来我将介绍java中常见的三种代理模式

### 静态代理

这种代理需要代理类和目标类实现同一接口，让我们一起来写一个demo吧

先创建一个接口IUser：

```java
public interface IUser {  
    public void sayName();  
}
```

实现类User:

```java
public class User implements IUser{  
  
    @Override  
 	public void sayName() {  
        System.out.println("tntaxin");  
 	}  
}
```

创建代理类，同样实现IUser接口，我们扩展下sayName()方法

```java
public class UserProxy implements IUser {  
    private IUser target;  
 	public UserProxy(IUser obj){  
        this.target = obj;  
 	}  
  
    @Override  
 	public void sayName() {  
        System.out.println("我是他的代理");  
 		this.target.sayName();  
 	}  
}

```

最后创建一个测试代理效果的类：

```java
public class App   
{  
    public static void main( String[] args )  
    {  
        IUser user = new User();  
 		// 创建静态代理  
 		IUser userProxy = new UserProxy(user);  
 		userProxy.sayName();  
	 }  
}
```

运行效果如下：

![](/Users/momo/javasec/dynamic_proxy/Pasted image 20211231165426.png)



### 动态代理

上面提到的静态代理很好理解，就是在目标对象外又包装了一层，那什么又是动态代理呢？它俩的区别如下：

-   静态代理在编译时就已经实现，编译完成后代理类是一个实际的class文件
-   动态代理是在运行时动态生成的，即编译完成后没有实际的class文件，而是在运行时动态生成类字节码，并加载到JVM中


动态代理不需要事先目标对象的接口，但是要求目标对象必须是要事先接口的，没有接口的对象是不能使用动态代理的



接下来我们还是通过编写demo的方式来学习吧


接口以及实现类还是上面用到的`IUser`以及`User`

除了上面两个类之外，我们再创建一个动态代理类：


```java
public class ProxyFactory {  
    private Object target;  
 		public ProxyFactory(Object target){  
        this.target = target;  
 }  
  
    public Object getProxyInstance(){  
        return Proxy.newProxyInstance(this.target.getClass().getClassLoader(), 			this.target.getClass().getInterfaces(),  
 				new InvocationHandler() {  
                    @Override  
 					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
                        System.out.println("我是他的代理");  
 						method.invoke(target, args);  
 					return null; }  
                });  
	 }  
}
```

可以看到，这个类实际上是一个工厂类，用于生产代理对象的


让我们把目光聚集到getProxyInstance方法，它使用到了`Proxy.newProxyInstance`方法：

```java
static Object    newProxyInstance(ClassLoader loader,  //指定当前目标对象使用类加载器
 Class<?>[] interfaces,    //目标对象实现的接口的类型
 InvocationHandler h      //事件处理器
) 
//返回一个指定接口的代理类实例，该接口可以将方法调用指派到指定的调用处理程序。
```

> 现在知道为啥使用动态代理的类必须实现接口了吧，因为这个方法中的第二个参数需要

从名字可以看出来，这个方法才是真正生成代理的方法，其中第三个参数是事件处理器`InvocationHandler`，实现该类必须要实现`invoke`方法

```java
 Object invoke(Object proxy, Method method, Object[] args) 
// 在代理实例上处理方法调用并返回结果。
```


在这个方法中我们一般会通过反射的方式调用目标对象的对应方法，我想，大家也能够从这个方法的参数中看出一点端倪

> [java反射机制](https://github.com/Maskhe/javasec/blob/master/1.java%E5%8F%8D%E5%B0%84%E6%9C%BA%E5%88%B6.md)

同样的，我们最后创建一个测试程序来测试下动态代理的效果：

```java
public class App   
{  
    public static void main( String[] args )  
    {  
        IUser user = new User();  
  
//         创建动态代理  
 		ProxyFactory proxyFactory = new ProxyFactory(user);  
 		IUser userProxy2 = (IUser)proxyFactory.getProxyInstance();  
		// 打印生成的代理对象的类名  
		System.out.println(userProxy2.getClass());
 		userProxy2.sayName();  
	}  
}
```

运行上面的类后，会得到如下输出：

```sh
class com.sun.proxy.$Proxy0
我是他的代理
tntaxin
```

这里，我们只是打印了一下动态生成的代理类的类名，其实我们还可以进一步用一些字节码操作工具把这个代理类的源码给输出到文件中，当然我刚刚还搜到更方便的一种方法，在代码中将`sun.misc.ProxyGenerator.saveGeneratedFiles`属性值设为true，就会在项目根目录下生成代理类的文件，改动后的代码如下：


```java
public class App   
{  
    public static void main( String[] args )  
    {  
        IUser user = new User();  
  
 		//生成$Proxy0的class文件 
		System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");  
 		// 创建动态代理  
 		ProxyFactory proxyFactory = new ProxyFactory(user);  
		IUser userProxy2 = (IUser)proxyFactory.getProxyInstance();  
 		// 打印生成的代理对象的类名  
 		System.out.println(userProxy2.getClass());  
 		userProxy2.sayName();  
 	}  
}
```

运行上述代码，生成的`$Proxy0.class`文件内容如下：

![](/Users/momo/javasec/dynamic_proxy/proxy.jpeg)

其中super.h其实就是我们在newProxyInstance方法中传入的invocationhandler了，看了这个代码，是不是把动态代理掌握的透透儿的了？


### cglib代理


cglib (Code Generation Library )是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。

cglib与动态代理最大的**区别**就是

-   使用动态代理的对象必须实现一个或多个接口
-   使用cglib代理的对象则无需实现接口，达到代理类无侵入。

同样的，我们来创建demo，先引入cglib包

```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.2.5</version>
</dependency>
```

然后我们使用cglib来创建一个代理工厂类：

```java

public class ProxyFactory implements MethodInterceptor {  
  
     private Object target;  
  
	 public ProxyFactory(Object target){  
			this.target = target;  
	 }  
  
		//为目标对象生成代理对象  
	 public Object getProxyInstance() {  
			//工具类  
			Enhancer en = new Enhancer();  
			//设置父类  
			 en.setSuperclass(target.getClass());  
			 //设置回调函数  
			en.setCallback(this);  
			 //创建子类对象代理  
			return en.create();  
	 }  

  
		@Override  
	 public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {  
			System.out.println("我是cglib生成的代理");  
			method.invoke(target, objects);  
			return null; }  
	}

```


还是很简单的吧

然后创建一个测试类：

```java
public class App   
{  
    public static void main( String[] args )  
    {  
        IUser user = new User();  
  		// cglib创建代理类  
		IUser userProxy3 = (IUser)new ProxyFactory2(user).getProxyInstance();  
		System.out.println(userProxy3.getClass());  
		userProxy3.sayName();
 		
 	}  
}
```

运行后会得到如下输出：

```sh
class org.example.User$$EnhancerByCGLIB$$8458a7f7
我是cglib生成的代理
tntaxin
```

### 总结

1.  静态代理实现较简单，只要代理对象对目标对象进行包装，即可实现增强功能，但静态代理只能为一个目标对象服务，如果目标对象过多，则会产生很多代理类。
2.  JDK动态代理需要目标对象实现业务接口，代理类只需实现InvocationHandler接口。
3.  动态代理生成的类为 lass com.sun.proxy.\$Proxy4，cglib代理生成的类为class com.cglib.UserDao\$\$EnhancerByCGLIB\$\$552188b6。
4.  静态代理在编译时产生class字节码文件，可以直接使用，效率高。
5.  动态代理必须实现InvocationHandler接口，通过反射代理方法，比较消耗系统性能，但可以减少代理类的数量，使用更灵活。
6.  cglib代理无需实现接口，通过生成类字节码实现代理，比反射稍快，不存在性能问题，但cglib会继承目标对象，需要重写方法，所以目标对象不能为final类。


ref: https://segmentfault.com/a/1190000011291179