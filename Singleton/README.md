# 单例设计模式(Singleton)

## 概念
  * 单例设计模式是软件开发中最常用的设计模式之一
  * 单例设计模式，即某个类在整个系统中只能有一个实例对象可被获取和使用的代码模式
## 要点与实现
* 类只能有一个实例 ===>  构造器私有化
* 类必须自行创建这个实例 ===> 含有一个该类的静态变量来保存这个唯一的实例
* 类必须自行向整个系统提供这个实例 ===> 对外提供获取该实例对象的方式 ===> public直接暴露或者用静态变量的get方法获取

## 几种常见形式

### 饿汉式：在类初始化时直接创建对象，不存在线程安全问题
* 直接实例化饿汉式(简洁直观)
```
public class Singleton1 {
	public static final Singleton1 INSTANCE = new Singleton1();
	private Singleton1() {	
  
	}
}
```
* 枚举式(最简洁,JDK1.5后强烈推荐)
    * 每个枚举实例都是public static final修饰的
    * 枚举的构造器默认且只能是private
```
public enum Singleton2 {
	INSTANCE;
}
```
使用代码
```
import com.atguigu.single.Singleton2;

public class TestSingleton2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Singleton2 s2 = Singleton2.INSTANCE;
		System.out.println(s2);
	}

}
```
* 静态代码块饿汉式(适合复杂实例化)
```
import java.io.IOException;
import java.util.Properties;

public class Singleton3 {
	public static final Singleton3 INSTANCE;
	private String info;
	
	static {
		
		Properties prop = new Properties();
		try {
			prop.load(Singleton3.class.getClassLoader().getResourceAsStream("single.properties"));
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		INSTANCE = new Singleton3(prop.getProperty("info"));
	}
	
	private Singleton3(String info) {
		this.info = info;
	}
}
```
single.properties(放在src目录下)
```
#key=value
info=laoma666
```
### 懒汉式：延迟创建对象
* 线程不安全(适用于单线程)
```
public class Singleton4 {
	private static Singleton4 instance;
	
	private Singleton4() {
		
	}
	
	public static Singleton4 getInstance() {
		if(null == instance) {
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			instance = new Singleton4();
		}
		return instance;
	}
}
```
执行代码
```
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import com.atguigu.single.Singleton4;

public class TestSingleton4 {
	public static void main(String[] args) throws InterruptedException, ExecutionException {
		/*Singleton4 s4 = Singleton4.getInstance();
		Singleton4 s4_2 = Singleton4.getInstance();
		
		System.out.println(s4 == s4_2);*/
		
		Callable<Singleton4> c = new Callable<Singleton4>() {

			@Override
			public Singleton4 call() throws Exception {
				// TODO Auto-generated method stub
				return Singleton4.getInstance();
			}
			
		};
		
		ExecutorService exec = Executors.newFixedThreadPool(2);
		Future<Singleton4> f1 = exec.submit(c);
		Future<Singleton4> f2 = exec.submit(c);
		
		Singleton4 s1 = f1.get();
		Singleton4 s2 = f2.get();
		
		System.out.println(s1 == s2);
		
		exec.shutdown();
	}
}
```
* 线程安全(适用于多线程)
```
public class Singleton5 {
	private static Singleton5 instance;
	
	private Singleton5() {
		
	}
	
	public static Singleton5 getInstance() {
		if(null == instance) {
			synchronized(Singleton5.class) {
				if(null == instance) {
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					instance = new Singleton5();
				}
			}
		}
		return instance;
	}
}
```
执行代码
```
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import com.atguigu.single.Singleton5;

public class TestSingleton5 {
	public static void main(String[] args) throws InterruptedException, ExecutionException {
		/*Singleton4 s4 = Singleton4.getInstance();
		Singleton4 s4_2 = Singleton4.getInstance();
		
		System.out.println(s4 == s4_2);*/
		
		Callable<Singleton5> c = new Callable<Singleton5>() {

			@Override
			public Singleton5 call() throws Exception {
				// TODO Auto-generated method stub
				return Singleton5.getInstance();
			}
			
		};
		
		ExecutorService exec = Executors.newFixedThreadPool(2);
		Future<Singleton5> f1 = exec.submit(c);
		Future<Singleton5> f2 = exec.submit(c);
		
		Singleton5 s1 = f1.get();
		Singleton5 s2 = f2.get();
		
		System.out.println(s1 == s2);
		
		exec.shutdown();
	}
}
```
* 静态内部类形式(适用于多线程,推荐使用)
```
/**
 * 在内部类被加载和初始化时，才创建INSTANCE实例对象
 * 静态内部类不会自动随着外部类的加载和初始化而初始化，它是要单独去加载和初始化的
 * 因为是在内部类加载和初始化时，创建的，因此是线程安全的
 *
 */
public class Singleton6 {
	private Singleton6() {
		
	}
	private static class Inner{
		private static final Singleton6 INSTANCE = new Singleton6();
	}
	
	public static Singleton6 getInstance() {
		return Inner.INSTANCE;
	}
}
```
