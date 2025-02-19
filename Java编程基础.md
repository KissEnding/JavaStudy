# Java 系统复习指南

## 目录
1. [基础语法](#1-基础语法)  
2. [面向对象编程](#2-面向对象编程)  
3. [异常处理](#3-异常处理)  
4. [集合框架](#4-集合框架)  
5. [IO流](#5-io流)  
6. [多线程](#6-多线程)  
7. [JDK8+新特性](#7-jdk8新特性)  
8. [JVM基础](#8-jvm基础)  
9. [设计模式](#9-设计模式)  
10. [进阶学习路线](#10-进阶学习路线)

---

关于java的数据类型知识，我想请你帮我出几道题，从简单到难，我想通过这些题较全面的掌握数据类型知识点，方便之后的项目开发

## 1. 基础语法

编写 Java 程序时，应注意以下几点：

- **大小写敏感**：Java 是大小写敏感的，这就意味着标识符 Hello 与 hello 是不同的。
- **类名**：对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 **MyFirstJavaClass** 。
- **方法名**：所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
- **源文件名**：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记 Java 是大小写敏感的），文件名的后缀为 **.java**。（如果文件名和类名不相同则会导致编译错误）。
- **主方法入口**：所有的 Java 程序由 **public static void main(String[] args)** 方法开始执行。

### 1.1 数据类型

```java
// 8种基本类型
int num = 10;          // 4字节
double price = 25.5;   // 8字节
char ch = 'A';         // 2字节
boolean flag = true;   // 1位

// 引用类型
String str = "Hello";
```

#### 1. 包装类陷阱

```Java
public class WrapperClass {
    public static void main(String[] args) {
        Integer a = 100;
        Integer b = 100;
        Integer c = 200;
        Integer d = 200;
        
        System.out.println(a == b);     // 输出结果？
        System.out.println(c == d);     // 输出结果？
    }
}
```

在 Java 中，`Integer` 是一个包装类，用于封装原始类型 `int`。当比较两个 `Integer` 对象时，`==` 比较的是它们的引用，而不是它们的值。这是因为 `Integer` 是一个对象，而 `==` 操作符在对象引用上是比较内存地址（即引用是否相同）。

然而，Java 中有一个自动装箱（Autoboxing）机制，会在某些范围内对 `Integer` 对象进行缓存。具体来说，初始化的 `Integer` 对象如果是在范围 `-128` 到 `127` 内的值，那么它们会被缓存起来，不会每次都创建新的对象。因此，在这个范围内创建的两个相同值的 `Integer` 对象实际上引用同一个对象。

如果需要比较值，应该使用 `equals` 方法，而不是 `==`。例如：

```java
System.out.println(a.equals(b)); // 输出 true
System.out.println(c.equals(d)); // 输出 true
```

#### 2. 字符串特性

```java
public class StringTest {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "Hello";
        String s3 = new String("Hello");
        String s4 = s3.intern();
        
        System.out.println(s1 == s2);  // 结果？ : true
        System.out.println(s1 == s3);  // 结果？ : false
        System.out.println(s1 == s4);  // 结果？ : true
    }
}
```

 **`intern()` 方法的作用**

- **检查常量池**：当调用 `intern()` 方法时，JVM 会检查字符串常量池中是否已经存在与当前字符串内容相同的字符串。
- **返回引用**：
	- 如果常量池中已经存在相同内容的字符串，则返回常量池中该字符串的引用。
	- 如果常量池中不存在相同内容的字符串，则将当前字符串对象添加到常量池，并返回其引用

#### 3.大数运算

```java
BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");

// 修复以下代码的错误
BigDecimal sum = a.add(b);
System.out.println(sum.compareTo(new BigDecimal("0.3")) == 0);  // 当前输出false
```



### 1.2 流程控制

```java
// if-else
if (score >= 90) {
    System.out.println("A");
} else if (score >= 60) {
    System.out.println("B");
} else {
    System.out.println("C");
}

// for循环
for (int i = 0; i < 5; i++) {
    System.out.print(i + " ");  // 0 1 2 3 4
}

// switch
switch (day) {
    case 1: System.out.println("Monday"); break;
    default: System.out.println("Unknown");
}
```

### 1.3 Java修饰符

像其他语言一样，Java可以使用修饰符来修饰类中方法和属性。主要有两类修饰符：

- 访问控制修饰符 : **default, public , protected, private**
- 非访问控制修饰符 : final, abstract, static, synchronized

在后面的章节中我们会深入讨论 Java 修饰符。

```java
// 默认包访问权限（default），项目中其他包无法访问这个类
class Person {
    // 默认访问权限字段：只能在当前包内访问
    String name;

    // public 方法：所有类都可以访问
    public void sayHello() {
        System.out.println("Hello, my name is " + name);
    }

    // protected 方法：当前包和子类可以访问
    protected void introduce() {
        System.out.println("This is my protected method.");
    }

    // private 字段：只能在当前类内访问
    private int age;

    // 私有方法：只能在当前类内访问
    private void setAge(int age) {
        this.age = age;
        System.out.println("Age set to: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice"; // 可以访问默认访问权限字段
        person.sayHello();    // 直接调用 public 方法
        person.introduce();   // 可以调用 protected 方法
    }
}
```



### 1.4 Java 数组

### 1.5 Java枚举

例如，我们为果汁店设计一个程序，它将限制果汁为小杯、中杯、大杯。这就意味着它不允许顾客点除了这三种尺寸外的果汁。

```java
class FreshJuice {
   enum FreshJuiceSize{ SMALL, MEDIUM , LARGE }
   FreshJuiceSize size;
}
 
public class FreshJuiceTest {
   public static void main(String[] args){
      FreshJuice juice = new FreshJuice();
      juice.size = FreshJuice.FreshJuiceSize.MEDIUM  ;
   }
}
```

**注意：** 枚举可以单独声明或者声明在类里面。方法、变量、构造函数也可以在枚举中定义。

### 1.6 Java关键字

| 类别                 | 关键字       | 说明                           |
| :------------------- | :----------- | :----------------------------- |
| 访问控制             | private      | 私有的                         |
|                      | protected    | 受保护的                       |
|                      | public       | 公共的                         |
|                      | default      | 默认                           |
| 类、方法和变量修饰符 | abstract     | 声明抽象                       |
|                      | class        | 类                             |
|                      | extends      | 扩充、继承                     |
|                      | final        | 最终值、不可改变的             |
|                      | implements   | 实现（接口）                   |
|                      | interface    | 接口                           |
|                      | native       | 本地、原生方法（非 Java 实现） |
|                      | new          | 创建                           |
|                      | static       | 静态                           |
|                      | strictfp     | 严格浮点、精准浮点             |
|                      | synchronized | 线程、同步                     |
|                      | transient    | 短暂                           |
|                      | volatile     | 易失                           |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |
|                      |              |                                |

## 2. 面向对象编程

### 2.1 类与对象

```java
public class Person {
    // 字段
    private String name;
    private int age;
    
    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 方法
    public void introduce() {
        System.out.println("I'm " + name + ", " + age + " years old");
    }
}

// 使用
Person p = new Person("Tom", 25);
p.introduce();
```

### 2.2 继承与多态

#### 继承：

在 Java 中，一个类可以由其他类派生。如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。

利用继承的方法，可以重用已存在类的方法和属性，而不用重写这些代码。被继承的类称为超类（super class），派生类称为子类（sub class）。

```java
// 父类
class Animal {
    public void sound() {
        System.out.println("Animal makes sound");
    }
}

// 子类
class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Wang Wang!");
    }
}

// 多态
Animal animal = new Dog();
animal.sound();  // 输出 Wang Wang!
```

### 2.3 接口与抽象类

#### 接口：

在 Java 中，接口可理解为对象间相互通信的协议。接口在继承中扮演着很重要的角色。

接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。

- 抽象类是一个部分实现的规范，可以包含抽象方法和具体实现的方法。
- 接口是一个完全抽象的规范，所有方法都是抽象的，没有实现。

```java
// 接口
interface Flyable {
    void fly();
}

// 抽象类
abstract class Bird {
    abstract void eat();
}

class Sparrow extends Bird implements Flyable {
    public void fly() {
        System.out.println("Sparrow flying");
    }
    
    void eat() {
        System.out.println("Eating seeds");
    }
}

```

#### 概括性问题

**问题**： 接口和抽象类作为面向对象编程的关键特性，如何影响大型系统的设计和实现？在设计复杂的 API 时，接口和抽象类如何相互协作以提供灵活性和可维护性？

==**答：**== 

##### *1. 接口和抽象类的作用*

**接口**：

- **定义契约**：接口规定了类必须实现的方法，确保不同类具有一致的行为。
- **解耦**：通过接口，类之间的依赖关系得以解耦，提升系统的灵活性和可扩展性。
- **多态性**：接口支持多态，允许不同类通过同一接口进行交互。

**抽象类**：

- **部分实现**：抽象类可以包含具体方法和抽象方法，提供部分实现，供子类继承和扩展。
- **代码复用**：通过继承抽象类，子类可以复用其代码，减少重复。
- **强制实现**：抽象类可以强制子类实现特定方法，确保一致的行为。

##### *2. 对大型系统设计的影响*

**模块化设计**：

- 接口和抽象类帮助将系统分解为多个模块，每个模块通过接口或抽象类定义其行为，降低模块间的耦合。

**可扩展性**：

- 新功能可以通过实现接口或继承抽象类来添加，无需修改现有代码，提升系统的可扩展性。

**可维护性**：

- 清晰的接口和抽象类定义使代码更易理解和维护，尤其在多人协作或长期维护中。

**测试和模拟**：

- 接口和抽象类便于单元测试，可以通过模拟接口或抽象类来测试依赖它们的代码。

------



## 3. 异常处理

### 3.1 异常体系

```java
Throwable
├── Error (不可恢复，如OutOfMemoryError)
└── Exception
    ├── Checked Exception (必须处理，如IOException)
    └── RuntimeException (未检查异常，如NullPointerException)
```

### 3.2 try-catch-finally

```java
try {
    FileReader file = new FileReader("test.txt");
} catch (FileNotFoundException e) {
    System.out.println("文件未找到");
} finally {
    System.out.println("始终执行的代码");
}
```

**例子**

```java
public class ExceptionHandlingExample {
    public static void main(String[] args) {
        // 调用可能抛出异常的方法
        try {
            System.out.println(divide(10, 0));
        } catch (ArithmeticException e) {
            System.err.println("发生了一个异常: " + e.getMessage());
            e.printStackTrace();
        } finally {
            System.out.println("无论是否发生异常，都会执行 finally 块。");
        }
    }

    // 一个可能抛出异常的方法
    public static int divide(int a, int b) throws ArithmeticException {
        return a / b;
    }
}
```



## 4. 集合框架

### 4.1 常用集合类

```java
List<String> list = new ArrayList<>();  // 有序可重复
list.add("Apple");
list.get(0);

Set<Integer> set = new HashSet<>();     // 唯一元素
set.add(100);

Map<String, Integer> map = new HashMap<>();  // 键值对
map.put("age", 25);
```

| 序号 | 接口描述                                                     |
| :--- | :----------------------------------------------------------- |
| 1    | **Collection 接口** <br />Collection 是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素, Java不提供直接继承自Collection的类，只提供继承于的子接口(如List和set)。Collection 接口存储一组不唯一，无序的对象。 |
| 2    | **List 接口** <br />List接口是一个有序的 Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引(元素在List中位置，类似于数组的下标)来访问List中的元素，第一个元素的索引为 0，而且允许有相同的元素。List 接口存储一组不唯一，有序（插入顺序）的对象。 |
| 3    | **Set**<br /> Set 具有与 Collection 完全一样的接口，只是行为上不同，Set 不保存重复的元素。Set 接口存储一组唯一，无序的对象。 |
| 4    | **SortedSet** <br />继承于Set保存有序的集合。                |
| 5    | **Map**<br /> Map 接口存储一组键值对象，提供key（键）到value（值）的映射。 |
| 6    | **Map.Entry** <br />描述在一个Map中的一个元素（键/值对）。是一个 Map 的内部接口。 |
| 7    | **SortedMap**<br /> 继承于 Map，使 Key 保持在升序排列。      |
| 8    | **Enumeration** <br />这是一个传统的接口和定义的方法，通过它可以枚举（一次获得一个）对象集合中的元素。这个传统接口已被迭代器取代。 |

#### ArrayList

ArrayList 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```java
import java.util.ArrayList; // 引入 ArrayList 类

ArrayList<E> objectName =new ArrayList<>();　 // 初始化

```

- **E**: 泛型数据类型，用于设置 objectName 的数据类型，**只能为引用数据类型**。
- **objectName**: 对象名。

1. #### 添加元素

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        System.out.println(sites);
	    }
	}
	```

	

2. #### 访问元素

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        System.out.println(sites.get(1));  // 访问第二个元素
	    }
	}
	```

	

3. #### 修改元素

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        sites.set(2, "Wiki"); // 第一个参数为索引位置，第二个为要修改的值
	        System.out.println(sites);
	    }
	}
	```

	

4. #### 删除元素

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        sites.remove(3); // 删除第四个元素
	        System.out.println(sites);
	    }
	}
	```

	

5. #### 计算大小

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        System.out.println(sites.size());
	    }
	}
	```

	

6. #### 迭代数组列表

	```java
	import java.util.ArrayList;
	
	public class RunoobTest {
	    public static void main(String[] args) {
	        ArrayList<String> sites = new ArrayList<String>();
	        sites.add("Google");
	        sites.add("Runoob");
	        sites.add("Taobao");
	        sites.add("Weibo");
	        for (int i = 0; i < sites.size(); i++) {
	            System.out.println(sites.get(i));
	        }
	    }
	}
	```

	

### 4.2 迭代器

```java
Iterator<String> it = list.iterator();
while(it.hasNext()) {
    System.out.println(it.next());
}
```

```java
package com.other;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorExample {
    public static void main(String[] args) {
        // 创建一个集合
        List<String> list = new ArrayList<>();
        list.add("你好!");
        list.add("欢迎");
        list.add("学习迭代器");

        // 获取迭代器
        Iterator<String> iterator = list.iterator();

        // 使用迭代器遍历集合
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }

        // 自定义类实现Iterable接口
        MyCollection<String> myCollection = new MyCollection<>();
        myCollection.addElement("元素1");
        myCollection.addElement("元素2");

        // 使用增强 for 循环 (背后调用迭代器)
        for (String element : myCollection) {
            System.out.println("自定义集合元素: " + element);
        }
    }
}

// 自定义集合，实现 Iterable 接口
class MyCollection<T> implements Iterable<T> {
    private List<T> elements = new ArrayList<>();

    public void addElement(T element) {
        elements.add(element);
    }

    @Override
    public Iterator<T> iterator() {
        // 返回自定义的迭代器
        return new MyIterator<>();
    }

    // 自定义迭代器
    private class MyIterator<T> implements Iterator<T> {
        private int currentIndex = 0;

        @Override
        public boolean hasNext() {
            return currentIndex < elements.size();
        }

        @Override
        public T next() {
            return (T) elements.get(currentIndex++);
        }
    }
}

```



## 5. IO流

### 5.1 文件读写

```java
// 字节流
try (FileInputStream fis = new FileInputStream("input.txt");
     FileOutputStream fos = new FileOutputStream("output.txt")) {
    int data;
    while ((data = fis.read()) != -1) {
        fos.write(data);
    }
}

// 字符流
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
String line;
while ((line = br.readLine()) != null) {
    System.out.println(line);
}
```

## 6. 多线程

### 6.1 线程创建

```java
// 方式1：继承Thread类
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running");
    }
}

// 方式2：实现Runnable接口
new Thread(() -> {
    System.out.println("Lambda线程");
}).start();
```

### 6.2 同步机制

```java
// synchronized示例
class Counter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
}
```

## 7. 设计模式

### 单例模式

```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## 练手项目

### 练手项目建议

- 电商系统（订单、支付模块）
- 分布式文件存储系统
- 高并发秒杀系统