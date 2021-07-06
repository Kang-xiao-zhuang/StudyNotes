
# Java反射学习笔记

写在前面

- 感谢韩老师的视频讲解！
- 学习地址：[【韩顺平讲Java】Java反射专题 ](https://www.bilibili.com/video/BV1g84y1F7df)

# 1. 反射机制

- 反射机制允许程序在执行期借助于`ReflectionAPI`取得任何类的内部信息，(比如成员变量，构造器，成员方法等)，并能操作对象的属性及方法，在设计模式和框架都会用到
- 加载完类后，在堆中就产生了一个`Class`类型的对象（一个类只有一个Class对象），这个对象包含了类的完整结构信息，通过对这个对象得到这类的结构，这个`Class`对象就像一面镜子，透过这个镜子看到类的结构，形象的称之为反射



## 1.1 反射机制原理示意图

[![2R8eL8.png](https://z3.ax1x.com/2021/06/10/2R8eL8.png)](https://imgtu.com/i/2R8eL8)



## 1.2 Java 反射机制可以完成

1. 在运行时判断任意一个对象所属的类
2. 在运行构造时任意一个类的对象
3. 在运行时得到任意一个类所具有的成员变量和方法
4. 在运行时调用任意一个对象的成员变量和方法
5. 生成动态代理



## 1.3 反射相关的主要类

[![2R8sQx.png](https://z3.ax1x.com/2021/06/10/2R8sQx.png)](https://imgtu.com/i/2R8sQx)

**示例**

```java
package com.zhuang.reflection;

import java.io.FileInputStream;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.util.Properties;

/**
 * @Classname Reflection01
 * @Description 反射类01
 * @Date 2021/6/10 19:51
 * @Created by dell
 */

public class Reflection01 {
    public static void main(String[] args) throws Exception{
        //使用Properties类 ，读写配置文件
        Properties properties = new Properties();
        properties.load(new FileInputStream("f:\\re.properties"));
        String classfullPath=properties.get("classfullpath").toString();
        String methodName=properties.get("method").toString();
        System.out.println(classfullPath);
        System.out.println(methodName);

        //使用反射机制解决
        //加载类，返回Class类型对象
        Class cls = Class.forName("com.zhuang.reflection.Cat");
        //通过cls的到加载类的对象实例
        Object o = cls.newInstance();
        System.out.println("o的运行类型="+o.getClass());
        //通过cls的加载的类 的方法
        Method method = cls.getMethod(methodName);
        System.out.println("方法是"+method);
        //通过method调用方法，通过方法对象来实现调用方法
        /*
        传统方法 对象.方法()
        反射机制 方法.invoke(对象)
         */
        method.invoke(o);

        //java.lang.reflect.Field: 代表类的成员变量, Field 对象表示某个类的成员变量
        //得到 name 字段
        //getField 不能得到私有的属性
        Field nameField = cls.getField("age");
        // 传统写法 对象.成员变量 , 反射 : 成员变量对象.get(对象)
        System.out.println(nameField.get(o));
    //java.lang.reflect.Constructor: 代表类的构造方法, Constructor 对象表示构造器
        //()中可以指定构造器参数类型, 返回无参构造器
        Constructor constructor = cls.getConstructor();
        System.out.println(constructor);
    }
}
```

[![2RJqiQ.png](https://z3.ax1x.com/2021/06/10/2RJqiQ.png)](https://imgtu.com/i/2RJqiQ)



## 1.4 反射优点和缺点

优点：

- 可以动态地创建和使用对象，灵活

缺点

- 使用反射基本是解释执行，对执行速度有影响

- **优化可以使用下列方法**

[![2RYAzR.png](https://z3.ax1x.com/2021/06/10/2RYAzR.png)](https://imgtu.com/i/2RYAzR)



# 2. Class类

基本介绍

[![2RYeL6.png](https://z3.ax1x.com/2021/06/10/2RYeL6.png)](https://imgtu.com/i/2RYeL6)

1. `Class`也是类，继承自`Object`类
2. `Class`类对象不是new出来的，而是系统创建的
3. 对于某个类的`Class`类对象，在内存中只有一份
4. 每个类的示例都会记得自己是由哪个`Class`实例所生成
5. 通过`Class`对象完整的一个类的完整结构，通过一系列API
6. `Class`对象是存放在堆里的
7. 类的字节码二进制数据，是在方法区的



```java
//ClassLoader 类, 仍然是通过 ClassLoader 类加载 Cat 类的 Class 对象
public Class<?> loadClass(String name) throws ClassNotFoundException {
	return loadClass(name, false);
}
```

**Class01**

```java
package com.zhuang.reflection;

/**
 * @Classname Class01
 * @Description 用一句话描述类的作用
 * @Date 2021/6/10 20:21
 * @Created by dell
 */

public class Class01 {
    public static void main(String[] args) throws Exception{
        //某个类的对象Class ，在内存中只有一份，类只加载一次
        Class<?> cls1 = Class.forName("com.zhuang.reflection.Cat");
        Class<?> cls2 = Class.forName("com.zhuang.reflection.Cat");

        System.out.println(cls1.hashCode());
        System.out.println(cls2.hashCode());
        /*
        22307196
		22307196
        */

    }
}
```

## 2.1 Class类常用方法

[![2RtH3j.png](https://z3.ax1x.com/2021/06/10/2RtH3j.png)](https://imgtu.com/i/2RtH3j)

```java
package com.zhuang.reflection;

import java.lang.reflect.Field;

/**
 * @Classname Class02
 * @Description 演示Class类的常用方法
 * @Date 2021/6/10 20:26
 * @Created by dell
 */

public class Class02 {
    public static void main(String[] args) throws Exception {
        String classAllPath = "com.zhuang.reflection.Car";
        //1 . 获取到 Car 类 对应的 Class 对象
        //<?> 表示不确定的 Java 类型
        Class<?> cls = Class.forName(classAllPath);

        //2. 输出 cls
        //显示 cls 对象, 是哪个类的 Class 对象 com.hspedu.Car
        System.out.println(cls);
        //输出 cls 运行类型 java.lang.Class
        System.out.println(cls.getClass());

        //3. 得到包名
        System.out.println(cls.getPackage().getName());
        //4. 得到全类名
        System.out.println(cls.getName());
        //5. 通过 cls 创建对象实例
        Car car = (Car) cls.newInstance();
        System.out.println(car);
        //6. 通过反射获取属性 brand
        Field brand = cls.getField("brand");
        System.out.println(brand.get(car));
        //7. 通过反射给属性赋值
        brand.set(car, "奔驰");
        System.out.println(brand.get(car));
        //8 我希望大家可以得到所有的属性(字段)
        System.out.println("=======所有的字段属性====");
        Field[] fields = cls.getFields();
        for (Field f : fields) {
            System.out.println(f.getName());
        }
    }
}
```

[![2RNXzd.png](https://z3.ax1x.com/2021/06/10/2RNXzd.png)](https://imgtu.com/i/2RNXzd)



## 2.2 获取Class类对象

1. 前提：已知一个类的全类名，且该类在类路径下，可通过`Class`类的静态方法`forName()`获取，
2. 前提：若已知具体的类，通过类的`class`，程序性能最高
3. 前提：已知某个类的实例，调用该实例的`getClass()`方法获取`Class`对象
4. 基本数据类型， 按照 `Class cls=基本数据类型.class`方法
5. 基本数据类型的包装类，可以通过`.TYPE`得到`Class`对象 `Class cls=包装类.TYPE`

```java
package com.zhuang.reflection;

/**
 * @Classname GetClass_
 * @Description 演示得到 Class 对象的各种方式(6)
 * @Date 2021/6/10 20:39
 * @Created by dell
 */

public class GetClass_ {
    public static void main(String[] args) throws Exception {
        //1. Class.forName
        String classAllPath = "com.zhuang.reflection.Car";
        Class<?> cls1 = Class.forName(classAllPath);
        System.out.println(cls1);
        //2. 类名.class , 应用场景: 用于参数传递
        Class cls2 = Car.class;
        System.out.println(cls2);
        //3. 对象.getClass(), 应用场景，有对象实例
        Car car = new Car();
        Class cls3 = car.getClass();
        System.out.println(cls3);
        //4. 通过类加载器【4 种】来获取到类的 Class 对象
        //(1)先得到类加载器 car
        ClassLoader classLoader = car.getClass().getClassLoader();
        //(2)通过类加载器得到 Class 对象
        Class cls4 = classLoader.loadClass(classAllPath);
        System.out.println(cls4);


        //5. 基本数据(int, char,boolean,float,double,byte,long,short) 按如下方式得到 Class 类对象
        Class<Integer> integerClass = int.class;
        Class<Character> characterClass = char.class;
        Class<Boolean> booleanClass = boolean.class;
        System.out.println(integerClass);
        System.out.println(characterClass);
        System.out.println(booleanClass);
        //6. 基本数据类型对应的包装类，可以通过 .TYPE 得到 Class 类对象
        Class<Integer> type1 = Integer.TYPE;
        Class<Character> type2 = Character.TYPE;
        System.out.println(type1);
        System.out.println(integerClass.hashCode());
        System.out.println(type1.hashCode());
        
    }
}
```

[![2Ra8N8.png](https://z3.ax1x.com/2021/06/10/2Ra8N8.png)](https://imgtu.com/i/2Ra8N8)



## 2.3 哪些类型有 Class 对象

```java
package com.zhuang.reflection;

import java.io.Serializable;

/**
 * @Classname AllTypeClass
 * @Description 演示哪些类型有 Class 对象
 * @Date 2021/6/10 21:08
 * @Created by dell
 */

public class AllTypeClass {
    public static void main(String[] args) throws Exception {
        //外部类
        Class<String> cls1 = String.class;
        //接口
        Class<Serializable> cls2 = Serializable.class;
        //数组
        Class<Integer[]> cls3 = Integer[].class;
        //二维数组
        Class<float[][]> cls4 = float[][].class;
        //注解
        Class<Deprecated> cls5 = Deprecated.class;
        //枚举
        Class<Thread.State> cls6 = Thread.State.class;
        //基本数据类型
        Class<Long> cls7 = long.class;
        //void 数据类型
        Class<Void> cls8 = void.class;
        Class<Class> cls9 = Class.class;
        System.out.println(cls1);
        System.out.println(cls2);
        System.out.println(cls3);
        System.out.println(cls4);
        System.out.println(cls5);
        System.out.println(cls6);
        System.out.println(cls7);
        System.out.println(cls8);
        System.out.println(cls9);
    }
}
```

[![2R0t3D.png](https://z3.ax1x.com/2021/06/10/2R0t3D.png)](https://imgtu.com/i/2R0t3D)



# 3. 类加载

- 反射机制是java实现冬天语言的关键，也就是通过反射实现类动态加载
- 静态加载：编译时加载相关的类，如果没有报错，**依赖性太强**
- 动态加载：运行时加载需要的类，如果运行时不用该类，即使不存在该类，则不报错，**降低了依赖性**



## 3.1 类加载时机

**静态加载**

1. 当创建对象时 `new `
2. 当子类被加载时，父类也加载
3. 调用类中的静态车成员时

**动态加载**

1. 通过反射



## 3.2 类加载过程图

[![2RBixe.png](https://z3.ax1x.com/2021/06/10/2RBixe.png)](https://imgtu.com/i/2RBixe)

## 3.3 类加载各阶段完成任务

[![2RBKG8.png](https://z3.ax1x.com/2021/06/10/2RBKG8.png)](https://imgtu.com/i/2RBKG8)

## 3.4 加载阶段

[![2RB8qs.png](https://z3.ax1x.com/2021/06/10/2RB8qs.png)](https://imgtu.com/i/2RB8qs)

## 3.5 连接阶段-验证

1. 目的是为了确保`Class`文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机的自身安全
2. 包括：文件格式验证，元数据验证，字节码验证，符号引用验证等



## 3.6 连接阶段-准备

- JVM会在该阶段对静态变量，分配内存并默认初始化(对应数据类型的默认初始值)，这些变量所使用的内存都将在方法区进行分配

```java
//1. n1 是实例属性, 不是静态变量，因此在准备阶段，是不会分配内存
//2. n2 是静态变量，分配内存 n2 是默认初始化 0 ,而不是 20
//3. n3 是 static final 是常量, 他和静态变量不一样, 因为一旦赋值就不变 n3 = 30
	public int n1 = 10;
	public static int n2 = 20;
	public static final int n3 = 30;
```



## 3.7 Initialization（初始化)

1. 到初始化阶段，真正开始执行类中的定义Java程序代码，此阶段执行<clinit>方法的过程
2. <clinit>方法是由编译器按语句在源文件中出现的顺序，**依次自动收集类中的所有的静态变量赋值动作和静态代码块的语句，并进行合并**
3. 虚拟机会保证一个类的<clinit>方法在多线程环境中正确地加锁，同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的<clinit>方法，其他线程都需要阻塞等待，知道活动线程执行<clinit>方法完毕



# 4. 通过反射获取类的结构信息

## 4.1 java.lang.Class 类

[![2RgBhn.png](https://z3.ax1x.com/2021/06/10/2RgBhn.png)](https://imgtu.com/i/2RgBhn)

## 4.2 java.lang.reflect.Field 类
[![2Rg0ts.png](https://z3.ax1x.com/2021/06/10/2Rg0ts.png)](https://imgtu.com/i/2Rg0ts)

## 4.3 java.lang.reflect.Method 类
[![2Rgwkj.png](https://z3.ax1x.com/2021/06/10/2Rgwkj.png)](https://imgtu.com/i/2Rgwkj)

## 4.4 java.lang.reflect.Constructor 类
[![2Rgs10.png](https://z3.ax1x.com/2021/06/10/2Rgs10.png)](https://imgtu.com/i/2Rgs10)



# 5. 通过反射创建对象

[![2W1iVg.png](https://z3.ax1x.com/2021/06/11/2W1iVg.png)](https://imgtu.com/i/2W1iVg)



- 测试 1：通过反射创建某类的对象，要求该类中必须有 public 的无参构造 

- 测试 2：通过调用某个特定构造器的方式，实现创建某类的对象



```java
package com.zhuang.reflection;

import java.lang.reflect.Constructor;

/**
 * @Classname ReflecCreateInstance
 * @Description 演示通过反射机制创建实例
 * @Date 2021/6/11 10:21
 * @Created by dell
 */

public class ReflecCreateInstance {
    public static void main(String[] args) throws Exception{
        //1. 先获取到 User 类的 Class 对象
        Class<?> userClass = Class.forName("com.zhuang.reflection.User");
        //2. 通过 public 的无参构造器创建实例
        Object o = userClass.newInstance();
        System.out.println(o);
        //3. 通过 public 的有参构造器创建实例
        /*
        constructor 对象就是
        public User(String name) {//public 的有参构造器
        this.name = name;
        }
        */
        //3.1 先得到对应构造器
        Constructor<?> constructor = userClass.getConstructor(String.class);
        //3.2 创建实例，并传入实参
        Object zk = constructor.newInstance("zk");
        System.out.println("zk=" + zk);
        //4. 通过非 public 的有参构造器创建实例
        //4.1 得到 private 的构造器对象
        Constructor<?> constructor1 = userClass.getDeclaredConstructor(int.class, String.class);
        //4.2 创建实例
        //暴破【暴力破解】 , 使用反射可以访问 private 构造器/方法/属性
        constructor1.setAccessible(true);
        Object user2 = constructor1.newInstance(100, "张三丰");
        System.out.println("user2=" + user2);
    }
}
class User { //User 类
    private int age = 10;
    private String name = "康小庄";
    public User() {//无参 public
    }
    public User(String name) {//public 的有参构造器
        this.name = name;
    }
    private User(int age, String name) {//private 有参构造器
        this.age = age;
        this.name = name;
    }
    @Override
    public String toString() {
        return "User [age=" + age + ", name=" + name + "]";
    }
}
```

[![2WGLOP.md.png](https://z3.ax1x.com/2021/06/11/2WGLOP.md.png)](https://imgtu.com/i/2WGLOP)

# 6. 访问方法

[![2WUEv9.png](https://z3.ax1x.com/2021/06/11/2WUEv9.png)](https://imgtu.com/i/2WUEv9)

```java
package com.zhuang.reflection;

import java.lang.reflect.Method;

/**
 * @Classname ReflecAccessMethod
 * @Description 演示通过反射调用方法
 * @Date 2021/6/11 10:39
 * @Created by dell
 */

public class ReflecAccessMethod {
    public static void main(String[] args) throws Exception{
        //得到Boss类对应的Class对象
        Class<?> bossClass = Class.forName("com.zhuang.reflection.Boss");
        //创建对象
        Object o = bossClass.newInstance();
        //调用public 方法 得到方法对象
        Method show = bossClass.getMethod("show");
        //调用
        show.invoke(o);

        //调用private方法
        Method say = bossClass.getDeclaredMethod("say",int.class);
        //say方法是private，需要设置权限
        say.setAccessible(true);
        say.invoke(o,5);
    }
}
class Boss{
    private int age;
    private String name;

    public Boss() {//无参 public
    }
    public Boss(String name) {//public 的有参构造器
        this.name = name;
    }
    private Boss(int age, String name) {//private 有参构造器
        this.age = age;
        this.name = name;
    }

    public void show(){
        System.out.println("我是老板哈哈哈");
    }
    private void say(int i){
        System.out.println("我是老板嘿嘿嘿->"+i);
    }
}
```



## 7. 作业练习

**HomeWork01**

```java
package com.zhuang.reflection.HomeWork;

import java.lang.reflect.Field;
import java.lang.reflect.Method;

/**
 * @Classname HomeWork01
 * @Description 作业1
 * @Date 2021/6/11 10:59
 * @Created by dell
 */

public class HomeWork01 {
    public static void main(String[] args) throws Exception{
        /**
         * 定义Student类，有私有name属性，并且属性值为hellokitty
         * 提供getName的公有方法
         * 创建Student的类，利用Class类得到私有的name属性，修改私有的name属性值，并调用getName()的方法打印name属性值
         */
        //1. 得到 Student类对应的Class对象
        Class<Student> studentClass = Student.class;
        //创建对象实例
        Student student = studentClass.newInstance();
        //得到name属性对象
        Field name = studentClass.getDeclaredField("name");
        // 设置权限
        name.setAccessible(true);
        //赋值
        name.set(student,"康小庄");
        //获得方法
        Method getName = studentClass.getMethod("getName");
        //调用
        Object invoke = getName.invoke(student);
        System.out.println(invoke);
    }
}

class Student {
    private String name = "hellokitty";
    //默认无参构造器
    public String getName() {
        return name;
    }
}
```

**HomeWork02**

```java
package com.zhuang.reflection.HomeWork;

import java.io.File;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * @Classname HomeWork02
 * @Description 作业2
 * @Date 2021/6/11 11:36
 * @Created by dell
 */
/**
 * 利用Class类的forName方法得到File类的class 对象
 * 在控制台打印File类的所有构造器
 * 通过newInstance的方法创建File对象，并创建F:\my.txt文件
 */
public class HomeWork02 {
    public static void main(String[] args) throws Exception{
        //获取类class对象
        Class<?> cls = Class.forName("java.io.File");
        //遍历cls类对象构造器
        for (Constructor<?> declaredConstructor : cls.getDeclaredConstructors()) {
            System.out.println("构造方法"+declaredConstructor);
        }
        //指定的得到 public java.io.File(java.lang.String)
        Constructor<?> declaredConstructor = cls.getDeclaredConstructor(String.class);
        String filePath="f:\\my.text";
        Object file = declaredConstructor.newInstance(filePath);
        //得到createNewFile 的方法对象
        Method createNewFile = cls.getMethod("createNewFile");
        createNewFile.invoke(file);

        System.out.println(file.getClass());
        System.out.println("文件创建成功"+filePath);
    }
}
```

[![2WcCpq.png](https://z3.ax1x.com/2021/06/11/2WcCpq.png)](https://imgtu.com/i/2WcCpq)

**写在最后**

- 许多基础仍需要补充
- 提高独立写代码和思考的能力
- 多练习，多思考:muscle:

