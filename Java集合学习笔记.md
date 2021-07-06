# Java集合

**写在前面**

**学习地址：**

**[[韩顺平讲Java]Java集合专题 ](https://www.bilibili.com/video/BV1YA411T76k)**

**感谢韩老师的讲解视频，十分感谢！！！**

**先上思维导图**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/mind.jpg)

# 1. 集合的种类

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-13-01.png)

![Snipaste_2021-05-24_22-13-06](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-13-06.png)

- 集合主要是两组(单列集合 , 双列集合)
- Collection 接口有两个重要的子接口 List Set , 他们的实现子类都是单列集合 
- Map 接口的实现子类 是双列集合，存放的 K-V

```java
package com.zhuang.collection;

import java.util.ArrayList;
import java.util.HashMap;

/**
 * @Classname Collection
 * @Description 认识集合
 * @Date 2021/5/24 20:59
 * @Created by dell
 */

public class Collection {
    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        arrayList.add("jack");
        arrayList.add("tom");
        HashMap hashMap = new HashMap();
        hashMap.put("NO1", "北京");
        hashMap.put("NO2", "上海");

        System.out.println(arrayList.toString());
        System.out.println(hashMap.toString());
    }
}
```

# 2. 集合常用方法

这里讲解常用的方法 具体的方法可以参考Java API

- add()
- remove()
- contains()
- size()
- isEmpty()
- addAll()
- containsAll()
- removeAll()

```java
package com.zhuang.collection;
import java.util.ArrayList;
import java.util.List;

/**
 * @Classname CollectionMethod
 * @Description 集合常用的方法
 * @Date 2021/5/24 21:01
 * @Created by dell
 */

public class CollectionMethod {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        List list = new ArrayList();
        // add:添加单个元素
        list.add("jack");
        list.add(10);//list.add(new Integer(10))
        list.add(true);
        System.out.println("list=" + list);
        // remove:删除指定元素
        //list.remove(0);//删除第一个元素
        list.remove(true);//指定删除某个元素
        System.out.println("list=" + list);
        // contains:查找元素是否存在
        System.out.println(list.contains("jack"));//T
        // size:获取元素个数
        System.out.println(list.size());//2
        // isEmpty:判断是否为空
        System.out.println(list.isEmpty());//F
        // clear:清空
        list.clear();
        System.out.println("list=" + list);
        // addAll:添加多个元素
        ArrayList list2 = new ArrayList();
        list2.add("红楼梦");
        list2.add("三国演义");
        list.addAll(list2);
        System.out.println("list=" + list);
        // containsAll:查找多个元素是否都存在
        System.out.println(list.containsAll(list2));//T
        // removeAll：删除多个元素
        list.add("聊斋");
        list.removeAll(list2);
        System.out.println("list=" + list);//[聊斋]
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-17-45.png)

# 3. 迭代器的使用

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-18-24.png)

![Snipaste_2021-05-24_22-18-42](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-18-42.png)

常用方法

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-19-31.png)

```java
package com.zhuang.collection;

import java.awt.print.Book;
import java.util.ArrayList;
import java.util.Iterator;

/**
 * @Classname CollectionIterator
 * @Description 迭代器的使用
 * @Date 2021/5/24 21:06
 * @Created by dell
 */

public class CollectionIterator {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.add(new Book("三国演义", "罗贯中", 10.1));
        list.add(new Book("小李飞刀", "古龙", 5.1));
        list.add(new Book("红楼梦", "曹雪芹", 34.6));

        Iterator iterator = list.iterator();

        while (iterator.hasNext()) {
            Object obj =  iterator.next();
            System.out.println("obj="+obj);
        }
                iterator = list.iterator();//注意 第二次遍历 需要重新获取iteartor
        System.out.println("===第二次遍历===");
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj=" + obj);
        }
    }

    static class Book{
        private String name;
        private String author;
        private double price;

        public Book(String name, String author, double price) {
            this.name = name;
            this.author = author;
            this.price = price;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getAuthor() {
            return author;
        }

        public void setAuthor(String author) {
            this.author = author;
        }

        public double getPrice() {
            return price;
        }

        public void setPrice(double price) {
            this.price = price;
        }

        @Override
        public String toString() {
            return "Book{" +
                    "name='" + name + '\'' +
                    ", author='" + author + '\'' +
                    ", price=" + price +
                    '}';
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-21-58.png)

**增强for循环遍历**

```java
package com.zhuang.collection;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * @Classname CollectionExercise
 * @Description 集合练习
 * @Date 2021/5/24 21:22
 * @Created by dell
 */

public class CollectionExercise {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add(new Dog("小黑", 3));
        list.add(new Dog("大黄", 100));
        list.add(new Dog("大壮", 8));
        System.out.println("增强for遍历");
        for (Object item : list) {
            System.out.println(item);
        }

        System.out.println("迭代器遍历");
        Iterator iterator = list.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println(next);
        }

    }
    static class Dog {
        private String name;
        private int age;

        public Dog(String name, int age) {
            this.name = name;
            this.age = age;
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
            this.age = age;
        }

        @Override
        public String toString() {
            return "Dog{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-25-18.png)

# 4. List的常用方法

- add 方法 往集合末尾添加一个元素
- void add(int index, Object ele):在 index 位置插入 ele 元素
- boolean addAll(int index, Collection eles):从 index 位置开始将 eles 中的所有元素添加进来
- Object get(int index):获取指定 index 位置的元素
- int indexOf(Object obj):返回 obj 在集合中首次出现的位置
- int lastIndexOf(Object obj):返回 obj 在当前集合中末次出现的位置
- Object remove(int index):移除指定 index 位置的元素，并返回此元素
- Object set(int index, Object ele):设置指定 index 位置的元素为 ele , 相当于是替换

```java
package com.zhuang.collection;

import java.util.ArrayList;

/**
 * @Classname ListMethod
 * @Description List集合常用方法
 * @Date 2021/5/24 21:28
 * @Created by dell
 */

public class ListMethod {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        //add 方法 往集合末尾添加一个元素
        list.add("刘备");
        list.add("曹操");
        // void add(int index, Object ele):在 index 位置插入 ele 元素
        list.add(1, "康小庄");
        System.out.println(list.toString());

    //boolean addAll(int index, Collection eles):从 index 位置开始将 eles 中的所有元素添加进来
        ArrayList list2 = new ArrayList();
        list2.add("诸葛亮");
        list2.add("关羽");
        list.addAll(list2);
        System.out.println(list.toString());

        // Object get(int index):获取指定 index 位置的元素
        // int indexOf(Object obj):返回 obj 在集合中首次出现的位置
        System.out.println(list.indexOf("曹操"));//2
// int lastIndexOf(Object obj):返回 obj 在当前集合中末次出现的位置
        list.add("吴用");
        System.out.println("list=" + list);
        System.out.println(list.lastIndexOf("吴用"));
// Object remove(int index):移除指定 index 位置的元素，并返回此元素
        list.remove(0);
        System.out.println("list=" + list);
// Object set(int index, Object ele):设置指定 index 位置的元素为 ele , 相当于是替换. list.set(1, "玛丽");
        System.out.println("list=" + list);
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-26-49.png)

## 4.1 List三种遍历方式

```java
package com.zhuang.collection;

import java.util.ArrayList;
import java.util.Iterator;

/**
 * @Classname ListFor
 * @Description List集合三种遍历方式
 * @Date 2021/5/24 21:36
 * @Created by dell
 */

public class ListFor {
    public static void main(String[] args) {
        ArrayList list = new ArrayList<>();
        list.add("三国演义");
        list.add("红楼梦");
        list.add("水浒传");
        list.add("西游记");

        System.out.println("===迭代器===");
        Iterator iterator = list.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println(next.toString());
        }

        System.out.println("===增强for===");
        for (Object o : list) {
            System.out.println(o.toString());
        }
        System.out.println("===普通for循环===");
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i).toString());
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-40-34.png)

 

**练习 遍历按照书的价格排序 使用冒泡排序**

```java
package com.zhuang.collection;

import java.util.ArrayList;
import java.util.LinkedList;

/**
 * @Classname ListExercise02
 * @Description List集合练习
 * @Date 2021/5/24 21:41
 * @Created by dell
 */

public class ListExercise02 {
    public static void main(String[] args) {
        //List list = new ArrayList();
        ArrayList list = new ArrayList();

        //List list = new Vector();
        list.add(new CollectionIterator.Book("红楼梦", "曹雪芹", 100));
        list.add(new CollectionIterator.Book("西游记", "吴承恩", 10));
        list.add(new CollectionIterator.Book("水浒传", "施耐庵", 19));
        list.add(new CollectionIterator.Book("三国", "罗贯中", 80));
        //如何对集合进行排序
        //遍历
        System.out.println("====排序前====");
        for (Object o : list) {
            System.out.println(o);
        }
        System.out.println("====排序后====");
        sort(list);
        for (Object o : list) {
            System.out.println(o);
        }
    }

    //静态方法
    //价格要求是从小到大  冒泡排序
    public static void sort(ArrayList list) {
        int listSize = list.size();
        for (int i = 0; i < listSize - 1; i++) {
            for (int j = 0; j < listSize - 1 - i; j++) {
                //取出对象 Book
                CollectionIterator.Book book1 = (CollectionIterator.Book) list.get(j);
                CollectionIterator.Book book2 = (CollectionIterator.Book) list.get(j + 1);
                if (book1.getPrice() > book2.getPrice()) {//交换
                    list.set(j, book2);
                    list.set(j + 1, book1);
                }
            }
        }
    }
}
```

# 5. ArrayList底层源码和结构

## 5.1 ArrayList的注意事项

- ArrayList可以加入多个元素 可以添加`null`
- ArrayList是有数组来实现数据存储
- ArrayList基本等同于`Vector` 除了**ArrayList是线程不安全！**

## 5.2 ArrayList的底层机制源码分析

- ArrayList中维护了一个Object类型的数组`elementData`
- 当创建ArrayList对象
  - 使用无参构造，初始`elementData`容量为0，第一次添加，扩容的`elementData`为10，**如果需要再次扩容，扩容的`elementData`为1.5倍**
  - 使用指定大小的构造器，初始化`elementData`为指定大小，**如果需要扩容，直接扩容`elementData`的1.5倍**

```java
package com.zhuang.collection;

import java.util.ArrayList;

/**
 * @Classname ArrayListSource
 * @Description ArrayList源码分析
 * @Date 2021/5/24 21:47
 * @Created by dell
 */

public class ArrayListSource {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        //使用无参构造器创建 ArrayList 对象
        ArrayList list = new ArrayList();
        //ArrayList list = new ArrayList(8);
        //使用 for 给 list 集合添加 1-10 数据
        for (int i = 1; i <= 10; i++) {
            list.add(i);
        }
        //使用 for 给 list 集合添加 11-15 数据
        for (int i = 11; i <= 15; i++) {
            list.add(i);
        }
        list.add(100);
        list.add(200);
        list.add(null);
    }
}
```

**debug一把 断点位置**

**进入方法依次看到下列重要的方法**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-54-19.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-11-34.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-54-56.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_21-56-54.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-00-20.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-00-51.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-02-53.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-05-35.png)

**如果指定参数大小的容量**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-24_22-09-32.png)



# 6. Vector集合

## 6.1 Vector集合的基本介绍

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-03-03.png)

写个示例

```java
package com.zhuang.collection;

import java.util.Vector;

/**
 * @Classname Vector_
 * @Description Vector集合
 * @Date 2021/5/26 20:41
 * @Created by dell
 */

public class Vector_ {
    public static void main(String[] args) {
        //这里指定大小为8
        Vector<Object> vector = new Vector<>(8);
        for (int i = 0; i < 100; i++) {
            vector.add(i);
        }
        System.out.println(vector.toString());
    }
}
```

来探究一下源码 debug走起！

**断点位置 如下**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-05-04.png)

- 首先是初始化大小
- 然后是判断容量是否够
  - 不够就用算法扩容

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_20-55-44.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_20-56-51.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_20-57-24.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_20-58-07.png)

**这里扩容的大小是原来的2倍**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-01-55.png)



## 6.2 Vector 和 ArrayList 的比较

|           | 底层结构         | 版本   | 线程安全同步(效率) |
| --------- | ---------------- | ------ | ------------------ |
| ArrayList | 可变数组         | jdk1.2 | 不安全，效率高     |
| Vector    | 可变数组Object[] | jdk1.0 | 安全，效率不高     |

**扩容倍数**

- ArrayList
  - 如果有参构造1.5倍
  - 如果是无参构造
    - 第一次10
    - 第二次开始按照1.5倍扩容
- Vector
  - 如果是无参 默认是10 满后 按照2倍开始扩容
  - 指定大小，每次按照2倍扩容



# 7. LinkedList集合

## 7.1 LinkedList基本介绍

- LinkedList底层实现了双向链表和双端队列特点
- 可以添加任意元素 可以重复，包括null
- 线程不安全，没有实现同步



## 7.2 LinkedList的底层操作机制

- LinkedList底层维护了一个双向链表
- LinkedList中维护了两个属性first和last分别指向 首节点和尾节点
- 每个节点 Node对象 里面又维护了pre next item 三个属性 通过pre指向前一个 通过next指向后一个节点，最终实现双向链表
- 所以LinkedList的元素的添加和删除，不是通过数组完成的，相对来说效率较高

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-49-16.png)



```java
package com.zhuang.collection;

/**
 * @Classname LinkedList01
 * @Description LinkedList例子
 * @Date 2021/5/26 21:15
 * @Created by dell
 */

public class LinkedList01 {
    public static void main(String[] args) {
        //模拟一个简单的双向链表
        Node jack = new Node("jack");
        Node tom = new Node("tom");
        Node kang = new Node("kang");
        //连接三个结点，形成双向链表
        //jack -> tom -> kang
        jack.next = tom;
        tom.next = kang;
        //kang -> tom -> jack
        kang.pre = tom;
        tom.pre = jack;
        //让 first 引用指向 jack,就是双向链表的头结点
        Node first = jack;
        //让 last 引用指向 kang,就是双向链表的尾结点
        Node last = kang;

        System.out.println("===从头到尾进行遍历===");
        while (true) {
            if (first==null){
                break;
            }
            System.out.println(first);
            //头节点指向下一个
            first = first.next;
        }

        System.out.println("====从尾到头的遍历====");
        while (true) {
            if(last == null) {
                break;
            }
        //输出 last 信息
            System.out.println(last);
            last = last.pre;
        }

        //新加一个node节点
        Node jerry=new Node("jerry");

        jerry.next=tom;
        jerry.pre = jack;
        tom.pre=jerry;
        jack.next=jerry;

        //让 first 再次指向 jack
        first = jack;//让 first 引用指向 jack,就是双向链表的头结点


        last=kang;
        System.out.println("===从头到尾进行遍历===");
        while (true) {
            if (first==null){
                break;
            }
            System.out.println(first);
            //头节点指向下一个
            first = first.next;
        }

        System.out.println("====从尾到头的遍历====");
        while (true) {
            if(last == null) {
                break;
            }
            //输出 last 信息
            System.out.println(last);
            last = last.pre;
        }
    }
    //定义一个 Node 类，Node 对象 表示双向链表的一个结点
    static class Node {
        public Object item; //真正存放数据
        public Node next; //指向后一个结点
        public Node pre; //指向前一个结点

        public Node(Object name) {
            this.item = name;
        }

        @Override
        public String toString() {
            return "Node name{" +
                    "item=" + item +
                    '}';
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-27-10.png)

探究下源码 **看看增删的操作**

```java
package com.zhuang.collection;

import java.util.Iterator;
import java.util.LinkedList;

/**
 * @Classname LinkedListCRUD
 * @Description 链表的增删改查
 * @Date 2021/5/26 21:27
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class LinkedListCRUD {
    public static void main(String[] args) {
        LinkedList linkedList = new LinkedList();
        linkedList.add(1);
        linkedList.add(2);
        linkedList.add(3);
        System.out.println("linkedList=" + linkedList);

        //演示一个删除结点的
        // 这里默认删除的是第一个结点
        linkedList.remove();

        System.out.println("linkedList=" + linkedList);

        //修改某个结点对象
        linkedList.set(1, 999);
        System.out.println("linkedList=" + linkedList);

        //得到某个结点对象
        //get(1) 是得到双向链表的第二个对象

        Object o = linkedList.get(1);
        System.out.println(o);//999

        //因为 LinkedList 是 实现了 List 接口, 遍历方式
        System.out.println("===LinkeList 遍历迭代器====");
        Iterator iterator = linkedList.iterator();
        while (iterator.hasNext()) {
            Object next = iterator.next();
            System.out.println("next=" + next);
        }

        System.out.println("===LinkeList 遍历增强 for====");
        for (Object o1 : linkedList) {
            System.out.println("o1=" + o1);
        }
        System.out.println("===LinkeList 遍历普通 for====");
        for (int i = 0; i < linkedList.size(); i++) {
            System.out.println(linkedList.get(i));
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-51-19.png)

debug断点位置

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-50-14.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-36-43.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-38-48.png)

![Snipaste_2021-05-26_21-39-26](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-39-26.png)

![Snipaste_2021-05-26_21-40-07](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-40-07.png)

![Snipaste_2021-05-26_21-40-46](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-40-46.png)

![Snipaste_2021-05-26_21-41-37](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-26_21-41-37.png)

## 7.3 ArrayList和LinkedList比较

|            | 底层结构 | 增删的效率        | 改查的效率 |
| ---------- | -------- | ----------------- | ---------- |
| ArrayList  | 可变数组 | 较低 数组扩容     | 较高       |
| LinkedList | 双向链表 | 较高 通过链表追加 | 较低       |

**如何选择两者中？**

- 改查操作多，选择ArrayList
- 增删操作多，选择LinkedList



# 8. Set集合

## 8.1 基本介绍

- 无序，**没有索引**
- 不允许重复元素，所以最多包含一个null
- 实现类有
  - TreeSet
  - HashSet

## 8.2 常用方法

和List接口一样 Set接口也是Collection子接口，常用方法和Collection接口一样

**遍历方式**

- 可以使用迭代器
- 增强for
- 不能使用索引的方式来获取

```java
package com.zhuang.collection;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

/**
 * @Classname SetMethod
 * @Description Set集合
 * @Date 2021/5/26 21:57
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class SetMethod {
    public static void main(String[] args) {
        //1. 以 Set 接口的实现类 HashSet 来讲解 Set 接口的方法
        //2. set 接口的实现类的对象(Set 接口对象), 不能存放重复的元素, 可以添加一个 null
        //3. set 接口对象存放数据是无序(即添加的顺序和取出的顺序不一致)
       //4. 注意：取出的顺序的顺序虽然不是添加的顺序，但是他的固定.
    Set set = new HashSet();
        set.add("john");
        set.add("lucy");
        //重复
        set.add("john");
        set.add("jack");
        set.add("zk");
        set.add("mary");
        set.add(null);
        set.add(null);//再次添加 null

        System.out.println("set=" + set);

        //遍历
        System.out.println("=====使用迭代器====");
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj=" + obj);
        }

        set.remove(null);
        //方式 2: 增强 for
        System.out.println("=====增强 for====");
        for (Object o : set) {
            System.out.println("o=" + o);
        }
        //set 接口对象，不能通过索引来获取
    }
}
```



# 9. HashSet集合

## 9.1 HashSet集合基本介绍

- HashSet实际是Set接口
- HashSet实际是HashMap 看源码图
- 可以存放null值，但是只能一个null
- HashSet不保证元素是有序的，取决于hash后，再确定索引的结果 **即 不保证存放的元素和取出顺序一致**
- 不能有重复的元素/对象，在前面Set接口中提到过

```java
package com.zhuang.collection;
import java.util.HashSet;
import java.util.Set;

/**
 * @Classname HashSet_
 * @Description HashSet集合
 * @Date 2021/5/27 21:37
 * @Created by dell
 */

public class HashSet_ {
    public static void main(String[] args) {
        Set hashSet = new HashSet();
        hashSet.add(null);
        hashSet.add(null);
        System.out.println("hashSet=" + hashSet);
    }
}
```

**输出的值为 null**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-40-46.png)

由源码可见 **HashSet就是HashMap**



```java
package com.zhuang.collection;

import java.util.HashSet;

/**
 * @Classname HashSet01
 * @Description 用一句话描述类的作用
 * @Date 2021/5/27 21:43
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class HashSet01 {
    public static void main(String[] args) {
        HashSet set = new HashSet();

        //1. 在执行 add 方法后，会返回一个 boolean 值
        //2. 如果添加成功，返回 true, 否则返回 false
        //3. 可以通过 remove 指定删除哪个对象
        //4. 元素不能重复
        System.out.println(set.add("john"));//T
        System.out.println(set.add("lucy"));//T
        System.out.println(set.add("john"));//F
        System.out.println(set.add("jack"));//T
        System.out.println(set.add("Rose"));//T
        set.remove("john");
        System.out.println("set=" + set);//3 个


        set.add("lucy");//添加成功
        set.add("lucy");//加入不了
        set.add(new Dog("tom"));//OK
        set.add(new Dog("tom"));//Ok
        System.out.println("set=" + set);
    }

    static class Dog { //定义了 Dog 类
        private String name;

        public Dog(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "Dog{" +
                    "name='" + name + '\'' +
                    '}';
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-49-03.png)



## 9.2 HashSet的底层机制源码分析

```java
package com.zhuang.collection;

import java.util.HashSet;

/**
 * @Classname HashSetSource
 * @Description HashSet的源码探究
 * @Date 2021/5/27 21:52
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class HashSetSource {
    public static void main(String[] args) {
        HashSet hashSet = new HashSet();
        hashSet.add("java");//到此位置，第 1 次 add 分析完毕. hashSet.add("php");//到此位置，第 2 次 add 分析完毕
        hashSet.add("java");
        System.out.println("set=" + hashSet);
    }
}
```

**debug 一把 断点位置**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_10-53-37.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_22-02-33.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-40-46.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-53-41.png)

![Snipaste_2021-05-27_21-54-05](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-54-05.png)

![Snipaste_2021-05-27_21-55-50](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-55-50.png)

**由于该方法太长 单独拿出来分析 核心源码！**

![image-20210527220005260](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-59-35.png)



```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    //定义了辅助变量
        Node<K,V>[] tab; Node<K,V> p; int n, i;
    //table 就是 HashMap的一个数组 类型是 Node[]
    /*
    if语句表示如果当前的table 是null 或者大小为0
    就是第一次扩容 16个空间
    */
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
    /*
    (1)根据 key，得到 hash 去计算该 key 应该存放到 table 表的哪个索引位置并把这个位置的对象，赋给 p
	(2)判断 p 是否为 null
	(2.1) 如果 p 为 null, 表示还没有存放元素, 就创建一个 Node (key="java",value=PRESENT)
	(2.2) 就放在该位置 tab[i] = newNode(hash, key, value, null)
	*/
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            /*
            如果当前索引位置对应的链表的第一个元素和准备添加的key的hash值一样
            并且满足下面两个条件之一
            1.准备加入的key和p指向的Node节点的key是同一个对象
            2.p指向的Node节点的key的equals和准备加入的key比较后相同就不能加入 这就是不允许重复的值
            */
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            /*
            再判断p是不是一颗红黑树
            如果是红黑树，就调用putTreeVal，进行添加
            */
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                /*
	    	如果 table 对应索引位置，已经是一个链表, 就使用 for 循环比较
		(1) 依次和该链表的每一个元素比较后，都不相同, 则加入到该链表的最后注意在把元素添加到链表后，
		立即判断 该链表是否已经达到 8 个结点，达到8个结点，	
		就调用 treeifyBin() 对当前这个链表进行树化(转成红黑树)
		注意，在转成红黑树时，要进行判断, 判断条件
		if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY(64))
		resize();
 		如果上面条件成立，先 table 扩容. // 只有上面条件不成立时，才进行转成红黑树
		(2) 依次和该链表的每一个元素比较过程中，如果有相同情况,就直接 break
				*/
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
    	//size 就是我们每加入一个结点 Node(k,v,h,next), 
        if (++size > threshold)
            resize(); //扩容的方法
        afterNodeInsertion(evict);
        return null;
    }
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_10-55-09.png)

**扩容机制**

> - HashSet 底层是 HashMap, 第一次添加时，table 数组扩容到 16，
>           临界值(threshold)是 16*加载因子(loadFactor)是 0.75 = 12
>           如果 table 数组使用到了临界值 12,就会扩容到 16 * 2 = 32
>           新的临界值就是 32*0.75 = 24, 依次类推
>
> - 在 Java8 中, 如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是 8 )，
>       并且 table 的大小 >= MIN_TREEIFY_CAPACITY(默认 64),就会进行树化(红黑树),
>       否则仍然采用数组扩容机制

```java
package com.zhuang.collection;

import java.util.HashSet;

/**
 * @Classname HashSetIncrement
 * @Description 扩容源码探究
 * @Date 2021/5/27 22:20
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class HashSetIncrement {
    public static void main(String[] args) {
        HashSet hashSet = new HashSet();
        for (int i = 0; i <20; i++) {
            hashSet.add(i);
        }
    }
    /*
         HashSet 底层是 HashMap, 第一次添加时，table 数组扩容到 16，
        临界值(threshold)是 16*加载因子(loadFactor)是 0.75 = 12
        如果 table 数组使用到了临界值 12,就会扩容到 16 * 2 = 32
        新的临界值就是 32*0.75 = 24, 依次类推


        在 Java8 中, 如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是 8 )，
        并且 table 的大小 >= MIN_TREEIFY_CAPACITY(默认 64),就会进行树化(红黑树),
        否则仍然采用数组扩容机制
     */
}
```

## 9.3 HashSet练习

> 定义一个 Employee 类，该类包含：private 成员属性 name,age 
>
> 要求: 创建 3 个 Employee 对象放入 HashSet 中 当 name 和 age 的值相同时，认为是相同员工, 不能添加到 HashSet 集合中

```java
package com.zhuang.collection;

import java.util.HashSet;
import java.util.Objects;

/**
 * @Classname HashSetExercise
 * @Description Hash练习
 * @Date 2021/5/28 9:39
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class HashSetExercise {

    public static void main(String[] args) {
        HashSet hashSet = new HashSet();
        hashSet.add(new Employee("milan", 18));//ok
        hashSet.add(new Employee("smith", 28));//ok
        hashSet.add(new Employee("milan", 18));//加入不成功.
        System.out.println(hashSet);
    }

    static class Employee{
        private String name;
        private int age;

        public Employee(String name, int age) {
            this.name = name;
            this.age = age;
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
            this.age = age;
        }

        @Override
        public String toString() {
            return "Employee{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Employee employee = (Employee) o;
            return age == employee.age && Objects.equals(name, employee.name);
        }

        @Override
        public int hashCode() {
            return Objects.hash(name, age);
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_09-46-11.png)



# 10. LinkedHashSet集合

## 10.1 LinkedHashSet基础介绍

- LinkedHashSet是HashSet的子类
- LinkedHashSet底层是一个LinkedHashMap,底层维护了一个数组+双向链表
- LinkedHashSet根据元素的hashCode值来决定元素的存储位置，同时使用链表维护元素的次序，使得元素看起来是插入顺序保存的
- LinkedHashSet不允许添加重复元素

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-02-34.png)

## 10.2 LinkedHashSet练习

> Car 类(属性:name,price)， 如果 name 和 price 一样， 则认为是相同元素，就不能添加。

```java
package com.zhuang.collection;

import java.util.LinkedHashSet;
import java.util.Objects;

/**
 * @Classname LinkedHashSetExercise
 * @Description LinkedHashSet练习
 * @Date 2021/5/28 9:46
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class LinkedHashSetExercise {
    public static void main(String[] args) {
        LinkedHashSet linkedHashSet = new LinkedHashSet();
        linkedHashSet.add(new Car("奥拓", 1000));//OK
        linkedHashSet.add(new Car("奥迪", 300000));//OK
        linkedHashSet.add(new Car("法拉利", 10000000));//OK
        linkedHashSet.add(new Car("奥迪", 300000));//加入不了
        linkedHashSet.add(new Car("保时捷", 70000000));//OK
        linkedHashSet.add(new Car("奥迪", 300000));//加入不了
        System.out.println("linkedHashSet=" + linkedHashSet);
    }


    static class Car{
        private String name;

        private double price;

        public Car(String name, double price) {
            this.name = name;
            this.price = price;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public double getPrice() {
            return price;
        }

        public void setPrice(double price) {
            this.price = price;
        }

        @Override
        public String toString() {
            return "\nCar{" +
                    "name='" + name + '\'' +
                    ", price=" + price +
                    '}';
        }

        //重写 equals 方法 和 hashCode
        //当 name 和 price 相同时， 就返回相同的 hashCode 值, equals 返回 true

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Car car = (Car) o;
            return Double.compare(car.price, price) == 0 && Objects.equals(name, car.name);
        }

        @Override
        public int hashCode() {
            return Objects.hash(name, price);
        }
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_09-51-06.png)



# 11.Map接口

## 11.1 Map接口基本介绍

- Map与Collection并列存在，用于保存具有映射关系的数据：Key-Value
- Map中的key和value可以是任何引用类型的数据，会封装到HashMap$Node对象中
- Map中的key不允许重复
- Map中的value可以重复
- Map的key可以为null，value也可以为null，key为null，只能一个，value为null，可以多个
- key和value之间存在单向一对一关系，通过指定的key总能找到对应的value

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-11-56.png)



## 11.2 Map接口常用方法

```java
package com.zhuang.collection;

import java.util.HashMap;
import java.util.Map;

/**
 * @Classname MapMethod
 * @Description Map的常用方法
 * @Date 2021/5/28 9:57
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class MapMethod {
    public static void main(String[] args) {

        Map map = new HashMap();
        map.put("1", new Book("java", 100));//OK
        map.put("2", "二号");//替换-> 一会分析源码
        map.put("3", "三号");//OK
        map.put("4", "四号");//OK
        map.put("5", null);//OK
        map.put(null, "空");//OK
        map.put("6", "六号");//OK
        map.put("7", "七号");
        System.out.println("map=" + map);


        // remove:根据键删除映射关系
        map.remove(null);
        System.out.println("map=" + map);
        // get：根据键获取值
        Object val = map.get("2");
        System.out.println("val=" + val);
        // size:获取元素个数
        System.out.println("k-v=" + map.size());
        // isEmpty:判断个数是否为 0
        System.out.println(map.isEmpty());//F
        // clear:清除 k-v
        //map.clear();
        System.out.println("map=" + map);
        // containsKey:查找键是否存在
        System.out.println("结果=" + map.containsKey("6"));//T
    }
}
    class Book {
        private String name;
        private int num;
        public Book(String name, int num) {
            this.name = name;
            this.num = num;
        }

        @Override
        public String toString() {
            return "Book{" +
                    "name='" + name + '\'' +
                    ", num=" + num +
                    '}';
        }
    }
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_10-04-53.png)



## 11.3 Map接口遍历方法

```java
package com.zhuang.collection;

import java.util.*;
import java.util.Collection;

/**
 * @Classname MapFor
 * @Description Map集合遍历
 * @Date 2021/5/28 10:05
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class MapFor {
    public static void main(String[] args) {
        Map map = new HashMap();
        map.put("1", "一号");
        map.put("2", "二号");
        map.put("3", "三号");
        map.put("4", "四号");

        //第一组：先取出 所有的 Key ,通过 Key 取出对应的 Value
        Set keyset = map.keySet();

        //(1) 增强 for
        System.out.println("-----第一种方式-------");
        for (Object key : keyset) {
            System.out.println(key + "-" + map.get(key));
        }

        //(2) 迭代器
        System.out.println("----第二种方式--------");
        Iterator iterator = keyset.iterator();
        while (iterator.hasNext()) {
            Object key = iterator.next();
            System.out.println(key + "-" + map.get(key));
        }

        //第二组: 把所有的 values 取出
        Collection values = map.values();
        //这里可以使用所有的 Collections 使用的遍历方法
        //(1) 增强 for
        System.out.println("---取出所有的 value 增强 for----");
        for (Object value : values) {
            System.out.println(value);
        }
        //(2) 迭代器
        System.out.println("---取出所有的 value 迭代器----");
        Iterator iterator2 = values.iterator();
        while (iterator2.hasNext()) {
            Object value = iterator2.next();
            System.out.println(value);
        }


        //第三组: 通过 EntrySet 来获取 k-v
        Set entrySet = map.entrySet();// EntrySet<Map.Entry<K,V>>
        //(1) 增强 for
        System.out.println("----使用 EntrySet 的 for 增强(第 3 种)----");
        for (Object entry : entrySet) {
        //将 entry 转成 Map.Entry
            Map.Entry m = (Map.Entry) entry;
            System.out.println(m.getKey() + "-" + m.getValue());
        }
        //(2) 迭代器
        System.out.println("----使用 EntrySet 的 迭代器(第 4 种)----");
        Iterator iterator3 = entrySet.iterator();
        while (iterator3.hasNext()) {
            Object entry = iterator3.next();
        //System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
        //向下转型 Map.Entry
            Map.Entry m = (Map.Entry) entry;
            System.out.println(m.getKey() + "-" + m.getValue());
        }
    }
}
```



## 11.4 HashMap小结

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-15-57.png)

## 11.5 HashMap 底层机制及源码剖析

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-16-59.png)

![Snipaste_2021-05-28_11-17-09](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-17-09.png)

```java
package com.zhuang.collection;

import java.util.HashMap;

/**
 * @Classname HashMapSource1
 * @Description HashMap源码探究
 * @Date 2021/5/28 10:15
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class HashMapSource1 {
    public static void main(String[] args) {
        HashMap map = new HashMap();
        map.put("java", 10);//ok
        map.put("php", 10);//ok
        map.put("java", 20);//替换 value
        System.out.println("map=" + map);
    }
}
```

**断点位置 如下 debug一把**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-17-56.png)

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-27_21-59-35.png)

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    //定义辅助变量
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            //如果底层的table数组为null，或者length=0，就扩容到16
            n = (tab = resize()).length;
    //取出hash值对应的table索引位置的Node，如果为null 就直接加入 k-v 
    // 创建一个Node,加入该位置即可
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            //辅助变量
            Node<K,V> e; K k;
            /*
            如果table的索引位置的key的hash相同和新的key的hash值相同
            并满足(table现有的节点的key和准备添加的key是同一个对象 ||equals返回真)
            就认为不能加入新的 k-v
            */
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            //如果当前的table的已有的Node是红黑树，就按照红黑树的方式处理
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                //如果找到节点，后面是链表，就循环比较
                //死循环
                for (int binCount = 0; ; ++binCount) {
                    //如果整个链表，没有和他相同，就添加到该链表最后
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    //循环比较过程中，发现有相同，就break，只是替换value
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;//替换
                afterNodeAccess(e);
                return oldValue;
            }
        }
    //每增加一个Node，就size++
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_10-37-23.png)

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    //关于树化 转化成红黑树 
    /*
    如果table为null，或者大小没到64.暂时不树化，而是进行扩容
    */
        int n, index; Node<K,V> e;
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            do {
                TreeNode<K,V> p = replacementTreeNode(e, null);
                if (tl == null)
                    hd = p;
                else {
                    p.prev = tl;
                    tl.next = p;
                }
                tl = p;
            } while ((e = e.next) != null);
            if ((tab[index] = hd) != null)
                hd.treeify(tab);
        }
    }
```



# 12. HashTable集合

## 12.1 HashTable 基本介绍

- 存放的元素是键值对:即K-V
- hashtable的键和值都不能为null，否则会抛出NullPointerException
-  hashTable使用方法基本上和HashMap一样
- hashTable是线程安全的(synchronized)， hashMap是线程不安全的



## 12.2 Hashtable和HashMap 对比

|           | 版本 | 线程安全(同步) | 效率 | 允许null键null值 |
| --------- | ---- | -------------- | ---- | ---------------- |
| HashMap   | 1.2  | 不安全         | 高   | 可以             |
| HashTable | 1.0  | 安全           | 较低 | 不可以           |



# 13. Properties集合

- Properties类继承自Hashtable类并且实现了Map接口，也是使用一种键值对的形式来保存数据。
- Properties 还可以用于从xoxx.properties文件中，加载数据到Properties类对象,并进行读取和修改
- 使用特点和Hashtable类似

```java
package com.zhuang.collection;

import java.util.Properties;

/**
 * @Classname Properties_
 * @Description 用一句话描述类的作用
 * @Date 2021/5/28 10:48
 * @Created by dell
 */

public class Properties_ {
    public static void main(String[] args) {
        //1. Properties 继承 Hashtable
        //2. 可以通过 k-v 存放数据，当然 key 和 value 不能为 null
        //增加
        Properties properties = new Properties();
        //properties.put(null, "abc");//抛出 空指针异常
         //properties.put("abc", null); //抛出 空指针异常
        properties.put("john", 100);//k-v
        properties.put("lucy", 100);
        properties.put("lic", 100);
        properties.put("lic", 88);//如果有相同的 key ， value 被替换
        System.out.println("properties=" + properties);

        //通过 k 获取对应值
        System.out.println(properties.get("lic"));//88
        //删除
        properties.remove("lic");
        System.out.println("properties=" + properties);
        //修改
        properties.put("john", "约翰");
        System.out.println("properties=" + properties);
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_10-49-44.png)



# 14. 实际应用中如何选择集合实现类

在开发中，选择什么集合实现类，主要取决于业务操作特点，然后根据集合实现类特性进行选择，分析如下:

（1)先判断存储的类型(一组对象[单列]或一组键值对[双列)

(2)一组对象[单列]:Collection接口

- 允许重复 List
  - 增删多:LinkedList[底层维护了一个双向链表]
  - 改查多:ArrayList[底层维护了Object类型的可变数组]
- 不允许重复:Set
  - 无序: HashSet[底层是HashMap，维护了一个哈希表即(数组+链表+红黑树)]
  - 排序:TreeSet
  - 插入和取出顺序一致:LinkedHashSet，维护数组+双向链表

(3)一组键值对[双列:Map

- 键无序: HashMap [底层是:哈希表jdk7:数组+链表，jdk8:数组+链表+红黑树]
- 键排序:TreeMap

- 键插入和取出顺序一致:LinkedHashMap
- 读取文件Properties

**比较器的使用**

```java
package com.zhuang.collection;

import java.util.Comparator;
import java.util.TreeSet;

/**
 * @Classname TreeSet_
 * @Description TreeSet比较器的使用
 * @Date 2021/5/28 11:36
 * @Created by dell
 */

public class TreeSet_ {
    public static void main(String[] args) {
        TreeSet treeSet = new TreeSet(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
        //下面 调用 String 的 compareTo 方法进行字符串大小比较
        //要求加入的元素，按照长度大小排序
        //return ((String) o2).compareTo((String) o1);
                return ((String) o1).length() - ((String) o2).length();
            }
        });

        treeSet.add("tom");//3
        treeSet.add("sp");
        treeSet.add("a");
        treeSet.add("abc");//3

        System.out.println("treeSet=" + treeSet);
    }
}
```

# 15 工具类

-  Collections是一个操作Set、List和 Map等集合的工具类
- Collections中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-56-00.png)

![Snipaste_2021-05-28_11-55-49](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-55-49.png)

```java
package com.zhuang.collection;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

/**
 * @Classname Collections_
 * @Description Collections工具类
 * @Date 2021/5/28 11:46
 * @Created by dell
 */
@SuppressWarnings({"all"})
public class Collections_ {
    public static void main(String[] args) {
        //创建 ArrayList 集合，用于测试.
        ArrayList list = new ArrayList();
        list.add("tom");
        list.add("smith");
        list.add("king");
        list.add("milan");
        list.add("tom");
        // reverse(List)：反转 List 中元素的顺序
        Collections.reverse(list);
        System.out.println("list=" + list);
        // shuffle(List)：对 List 集合元素进行随机排序
        for (int i = 0; i < 5; i++) {
            Collections.shuffle(list);
            System.out.println("list=" + list);
        }
        // sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
        Collections.sort(list);
        System.out.println("自然排序后");
        System.out.println(list);

        // sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
        //我们希望按照 字符串的长度大小排序
        Collections.sort(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
        //可以加入校验代码.
                return ((String) o2).length() - ((String) o1).length();
            }
        });
        System.out.println("字符串长度大小排序=" + list);
        // swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换
        Collections.swap(list, 0, 1);
        System.out.println("交换后的情况");
        System.out.println("list=" + list);
        //Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
        System.out.println("自然顺序最大元素=" + Collections.max(list));
        //Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
        //比如，我们要返回长度最大的元素

        //比如，我们要返回长度最大的元素
        Object maxObject = Collections.max(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                return ((String) o1).length() - ((String) o2).length();
            }
        });
        System.out.println("长度最大的元素=" + maxObject);
        //Object min(Collection)
        //Object min(Collection，Comparator)
        //上面的两个方法，参考 max 即可
        //int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
        System.out.println("tom 出现的次数=" + Collections.frequency(list, "tom"));
        //void copy(List dest,List src)：将 src 中的内容复制到 dest 中
        ArrayList dest = new ArrayList();
        //为了完成一个完整拷贝，我们需要先给 dest 赋值，大小和 list.size()一样
        for (int i = 0; i < list.size(); i++) {
            dest.add("");
        }
        //拷贝
        Collections.copy(dest, list);
        System.out.println("dest=" + dest);
        //boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
        //如果 list 中，有 tom 就替换成 汤姆
        Collections.replaceAll(list, "tom", "汤姆");
        System.out.println("list 替换后=" + list);
    }
}
```

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Java%E9%9B%86%E5%90%88%E5%9B%BE%E7%89%87/Snipaste_2021-05-28_11-52-40.png)

**写在最后：用韩老师的一句话来鞭策自己"我亦无他，惟手熟尔！"**



**若有错误，还请各位指出错误，及时更改！**

