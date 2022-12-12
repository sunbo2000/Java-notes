##  深拷贝,浅拷贝,引用拷贝(赋值)

关于深拷贝和浅拷贝区别，我这里先给结论：

- **浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
- **深拷贝** ：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。

上面的结论没有完全理解的话也没关系，我们来看一个具体的案例！

**浅拷贝**

浅拷贝的示例代码如下，我们这里实现了 `Cloneable` 接口，并重写了 `clone()` 方法。

`clone()` 方法的实现很简单，直接调用的是父类 `Object` 的 `clone()` 方法。



```java
public class Address implements Cloneable{
    private String name;
    // 省略构造函数、Getter&Setter方法
    @Override
    public Address clone() {
        try {
            return (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

public class Person implements Cloneable {
    private Address address;
    // 省略构造函数、Getter&Setter方法
    @Override
    public Person clone() {
        try {
            Person person = (Person) super.clone();
            return person;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```



测试 ：

```java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// true
System.out.println(person1.getAddress() == person1Copy.getAddress());
```

从输出结构就可以看出， `person1` 的克隆对象和 `person1` 使用的仍然是同一个 `Address` 对象。

**深拷贝**

这里我们简单对 `Person` 类的 `clone()` 方法进行修改，连带着要把 `Person` 对象内部的 `Address` 对象一起复制。



```java
@Override
public Person clone() {
    try {
        Person person = (Person) super.clone();
        person.setAddress(person.getAddress().clone());
        return person;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

测试 ：



```java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// false
System.out.println(person1.getAddress() == person1Copy.getAddress());
```



从输出结构就可以看出，虽然 `person1` 的克隆对象和 `person1` 包含的 `Address` 对象已经是不同的了。

**那什么是引用拷贝呢？** 

简单来说，引用拷贝就是两个不同的引用指向同一个对象。

对象的引用拷贝(直接赋值)是将 A 在栈中的引用地址赋值给了 b，当 b 修改实例数据的时候是修改堆当中的数据，所以当 b 修改完后我们再查看 a 的数据时是被修改后的数据。

我专门画了一张图来描述浅拷贝、深拷贝、引用拷贝：

![浅拷贝、深拷贝、引用拷贝示意图](D:\A.学习资料\A.笔记\1. JVM\assets\shallow&deep-copy.png)

## 实现深拷贝

浅拷贝使用默认的 clone() 方法来实现,不用重写

```java
public class Demo2 implements Cloneable, Serializable {
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```



### 深拷贝实现方法1: 重写 clone() 方法

```java
public class Demo2 implements Cloneable, Serializable {

    private int a;
    private String str;
    private Date date;
    private int[] ints;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        // 完成对基本数据类型和 String 的拷贝
        Demo2 demo2 = (Demo2) super.clone();
        // 对引用类型的拷贝
        demo2.setDate((Date) date.clone());
        demo2.setInts(ints.clone());
        return super.clone();
    }
}

```

### 深拷贝实现方法2:通过对象序列化实现(推荐)

虽然层次调用clone方法可以实现深拷贝，但是显然代码量实在太大。特别对于属性数量比较多、层次比较深的类而言，每个类都要重写clone方法太过繁琐。

将对象 **`序列化`** 为字节序列后，默认会将该对象的整个对象图进行序列化，再通过 **`反序列`** 即可完美地实现深拷贝。