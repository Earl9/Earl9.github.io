---
title: "ArrayList经典面试题"
date: 2021-12-31T00:06:54+08:00
draft: false
slug: ad0a121e
description: "ArrayList源码分析"
tags: [Java]
categories: [Java]
weight: 9
---

## 【 ArrayList集合 】

## 关于集合的常见面试题及源码分析

### 1. 什么是集合?

* **集合**：集合是java中提供的一种容器，可以把多个对象（实际上是对象的引用，但习惯上都称对象）“丢进”该容器中。大致可以分为Set、List、Queue和Map四种体系，其中Set代表无序、不可重复的集合；List代表有序、可重复的集合；而Map则代表具有映射关系的集合，Java 5 又增加了Queue体系集合，代表一种队列集合实现。


### 2. 集合类之间的继承关系?

集合类主要由两个接口派生而出，分别是单列集合Collection和双列集合Map，它们是Java集合框架的根接口。

* **Collection**：单列集合类的根接口，用于存储一系列符合某种规则的元素，它有两个重要的子接口，分别是`List`和`Set`。其中，`List`的特点是元素有序、元素可重复。`Set`的特点是元素无序，而且不可重复。`List`接口的主要实现类有`ArrayList`和`LinkedList`，`Set`接口的主要实现类有`HashSet`和`TreeSet`。

从上面的描述可以看出JDK中提供了丰富的集合类库，为了便于系统地学习，接下来通过一张图来描述集合类的继承体系。

![](https://gitee.com/earl9/img/raw/master/img/2021/12/311231000314.webp)

图中，ArrayList,HashSet,LinkedList,TreeSet是我们经常会有用到的已实现的集合类。


Map实现类用于保存具有映射关系的数据。Map保存的每项数据都是key-value对，也就是由key和value两个值组成。Map里的key是不可重复的。


![Map](https://gitee.com/earl9/img/raw/master/img/2021/12/311231000038.jpeg)

图中，HashMap，TreeMap是我们经常会用到的集合类。


### 3. 集合和数组的区别?

​		1:长度区别
​            数组固定
​            集合可变
​        2:内容区别
​            数组可以是基本类型，也可以是引用类型
​            集合只能是引用类型
​        3:类型区别
​            数组只能存储同一种类型
​            集合可以存储不同类型

**如果存储的数据长度经常发生改变，推荐使用集合**


### 4. ArrayList扩容原理？

###### 4.1.步骤

​     1、扩容

​                 把原来的数组复制到另一个内存空间更大的数组中

​      2、添加元素

​                 把新元素添加到扩容以后的数组中

###### 4.2.源码(JKD1.8)

先把ArrayList中定义的一些属性粘出来方便下面源码分析

`java`

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    /**
     * 默认初始容量
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * 空数组实例
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     *空数组实例，用于默认大小的空实例
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    /**
	 *如果新建ArrayList对象时没有指定大小，那么会将
	 *DEFAULTCAPACITY_EMPTY_ELEMENTDATA赋值给elementData，
	 *并在第一次添加元素时，将列表容量设置为DEFAULT_CAPACITY
     */
    transient Object[] elementData;
    
    /**
     * 列表大小，elementData中存储的元素个数
     */
    private int size;
}
```

**补充**：

通过上述继承关系我们发现一个很奇怪的现象，就是 ArrayList已经继承了AbstractList而 AbstractList类实现了List接口，那为什么 ArrayList还要在实现 List接口呢？同样在 HashMap中 LinkedList 中都是这种结构。

据 Java 集合框架的创始人 Josh Bloch 描述，这样的写法是一个失误。在 Java 集合框架中，类似这样的写法很多，最开始写 Java 集合框架的时候，他认为这样写，在某些地方可能是有价值的，直到他意识到错了。显然，jdk 的维护者，后来不认为这个小小的失误值得去修改，所以就这样保留下来了。



**transient** ? ? ? 为 Java 关键字，为变量修饰符，如果用 transient 声明一个实例变量，当对象存储时，它的值不需要维持。

首先介绍一下序列化**Serializable**

通常一个类实现序列化方式是实现序列化接口：  class XXX implements Serializable

序列化的作用：把数据长久的保存在磁盘中，磁盘和内存是不同的，内存一般在程序运行时占用，数据保存周期短，随程序结束而结束，磁盘可以长久保存数据

transient关键字的作用，在已实现序列化的类中，有的变量不需要保存在磁盘中，就要transient关键字修饰，如银行卡密码等，就这个作用------在已序列化的类中使变量不序列化



这里 Object[] elementData，就是我们的 ArrayList 容器，下面介绍的基本操作都是基于该 elementData 变量来进行操作的。



接下来看一下ArrayList的两个常用构造方法：

`java`

```java
//带初始容量参数的构造函数。（用户自己指定容量）
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {//初始容量大于0
    
        //创建initialCapacity大小的数组
        this.elementData = new Object[initialCapacity];
        
    } else if (initialCapacity == 0) {//初始容量等于0
    
        //创建空数组
        this.elementData = EMPTY_ELEMENTDATA;
        
    } else {//初始容量小于0，抛出异常
    
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}

//默认构造函数，构造一个空列表(无参数构造)
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

在无参构造中来创建对象的时候,其实就是创建了一个空数组，集合长度为0,没有分配容量,当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10。之后扩容会按照1.5倍增长。

在有参构造中，传入的参数是正整数就按照传入的参数来确定创建数组的大小，否则异常。



接下来**重点**，看**扩容**，扩容的方法就是 **add(E e)**

```java
public boolean add(E e) {
	ensureCapacityInternal(size + 1);  // list的size+1
	elementData[size++] = e; // 将数据放到数组最后一个
	return true;
}
```

通过源码可以发现，其实add方法就两步，第一步：增加长度，第二步：添加元素到数组，第二步没什么说的，我们看看ensureCapacityInternal(int minCapacity)这个增加长度的方法。

![扩容源码](https://gitee.com/earl9/img/raw/master/img/2021/12/311231000049.jpeg)

这个地方可以看到，如果在添加的时候数组是空的，就直接给一个默认10的长度，否则的话就加1

```java
 if (minCapacity - elementData.length > 0)
            grow(minCapacity);
```

通过上图这个判断才是真正的增加长度，当需要的长度大于原来数组长度的时候就需要扩容了，相反的则不需要扩容。



**grow（）方法**

`java`

```java
//要分配的最大数组大小
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

private void grow(int minCapacity) {
    // oldCapacity为旧容量，newCapacity为新容量
    int oldCapacity = elementData.length;
    
    //将oldCapacity 右移一位，其效果相当于oldCapacity /2(如果oldCapacity是奇数,先减1再除以2)
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    
    //然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
        
    // 如果新容量大于 MAX_ARRAY_SIZE,进入(执行) `hugeCapacity()` 方法来比较 minCapacity 和 MAX_ARRAY_SIZE
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
   
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

**我们来仔细分析一下：**

- **当我们要 add 第1个元素到 ArrayList 时，elementData.length 为0 （因为还是一个空的 list），因为执行了 ensureCapacityInternal() 方法 ，所以 minCapacity 此时为10。此时，minCapacity - elementData.length > 0 成立，所以会进入 grow(minCapacity) 方法。**
- **当add第2个元素时，minCapacity 为2，此时elementData.length(容量)在添加第一个元素后扩容成 10 了。此时，minCapacity - elementData.length > 0 不成立，所以不会进入 （执行）grow(minCapacity) 方法。**
- **添加第3、4···到第10个元素时，依然不会执行grow方法，数组容量都为10。**

　　**直到添加第11个元素，minCapacity(为11)比elementData.length（为10）要大。进入grow方法进行扩容。**

​	    **再次添加第23个元素，minCapacity(为23)比elementData.length（为10）要大。进入grow方法进行扩容。**

​		**......**



**以上的一切都是ArrayList扩容的第一步，第二步就没啥说的了，就是把需要添加的元素添加到数组的最后一位。**



```java
总结：在arraylist中增加一个对象的时候，Java会去检查arraylist，以确保已存在的数组中有足够的容量来存储这个新的对象。如果没有足够容量的话，那么就会新建一个长度更长的数组，旧的数组就会使用Arrays.copyOf()方法被复制到新的数组中去，现有的数组引用指向了新的数组。

小疑问 ? ? 为什么每次扩容处理会是 1.5 倍，而不是 2.5、3、4 倍呢？源代码定义1.5倍,是经过了大量的实验，发现 1.5 倍的扩容是最好的倍数。因为一次性扩容太大(例如 2.5 倍)可能会浪费更多的内存(1.5 倍最多浪费 33%，而 2.5 被最多会浪费 60%，3.5 倍则会浪费 71%……)。但是一次性扩容太小，需要多次对数组重新分配内存，对性能消耗比较严重。所以 1.5 倍刚刚好，既能满足性能需求，也不会造成很大的内存消耗。
```



### 5. 什么是线程不安全和线程安全？

​		线程安全就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。线程不安全就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据。

### 6. 为什么说ArrayList是线程不安全的？

##### 6.1  代码复现

`java`

```java
	private static List<Integer> list = new ArrayList<Integer>();
    private static ExecutorService executorService = Executors.newFixedThreadPool(500); //定长线程池500

    private static class IncreaseTask extends Thread{
        @Override
        public void run() {
            System.out.println("ThreadId:" + Thread.currentThread().getId() + " start!");
            for(int i =0; i < 100; i++){
                list.add(i);
            }
            System.out.println("ThreadId:" + Thread.currentThread().getId() + " finished!");
        }
    }

    public static void main(String[] args){
        //返回当前时间
        long start = System.currentTimeMillis();

        for(int i=0; i < 500; i++){ //开启500个线程
            executorService.submit(new IncreaseTask());
        }
        //停止接收新任务，原来的任务继续执行
        executorService.shutdown();

        while (!executorService.isTerminated()){//所有提交的任务没完成
            try {
                Thread.sleep(500*10);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }
        System.out.println("list 长度为: " + list.size());
        long end = System.currentTimeMillis();
        System.out.println("用时:"+(double)(end-start)/1000+"秒");
    }


```

打印结果应为：50000，实际测试部分结果如下：

list 长度为: 49927

list 长度为: 49901

由此可见是ArrayList做add操作时候，会丢失一些数据，所以所ArrayList是线程不安全的。

##### 6.2  源码分析(JKD1.8)

`java`

```java
// Object[] elementData:ArrayList的数据结构是数组类型，list存放的数据就是存放在elementData里面的
// 第1步
public boolean add(E e) {
	ensureCapacityInternal(size + 1);  // list的size+1
	elementData[size++] = e; // 将数据放到数组最后一个
	return true;
}
 
 
// 第2步，判断如果将当前的新元素加到列表后面，列表的elementData数组的大小是否满足，
// 如果size + 1的这个需求长度大于了elementData这个数组的长度，那么就要对这个数组进行扩容
private void ensureCapacityInternal(int minCapacity) {
	if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
		minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
	}
 
	// 进入ensureExplicitCapacity方法
	ensureExplicitCapacity(minCapacity);
}
 
 
// 第3步，元素有变化，那么就调用grow方法
private void ensureExplicitCapacity(int minCapacity) {
	modCount++;
	// elementData：list的数组元素
	// minCapacity: add操作后的容量
	if (minCapacity - elementData.length > 0) 
		grow(minCapacity);
}
 
 
// 第4步
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8; // 为什么要-8，是因为有些虚拟机有一些hear的key
private void grow(int minCapacity) {
 
	// 原始list的容量（容量不是list.size）
	int oldCapacity = elementData.length; 
	
	//现在list的容量，此时是做讲原始容量扩大0.5倍，oldCapacity >> 1：2进制右位移，就是除以2的意思
	int newCapacity = oldCapacity + (oldCapacity >> 1);
	if (newCapacity - minCapacity < 0)
		newCapacity = minCapacity;
	// 一般不会进入hugeCapacity这个方法，
	if (newCapacity - MAX_ARRAY_SIZE > 0)
		newCapacity = hugeCapacity(minCapacity);
		
	// 复制elementData返回一个新的数组对象，这个时候list.add完成
	elementData = Arrays.copyOf(elementData, newCapacity);
}
```

由此看到List集合做add（）时，第1步到第3步，都不会改变elementData对象，只有在第4步Arrays.copyOf的时候，返回一个新的数组对象。
因此：当有线程A、B同时进入grow方法，两个线程都会执行Arrays.copyOf()方法，返回2个不同的elementData对象，
假如，A先返回，B后返回，那么List.elementData == A.elementData，
然后B也返回后，这时List.elementData == B.elementData
这时，B.elementData就把A.elementData数据给覆盖了。导致A.elementData被丢失

**这样就出现了导致线程不安全的隐患，在多个线程进行add操作时可能会抛出并发读写异常和数据丢失，覆盖等问题。**

### 7.  如何避免ArrayList的并发问题?

###### 1.使用Collections.synchronizedList()方法对ArrayList对象进行包装

```java
ArrayList<Integer> arraylist = Collections.synchronizedList(new ArrayList());
```

进行测试，结果正确，没有发现数据丢失问题。

为什么synchronizedList（）方法可以解决并发问题？直接上源码(JKD1.8)

`java`

```java
public static <T> List<T> synchronizedList(List<T> list) {
    return (list instanceof RandomAccess ?
            new SynchronizedRandomAccessList<>(list) :
            new SynchronizedList<>(list));
}
SynchronizedList(List<E> list) {
	    super(list);
	    this.list = list;
	}
	SynchronizedList(List<E> list, Object mutex) {
            super(list, mutex);
	    this.list = list;
        }
 
	public boolean equals(Object o) {
	    synchronized(mutex) {return list.equals(o);}
        }
	public int hashCode() {
	    synchronized(mutex) {return list.hashCode();}
        }
 
	public E get(int index) {
	    synchronized(mutex) {return list.get(index);}
        }
	public E set(int index, E element) {
	    synchronized(mutex) {return list.set(index, element);}
        }
	public void add(int index, E element) {
	    synchronized(mutex) {list.add(index, element);}
        }
	public E remove(int index) {
	    synchronized(mutex) {return list.remove(index);}
        }
 
	public int indexOf(Object o) {
	    synchronized(mutex) {return list.indexOf(o);}
        }
	public int lastIndexOf(Object o) {
	    synchronized(mutex) {return list.lastIndexOf(o);}
        }
 
	public boolean addAll(int index, Collection<? extends E> c) {
	    synchronized(mutex) {return list.addAll(index, c);}
        }
 
	public ListIterator<E> listIterator() {
	    return list.listIterator(); // Must be manually synched by user
        }
 
	public ListIterator<E> listIterator(int index) {
	    return list.listIterator(index); // Must be manually synched by user
        }
 
	public List<E> subList(int fromIndex, int toIndex) {
	    synchronized(mutex) {
                return new SynchronizedList<E>(list.subList(fromIndex, toIndex),
                                            mutex);
            }
        }

```

关于mutex的定义：


```java
final Collection<E> c;  // Backing Collection
final Object mutex;     // Object on which to synchronize
 
SynchronizedCollection(Collection<E> c) {
        if (c==null)
            throw new NullPointerException();
    this.c = c;
        mutex = this;
    }
SynchronizedCollection(Collection<E> c, Object mutex) {
    this.c = c;
        this.mutex = mutex;
    }
```

从源码中我们可以看到SynchronizedList是通过对mutex做同步来控制线程安全的，而mutex定义在其父类SynchronizedCollection中。mutex指向的就是当前对象自己，所以SynchronizedList是线程安全的根本原因是使用Synchronized对SynchronizedList的add,delete等操作进行加锁，但是这种锁的力度很大，效率比较低。

###### 2.使用并发容器CopyOnWriteArrayList

CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<Integer>();
源码(JKD1.8)：

```java
private static final long serialVersionUID = 8673264195747942595L;
 
transient final ReentrantLock lock = new ReentrantLock();
 
private volatile transient Object[] array;
 
final Object[] getArray() {
    return array;
}
 
final void setArray(Object[] a) {
    array = a;
}

public boolean add(E e) {
	final ReentrantLock lock = this.lock;
	lock.lock();
	try {
	    Object[] elements = getArray();
	    int len = elements.length;
	    Object[] newElements = Arrays.copyOf(elements, len + 1);
	    newElements[len] = e;
	    setArray(newElements);
	    return true;
	} finally {
	    lock.unlock();
	}
}
```


从add方法知道：CopyOnWriteArrayList底层数组的扩容方式是一个一个地增加，而且每次把原来的元素通过Arrays.copy()方法copy到新数组中，然后在尾部加上新元素e.它的底层并发安全的保证是通过ReentrantLock进行保证的，CopyOnWriteArrayList和SynchronizedList的底层实现方式是不一样的，前者是通过Lock机制进行加锁，而后者是通过Synchronized进行加锁。





