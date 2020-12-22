## Java面向对象



### 1 类与对象

面向对象编程，是一种通过对象的方式，把现实世界映射到计算机模型的一种编程方法。

如：“书”是一种抽象的概念，所以它是类，而《Java核心技术》、《Java编程思想》是对象。

类是由属性(Field)和方法(Method)组成。

```java
//定义类语法：
修饰符 class 类名 {
    属性声明;
    方法声明;
}
```

```java
//属性声明语法：
修饰符 数据类型 属性名 = 初始化值 ;
```



```java
// 测试定义类
class Person {
    public String name;
    public int age;
}
```

```java
// 测试创建对象
Person p = new Person();
```



#### 1.1 修饰符

|               | 同一个类 | 同一个包 | 子类 | 所有类 |
| ------------- | -------- | -------- | ---- | ------ |
| private       | √        | ×        | ×    | ×      |
| default(默认) | √        | √        | ×    | ×      |
| protected     | √        | √        | √    | ×      |
| public        | √        | √        | √    | √      |



### 2 属性

#### 2.1 属性变量初始值

| 成员变量类型 | 初始默认值 |
| ------------ | ---------- |
| byte         | 0          |
| short        | 0          |
| int          | 0          |
| long         | 0L         |
| float        | 0.0L       |
| double       | 0.0        |
| char         | 0          |
| boolean      | false      |
| 引用类型     | null       |

Java 语言中除基本类型之外的变量类型都称之为引用类型。



### 3 方法

```java
// 定义方法语法：
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

方法返回值通过`return`语句实现，如果没有返回值，返回类型设置为`void`，可以省略`return`。

```java
// 测试方法 
public class TestMethod {
	public static void main(String[] args) {
		Person p=new Person();
		p.setBirth(1985);
		System.out.println(p.getAge());
	}
}
 
class Person{
	private String name;
	private int birth;
	
	public void setBirth(int birth) {  //普通方法
		this.birth=birth;
	}
	
	public int getAge() {	//普通方法
		return calcAge(2020);
	}
	
	private int calcAge(int currentYear) {	//private方法
		return currentYear-this.birth;
	}
}
```



#### 3.1 方法参数

方法可以包含0个或任意个参数。方法参数用于接收传递给方法的变量值。调用方法时，必须严格按照参数的定义一一传递。



#### 3.2 可变参数

可变参数用`类型...`定义，可变参数相当于数组类型。

```java
class Group {
    private String[] names;
 
    public void setNames(String... names) {
        this.names = names;
    }
}
```

```java
// 测试可变参数
Group g=new Group();
g.setNames();			   //传0个参数
g.setNames("林一");		 //传1个参数
g.setNames("林一","林二");	//传2个参数
```



#### 3.3 参数传递

【1】基本类型参数传递，传递的是数值。

```java
// 测试基本类型参数传递
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15;
        p.setAge(n); 	//setAge()方法获得的参数，复制了n的值给p.age，后面n改变不影响p.age
        System.out.println(p.getAge()); // 打印15
        n = 20;
        System.out.println(p.getAge()); // 打印15
    }
}

class Person {
    private int age;
 
    public int getAge() {
        return this.age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
}
```



【2】引用类型参数的传递，传递的是对象地址。

```java
// 测试引用类型参数的传递
public class TestMethod {
	public static void main(String[] args) {
		Person p=new Person();
		String[] f=new String[] {"林一","林二"};
		p.setName(f);  //数组是一个对象，传入数组只是传引用地址
		System.out.println(p.getName());
		f[1]="林三";  //修改数组元素
		System.out.println(p.getName());  //引用地址没变，数组元素改变，结果也改变
	}
}
 
class Person{
	private String[] name;
	
	public String getName() {
		return this.name[0]+" "+this.name[1];
	}
	
	public void setName(String[] name) {
		this.name=name;
	}
}
```



#### 3.4 构造方法

构造方法作用：是初始化类对象。

- 构造方法的名称就是类名。
- 构造方法的参数没有限制，在方法内部，也可以编写任意语句。
- 构造方法没有返回值（也没有`void`），调用构造方法，必须用`new`操作符。
- 所有类都有默认构造方法。

```java
// 测试构造方法
public class TestMethod {
	public static void main(String[] args) {
		Person p=new Person("林一",18);
		System.out.println(p.getName());
		System.out.println(p.getAge());
	}
}

class Person{
	private String name;
	private int age;
	
	public Person(String name,int age) {	//构造方法
		this.name=name;
		this.age=age;
	}
	
	public String getName() {
		return this.name;
	}
	
	public int getAge() {
		return this.age;
	}
}
```



如一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句。

如自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法。

```java
class Person {
    public Person() {}	//默认构造方法，无参数，无语句
}
```



多个构造方法通过构造方法的参数数量、位置和类型自动区分。

一个构造方法可以调用其他构造方法。

```java
// 测试多个构造方法
class Person {
    private String name;
    private int age;
 
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }
 
    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```



#### 3.5 ==与equals方法

==代表比较双方是否相同。如是基本类型则表示值相等，如是引用类型则表示地址相等即是同一个对象。

Object 的 equals 方法默认就是比较两个对象的hashcode，是同一个对象的引用时返回 true 否则返回 false。

【eclipse把光标放String类，按ctrl键+点击鼠标左键看类源码】

```java
// 测试==与equals方法
public class TestEquals {
	public static void main(String[] args) {
		Person p1=new Person();
		Person p2=new Person();
		System.out.println(p1==p2);	//false
		System.out.println(p1.equals(p2));	//false
		
		String s1=new String();
		String s2=new String();
		System.out.println(s1==s2);	//false
		System.out.println(s1.equals(s2));	//true,equals重构过
	}
}
 
class Person{
	int id;
	String name;
	public void showName() {
		System.out.println(id+" "+name);
	}
}
```



#### 3.6 方法重载

方法名相同，但各自的参数不同，称为方法重载（`Overload`）。

**注意**：方法重载的返回值类型通常都是相同的。

方法重载的目的：功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。

```java
// 测试方法重载
class Hello {
    public void hello() {
        System.out.println("Hello, world!");
    }
 
    public void hello(String name) {    // 方法重载
        System.out.println("Hello, " + name + "!");
    }
}
```





### 4 关键字

#### 4.1 this关键字

变量`this`，它始终指向当前对象。通过`this.field`就可以访问当前对象的字段。

如没有命名冲突，可以省略`this`。

如有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`。

```java
// 测试this关键字
class Person {
    private String name; 
    public String getName() {
        return name; // 相当于this.name
    }
    public void setName(String name) {
        this.name = name; // this不可少，this.name为成员变量
    }
}
```



#### 4.2 super关键字

`super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。

```java
// 测试super关键字
class Student extends Person {
    protected int score;
    
    public String hello() {
        return "Hello, " + super.name;	//调用父类属性
    }
    
    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
}
```



#### 4.3 static关键字

static声明的成员变量为静态属性。

static声明的方法为静态方法。

- 静态属性被该类的所有实例共享，在类被载入时被显式初始化。
- 静态方法不需要对象， 就可以调用(如：类名.方法名)。
- 在调用静态方法时， 不会将对象的引用传递给它， 所以在static方法中不可访问非static的成员。
- 静态方法不能以任何方式引用this和super关键字。
- 类第一次被载入时先执行static代码块； 类多次载入时， static代码块只执行一次； Static经常用来进行static变量的初始化。
- 静态初始化块中不能访问非static成员。



```java
// 测试static关键字
public class TestStatic {
	int a;
	static int width;	//静态成员变量、类属性
    
    static { 	//静态初始化块，类载入时加载
		System.out.println("静态初始化块");
	}
	
	static void gg() {	//静态方法
		System.out.println("gg");
	}
	
	void tt() {	//普通方法
		System.out.println("tt");
	}
	
	public static void main(String[] args) {	//java程序执行入口
		TestStatic hi=new TestStatic();
		TestStatic.width=2;		//调用静态成员变量
		TestStatic.gg();   //调用静态方法
	}	
}
```



### 5 变量

方法外变量，为成员变量。

方法内变量，为局部变量。

```java
class MyClass{
    String name;      //成员变量
    int age;
    public void sayScore(){
        int score;    //局部变量
        ......
    }
}
```



### 6 内存分析

堆（ Heap）：存储对象实例。

栈（ Stack）： 存储局部变量。

方法区（Method Area） ：存储类信息、 常量、 静态变量、 静态方法、代码。



### 7 继承

Java使用`extends`关键字来实现继承。

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```



### 8 多态

Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型，称之为**多态**。

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）。

加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。

```java
// 测试多态
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 打印Student.run
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```



```java
//测试多态
package com.test;
public class TestPolymorphic {
	public static void main(String[] args) {
		Income[] incomes=new Income[] {
				new Income(3000),
				new Salary(7500),
				new SSA(15000)
		};
		System.out.println(totalTax(incomes));
	}
	
	public static double totalTax(Income... incomes) {
		double total=0;
		for(Income income:incomes) {
			total+=income.getTax();
		}
		return total;
	}
	
}

class Income{
	protected double income;
	public Income(double income) {
		this.income=income;
	}
	public double getTax() {
		return income*0.1;
	}
}

class Salary extends Income{
	public Salary(double income) {
		super(income);
	}
	
	@Override
	public double getTax() {
		if(income<=5000) {
			return 0;
		}
		return (income-5000)*0.2;
	}
}

class SSA extends Income{
	public SSA(double income) {
		super(income);
	}
	
	@Override
	public double getTax() {
		return 0;
	}
}
```



#### 8.1 覆写Object方法

- `toString()`：把instance输出为`String`；
- `equals()`：判断两个instance是否逻辑相等；
- `hashCode()`：计算一个instance的哈希值。

```java
class Person{
	private String name;
	
	// 显示更有意义的字符串
	@Override
	public String toString() {
		return "name="+name;
	}
	
	// 比较是否相等
	@Override
	public boolean equals(Object o) {
		if(o instanceof Person) {
			Person p=(Person) o;
			return this.name.equals(p.name);
		}
		return false;
	}
	
	// 计算hash
	@Override
	public int hashCode() {
		return this.name.hashCode();
	}
}
```



用`final`修饰的方法不能被`Override`。

用`final`修饰的类不能被继承。

用`final`修饰的字段在初始化后不能被修改。

```java
class Person {
    protected String name;
    public final String hello() {	//不允许子类Override的方法
        return "Hello, " + name;
    }
}
```

```java
final class Person {	//不允许子类Override的类
    protected String name;
}
```

```java
class Person {
    public final String name = "Unamed";	//成员变量name不允许修改
}
```









