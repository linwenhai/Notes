## Java面向对象



### 一、类与对象

面向对象编程，是一种通过对象的方式，把现实世界映射到计算机模型的一种编程方法。

**类（Class）**：是对一类事物的描述 ，是抽象的 、概念上的定义。由属性（Field）、方法（Method）和构造方法（Constructor ）组成。

**对象（Object）**：是实际存在的该类事物的每个个体，因而也称为实例（instance）。

如：“书”是一种抽象的概念，所以它是类，而《Java核心技术》、《Java编程思想》是对象。



#### 1 定义类

```java
/* 类定义语法 */
修饰符 class 类名 {
    //属性定义语法
    修饰符 数据类型 属性名 = 初始化值 ;
    
    //方法定义语法
    修饰符 方法返回类型 方法名(方法参数列表) {
        若干方法语句;
        return 方法返回值;
    }
}
```

```java
/* 测试定义类 */
public class Person{    //定义类
	public String name;		//声明属性
	public int age;
 
    public void sayHello() {    //声明方法
    	System.out.println("Hello");
    }
}
```



#### 2 创建对象

```java
//属性声明语法：
修饰符 数据类型 属性名 = 初始化值 ;
```

```java
// 测试创建对象
TestStu s=new TestStu();
```



#### 3 修饰符

|               | 同一个类 | 同一个包 | 子类 | 所有类 |
| ------------- | -------- | -------- | ---- | ------ |
| private       | √        | ×        | ×    | ×      |
| default(默认) | √        | √        | ×    | ×      |
| protected     | √        | √        | √    | ×      |
| public        | √        | √        | √    | √      |



#### 4 属性

| 数据类型     | 初始化默认值 |
| ------------ | ------------ |
| 整型         | 0            |
| 浮点型       | 0.0          |
| 字符型       | '\u0000'     |
| 布尔型       | false        |
| 所有引用类型 | null         |

Java 语言中除基本类型之外的变量类型都称之为引用类型。



#### 5 变量

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



### 二、内存分析

堆（ Heap）：存储对象实例

栈（ Stack）： 存储局部变量

方法区（Method Area） ：存储类信息、 常量、 静态变量、 静态方法、代码

![img](D:\Notes\Java\image\20201210134239616.png)



### 三、方法

```java
// 定义方法语法：
[修饰符] 方法返回类型 方法名(参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

方法返回值通过`return`语句实现，如果没有返回值，返回类型设置为`void`，可以省略`return`。

```java
package com.test;
 
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
	
	public int getAge() {
		return calcAge(2020);
	}
	
	private int calcAge(int currentYear) {	//private方法，只能同一个类调用
		return currentYear-this.birth;
	}
}
```



#### 1 方法参数

方法可以包含0个或任意个参数。方法参数用于接收传递给方法的变量值。调用方法时，必须严格按照参数的定义一一传递。



#### 2 可变参数

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



#### 3 参数传递

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



#### 4 方法重载（Overload）

方法名相同，但各自的参数不同，称为方法重载。

注意：方法重载的返回值类型通常都是相同的。

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。



#### 5 构造方法

构造方法(constructor)，是一个创建对象时被自动调用的特殊方法，目的是对象的初始化。

【构造方法要点】：

1. 通过new关键字调用。
2. 构造方法有返回值，但不能定义返回值类型，不能在构造器里使用return返回某个值。
3. 如没有定义构造方法，则编译器会自动定义一个无参的构造方法。如已定义则编译器不会自动添加。
4. 构造器的方法名必须和类名一致。

```java
package com.test;
/**
 * 测试定义构造方法
 * @author 林
 *
 */
public class TestStu {
	public static void main(String[] args) {
		Stu s=new Stu("林一",17);		// 创建对象会自动调用构造方法
		System.out.println(s.getName());
	}
}
 
class Stu{
	private String name;
	private int age;
	
	public Stu(String name,int age) {	//定义构造方法
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



【1】默认构造方法

如一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句。

如自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法。

 

【2】构造方法重载（Overload）

构造方法重载，根据不同参数区别不同得构造方法。

```java
/* 测试构造方法重载 */
class Person {
    private String name;
    private int age;
 
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    public Person(String name) {
        this.name = name;
        this.age = 12;
    }
 
    public Person() {
    }
}
```



#### 6 equals与==方法

Object 的 equals 方法默认就是比较两个对象的hashcode，是同一个对象的引用时返回 true 否则返回 false。

==代表比较双方是否相同。如果是基本类型则表示值相等，如果是引用类型则表示地址相等即是同一个对象。

【eclipse把光标放String类，按ctrl键+点击鼠标左键看类源码】

```java
package com.test;
 
/**
 * 测试equals与==方法
 * @author 林
 *
 */
 
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



#### 7 toString方法

Object类中定义有public String toString()方法，其返回值是 String 类型。

Object类中toString方法的源码为：

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

```java
package com.test;
/**
 * 测试toString默认输出值
 * @author 林
 *
 */
public class TestToString {
	public static void main(String[] args) {
		Per p=new Per();
		System.out.println(p);		
	}
}
 
class Per{
	String name;
	int age;
}
```

> com.test.Per@15db9742



```java
package com.test;
/**
 * 测试重写toString方法
 * @author 林
 *
 */
public class TestToString {
	public static void main(String[] args) {
		Per p=new Per();
		p.name="林一";
		p.age=18;
		System.out.println(p);		
	}
}
 
class Per{
	String name;
	int age;
	@Override
	public String toString() {	//重写toString方法
		return name+" "+age;
	}
}
```

>林一 18



### 四、关键字

#### 1 this关键字

在方法内部，可以使用一个隐含的变量`this`，它始终指向当前对象。因此，通过`this.field`就可以访问当前对象的字段。

如果没有命名冲突，可以省略`this`。

```java
class Person {
    private String name;
    public String getName() {
        return name; // 相当于this.name
    }
}
```

如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`。

```java
class Person {
    private String name; 
    public void setName(String name) {
        this.name = name; // this不可少，this.name为成员变量
    }
}
```



#### 2 super关键字

`super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。

```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;	//调用父类属性
    }
}
```

```java
class Student extends Person {
    protected int score;
    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```



#### 3 static关键字

static声明的成员变量为静态属性。

static声明的方法为静态方法

- 静态属性被该类的所有实例共享，在类被载入时被显式初始化。
- 静态属性使用 “类名.类属性” 调用。
- 静态方法不需要对象， 就可以调用(如：类名.方法名)。
- 调用静态方法时， 不会将对象的引用传递给它， 所以在static方法中不可访问非static的成员。
- 静态方法不能以任何方式引用this和super关键字。

```java
/**
 * 测试static关键字
 * @author 林
 *
 */
public class TestStatic {
	int a;
	static int width;	//静态成员变量、类属性
	
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

【1】静态初始化块

- 如果希望加载后， 对整个类进行某些初始化操作， 可以使用static初始化块。
- 类第一次被载入时先执行static代码块； 类多次载入时， static代码块只执行一次； Static经常用来进行static变量的初始化。
- 经常用来进行static变量的初始化。
- 静态初始化块中不能访问非static成员。

```java
/**
 * 测试静态初始化块
 * @author 海
 *
 */
public class TestStaticBlock {
	static { //静态初始化块，类载入时加载
		System.out.println("静态初始化块");
	}
	
	public static void main(String[] args) {
		System.out.println("main方法");
	}
}
```



#### 4 final关键字

final作用：

1. 修饰变量: 被他修饰的变量不可改变。一旦赋了初值，就不能被重新赋值。
2. 修饰方法：该方法不可被子类重写。但是可以被重载。
3. 修饰类: 修饰的类不能被继承。比如：Math、String等。



### 五、包Package

包机制是Java中管理类的重要手段，通过包我们很容易对解决类重名的问题，也可以实现对类的有效管理。

【二个要点】：

- 通常是类的第一句非注释性语句。
- 包名：域名倒着写即可，再加上模块名，便于内部管理类。

```java
package com.test;
 
public class TestPerson {
 
}
```



#### 1 Java常用包

| Java常用包 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| java.lang  | 包含一些Java语言的核心类，如String、Math、Integer、System和Thread，提供常用功能。 |
| java.awt   | 包含了构成抽象窗口工具集（abstract window toolkits）的多个类，这些类被用来构建和管理应用程序的图形用户界面(GUI)。 |
| java.net   | 包含执行与网络相关的操作的类。                               |
| java.io    | 包含能提供多种输入/输出功能的类。                            |
| java.util  | 包含一些实用工具类，如定义系统特性、使用与日期日历相关的函数。 |



#### 2 import导入

【要点】：

- Java会默认导入java.lang包下所有的类，因此这些类我们可以直接使用。
- 如果导入两个同名的类，只能用包名+类名来显示调用相关类。

```java
package com.test;
/**
 * 测试导入同名类的处理
 * @author 海
 *
 */
 
import java.sql.Date;
import java.util.*;
 
public class Test {
	public static void main(String[] args) {
		Date now;
		java.util.Date now2=new java.util.Date();	//java.util.Date和java.sql.Date类同名，需要完整路径
		System.out.println(now2);
		Scanner input=new Scanner(System.in);
	}
}
```



#### 3 静态导入

静态导入：其作用是用于导入指定类的静态属性，这样可以直接使用静态属性。

```java
package com.test;
 
/**
 * 测试静态属性导入的使用
 * @author 海
 *
 */
 
import static java.lang.Math.*;		//导入Math类的所有静态属性
import static java.lang.Math.PI;
 
public class Test {
	public static void main(String[] args) {
		System.out.println(PI);
		System.out.println(random());
	}
}
```



### 六、继承

- Java使用extends关键字来实现继承。
- 如果定义一个类时，没有调用extends，则它的父类是：java.lang.Object。
- 子类继承父类，可得到父类的全部属性和方法 (除了父类的构造方法)，但父类私有的属性和方法不可以直接访问。
- 子类不会继承任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。

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



#### 1 方法重写（Override）

子类通过重写父类的方法，可以用自身的行为替换父类的行为。

方法的重写是实现多态的必要条件。

```java
package com.test;
/**
 * 测试方法重写Override
 * @author 林
 *
 */
 
public class TestOverride {
	public static void main(String[] args) {
		Vehicle v=new Vehicle();
		Vehicle p=new Plane();
		v.run();
		p.run();
	}
}
 
class Vehicle{	//交通工具类
	public void run() {
		System.out.println("交通工具运行");
	}
}
 
class Plane extends Vehicle{
	public void run() {		//重写父类方法
		System.out.println("飞机天上飞");
	}
}
```



#### 2 继承树追溯

构造方法第一句总是：super(…)来调用父类对应的构造方法。

流程就是：先向上追溯到Object，然后再依次向下执行类的初始化块和构造方法，直到当前子类为止。

```java
package com.test;
/**
 * 测试super
 * @author 林
 *
 */
public class TestSuper {
	public static void main(String[] args) {
		System.out.println("创建Class");
		new ChildClass();
	}
}
 
class FatherClass{
	public FatherClass() {
		System.out.println("创建FatherClass");
	}
}
 
class ChildClass extends FatherClass{
	public ChildClass() {
		System.out.println("创建ChildClass");
	}
}
```

>创建Class
>创建FatherClass
>创建ChildClass



### 七、封装

Java中4种“访问控制符”分别为private、default、protected、public，可以修饰属性和方法。

Java封装就是隐藏细节，提供对外API接口。

封装的使用细节：

1. 一般使用private访问权限。
2. 提供相应的get/set方法来访问相关属性，这些方法通常是public修饰的，以提供对属性的赋值与读取操作。(注意：boolean变量的get方法是is开头!)
3. 一些只用于本类的辅助性方法可以用private修饰，希望其他类调用的方法用public修饰

```java
package com.test;
/**
 * 测试封装
 * @author 林
 *
 */
public class TestFZ {
	public static void main(String[] args) {
		Stu s1=new Stu("林一",18);
		System.out.println(s1);
		Stu s2=new Stu("林二",-11);
		System.out.println(s2);
	}
}
 
class Stu{
	private String name;
	private int age;
	private boolean flag;
	
	public Stu(String name,int age) {	//构造方法
		this.name=name;
		setAge(age);
	}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		if(age<0) {		//在赋值之前先判断年龄是否合法
			System.out.println("输入年龄错误。");
		}else {
			this.age = age;
		}
	}
	public boolean isFlag() {
		return flag;
	}
	public void setFlag(boolean flag) {
		this.flag = flag;
	}
	
	@Override
	public String toString() {
		return getName()+" "+getAge();
	}
}
```

>林一 18
>输入年龄错误。
>林二 0



### 八、多态

多态指的是同一个方法调用，由于对象不同可能会有不同的行为。

多态的要点：

1. 多态是方法的多态，不是属性的多态。
2. 多态的存在要有3个必要条件：继承，方法重写，父类引用指向子类对象。
3. 父类引用指向子类对象后，用该父类引用调用子类重写的方法，此时多态就出现了。

多态最为常见的一种用法：即父类引用做方法的形参，实参可以是任意的子类对象，可以通过不同的子类对象实现不同的行为方式。 

```java
package com.test;
/**
 * 测试多态
 * @author 林
 *
 */
public class TestPolym {
	public static void main(String[] args) {
		Animal a1=new Dog();	//父类引用指向子类对象,向上可以自动转型
		Animal a2=new Cat();
		animalCry(a1);	//传的具体是哪一个类就调用哪一个类的方法。
		animalCry(a2);
		Dog d1=(Dog)a1;	//向下需要强制类型转换
		d1.seeDoor();
		
	}
	
	/* 有了多态，只需要让增加的这个类继承Animal类就可以了 */
	static void animalCry(Animal a) {
		a.shout();
	}
}
 
class Animal{
	public void shout() {
		System.out.println("叫了一声");
	}
}
 
class Dog extends Animal{
	public void shout() {	//重写父类方法
		System.out.println("汪汪汪");
	}
	public void seeDoor() {
		System.out.println("看门");
	}
}
 
class Cat extends Animal{
	public void shout() {
		System.out.println("喵喵喵");
	}
}
```

>汪汪汪
>喵喵喵
>看门



#### 1 对象的转换

父类引用指向子类对象，我们称这个过程为向上转型，属于自动类型转换。

 

#### 2 抽象类与方法

使用abstract修饰的方法，没有方法体，只有声明。

通过abstract方法定义规范，然后要求子类必须定义具体实现。

抽象类使用要点：

1. 有抽象方法的类只能定义成抽象类。
2. 抽象类不能实例化，即不能用new来实例化抽象类。
3. 抽象类可以包含属性、方法、构造方法。但是构造方法不能用来new实例，只能用来被子类调用。
4. 抽象类只能用来被继承。
5. 抽象方法必须被子类实现。

```java
package com.test;
/**
 * 测试抽象类与方法
 * @author 林
 *
 */
public class TestAbs {
	public static void main(String[] args) {
		Dog d=new Dog();
		d.shout();
	}
}
 
abstract class Animal{	//抽象类
	abstract public void shout();	//抽象方法
}
 
class Dog extends Animal{
	public void shout() {	//子类必须实现父类的抽象方法，否则编译错误
		System.out.println("汪汪汪");
	}
}
```





### 九、接口

#### 1 定义接口

```java
/* 接口定义语法 */
[访问修饰符]  interface 接口名   [extends  父接口1，父接口2…]  {
    常量定义；  
    方法定义；
}
```

【接口定义参数说明】：

1. 访问修饰符：只能是public或默认。
2. 接口名：和类名采用相同命名机制。
3. extends：接口可以多继承。
4. 常量：接口中的属性只能是常量，总是：public static final 修饰。不写也是。
5. 方法：接口中的方法只能是：public abstract。 省略的话，也是public abstract。

```java
package com.test;
/**
 * 测试定义接口
 * @author 林
 *
 */
public class TestInterface {
	public static void main(String[] args) {
		Volant v=new Angel();
		v.fly();
		Honest h=new GoodMan();
		h.helpOther();
	}
}
 
interface Volant{	//飞行接口
	int FLY_HIGHT=100;	//常量
	void fly();
}
 
interface Honest{	//善良接口
	void helpOther();
}
 
class Angel implements Volant,Honest{
	public void fly() {
		System.out.println("天使，飞起来");
	}
	public void helpOther() {
		System.out.println("做好事");
	}
}
 
class GoodMan implements Honest{
	public void helpOther() {
		System.out.println("做好事");
	}
}
 
class BirdMan implements Volant{
	public void fly() {
		System.out.println("鸟人，正在飞");
	}
}
```



#### 2 接口多继承

接口完全支持多继承。和类的继承类似，子接口扩展某个父接口，将会获得父接口中所定义的一切。

```java
interface A {
    void testa();
}
interface B {
    void testb();
}
/**接口可以多继承：接口C继承接口A和B*/
interface C extends A, B {
    void testc();
}
public class Test implements C {
    public void testc() {
    }
    public void testa() {
    }
    public void testb() {
    }
}
```









