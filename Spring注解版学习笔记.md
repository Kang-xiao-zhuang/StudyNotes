# Spring注解版学习笔记

**学习地址：https://www.bilibili.com/video/BV1gW411W7wy?p=1**

# 一，IOC

## 1. 使用@Configuration和@Bean给容器中注册组件

### 1.1 配置文件方式

- 新建Maven工程
- 导入依赖
- 创建工程目录
- 编写目录

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

创建 beans,xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
    <bean id="person" class="com.zhuang.dao.Person">
        <property name="name" value="zhuangkang"/>
        <property name="id" value="15"/>
    </bean>
</beans>
```

目录

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-00-14.png)



**Person**

```java
package com.zhuang.dao;

/**
 * @Classname Person
 * @Description person
 * @Date 2021/4/3 12:57
 * @Created by dell
 */

public class Person {
    private String name;
    private int id;

    public Person() {
    }

    public Person(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", id=" + id +
                '}';
    }
}
```

**MainTest**

```java
package com.zhuang;

import com.zhuang.config.MainConfig;
import com.zhuang.dao.Person;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @Classname MainTest
 * @Description 测试类
 * @Date 2021/4/3 12:57
 * @Created by dell
 */

public class MainTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}

```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-03-41.png)

### 1.2 注解方式

**MainConfig**

```java
package com.zhuang.config;

import com.zhuang.dao.Person;
import org.springframework.context.annotation.*;

/**
 * @Classname MainConfig
 * @Description 配置类
 * @Date 2021/4/3 13:02
 * @Created by dell
 */
@Configuration
@ComponentScan(value = "com.zhuang",includeFilters =
        {@ComponentScan.Filter(type = FilterType.CUSTOM,classes = MyTypeFliter.class)})  //包扫描注解
public class MainConfig {

    @Bean //给容器中注册bean
    public Person person(){
        return new Person("康康",16);
    }
}

```

**MainTest**

```java
package com.zhuang;

import com.zhuang.config.MainConfig;
import com.zhuang.dao.Person;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @Classname MainTest
 * @Description 测试类
 * @Date 2021/4/3 12:57
 * @Created by dell
 */

public class MainTest {
    public static void main(String[] args) {
        //注解式
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println(bean);
    }
}
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-06-41.png)

通过XML配置文件和注解这两种方式都可以将JavaBean注入到Spring的IOC容器中。那么，使用注解将JavaBean注入到IOC容器中时，使用的bean的名称又是什么呢？我们可以在MainTest类的main方法中添加如下代码来获取Person这个类型的组件在IOC容器中的名字。



在**MainTest**类中添加下列代码

```java
String[] names = applicationContext.getBeanNamesForType(Person.class);
        for (String name : names) {
            System.out.println(name);
        }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-12-15.png)

**使用注解注入JavaBean时，bean在IOC容器中的名称就是使用@Bean注解标注的方法名称。**

注解Bean中赋值也可以

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-14-07.png)

**总结**

 **我们在使用注解方式向Spring的IOC容器中注入JavaBean时，如果没有在@Bean注解中明确指定bean的名称，那么就会使用当前方法的名称来作为bean的名称；如果在@Bean注解中明确指定了bean的名称，那么就会使用@Bean注解中指定的名称来作为bean的名称。**



## 2. 使用@ComponentScan自动扫描组件并指定扫描规则

凡是在指定的包或其子包中的类上标注了@Repository、@Service、@Controller、@Component注解的类都会被扫描到，并将这个类注入到Spring容器中

```xml
<!--    包扫描组件 @Service @Repository @Controller 都会扫描到-->
<context:component-scan base-package="com.zhuang"/>
```

**分别创建BookDao、BookService以及BookController这三个类，并在这三个类中分别添加@Repository、@Service、@Controller注解**

**BookDao**

```java
package com.zhuang.dao;

import org.springframework.stereotype.Repository;

/**
 * @Classname BookDao
 * @Description BookDao
 * @Date 2021/4/3 13:08
 * @Created by dell
 */
@Repository
public class BookDao {
    
}
```

**BookService**

```java
package com.zhuang.service;

import org.springframework.stereotype.Service;

/**
 * @Classname BookService
 * @Description BookService
 * @Date 2021/4/3 13:08
 * @Created by dell
 */
@Service 
public class BookService {
    
}
```

**BookController**

```java
package com.zhuang.controller;

import org.springframework.stereotype.Controller;

/**
 * @Classname BookController
 * @Description BookController
 * @Date 2021/4/3 13:08
 * @Created by dell
 */
@Controller
public class BookController {
}
```



**在MainTest中添加下列代码**

```java
String[] names = applicationContext.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-20-02.png)

可以看到，除了输出我们自己创建的bean的名称之外，也输出了Spring内部使用的一些重要的bean的名称。



### 2.1 @ComponentScan 注解

**扫描时排除注解标注的类**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-23-34.png)

```java
Filter[] includeFilters()  //包括哪些规则

Filter[] excludeFilters()  //排除哪些规则
```



**自定义过滤器**

**MyTypeFliter**

```java
FilterType.ASSIGNABLE_TYPE //按照给定的类型进行包含或者排除
FilterType.ANNOTATION //注解形式
FilterType.REGE //按照正则表达式进行包含或者排除
metadataReader //读取当前正在扫描的类信息
metadataReaderFactory //可以获取到任何其他的信息
    
```

```java
package com.zhuang.config;

import org.springframework.core.io.Resource;
import org.springframework.core.type.AnnotationMetadata;
import org.springframework.core.type.ClassMetadata;
import org.springframework.core.type.classreading.MetadataReader;
import org.springframework.core.type.classreading.MetadataReaderFactory;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;

/**
 * @Classname MyTypeFliter
 * @Description 自定义过滤器
 * @Date 2021/4/3 13:26
 * @Created by dell
 */

public class MyTypeFliter implements TypeFilter {
    @Override
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
        //获取当前的注解的信息
        AnnotationMetadata annotationMetadata = metadataReader.getAnnotationMetadata();
        //获取当前正在扫描的类的信息
        ClassMetadata classMetadata = metadataReader.getClassMetadata();
        //获取当前类的资源
        Resource resource = metadataReader.getResource();
        String className=classMetadata.getClassName();
        System.out.println("className-->"+className);
        return false;
    }
}
```

**MainConfig 中添加下列代码**

```java
@ComponentScan(value = "com.zhuang",includeFilters=
        {@ComponentScan.Filter(type = FilterType.CUSTOM,classes = MyTypeFliter.class)}
        )  //包扫描注解
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_17-35-52.png)

**小结**

我们可以使用@ComponentScan注解来指定Spring扫描哪些包，可以使用excludeFilters()方法来指定扫描时排除哪些组件，也可以使用includeFilters()方法来指定扫描时只包含哪些组件。当使用includeFilters()方法指定只包含哪些组件时，需要禁用掉默认的过滤规则

## 3. 使用@Scope注解设置组件的作用域

Spring容器中的组件默认是单例的，在Spring启动时就会实例化并初始化这些对象，并将其放到Spring容器中，之后，每次获取对象时，直接从Spring容器中获取，而不再创建对象。如果每次从Spring容器中获取对象时，都要创建一个新的实例对象，那么该如何处理呢？此时就需要使用@Scope注解来设置组件的作用域了

### 3.1@Scope注解概述

**从@Scope注解类的源码中可以看出，在@Scope注解中可以设置如下值：**

```java
ConfigurableBeanFactory#SCOPE_PROTOTYPE
ConfigurableBeanFactory#SCOPE_SINGLETON
org.springframework.web.context.WebApplicationContext#SCOPE_REQUEST
org.springframework.web.context.WebApplicationContext#SCOPE_SESSION
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_19-57-19.png)

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_19-59-12.png)

### 3.2 单实例bean作用域

测试类中测试是否为单例对象

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig.class);
        Person bean = applicationContext.getBean(Person.class);
        Person bean1 = applicationContext.getBean(Person.class);
        System.out.println(bean==bean1); //true
```

**对象在Spring容器中默认是单实例的，Spring容器在启动时就会将实例对象加载到Spring容器中，之后，每次从Spring容器中获取实例对象，都是直接将对象返回，而不必再创建新的实例对象了**



### 3.3 多实例bean作用域

修改@Scope注解为 （"prototype"）

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-05-50.png)

再次测试

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig.class);
        Person bean = applicationContext.getBean(Person.class);
        Person bean1 = applicationContext.getBean(Person.class);
        System.out.println(bean==bean1); //false
```



### 3.4 单实例bean注意的事项
单实例bean是整个应用所共享的，所以需要考虑到线程安全问题，SpringMVC中的Controller默认是单例的，有些开发者在Controller中创建了一些变量，那么这些变量实际上就变成共享的了，Controller又可能会被很多线程同时访问，这些线程并发去修改Controller中的共享变量，此时很有可能会出现数据错乱的问题，所以使用的时候需要特别注意。

### 3.5 多实例bean注意的事项
多实例bean每次获取的时候都会重新创建，如果这个bean比较复杂，创建时间比较长，那么就会影响系统的性能，因此这个地方需要注意点。



## 4. 懒加载

懒加载就是Spring容器启动的时候，先不创建对象，在第一次使用（获取）bean的时候再来创建对象，并进行一些初始化

测试一把 没有加入懒加载@Lazy 注解

```java
@Bean("kangxiaozhuang") //给容器中注册bean
    @Scope
    public Person person(){
        System.out.println("给容器添加Bean对象...");
        return new Person("康康",16);
    }
```

```java
@SuppressWarnings("sources")
    @Test
    public void test01() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig.class);
        Person bean = applicationContext.getBean(Person.class);
        System.out.println("IOC容器创建完成");
    }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-17-13.png)

**可以看到，单实例bean在Spring容器启动的时候就会被创建，并且还加载到Spring容器中去了。**



**懒加载模式**

添加注解@Lazy

```java
	@Lazy
    @Bean("kangxiaozhuang") //给容器中注册bean
    @Scope
    public Person person(){
        System.out.println("给容器添加Bean对象...");
        return new Person("康康",16);
    }
```

```java
@SuppressWarnings("sources")
    @Test
    public void test01() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfig.class);
        System.out.println("IOC容器创建完成");
    }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-23-07.png)

**此时只是打印出一条信息，说明此时只创建了IOC容器，而并没有创建bean对象**

使用@Lazy注解标注后，单实例bean对象只是在第一次从Spring容器中获取时被创建，以后每次获取bean对象时，直接返回创建好的对象

小结

**懒加载，也称延时加载，仅针对单实例bean生效。 单实例bean是在Spring容器启动的时候加载的，添加@Lazy注解后就会延迟加载，在Spring容器启动的时候并不会加载，而是在第一次使用此bean的时候才会加载，但当你多次获取bean的时候并不会重复加载，只是在第一次获取的时候才会加载，这不是延迟加载的特性，而是单实例bean的特性**



## 5. 按照条件向Spring容器中注册Bean

- 当bean是单实例，并且没有设置懒加载时，**Spring容器启动时，就会实例化bean**，并将bean注册到IOC容器中，以后每次从IOC容器中获取bean时，**直接返回IOC容器中的bean，而不用再创建新的bean了。**

- 若bean是单实例，并且使用@Lazy注解设置了懒加载，则Spring容器启动时，不会立即实例化bean，自然就不会将bean注册到IOC容器中了，**只有第一次获取bean的时候，才会实例化bean，并且将bean注册到IOC容器中。**

- 若bean是多实例，则Spring容器启动时，不会实例化bean，也不会将bean注册到IOC容器中，**只是在以后每次从IOC容器中获取bean的时候，都会创建一个新的bean返回。**

### 5.1 @Conditional注解

可以按照一定的条件进行判断，满足条件向容器中注册bean，不满足条件就不向容器中注册bean

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-30-09.png)

**从@Conditional注解的源码来看，@Conditional注解不仅可以添加到类上，也可以添加到方法上。**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-33-57.png)

**所含方法**

```java
		// 1. 获取到bean的创建工厂（能获取到IOC容器使用到的BeanFactory，它就是创建对象以及进行装配的工厂）
        ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
        // 2. 获取到类加载器
        ClassLoader classLoader = context.getClassLoader();
        // 3. 获取当前环境信息，它里面就封装了我们这个当前运行时的一些信息，包括环境变量，以及包括虚拟机的一些变量
        Environment environment = context.getEnvironment();
        // 4. 获取到bean定义的注册类
        BeanDefinitionRegistry registry = context.getRegistry();
```

```java
boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata);
点进ConditionContext参数中看源码 
```



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-37-08.png)

**所有方法**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-47-36.png)



其他注解拓展

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-49-32.png)



## 6. 使用@Import注解给容器中导入一个组件

### 6.1 注册bean的方式
向Spring容器中注册bean通常有以下几种方式：

包扫描+给组件标注注解（@Controller、@Servcie、@Repository、@Component），但这种方式比较有局限性，局限于我们自己写的类
@Bean注解，通常用于导入第三方包中的组件
@Import注解，快速向Spring容器中导入一个组件
### 6.2 @Import注解概述
Spring 3.0之前，创建bean可以通过XML配置文件与扫描特定包下面的类来将类注入到Spring IOC容器内。而在Spring 3.0之后提供了JavaConfig的方式，也就是将IOC容器里面bean的元信息以Java代码的方式进行描述，然后我们可以通过@Configuration与@Bean这两个注解配合使用来将原来配置在XML文件里面的bean通过Java代码的方式进行描述。

### 6.3 @Import注解的使用方式

**@Import注解的三种用法主要包括：**

1. 直接填写class数组的方式
2. **ImportSelector接口的方式**
3. ImportBeanDefinitionRegistrar接口方式，即手工注册bean到容器中



创建一个Student类

```java
public class Student {
}
```

测试一把

```java
String[] names = applicationContext.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_20-58-04.png)

添加注解

```java
@Import(Student.class)
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-01-36.png)

**@Import 支持 一次性导入多个类 用,分隔**



## 7. 在@Import注解中使用ImportSelector接口导入bean

源码

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-05-29.png)

- 其主要作用是收集需要导入的配置类，**selectImports()方法的返回值就是我们向Spring容器中导入的类的全类名。**如果该接口的实现类同时实现EnvironmentAware，BeanFactoryAware，BeanClassLoaderAware或者ResourceLoaderAware，那么在调用其selectImports()方法之前先调用上述接口中对应的方法，**如果需要在所有的@Configuration处理完再导入时，那么可以实现DeferredImportSelector接口。**

- 在ImportSelector接口的selectImports()方法中，**存在一个AnnotationMetadata类型的参数，这个参数能够获取到当前标注@Import注解的类的所有注解信息，也就是说不仅能获取到@Import注解里面的信息，还能获取到其他注解的信息。**



## 8. 在@Import注解中使用ImportBeanDefinitionRegistrar向容器中注册bean

源码

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-09-21.png)

使用步骤

ImportBeanDefinitionRegistrar需要配合@Configuration和@Import这俩注解，其中，@Configuration注解定义Java格式的Spring配置文件，@Import注解导入实现了ImportBeanDefinitionRegistrar接口的类

```java
@Import({Student.class, MyImportBeanDefinitionRegistrar.class})
```

**自定义 MyImportBeanDefinitionRegistrar**

```java
package com.zhuang.condition;

import com.zhuang.bean.School;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.beans.factory.support.RootBeanDefinition;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.core.type.AnnotationMetadata;

/**
 * @Classname MyImportBeanDefinitionRegistrar
 * @Description MyImportBeanDefinitionRegistrar 实现 ImportBeanDefinitionRegistrar
 * @Date 2021/4/3 21:10
 * @Created by dell
 */

public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    /**
     * AnnotationMetadata：当前类的注解信息
     * BeanDefinitionRegistry：BeanDefinition注册类
     *
     * 我们可以通过调用BeanDefinitionRegistry接口中的registerBeanDefinition方法，手动注册所有需要添加到容器中的bean
     */
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        boolean definition1 = registry.containsBeanDefinition("com.zhuang.bean.Student");
        boolean definition2 = registry.containsBeanDefinition("com.zhuang.bean.Teacher");
        if (definition1 && definition2){
            //指定bean的定义信息 包括类型 作用域
            //RootBeanDefinition 是BeanDefinition接口的子实现类
            BeanDefinition rootBeanDefinition = new RootBeanDefinition(School.class);
            //注册一个bean 指定其名称
            registry.registerBeanDefinition("school",rootBeanDefinition);
			
        }
    }
}
```

**输出 school **

说明Spring容器中已经成功注册了以school命名的bean



## 9. 使用FactoryBean向Spring容器中注册bean

一般情况下，**Spring是通过反射机制利用bean的class属性指定实现类来实例化bean的**。在某些情况下，实例化bean过程比较复杂，如果按照传统的方式，那么则需要在标签中提供大量的配置信息，配置方式的灵活性是受限的，这时采用编码的方式可以得到一个更加简单的方案。**Spring为此提供了一个org.springframework.bean.factory.FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化bean的逻辑。**

**FactoryBean 源码**

```java
T getObject() //返回由FactoryBean创建的bean实例，如果isSingleton()返回true，那么该实例会放到Spring容器中单实例缓存池中
boolean isSingleton() //返回由FactoryBean创建的bean实例的作用域是singleton还是prototype
Class getObjectType() //返回FactoryBean创建的bean实例的类型
```

**创建SchoolFactory 实现FactoryBean接口**

```java
package com.zhuang.bean;

import org.springframework.beans.factory.FactoryBean;

/**
 * @Classname SchoolFactory
 * @Description   SchoolFactory 实现FactoryBean接口
 * @Date 2021/4/3 21:29
 * @Created by dell
 */

public class SchoolFactory implements FactoryBean<School> {
    /*
    T 泛型 指定我们创建什么对象
     */
    @Override
    public School getObject() throws Exception {
        System.out.println("SchoolFactory-->getObject()");
        return new School();
    }

    @Override
    public Class<?> getObjectType() {
        //返回对象类型
        return School.class;
    }
    /*
    是否为单例 true 是
     */
    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

添加注解

```java
@Bean
    public SchoolFactory schoolFactory() {
        return new SchoolFactory();
    }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-34-56.png)

测试类中添加

```java
Object bean1 = applicationContext.getBean("schoolFactory");
        System.out.println(bean1.getClass());
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-39-37.png)

**虽然我在代码中使用@Bean注解注入的是SchoolFactory对象，但是实际上从Spring容器中获取到的bean对象却是调用SchoolFactory类中的getObject()方法获取到的School对象**



在SchoolFactory类中，我们将School对象设置为单实例bean，即让isSingleton()方法返回true。接下来，我们在测试类中的testImport()方法里面多次获取School对象，并判断一下多次获取的对象是否为同一对象

```java
Object bean1 = applicationContext.getBean("schoolFactory");
        Object bean2 = applicationContext.getBean("schoolFactory");
        System.out.println(bean1==bean2);
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-50-25.png)

**如何在Spring容器中获取到FactoryBean对象本身呢？**

只需要在获取工厂Bean本身时，在id前面加上&符号即可，例如&schoolFactoryBean

```java
 Object bean3 = applicationContext.getBean("&schoolFactory");
        System.out.println(bean3.getClass());
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-53-32.png)

**进入BeanFactory接口中寻找答案**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_21-54-24.png)



## 10. 使用@Bean注解指定初始化和销毁的方法

### 10.1bean的生命周期
通常意义上讲的bean的生命周期，指的是bean从创建到初始化，经过一系列的流程，最终销毁的过程。只不过，在Spring中，bean的生命周期是由Spring容器来管理的。**在Spring中，我们可以自己来指定bean的初始化和销毁的方法。我们指定了bean的初始化和销毁方法之后，当容器在bean进行到当前生命周期的阶段时，会自动调用我们自定义的初始化和销毁方法。**



### 10.2 如何定义初始化和销毁方法？

xml配置

```xml
<bean id="person" class="com.zhuang.bean.Person" init-method="init" destroy-method="destroy">
    <property name="age" value="18"></property>
    <property name="name" value="zhaungkang"></property>
</bean>
```

在我们自己写的Person类中，需要存在init()方法和destroy()方法。而且Spring中还规定，这里的init()方法和destroy()方法必须是无参方法，但可以抛出异常。



新建Car类

```java
package com.zhuang.config;

/**
 * @Classname Car
 * @Description 汽车类 bean的生命周期
 * @Date 2021/4/3 22:00
 * @Created by dell
 */

public class Car {

    public Car() {
        System.out.println("car 创建了...");
    }

    public void init() {
        System.out.println("car init...");
    }

    public void destroy() {
        System.out.println("car destroy...");
    }
}
```

添加@Bean注解

```java
@Bean
    public Car car() {
        return new Car();
    }
```

测试一把

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-03-55.png)

可以看到，在Spring容器创建完成时，会自动调用单实例bean的构造方法，对单实例bean进行了实例化操作。

**总之，对于单实例bean来说，会在Spring容器启动的时候创建对象；对于多实例bean来说，会在每次获取bean的时候创建对象。**



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-07-35.png)



### 10.3 指定初始化和销毁方法的使用场景
一个典型的使用场景就是对于数据源的管理。例如，在配置数据源时，在初始化的时候，会对很多的数据源的属性进行赋值操作；在销毁的时候，我们需要对数据源的连接等信息进行关闭和清理。这个时候，我们就可以在自定义的初始化和销毁方法中来做这些事情了！

- 初始化和销毁方法调用的时机
  - 初始化方法和销毁方法是在什么时候被调用的啊？

bean对象的初始化方法调用的时机：

- 对象创建完成，如果对象中存在一些属性，并且这些属性也都赋好值之后，那么就会调用bean的初始化方法。对于单实例bean来说，在Spring容器创建完成后，Spring容器会自动调用bean的初始化方法；对于多实例bean来说，在每次获取bean对象的时候，调用bean的初始化方法。

bean对象的销毁方法调用的时机：

- 对于单实例bean来说，在容器关闭的时候，会调用bean的销毁方法；对于多实例bean来说，Spring容器不会管理这个bean，也就不会自动调用这个bean的销毁方法了。可以手动调用多实例bean的销毁方法。
  

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-18-54.png)

**添加getBean()**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-19-56.png)



**可以看到，多实例的bean在容器关闭的时候是不进行销毁的，也就是说你每次获取时，IOC容器帮你创建出对象交还给你，至于要什么时候销毁这是你自己的事，Spring容器压根就不会再管理这些多实例的bean了**



## 11. 使用InitializingBean和DisposableBean来管理bean的生命周期

### 11.1 InitializingBean接口

Spring中提供了一个InitializingBean接口，该接口为bean提供了属性初始化后的处理方法，它只包括afterPropertiesSet方法，凡是继承该接口的类，在bean的属性初始化后都会执行该方法。

源码

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-29-38.png)

**定位到org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory中**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-41-40.png)

- Spring为bean提供了两种初始化的方式，实现InitializingBean接口（也就是要实现该接口中的afterPropertiesSet方法），或者在配置文件或@Bean注解中通过init-method来指定，两种方式可以同时使用。
- 实现InitializingBean接口是直接调用afterPropertiesSet()方法，与通过反射调用init-method指定的方法相比，效率相对来说要高点。但是init-method方式消除了对Spring的依赖。
- 如果调用afterPropertiesSet方法时出错，那么就不会调用init-method指定的方法了。

**Spring为bean提供了两种初始化的方式，第一种方式是实现InitializingBean接口（也就是要实现该接口中的afterPropertiesSet方法），第二种方式是在配置文件或@Bean注解中通过init-method来指定，这两种方式可以同时使用，同时使用先调用afterPropertiesSet方法，后执行init-method指定的方法。**



### 11.2 DisposableBean接口

实现org.springframework.beans.factory.DisposableBean接口的bean在销毁前，Spring将会调用DisposableBean接口的destroy()方法

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-47-34.png)

可以看到，在DisposableBean接口中只定义了一个destroy()方法。

在bean生命周期结束前调用destroy()方法做一些收尾工作，亦可以使用destroy-method。**前者与Spring耦合高，使用类型强转.方法名()，效率高；后者耦合低，使用反射，效率相对来说较低。**



**DisposableBean接口注意事项**

- 多实例bean的生命周期不归Spring容器来管理，这里的DisposableBean接口中的方法是由Spring容器来调用的，所以如果一个多实例bean实现了DisposableBean接口是没有啥意义的，因为相应的方法根本不会被调用，当然了，在XML配置文件中指定了destroy方法，也是没有任何意义的。所以，在多实例bean情况下，Spring是不会自动调用bean的销毁方法的。



### 11.3 单实例案例

创建一个Cat类实现 InitializingBean, DisposableBean

```java
package com.zhuang.bean;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

/**
 * @Classname Cat
 * @Description 猫类 实现 InitializingBean, DisposableBean
 * @Date 2021/4/3 22:51
 * @Created by dell
 */

public class Cat implements InitializingBean, DisposableBean {
    public Cat() {
        System.out.println("cat 创建了...");
    }

    @Override
    public void destroy() {
        System.out.println("cat destroy...");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("cat afterPropertiesSet...");
    }
}
```

配置类中添加下列代码

```java
@Bean()
    public Cat cat() {
        return new Cat();
    }
}
```

测试一把

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_22-55-58.png)

### 11.4 多实例案例

添加 prototype 注解

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-07-32.png)



## 12. @PostConstruct注解和@PreDestroy注解

### 12.1 @PostConstruct注解

@PostConstruct注解被用来修饰一个非静态的void()方法。被@PostConstruct注解修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。被@PostConstruct注解修饰的方法通常在构造函数之后，init()方法之前执行。

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-13-47.png)

**该注解的方法在整个bean初始化中的执行顺序**

- Constructor（构造方法）→@Autowired（依赖注入）→@PostConstruct（注释的方法）



### 12.2 @PreDestroy注解

被@PreDestroy注解修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。被@PreDestroy注解修饰的方法会在destroy()方法之后，Servlet被彻底卸载之前执行

**该注解的方法在整个bean初始化中的执行顺序**

- 调用destroy()方法→@PreDestroy→destroy()方法→bean销毁

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-14-59.png)

**@PostConstruct和@PreDestroy是Java规范JSR-250引入的注解，定义了对象的创建和销毁工作，同一期规范中还有@Resource注解，Spring也支持了这些注解。**

### 12.3 案例

创建Dog类

```java
package com.zhuang.bean;

import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

/**
 * @Classname Dog
 * @Description 狗类
 * @Date 2021/4/3 23:16
 * @Created by dell
 */
@Component
public class Dog {
    public Dog() {
        System.out.println("dog constructor...");
    }

    // 在对象创建完成并且属性赋值完成之后调用
    @PostConstruct
    public void init() {
        System.out.println("dog...@PostConstruct...");
    }

    // 在容器销毁（移除）对象之前调用
    @PreDestroy
    public void destory() {
        System.out.println("dog...@PreDestroy...");
    }
}
```

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-18-32.png)

被@PostConstruct注解修饰的方法是在bean创建完成并且属性赋值完成之后才执行的，而被@PreDestroy注解修饰的方法是在容器销毁bean之前执行的



## 13. BeanPostProcessor后置处理器

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-22-36.png)



从源码可以看出，BeanPostProcessor是一个接口，其中有两个方法，即**postProcessBeforeInitialization和postProcessAfterInitialization这两个方法**，这两个方法分别是在Spring容器中的bean初始化前后执行，**所以Spring容器中的每一个bean对象初始化前后，都会执行BeanPostProcessor接口的实现类中的这两个方法。**

也就是说，postProcessBeforeInitialization方法**会在bean实例化和属性设置之后，自定义初始化方法之前被调用**，而postProcessAfterInitialization方法**会在自定义初始化方法之后被调用**。当容器中存在多个BeanPostProcessor的实现类时，会按照它们在容器中注册的顺序执行。对于自定义的BeanPostProcessor实现类，还可以让其实现Ordered接口自定义排序。

因此我们可以在每个bean对象初始化前后，加上自己的逻辑。实现方式是自定义一个BeanPostProcessor接口的实现类，例如MyBeanPostProcessor，然后在该类的postProcessBeforeInitialization和postProcessAfterInitialization这俩方法中写上自己的逻辑。

### 13.1 BeanPostProcessor后置处理器实例

**创建自定义的MyBeanPostProcessor**

```java
package com.zhuang.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.stereotype.Component;

/**
 * @Classname MyBeanPostProcessor
 * @Description 后置处理器，在初始化前后进行处理工作
 * @Date 2021/4/3 23:25
 * @Created by dell
 */
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessBeforeInitialization..." + beanName + "=>" + bean);
        return null;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessAfterInitialization..." + beanName + "=>" + bean);
        return null;
    }
}
```

Cat类

```java
package com.zhuang.bean;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.stereotype.Component;

/**
 * @Classname Cat
 * @Description 猫类 实现 InitializingBean, DisposableBean
 * @Date 2021/4/3 22:51
 * @Created by dell
 */
@Component
public class Cat implements InitializingBean, DisposableBean {
    public Cat() {
        System.out.println("cat 创建了...");
    }

    @Override
    public void destroy() {
        System.out.println("cat destroy...");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("cat afterPropertiesSet...");
    }
}
```

Dog类

```java
package com.zhuang.bean;

import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

/**
 * @Classname Dog
 * @Description 狗类
 * @Date 2021/4/3 23:16
 * @Created by dell
 */
@Component
public class Dog {
    public Dog() {
        System.out.println("dog constructor...");
    }

    // 在对象创建完成并且属性赋值完成之后调用
    @PostConstruct
    public void init() {
        System.out.println("dog...@PostConstruct...");
    }

    // 在容器销毁（移除）对象之前调用
    @PreDestroy
    public void destory() {
        System.out.println("dog...@PreDestroy...");
    }
}
```

Car类

```java
package com.zhuang.config;

import org.springframework.stereotype.Component;

/**
 * @Classname Car
 * @Description 汽车类 bean的生命周期
 * @Date 2021/4/3 22:00
 * @Created by dell
 */
@Component
public class Car {

    public Car() {
        System.out.println("car 创建了...");
    }

    public void init() {
        System.out.println("car init...");
    }

    public void destroy() {
        System.out.println("car destroy...");
    }
}
```



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-28-41.png)



### 13.2 BeanPostProcessor后置处理器作用
- 后置处理器可用于bean对象初始化前后进行逻辑增强。Spring提供了BeanPostProcessor接口的很多实现类，例如AutowiredAnnotationBeanPostProcessor用于@Autowired注解的实现，AnnotationAwareAspectJAutoProxyCreator用于Spring AOP的动态代理等等。

- 除此之外，我们还可以自定义BeanPostProcessor接口的实现类，在其中写入咱们需要的逻辑。下面我会以AnnotationAwareAspectJAutoProxyCreator为例，简单说明一下后置处理器是怎样工作的。

- 我们都知道spring AOP的实现原理是动态代理，最终放入容器的是代理类的对象，而不是bean本身的对象，那么Spring是什么时候做到这一步的呢？就是在AnnotationAwareAspectJAutoProxyCreator后置处理器的postProcessAfterInitialization方法中，即bean对象初始化完成之后，后置处理器会判断该bean是否注册了切面，若是，则生成代理对象注入到容器中。这一部分的关键代码是在哪儿呢？**我们定位到AbstractAutoProxyCreator抽象类中的postProcessAfterInitialization方法处便能看到了**
  

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-38-19.png)

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-39-58.png)



## 14. BeanPostProcessor的执行流程

### 14.1

**通过@Bean指定init-method和destroy-method**

### 14.2

**通过让bean实现InitializingBean和DisposableBean接口**

### 14.3

**使用JSR-250规范里面定义的@PostConstruct和@PreDestroy这俩注解**

### 14.4

**通过让bean实现BeanPostProcessor接口**



**Bean的生命周期**

- bean的实例化：调用bean的构造方法，我们可以在bean的无参构造方法中执行相应的逻辑。
- bean的初始化：在初始化时，可以通过BeanPostProcessor的postProcessBeforeInitialization()方法和postProcessAfterInitialization()方法进行拦截，执行自定义的逻辑；通过@PostConstruct注解、InitializingBean和init-method来指定bean初始化前后执行的方法，在该方法中咱们可以执行自定义的逻辑。
- bean的销毁：可以通过@PreDestroy注解、DisposableBean和destroy-method来指定bean在销毁前执行的方法，在该方法中咱们可以执行自定义的逻辑。
  

## 15. BeanPostProcessor在Spring底层是如何使用

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-22-36.png)

在BeanPostProcessor接口中，提供了两个方法：postProcessBeforeInitialization()方法和postProcessAfterInitialization()方法

- postProcessBeforeInitialization()方法会在bean初始化之前调用
- postProcessAfterInitialization()方法会在bean初始化之后调用
  

要想使用ApplicationContextAwareProcessor类向组件中注入IOC容器，我们就不得不提Spring中的另一个接口了，即ApplicationContextAware。如果需要向组件中注入IOC容器，那么可以让组件实现ApplicationContextAware接口。

```java
@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException { 
		// TODO Auto-generated method stub
		this.applicationContext = applicationContext;
	}
```

### 15.1 **ApplicationContextAwareProcessor**

** **ApplicationContextAwareProcessor类中对于postProcessBeforeInitialization()方法的实现**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-51-05.png)

在bean初始化之前，首先对当前bean的类型进行判断，如果当前bean的类型不是EnvironmentAware，不是EmbeddedValueResolverAware，不是ResourceLoaderAware，不是ApplicationEventPublisherAware，不是MessageSourceAware，也不是ApplicationContextAware，那么直接返回bean。**如果是上面类型中的一种类型，那么最终会调用invokeAwareInterfaces()方法，并将bean传递给该方法。**


**看invokeAwareInterfaces()方法的源码**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-52-42.png)

可以看到invokeAwareInterfaces()方法的源码比较简单，就是判断当前bean属于哪种接口类型，然后将bean强转为哪种接口类型的对象，接着调用接口中的方法，将相应的参数传递到接口的方法中。这里，我们在创建Dog类时，实现的是ApplicationContextAware接口，所以，在invokeAwareInterfaces()方法中，会执行如下的逻辑代码。

```java
if (bean instanceof ApplicationContextAware) {
				((ApplicationContextAware) bean).setApplicationContext(this.applicationContext);
			}
```



### 15.2 BeanValidationPostProcessor类

org.springframework.validation.beanvalidation.BeanValidationPostProcessor类主要是**用来为bean进行校验操作的**，当我们创建bean，并为bean赋值后，我们可以通过BeanValidationPostProcessor类为bean进行校验操作


![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-56-22.png)

在postProcessBeforeInitialization()方法和postProcessAfterInitialization()方法中的主要逻辑**都是调用doValidate()方法对bean进行校验**，只不过在这两个方法中都会对afterInitialization这个boolean类型的成员变量进行判断，**若afterInitialization的值为false，则在postProcessBeforeInitialization()方法中调用doValidate()方法对bean进行校验；若afterInitialization的值为true，则在postProcessAfterInitialization()方法中调用doValidate()方法对bean进行校验。**


![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-03_23-57-52.png)



### 15.3 InitDestroyAnnotationBeanPostProcessor类

org.springframework.beans.factory.annotation.InitDestroyAnnotationBeanPostProcessor类主要用来处理@PostConstruct注解和@PreDestroy注解

### 15.4 AutowiredAnnotationBeanPostProcessor类

org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor类主要是用于处理标注了@Autowired注解的变量或方法。



## 16. 使用@Value注解为bean的属性赋值

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-05-54.png)



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-05-27.png)

**Value源码**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-07-26.png)

- 注入普通字符串

@Value("张三")
private String name; // 注入普通字符串

- 注入操作系统属性

@Value("#{systemProperties['os.name']}")
private String systemPropertiesName; // 注入操作系统属性

- 注入SpEL表达式结果

@Value("#{ T(java.lang.Math).random() * 100.0 }")
private double randomNumber; //注入SpEL表达式结果


- 注入其他bean中属性的值

@Value("#{person.name}")
private String username; // 注入其他bean中属性的值，即注入person对象的name属性中的值


- 注入文件资源

@Value("classpath:/config.properties")
private Resource resourceFile; // 注入文件资源


- 注入URL资源

@Value("http://www.baidu.com")
private Resource url; // 注入URL资源


- `#{···}`：用于执行SpEl表达式，并将内容赋值给属性
- `${···}`：主要用于加载外部属性文件中的值
- `${···}`和`#{···}`可以混合使用，但是必须`#{}`在外面，`${}`在里面



## 17. 使用@PropertySource加载配置文件

@PropertySource注解是Spring 3.1开始引入的配置类注解。通过@PropertySource注解可以将properties配置文件中的key/value存储到Spring的Environment中，Environment接口提供了方法去读取配置文件中的值，参数是properties配置文件中定义的key值

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-10-50.png)



**@PropertySources注解的源码比较简单，只有一个PropertySource[]数组类型的value属性**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-11-57.png)

例如

```java
@PropertySources(value={
    @PropertySource(value={"classpath:/person.properties"}),
    @PropertySource(value={"classpath:/car.properties"}),
})
```



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-23-44.png)

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-24-12.png)

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-24-04.png)

测试一把

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-23-04.png)



### 17.1 使用Environment获取值

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-27-00.png)



## 18. 使用@Autowired、@Qualifier、@Primary这三大注解自动装配组件

### 18.1 @Autowired注解

@Autowired注解可以对类成员变量、方法和构造函数进行标注，完成自动装配的工作。@Autowired注解可以放在类、接口以及方法上。

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-30-17.png)

**对@Autowired注解说明一下：**

@Autowired注解默认是优先按照类型去容器中找对应的组件，相当于是调用了如下这个方法：

- applicationContext.getBean(类名.class);
  若找到则就赋值。

如果找到多个相同类型的组件，那么是将属性名称作为组件的id，到IOC容器中进行查找，这时就相当于是调用了如下这个方法：

- applicationContext.getBean("组件的id");
  

### 18.2 @Qualifier注解

@Autowired是**根据类型进行自动装配的**，如果需要按名称进行装配，那么就需要配合@Qualifier注解来使用了

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-32-20.png)



### 18.3 @Primary注解

在Spring中使用注解时，常常会使用到@Autowired这个注解，它默认是根据类型Type来自动注入的。但有些特殊情况，对同一个接口而言，可能会有几种不同的实现类，而在默认只会采取其中一种实现的情况下，就可以**使用@Primary注解来标注优先使用哪一个实现类。**
![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-34-02.png)

**自动装配**

Spring组件的自动装配就是**Spring利用依赖注入，也就是我们通常所说的DI，完成对IOC容器中各个组件的依赖关系赋值。**



## 19. 使用@Resource注解和@Inject注解

### 19.1 @Resource注解
@Resource注解是Java规范里面的，也可以说它是JSR250规范里面定义的一个注解。该注解默认按照名称进行装配，名称可以通过name属性进行指定，如果没有指定name属性，当注解写在字段上时，那么默认取字段名将其作为组件的名称在IOC容器中进行查找，如果注解写在setter方法上，那么默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的一点是，如果name属性一旦指定，那么就只会按照名称进行装配

**源码**

```java
package javax.annotation;

import java.lang.annotation.*;
import static java.lang.annotation.ElementType.*;
import static java.lang.annotation.RetentionPolicy.*;


@Target({TYPE, FIELD, METHOD})
@Retention(RUNTIME)
public @interface Resource {
    
    String name() default "";

   
    String lookup() default "";

  
    Class<?> type() default java.lang.Object.class;

    /**
     * The two possible authentication types for a resource.
     */
    enum AuthenticationType {
            CONTAINER,
            APPLICATION
    }

  
    AuthenticationType authenticationType() default AuthenticationType.CONTAINER;

    boolean shareable() default true;

   
    String mappedName() default "";

   
    String description() default "";
}
```

### 19.2 @Inject注解

@Inject注解也是Java规范里面的，也可以说它是JSR330规范里面定义的一个注解。该注解默认是根据参数名去寻找bean注入，支持Spring的@Primary注解优先注入，@Inject注解还可以增加@Named注解指定要注入的bean

使用前先导入相关依赖

```xml
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```

**注意**

- @Resource注解和@Autowired注解的功能是一样的，都能实现自动装配，只不过@Resource注解默认是按照组件名称（即属性的名称）进行装配的。虽然@Resource注解具备自动装配这一功能，但是它是不支持@Primary注解优先注入的功能的，而且也不能像@Autowired注解一样能添加required=false属性

- @Inject注解和@Autowired注解的功能是一样的，都能实现自动装配，而且它俩都支持@Primary注解优先注入的功能。只不过，@Inject注解不能像@Autowired注解一样能添加`required=false`属性，因为它里面没啥属性

**@Resource和@Inject与@Autowired注解的区别**
**不同点**

- @Autowired是Spring中的专有注解，而@Resource是Java中JSR250规范里面定义的一个注解，@Inject是Java中JSR330规范里面定义的一个注解
- @Autowired支持参数required=false，而@Resource和@Inject都不支持
- @Autowired和@Inject支持@Primary注解优先注入，而@Resource不支持
- @Autowired通过@Qualifier指定注入特定bean，@Resource可以通过参数name指定注入bean，而@Inject需要通过@Named注解指定注入bean

**相同点**

- 三种注解都可以实现bean的自动装配。



**如果方法只有一个IOC容器中的对象作为参数，当@Autowired注解标注在这个方法的参数上时，我们可以将@Autowired注解省略掉。也就说@Bean注解标注的方法在创建对象的时候，方法参数的值是从IOC容器中获取的，此外，标注在这个方法的参数上的@Autowired注解可以省略。**

## 20. 使用@Profile注解实现开发、测试和生产环境的配置和切换

### 20.1 @Profile注解概述
在容器中如果存在同一类型的多个组件，那么可以使用@Profile注解标识要获取的是哪一个bean。也可以说@Profile注解是Spring为我们提供的可以根据当前环境，动态地激活和切换一系列组件的功能。这个功能在不同的环境使用不同的变量的情景下特别有用，例如，开发环境、测试环境、生产环境使用不同的数据源，在不改变代码的情况下，可以使用这个注解来动态地切换要连接的数据库

![]()![Snipaste_2021-04-04_00-44-16](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_00-44-16.png)





# 二，AOP

## 1. 搭建一个AOP测试环境

AOP概念

AOP（Aspect Orient Programming），直译过来就是面向切面编程。AOP是一种编程思想，是面向对象编程（OOP）的一种补充。面向对象编程将程序抽象成各个层次的对象，而面向切面编程是将程序抽象成各个切面。

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-40-39.png)

**1.先导入相关依赖**

```xml
		<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
```

**2.定义目标类**

```java
package com.zhuang.aop;

/**
 * @Classname MathCalculator
 * @Description 计算器类 测试aop
 * @Date 2021/4/4 12:52
 * @Created by dell
 */

public class MathCalculator {

    public int div(int i, int j) {
        System.out.println("MathCalculator...div...");
        return i / j;
    }
}
```

**3.配置类**

```java
package com.zhuang.config;

import com.zhuang.aop.LogAspects;
import com.zhuang.aop.MathCalculator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

/**
 * @Classname MainConfigOfAOP
 * @Description 用一句话描述类的作用
 * @Date 2021/4/4 12:55
 * @Created by dell
 */
@EnableAspectJAutoProxy
@Configuration
public class MainConfigOfAOP {
    /*
     AOP：面向切面编程，其底层就是动态代理
      指在程序运行期间动态地将某段代码切入到指定方法指定位置进行运行的编程方式。
     */
    // 将业务逻辑类（目标方法所在类）加入到容器中
    @Bean
    public MathCalculator calculator() {
        return new MathCalculator();
    }

    // 将切面类加入到容器中
    @Bean
    public LogAspects logAspects() {
        return new LogAspects();
    }
}
```

**4.定义切面类**

```java
package com.zhuang.aop;

import org.aspectj.lang.annotation.*;

/**
 * @Classname LogAspects
 * @Description 切面类
 * @Date 2021/4/4 12:53
 * @Created by dell
 */
@Aspect //告诉Spring是一个切面类
public class LogAspects {

    @Pointcut("execution(public int com.zhuang.aop.MathCalculator.*(..))")
    public void pointCut() {

    }
    // @Before：在目标方法（即div方法）运行之前切入，public int com.meimeixia.aop.MathCalculator.div(int, int)这一串就是切入点表达式，指定在哪个方法切入
    @Before("pointCut()")
    public void logStart() {
        System.out.println("除法运行......@Before，参数列表是：{}");
    }

    // 在目标方法（即div方法）结束时被调用
    @After("pointCut()")
    public void logEnd() {
        System.out.println("除法结束......@After");
    }

    // 在目标方法（即div方法）正常返回了，有返回值，被调用
    @AfterReturning("pointCut()")
    public void logReturn() {
        System.out.println("除法正常返回......@AfterReturning，运行结果是：{}");
    }

    // 在目标方法（即div方法）出现异常，被调用
    @AfterThrowing("pointCut()")
    public void logException() {
        System.out.println("除法出现异常......异常信息：{}");
    }
}
```

**结果**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-14-17.png)



**AOP中的通知方法及其对应的注解与含义如下：**

- 前置通知（对应的注解是@Before）：在目标方法运行之前运行
- 后置通知（对应的注解是@After）：在目标方法运行结束之后运行，无论目标方法是正常结束还是异常结束都会执行
- 返回通知（对应的注解是@AfterReturning）：在目标方法正常返回之后运行
- 异常通知（对应的注解是@AfterThrowing）：在目标方法运行出现异常之后运行
- 环绕通知（对应的注解是@Around）：动态代理，我们可以直接手动推进目标方法运行（joinPoint.procced()）
  

<font color="red">必须告诉Spring哪个类是切面类只需要给切面类上加上一个@Aspect注解即可</font>

```java
package com.zhuang.aop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;

import java.util.Arrays;

/**
 * @Classname LogAspects
 * @Description 切面类
 * @Date 2021/4/4 12:53
 * @Created by dell
 */
@Aspect //告诉Spring是一个切面类
public class LogAspects {
	// 抽取公共的切面
    @Pointcut("execution(public int com.zhuang.aop.MathCalculator.*(..))")
    public void pointCut() {

    }
    // @Before：在目标方法（即div方法）运行之前切入，public int com.meimeixia.aop.MathCalculator.div(int, int)这一串就是切入点表达式，指定在哪个方法切入
    @Before("pointCut()")
    public void logStart(JoinPoint joinPoint) {
        Object[] args = joinPoint.getArgs();
        System.out.println(joinPoint.getSignature().getName()+"运行......@Before，参数列表是：{"+ Arrays.asList(args) +"}");
    }

    // 在目标方法（即div方法）结束时被调用
    @After("pointCut()")
    public void logEnd(JoinPoint joinPoint) {
        System.out.println(joinPoint.getSignature().getName()+"除法结束......@After");
    }

    // 在目标方法（即div方法）正常返回了，有返回值，被调用
 //   @AfterReturning("pointCut()")
    @AfterReturning(value="pointCut()", returning="result") // returning来指定我们这个方法的参数谁来封装返回值
    public void logReturn(JoinPoint joinPoint,Object result) {
        System.out.println(joinPoint.getSignature().getName()+"除法正常返回......@AfterReturning，运行结果是：{"+result+"}");
    }

    // 在目标方法（即div方法）出现异常，被调用
 //   @AfterThrowing("pointCut()")
    @AfterThrowing(value = "pointCut()",throwing = "exception")
    public void logException(JoinPoint joinPoint,Exception exception) {
        System.out.println(joinPoint.getSignature().getName()+"除法出现异常......异常信息：{"+exception+"}");
    }
}
```



**需要在切面类中打印出参数列表和运行结果 按照图片修改代码 **

<font color="red">**需要注意的是，JoinPoint参数一定要放在参数列表的第一位，否则Spring是无法识别的**</font>



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-23-50.png)

**结果**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-21-55.png)

修改测试类中的参数

```java
@Test
    public void tes04() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainConfigOfAOP.class);
        MathCalculator bean = applicationContext.getBean(MathCalculator.class);
        bean.div(6,0);//搞个异常出来
    }
}
```

**结果**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-28-59.png)



**小结AOP步骤**

- 将切面类和业务逻辑组件（目标方法所在类）都加入到容器中，并且要告诉Spring哪个类是切面类（标注了@Aspect注解的那个类）。
- 在切面类上的每个通知方法上标注通知注解，告诉Spring何时何地运行，当然最主要的是要写好切入点表达式，这个切入点表达式可以参照官方文档来写。
- 开启基于注解的AOP模式，即加上@EnableAspectJAutoProxy注解，这是最关键的一点。



## 2. @EnableAspectJAutoProxy注解

在配置类上添加@EnableAspectJAutoProxy注解，便能够开启注解版的AOP功能。也就是说，如果要使注解版的AOP功能起作用的话，那么就得需要在配置类上添加@EnableAspectJAutoProxy注解



**进入@EnableAspectJAutoProxy注解**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-42-16.png)

**从源码中可以看出，@EnableAspectJAutoProxy注解使用@Import注解给容器中引入了AspectJAutoProxyRegister组件**



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-43-29.png)

**看到AspectJAutoProxyRegistrar类实现了ImportBeanDefinitionRegistrar接口**

**可以通过ImportBeanDefinitionRegistrar接口实现将自定义的组件添加到IOC容器中**

得出结论

**<font color="red">@EnableAspectJAutoProxy注解使用AspectJAutoProxyRegistrar对象自定义组件，并将相应的组件添加到了IOC容器中。</font>**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-44-52.png)

程序已经暂停在断点位置

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-46-57.png)

在AspectJAutoProxyRegistrar类的registerBeanDefinitions()方法里面，首先调用了AopConfigUtils类的**registerAspectJAnnotationAutoProxyCreatorIfNecessary()方法来注册registry**，字面含义就是：如果需要的话，那么就注册一个AspectJAnnotationAutoProxyCreator组件。



进入到AopConfigUtils类的**registerAspectJAnnotationAutoProxyCreatorIfNecessary()**方法中

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-47-42.png)

在AopConfigUtils类的registerAspectJAnnotationAutoProxyCreatorIfNecessary()方法中，**直接调用了重载的registerAspectJAnnotationAutoProxyCreatorIfNecessary()方法**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-48-15.png)

可以看到在重载的registerAspectJAnnotationAutoProxyCreatorIfNecessary()方法中直接**调用了registerOrEscalateApcAsRequired()方法，并且在registerOrEscalateApcAsRequired()方法中，传入了AnnotationAwareAspectJAutoProxyCreator.class对象**
![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-49-31.png)

在registerOrEscalateApcAsRequired()方法中，接收到的Class对象的类型为org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator。

除此之外，我们还可以看到，在registerOrEscalateApcAsRequired()方法中会做一个判断，**即首先判断registry（也就是IOC容器）是否包含名称为org.springframework.aop.config.internalAutoProxyCreator的bean**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-11-22.png)

如果registry中包含名称为org.springframework.aop.config.internalAutoProxyCreator的bean，那么就进行相应的处理。从Spring的源码来看，就是将名称为org.springframework.aop.config.internalAutoProxyCreator的bean从容器中取出，并且判断cls对象的name值和apcDefinition的beanClassName值是否相等，若不相等，则获取apcDefinition和cls它俩的优先级，如果apcDefinition的优先级小于cls的优先级，那么将apcDefinition的beanClassName设置为cls的name值。

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-51-31.png)



这里会使用RootBeanDefinition来创建一个bean的定义信息（即beanDefinition），**并且将org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator的Class对象作为参数传递进来。**



最终在AopConfigUtils类的registerOrEscalateApcAsRequired()方法中，**会通过registry调用registerBeanDefinition()方法注册组件**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-53-29.png)

**注册的组件的类型是**org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator，组件的名字是org.springframework.aop.config.internalAutoProxyCreator。



查看AspectJAutoProxyRegistrar类中的**registerBeanDefinitions()方法的源码**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-56-22.png)

通过AnnotationConfigUtils类的**attributesFor()方法**来获取@EnableAspectJAutoProxy注解的信息。接着，就是**判断proxyTargetClass属性的值是否为true**，若为true则调用AopConfigUtils类的**forceAutoProxyCreatorToUseClassProxying()方法**；**继续判断exposeProxy属性的值是否为true**，若为true则调用AopConfigUtils类的**forceAutoProxyCreatorToExposeProxy()方法，**其实就是暴露一些什么代理的这些bean.

**综上，向Spring的配置类上添加@EnableAspectJAutoProxy注解之后，会向IOC容器中注册AnnotationAwareAspectJAutoProxyCreator，翻译过来就叫注解装配模式的AspectJ切面自动代理创建器。**

**AnnotationAwareAspectJAutoProxyCreator类的结构图**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_13-59-58.png)

**查看继承关系可以发现，此类实现了Aware与BeanPostProcessor接口，这两个接口都和Spring bean的初始化有关，由此可以推测此类的主要处理方法都来自于这两个接口中的实现方法。**

> AnnotationAwareAspectJAutoProxyCreator
>     ->AspectJAwareAdvisorAutoProxyCreator（父类）
>         ->AbstractAdvisorAutoProxyCreator（父类）
>             ->AbstractAutoProxyCreator（父类）
>                 implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware（两个接口）



## 3.为AnnotationAwareAspectJAutoProxyCreator组件里面和后置处理器以及Aware接口有关的方法打上断点

通过以上继承关系，我们也知道了，它最终会实现两个接口，分别是：

- BeanPostProcessor：后置处理器，即在bean初始化完成前后做些事情
- BeanFactoryAware：自动注入BeanFactory

也就是说，AnnotationAwareAspectJAutoProxyCreator不仅是一个后置处理器，还是一个BeanFactoryAware接口的实现类。那么我们就来分析它作为后置处理器，到底做了哪些工作，以及它作为BeanFactoryAware接口的实现类，又做了哪些工作



我们找到该抽象类，并在里面查找与Aware接口以及BeanPostProcessor接口有关的方法，结果都是可以找到的。**该抽象类中的setBeanFactory()方法就是与Aware接口有关的方法，因此我们将断点打在该方法上**



![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-32-48.png) 



找到该抽象类中与BeanPostProcessor接口有关的方法，即只要发现有与后置处理器相关的逻辑，就给所有与后置处理器有关的逻辑都打上断点。打的断点有两处，**一处是在postProcessBeforeInstantiation()方法上**



![Snipaste_2021-04-04_14-33-24](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-33-24.png)



**一处是在postProcessAfterInitialization()方法上**

![Snipaste_2021-04-04_14-34-26](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-34-26.png)

再来看它的子类（即AbstractAdvisorAutoProxyCreator），从顶层开始一点一点往上分析。

**在该抽象类中，我们只能找到一个与Aware接口有关的方法，即setBeanFactory()方法，虽然父类有setBeanFactory()方法，但是在这个子类里面已经把它重写了，因此最终调用的应该就是它**

![Snipaste_2021-04-04_14-35-15](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-35-15.png)



在重写的时候，在setBeanFactory()方法里面会调用一个initBeanFactory()方法。**除此之外，该抽象类中就没有跟后置处理器有关的方法了。**

接下来，来看AspectJAwareAdvisorAutoProxyCreator这个类了，但由于这个类里面没有跟BeanPostProcessor接口有关的方法，所以我们就不必看这个类了，略过。

接下来，我们就要来看最顶层的类了，即AnnotationAwareAspectJAutoProxyCreator。查看该类时，发现有这样一个initBeanFactory()方法，我们在该方法上打上一个断点

![Snipaste_2021-04-04_14-36-10](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-36-10.png)

**为什么在该类里面会有这个方法呢？因为我们在它的父类里面会调用setBeanFactory()方法，而在该方法里面又会调用initBeanFactory()方法，虽然父类里面有写，但是又被它的子类给重写了，所以说相当于父类中的setBeanFactory()方法还是得调用它。**



**为MainConfigOfAOP配置类中的如下两个方法打上断点**

![Snipaste_2021-04-04_14-36-53](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-36-53.png)



## 4. 分析创建和注册AnnotationAwareAspectJAutoProxyCreator的过程

**bebug一把**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-52-00.png)

![Snipaste_2021-04-04_14-52-31](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-52-31.png)

传入主配置类来创建IOC容器使用的是AnnotationConfigApplicationContext类的有参构造器，它具体分为下面三步：

- 首先使用无参构造器创建对象
- 再来把主配置类注册进来
- 最后调用refresh()方法刷新容器，刷新容器就是要把容器中的所有bean都创建出来，也就是说这就像初始化容器一样

重要的代码

```java
// Register bean processors that intercept bean creation.  注册bean的后置处理器
registerBeanPostProcessors(beanFactory);
```



![Snipaste_2021-04-04_14-53-40](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-53-40.png)



**定位到PostProcessorRegistrationDelegate类的registerBeanPostProcessors()方法中**

**怎么注册bean的后置处理器的。**

先按照类型拿到IOC容器中所有需要创建的后置处理器，即先获取IOC容器中已经定义了的需要创建对象的所有BeanPostProcessor。这可以从如下这行代码中得知：

```java
String[] postProcessorNames = beanFactory.getBeanNamesForType(BeanPostProcessor.class, true, false);
```

为什么IOC容器中会有一些已定义的BeanPostProcessor呢？这是因为在前面创建IOC容器时，需要先传入配置类，而我们在解析配置类的时候，由于这个配置类里面有一个@EnableAspectJAutoProxy注解，对于该注解，它会为我们容器中注册一个AnnotationAwareAspectJAutoProxyCreator（后置处理器），这还仅仅是这个@EnableAspectJAutoProxy注解做的事，除此之外，容器中还有一些默认的后置处理器的定义。

程序运行到这，容器中已经有一些我们将要用的后置处理器了，只不过现在还没创建对象，都只是一些定义，也就是说容器中有哪些后置处理器。

继续往下看这个registerBeanPostProcessors()方法，可以看到它里面还有其他的逻辑，如下所示：

```java
beanFactory.addBeanPostProcessor(new BeanPostProcessorChecker(beanFactory, beanProcessorTargetCount));
```

说的是给beanFactory中额外还加了一些其他的BeanPostProcessor，也就是说给容器中加别的BeanPostProcessor。

继续往下看这个registerBeanPostProcessors()方法，发现它里面还有这样的注释，如下所示：

```java
// Separate between BeanPostProcessors that implement PriorityOrdered,
// Ordered, and the rest.
List<BeanPostProcessor> priorityOrderedPostProcessors = new ArrayList<BeanPostProcessor>();
```


说的是分离这些BeanPostProcessor，看哪些是实现了PriorityOrdered接口的，哪些又是实现了Ordered接口的，包括哪些是原生的没有实现什么接口的。所以，在这儿，对这些BeanPostProcessor还做了一些处理，所做的处理看以下代码便一目了然。

```java
for (String ppName : postProcessorNames) {
    if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
        BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
        priorityOrderedPostProcessors.add(pp);
        if (pp instanceof MergedBeanDefinitionPostProcessor) {
            internalPostProcessors.add(pp);
        }
    }
    else if (beanFactory.isTypeMatch(ppName, Ordered.class)) {
        orderedPostProcessorNames.add(ppName);
    }
    else {
        nonOrderedPostProcessorNames.add(ppName);
    }
}
```

**IOC容器中的那些BeanPostProcessor是有优先级排序的**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-27-46.png)

**IOC容器中的那些BeanPostProcessor可以实现PriorityOrdered以及Ordered这些接口来定义它们工作的优先级，即谁先前谁先后**



在这儿将这些BeanPostProcessor做了一下划分，如果BeanPostProcessor实现了PriorityOrdered接口，那么就将其保存在名为priorityOrderedPostProcessors的List集合中，并且要是该BeanPostProcessor还是MergedBeanDefinitionPostProcessor这种类型的，则还得将其保存在名为internalPostProcessors的List集合中

**继续往下看这个registerBeanPostProcessors()方法，主要是看其中的注释，不难发现有以下三步：**

- 优先注册实现了PriorityOrdered接口的BeanPostProcessor

- 再给容器中注册实现了Ordered接口的BeanPostProcessor

- 最后再注册没实现优先级接口的BeanPostProcessor

  那么，所谓的注册BeanPostProcessor又是什么呢？我们还是来到程序停留的地方，为啥子程序会停留在这儿呢？因为咱们现在即将要创建的名称为internalAutoProxyCreator的组件（其实它就是我们之前经常讲的AnnotationAwareAspectJAutoProxyCreator）实现了Ordered接口，这只要查看AnnotationAwareAspectJAutoProxyCreator类的源码便知，一级一级地往上查
  

![Snipaste_2021-04-04_14-55-12](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-55-12.png)



**先拿到要注册的BeanPostProcessor的名字，然后再从beanFactory中来获取。**



![Snipaste_2021-04-04_14-56-20](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-56-20.png)

**接下来，我们就要获取相应名字的BeanPostProcessor了，怎么获取呢？继续跟进方法调用栈，如下图所示，可以看到现在是定位到了AbstractBeanFactory抽象类的getBean()方法中。**

![Snipaste_2021-04-04_14-56-46](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-56-46.png)



**定位到了AbstractBeanFactory抽象类的doGetBean()方法中**



![Snipaste_2021-04-04_14-59-10](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_14-59-10.png)



**定位到了DefaultSingletonBeanRegistry类的getSingleton()方法中**



![Snipaste_2021-04-04_15-00-04](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-00-04.png)

![Snipaste_2021-04-04_15-00-23](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-00-23.png)



**现在又定位到了AbstractBeanFactory抽象类的doGetBean()方法中**

可以发现，现在就是来创建bean的，也就是说如果获取不到那么就创建bean。咱们现在就是需要注册BeanPostProcessor，说白了，实际上就是创建BeanPostProcessor对象，然后保存在容器中。

那么接下来，我们就来看看是如何创建出名称为internalAutoProxyCreator的BeanPostProcessor的，它的类型其实就是我们之前经常说的AnnotationAwareAspectJAutoProxyCreator。我们就以它为例，来看看它这个对象是怎么创建出来的。

**我们继续跟进方法调用栈，如下图所示，可以看到现在是定位到了AbstractAutowireCapableBeanFactory抽象类的createBean()方法中。**

![Snipaste_2021-04-04_15-02-13](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-02-13.png)

程序停留在这儿，就是在初始化bean实例，说明bean实例已经创建好了,会看到一个createBeanInstance()方法，说的就是bean实例的创建。创建的是哪个bean实例呢？就是名称为internalAutoProxyCreator的实例，该实例的类型就是我们之前经常说的AnnotationAwareAspectJAutoProxyCreator，即创建这个类型的实例。创建好了之后，就在程序停留的地方进行初始化。

整个的过程

- 首先创建bean的实例
- 然后给bean的各种属性赋值（即调用populateBean()方法）
- 接着初始化bean（即调用initializeBean()方法），这个初始化bean其实特别地重要，因为我们这个后置处理器就是在bean初始化的前后进行工作的。
- 接下来，我们就来看看这个bean的实例是如何初始化的。继续跟进方法调用栈，如下图所示，可以看到现在是定位到了AbstractAutowireCapableBeanFactory抽象类的initializeBean()方法中。



![Snipaste_2021-04-04_15-02-56](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-02-56.png)

![Snipaste_2021-04-04_15-03-40](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-03-40.png)





**分析一下初始化bean的流程**

首先进入invokeAwareMethods()这个方法

![Snipaste_2021-04-04_15-04-37](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-04-37.png)

这个方法是来判断我们这个bean对象是不是Aware接口的，如果是，并且它还是BeanNameAware、BeanClassLoaderAware以及BeanFactoryAware这几个Aware接口中的其中一个，那么**就调用相关的Aware接口方法，即处理Aware接口的方法回调。**

现在当前的这个bean叫internalAutoProxyCreator，并且这个bean对象已经被创建出来了，创建出来的这个bean对象之前我们也分析过，它是有实现BeanFactoryAware接口的，故而会调用相关的Aware接口方法，这也是程序为什么会停留在invokeAwareMethods()这个方法的原因




往下翻阅initializeBean()方法，会发现有一个叫applyBeanPostProcessorsBeforeInitialization的方法



该方法的意思其实就是应用后置处理器的postProcessBeforeInitialization()方法

可以看到，它是拿到所有的后置处理器，然后再调用后置处理器的postProcessBeforeInitialization()方法，也就是说bean初始化之前后置处理器的调用在这儿。

拿到所有的后置处理器，然后再调用后置处理器的postProcessAfterInitialization()方法。

![Snipaste_2021-04-04_15-05-43](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-05-43.png)

![Snipaste_2021-04-04_15-05-52](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-05-52.png)

**定位到了AbstractAdvisorAutoProxyCreator抽象类的setBeanFactory()方法中**

![Snipaste_2021-04-04_15-06-22](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-06-22.png)



**直接进去AnnotationAwareAspectJAutoProxyCreator这个类的initBeanFactory()方法中了，即调到了我们要给容器中创建的AspectJ自动代理创建器的initBeanFactory()方法中**

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-47-18.png)



initBeanFactory()方法创建了两个东西，一个叫ReflectiveAspectJAdvisorFactory，还有一个叫BeanFactoryAspectJAdvisorsBuilderAdapter，它相当于把之前创建的aspectJAdvisorFactory以及beanFactory重新包装了一下




![Snipaste_2021-04-04_15-06-57](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-06-57.png)

**后置处理器创建完以后会添加到我们已创建的那个bean集合里面**

![Snipaste_2021-04-04_15-11-07](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-11-07.png)

**调用sortPostProcessors()方法按照优先级给这些后置处理器们排一个序，程序再往下运行，就会调用到registerBeanPostProcessors()方法**

![Snipaste_2021-04-04_15-12-20](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_15-12-20.png)



注册流程： **拿到所有的BeanPostProcessor，然后调用beanFactory的addBeanPostProcessor()方法将BeanPostProcessor注册到BeanFactory中**



## 5. AnnotationAwareAspectJAutoProxyCreator作为后置处理器

参考文章：https://liayun.blog.csdn.net/article/details/111413608

## 6. 目标方法的拦截逻辑

参考文章：https://liayun.blog.csdn.net/article/details/111516728

## 7. 拦截器链的执行过程

参考文章：https://liayun.blog.csdn.net/article/details/111603408

## 8. AOP总结

参考文章：https://liayun.blog.csdn.net/article/details/111677048

```java
1）、传入配置类，创建ioc容器
 * 		2）、注册配置类，调用refresh（）刷新容器；
 * 		3）、registerBeanPostProcessors(beanFactory);注册bean的后置处理器来方便拦截bean的创建；
 * 			1）、先获取ioc容器已经定义了的需要创建对象的所有BeanPostProcessor
 * 			2）、给容器中加别的BeanPostProcessor
 * 			3）、优先注册实现了PriorityOrdered接口的BeanPostProcessor；
 * 			4）、再给容器中注册实现了Ordered接口的BeanPostProcessor；
 * 			5）、注册没实现优先级接口的BeanPostProcessor；
 * 			6）、注册BeanPostProcessor，实际上就是创建BeanPostProcessor对象，保存在容器中；
 * 				创建internalAutoProxyCreator的BeanPostProcessor【AnnotationAwareAspectJAutoProxyCreator】
 * 				1）、创建Bean的实例
 * 				2）、populateBean；给bean的各种属性赋值
 * 				3）、initializeBean：初始化bean；
 * 						1）、invokeAwareMethods()：处理Aware接口的方法回调
 * 						2）、applyBeanPostProcessorsBeforeInitialization()：应用后置处理器的postProcessBeforeInitialization（）
 * 						3）、invokeInitMethods()；执行自定义的初始化方法
 * 						4）、applyBeanPostProcessorsAfterInitialization()；执行后置处理器的postProcessAfterInitialization（）；
 * 				4）、BeanPostProcessor(AnnotationAwareAspectJAutoProxyCreator)创建成功；--》aspectJAdvisorsBuilder
 * 			7）、把BeanPostProcessor注册到BeanFactory中；
 * 				beanFactory.addBeanPostProcessor(postProcessor);
 * =======以上是创建和注册AnnotationAwareAspectJAutoProxyCreator的过程========
 * 
 * 			AnnotationAwareAspectJAutoProxyCreator => InstantiationAwareBeanPostProcessor
 * 		4）、finishBeanFactoryInitialization(beanFactory);完成BeanFactory初始化工作；创建剩下的单实例bean
 * 			1）、遍历获取容器中所有的Bean，依次创建对象getBean(beanName);
 * 				getBean->doGetBean()->getSingleton()->
 * 			2）、创建bean
 * 				【AnnotationAwareAspectJAutoProxyCreator在所有bean创建之前会有一个拦截，InstantiationAwareBeanPostProcessor，会调用postProcessBeforeInstantiation()】
 * 				1）、先从缓存中获取当前bean，如果能获取到，说明bean是之前被创建过的，直接使用，否则再创建；
 * 					只要创建好的Bean都会被缓存起来
 * 				2）、createBean（）;创建bean；
 * 					AnnotationAwareAspectJAutoProxyCreator 会在任何bean创建之前先尝试返回bean的实例
 * 					【BeanPostProcessor是在Bean对象创建完成初始化前后调用的】
 * 					【InstantiationAwareBeanPostProcessor是在创建Bean实例之前先尝试用后置处理器返回对象的】
 * 					1）、resolveBeforeInstantiation(beanName, mbdToUse);解析BeforeInstantiation
 * 						希望后置处理器在此能返回一个代理对象；如果能返回代理对象就使用，如果不能就继续
 * 						1）、后置处理器先尝试返回对象；
 * 							bean = applyBeanPostProcessorsBeforeInstantiation（）：
 * 								拿到所有后置处理器，如果是InstantiationAwareBeanPostProcessor;
 * 								就执行postProcessBeforeInstantiation
 * 							if (bean != null) {
								bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);
							}
 * 
 * 					2）、doCreateBean(beanName, mbdToUse, args);真正的去创建一个bean实例；和3.6流程一样；
 * 					3）、
 * 			
 * 		
 * AnnotationAwareAspectJAutoProxyCreator【InstantiationAwareBeanPostProcessor】	的作用：
 * 1）、每一个bean创建之前，调用postProcessBeforeInstantiation()；
 * 		关心MathCalculator和LogAspect的创建
 * 		1）、判断当前bean是否在advisedBeans中（保存了所有需要增强bean）
 * 		2）、判断当前bean是否是基础类型的Advice、Pointcut、Advisor、AopInfrastructureBean，
 * 			或者是否是切面（@Aspect）
 * 		3）、是否需要跳过
 * 			1）、获取候选的增强器（切面里面的通知方法）【List<Advisor> candidateAdvisors】
 * 				每一个封装的通知方法的增强器是 InstantiationModelAwarePointcutAdvisor；
 * 				判断每一个增强器是否是 AspectJPointcutAdvisor 类型的；返回true
 * 			2）、永远返回false
 * 
 * 2）、创建对象
 * postProcessAfterInitialization；
 * 		return wrapIfNecessary(bean, beanName, cacheKey);//包装如果需要的情况下
 * 		1）、获取当前bean的所有增强器（通知方法）  Object[]  specificInterceptors
 * 			1、找到候选的所有的增强器（找哪些通知方法是需要切入当前bean方法的）
 * 			2、获取到能在bean使用的增强器。
 * 			3、给增强器排序
 * 		2）、保存当前bean在advisedBeans中；
 * 		3）、如果当前bean需要增强，创建当前bean的代理对象；
 * 			1）、获取所有增强器（通知方法）
 * 			2）、保存到proxyFactory
 * 			3）、创建代理对象：Spring自动决定
 * 				JdkDynamicAopProxy(config);jdk动态代理；
 * 				ObjenesisCglibAopProxy(config);cglib的动态代理；
 * 		4）、给容器中返回当前组件使用cglib增强了的代理对象；
 * 		5）、以后容器中获取到的就是这个组件的代理对象，执行目标方法的时候，代理对象就会执行通知方法的流程；
 * 		
 * 	
 * 	3）、目标方法执行	；
 * 		容器中保存了组件的代理对象（cglib增强后的对象），这个对象里面保存了详细信息（比如增强器，目标对象，xxx）；
 * 		1）、CglibAopProxy.intercept();拦截目标方法的执行
 * 		2）、根据ProxyFactory对象获取将要执行的目标方法拦截器链；
 * 			List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
 * 			1）、List<Object> interceptorList保存所有拦截器 5
 * 				一个默认的ExposeInvocationInterceptor 和 4个增强器；
 * 			2）、遍历所有的增强器，将其转为Interceptor；
 * 				registry.getInterceptors(advisor);
 * 			3）、将增强器转为List<MethodInterceptor>；
 * 				如果是MethodInterceptor，直接加入到集合中
 * 				如果不是，使用AdvisorAdapter将增强器转为MethodInterceptor；
 * 				转换完成返回MethodInterceptor数组；
 * 
 * 		3）、如果没有拦截器链，直接执行目标方法;
 * 			拦截器链（每一个通知方法又被包装为方法拦截器，利用MethodInterceptor机制）
 * 		4）、如果有拦截器链，把需要执行的目标对象，目标方法，
 * 			拦截器链等信息传入创建一个 CglibMethodInvocation 对象，
 * 			并调用 Object retVal =  mi.proceed();
 * 		5）、拦截器链的触发过程;
 * 			1)、如果没有拦截器执行执行目标方法，或者拦截器的索引和拦截器数组-1大小一样（指定到了最后一个拦截器）执行目标方法；
 * 			2)、链式获取每一个拦截器，拦截器执行invoke方法，每一个拦截器等待下一个拦截器执行完成返回以后再来执行；
 * 				拦截器链的机制，保证通知方法与目标方法的执行顺序；
 * 		
 * 	总结：
 * 		1）、  @EnableAspectJAutoProxy 开启AOP功能
 * 		2）、 @EnableAspectJAutoProxy 会给容器中注册一个组件 AnnotationAwareAspectJAutoProxyCreator
 * 		3）、AnnotationAwareAspectJAutoProxyCreator是一个后置处理器；
 * 		4）、容器的创建流程：
 * 			1）、registerBeanPostProcessors（）注册后置处理器；创建AnnotationAwareAspectJAutoProxyCreator对象
 * 			2）、finishBeanFactoryInitialization（）初始化剩下的单实例bean
 * 				1）、创建业务逻辑组件和切面组件
 * 				2）、AnnotationAwareAspectJAutoProxyCreator拦截组件的创建过程
 * 				3）、组件创建完之后，判断组件是否需要增强
 * 					是：切面的通知方法，包装成增强器（Advisor）;给业务逻辑组件创建一个代理对象（cglib）；
 * 		5）、执行目标方法：
 * 			1）、代理对象执行目标方法
 * 			2）、CglibAopProxy.intercept()；
 * 				1）、得到目标方法的拦截器链（增强器包装成拦截器MethodInterceptor）
 * 				2）、利用拦截器的链式机制，依次进入每一个拦截器进行执行；
 * 				3）、效果：
 * 					正常执行：前置通知-》目标方法-》后置通知-》返回通知
 * 					出现异常：前置通知-》目标方法-》后置通知-》异常通知
 * 		
```



# 三，声明式事务

环境搭建

导入依赖

```xml
 			<dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.9</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
```

TxConfig 配置类

```java
package com.zhuang.tx;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;

import javax.sql.DataSource;

/**
 * @Classname TxConfig
 * @Description 事务配置类
 * @Date 2021/4/4 18:48
 * @Created by dell
 */
@Configuration
@ComponentScan("com.zhuang.tx")
public class TxConfig {

    //注册数据源
    @Bean
    public DataSource dataSource() throws Exception{
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setUser("root");
        dataSource.setPassword("*****");
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/jdbc");
        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate() throws Exception {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource());
        return jdbcTemplate;
    }

}
```

UserService

```java
package com.zhuang.tx;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * @Classname UserService
 * @Description Service层
 * @Date 2021/4/4 18:53
 * @Created by dell
 */
@Service
public class UserService {

    @Autowired
    private UserDao userDao;

    public void insert(){
        userDao.insert();
        System.out.println("数据插入成功");
    }
}
```

UserDao

```java
package com.zhuang.tx;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.UUID;

/**
 * @Classname UserDao
 * @Description Dao层
 * @Date 2021/4/4 18:53
 * @Created by dell
 */
@Repository
public class UserDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    public void insert() {
        String sql = "INSERT INTO `fruit` (`number`,`fruitname`,`price`,`unit`)\n" +
                "VALUES(?,?,?,?);";
        // 增删改都来调用这个方法
        jdbcTemplate.update(sql,"2","菠萝","5.5","kg");
    }
}
```

测试类

```java
@Test
    public void test01(){

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(TxConfig.class);
        UserService userService = applicationContext.getBean(UserService.class);
        userService.insert();
    }
```

结果

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-03-45.png)

![Snipaste_2021-04-04_19-03-54](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-03-54.png)

实现事务步骤

- 添加注解**@EnableTransactionManagement** 在配置类上
- 添加Bean

```java
// 注册事务管理器在容器中
@Bean
public PlatformTransactionManager platformTransactionManager() throws Exception {
    return new DataSourceTransactionManager(dataSource());
}
```

- UserService方法上加 **@Transactional**

随便搞个异常

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-12-34.png)

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-12-52.png)

**实现事务！**



原理解析

**@EnableTransactionManagement**

利用**TransactionManagementConfigurationSelector**给容器中会导入组件导入两个组件

- AutoProxyRegistrar

- ProxyTransactionManagementConfiguration



**AutoProxyRegistrar：**

给容器中注册一个 InfrastructureAdvisorAutoProxyCreator 组件；

InfrastructureAdvisorAutoProxyCreator：？

利用后置处理器机制在对象创建以后，包装对象，返回一个代理对象（增强器），代理对象执行方法利用拦截器链进行调用；

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-42-32.png)



**ProxyTransactionManagementConfiguration 做了什么？**



**给容器中注册事务增强器；**

- (1)、事务增强器要用事务注解的信息，AnnotationTransactionAttributeSource解析事务注解

- (2)、事务拦截器：

**TransactionInterceptor；保存了事务属性信息，事务管理器,是一个 MethodInterceptor；在目标方法执行的时候；执行拦截器链；事务拦截器：**

- (1)、先获取事务相关的属性

- (2)、再获取PlatformTransactionManager，如果事先没有添加指定任何transactionmanger

![](C:\Users\dell\Desktop\Notes\Spring注解版\img\Snipaste_2021-04-04_19-46-12.png)

**最终会从容器中按照类型获取一个PlatformTransactionManager；**

- (3)、执行目标方法

**如果异常，获取到事务管理器，利用事务管理回滚操作；**

**如果正常，利用事务管理器，提交事务**



# 四，Servlet 3.0



直接看别人的吧 ，版本不同，问题太多，比较麻烦，有空还是看看源码

**参考文章https://blog.csdn.net/yerenyuan_pku/category_10613855.html**



# 五，拓展

也看人家的吧。。。

**参考文章https://blog.csdn.net/yerenyuan_pku/category_10613855.html**

