---
layout: post
title: "java创建对象的几种方式"
date: 2017-03-27
categories: java
tags: java  
---

*  content
{:toc}

对于java开发来说，每次工作都会创建大量的对象，而创建对象的方式有多种，每一种在不同的应用场景中都有自己的特点。




## 使用new关键字  
这是非常简单也是最常用的方式，使用这种方法可以调用所有我们需要调用的构造方法。
```java
Person person;//仅仅是个引用，并没有分配内存
person = new Person();//在堆内存中占一块地盘，创建一个对象
```
---
## 使用newInstacne方法
一种是通过**[java.lang.Class](http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html#newInstance())**类的newInstance()调用无参的构造方法创建对象  

| T | newInstacne() |
|   | Creates a new instance of the class represented by this Class object. --java API_1.7 |

```java
//先取得了无参的构造方法,再调用构造方法的newInstance来创造对象
Person.class.newInstance();
Class.forName("com.entitys.Person").newInstance();
```
---
另一种是通过**[java.lang.reflect.Constructor](https://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Constructor.html#newInstance(java.lang.Object...))**的newInstance()，可以调用**带参**的构造方法创建对象  

| T | newInstance()
|   |Uses the constructor represented by this Constructor object to create and initialize a new instance of the constructor's declaring class, with the specified initialization parameters.--java API_1.7 |

```java
Person person = Person.class.getConstructor().newInstance();//调用无参的构造函数
Person person = Person.class.getConstructor(Integer.class).newInstance(1);//调用带参的构造函数
```
这些newInstance都是用反射的手段来创建的对象，**使用newInstance时对象中一定要放一个默认的无参的构造方法。否则会抛java.lang.NoSuchMethodException**  
new与newInstance的区别在[开源中国](http://www.oschina.net/question/256102_51317)看到一个很有趣的解释  
> new是自己下厨，做饭自己吃。newInstance是外面吃别人做给你吃。
自己做可以保证安全，而且可以有多个口味，什么酱，放多少自己定。new对象不会因为找不到类而出异常，而且你还可以调用多个构造函数，按照自己口味去new对象，去做一份符合自己口味的。
在外面吃，可能会你点的菜那个店里没有，或者是口味不对，你要吃番茄酱，但是人家只有花生酱。而且菜没洗干净你也不知道。newInstance也是如此，很可能你要的对象他不存在，或者是参数不对。

---
## 使用clone
clone顾名思义就是在内存中在找一块地方把原来的地盘上的原模原样的复制过去，无论什么时候使用clone都不会调用构造方法，而是JVM为我们创建一个对象并把之前的属性，方法复制到新对象中
```java
person.clone();
```
**使用clone()时对象要实现Cloneable接口，否则会抛java.lang.CloneNotSupportedException**  

## 使用反序列化
既然存在反序列化那么也应该存在的序列化。java序列化就是将对象转化为字节流，反序列化就是把字节流还原为对象的过程。  
**将一个对象进行序列化**
**[java.io.ObjectOutputStream](http://docs.oracle.com/javase/7/docs/api/java/io/ObjectOutputStream.html)**

| void | writeObject() |
|      | Write the specified object to the ObjectOutputStream --java API_1.7 |
| void | flush() |
|      | Flushes the stream --java API_1.7|

```java
Person person = new Person();
File file = new File("D://Serial.obj");
FileOutputStream fos = new FileOutputStream(file);
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(person);
oos.flush();//强制将缓冲区的数据发送出去，不必等到缓冲区满
oos.close();
fos.close();
```
**通过反序列化恢复对象**
**[java.io.ObjectInputStream](http://docs.oracle.com/javase/7/docs/api/java/io/ObjectInputStream.html)**

| Object | readObject() |
|        | Read an object from the ObjectInputStream --java API_1.7 |

```java
FileInputStream fis = new FileInputStream(file);
ObjectInputStream ois = new ObjcetInputStream(fis);
Person person = (Person)ois.readObject();
ois.close();
fis.close();

//java SE7的特性try-with-resources
try (ObjecInputStream ois = new ObjectInputStream(new FileInputStream(file));) {
	Person person = (Person)ois.readObject();
} catch (Exception e) {
	e.printStackTrace();
}
```
这样也可以不使用任何构造方法，通过反序列化也可以创建对象
## 测试
准备的Person类
```java
class Person implements Cloneable, Serializable{
	private static final long serialVersionUID = 323451069095354874L;
	private Integer id;
	private String name;
	public Person(){
		System.out.println("Person Constructor....");
	}
	public Person(Integer id,String name){
		this.id = id;
		this.name = name;
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((id == null) ? 0 : id.hashCode());
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Person other = (Person) obj;
		if (id == null) {
			if (other.id != null)
				return false;
		} else if (!id.equals(other.id))
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}
	@Override
	public String toString() {
		return "Person [id=" + id + ", name=" + name + "]";
	}
	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
}
```
用不同的方式创建类
```java
public class ObjectCreate{
	public static void main(String[] args) throws Exception{
		Person person1 = new Person();
		person1.setName("new");
		System.out.println("person1: " + person1);
		
		//使用Class的newInstance()
		Person person2 = (Person) Class.forName("org.test.objectCreate.Person").newInstance();
		//或者
		//Person person2 = Person.class.newInstance();
		person2.setName("ClassForName");
		System.out.println("person2: " + person2);
		
		//使用Constructor的newInstance()
		Person person3 = Person.class.getConstructor().newInstance();
		person3.setName("Constructor");
		System.out.println("person3: " + person3);
		
		//使用clone()
		Person person4 = (Person)person3.clone();
		person4.setId(1);
		person4.setName("clone");
		System.out.println("person4: " + person4);
		
		//序列化
		File file = new File("D:\\serializable.txt");
		file.createNewFile();
		FileOutputStream fos = new FileOutputStream(file);
		ObjectOutputStream oos = new ObjectOutputStream(fos);
		oos.writeObject(person4);
		oos.flush();
		oos.close();
		fos.close();
		
		//反序列化
		FileInputStream fis = new FileInputStream(file);
		ObjectInputStream ois = new ObjectInputStream(fis);
		Person person5 = (Person)ois.readObject();
		System.out.println("person5: " + person5);
		ois.close();
		fis.close();
	}
}
```
控制台的输出结果
```
Person Constructor....
person1: Person [id=null, name=new]
Person Constructor....
person2: Person [id=null, name=ClassForName]
Person Constructor....
person3: Person [id=null, name=Constructor]
person4: Person [id=1, name=clone]
person5: Person [id=1, name=clone]
```
---
参考:  
1. [http://blog.csdn.net/wangloveall/article/details/7992448/](http://blog.csdn.net/wangloveall/article/2 details/7992448/)  
2. [http://www.importnew.com/22405.html](http://www.importnew.com/22405.html)  
3. [http://blog.csdn.net/zhangjg_blog/article/details/18369201](http://blog.csdn.net/zhangjg_blog/article/details/18369201)  

---
**文章声明：自由转载-非商用-非衍生-保持署名**
