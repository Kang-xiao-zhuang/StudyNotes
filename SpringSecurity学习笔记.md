



添加一个配置类

```java
package com.zhuang.springsecurity;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

/**
 * @Classname SecurityConfig
 * @Description 添加一个配置类
 * @Date 2021/4/8 14:01
 * @Created by dell
 */
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin() //表单登录
                .and().authorizeRequests() //认证配置
                .anyRequest() //任何请求
                .authenticated();  //都需要验证
    }
}
```

启动  端口8080 访问

默认用户名 user 密码在控制台

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-22-07.png)

成功

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-23-21.png)

输入 用户名 密码  **404 表示我们没有这个控制器，但是我们可以访问了**

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-22-56.png)

2.3 权限管理中的相关概念
2.3.1 主体
英文单词：principal
使用系统的用户或设备或从其他系统远程登录的用户等等。简单说就是谁使用系
统谁就是主体。
2.3.2 认证
英文单词：authentication
权限管理系统确认一个主体的身份，允许主体进入系统。简单说就是“主体”证
明自己是谁。
笼统的认为就是以前所做的登录操作。
2.3.3 授权
英文单词：authorization
将操作系统的“权力”“授予”“主体”，这样主体就具备了操作系统中特定功
能的能力。
所以简单来说，授权就是给用户分配权限。



添加一个控制器 新建controller包

```java
package com.zhuang.springsecurity.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Classname IndexController
 * @Description 测试controller
 * @Date 2021/4/8 14:08
 * @Created by dell
 */
@RestController
@RequestMapping("/test")
public class IndexController {

    @GetMapping("hello")
   // @ResponseBody
    public String index() {
        return "success";
    }
}
```

启动访问 成功！

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-26-32.png)



2.5 SpringSecurity 基本原理
SpringSecurity 本质是一个过滤器链：
从启动是可以获取到过滤器链：

```java
org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFil
ter
org.springframework.security.web.context.SecurityContextPersistenceFilter
org.springframework.security.web.header.HeaderWriterFilter
org.springframework.security.web.csrf.CsrfFilter
org.springframework.security.web.authentication.logout.LogoutFilter
org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter
org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter
org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter
org.springframework.security.web.savedrequest.RequestCacheAwareFilter
org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter
org.springframework.security.web.authentication.AnonymousAuthenticationFilter
org.springframework.security.web.session.SessionManagementFilter
org.springframework.security.web.access.ExceptionTranslationFilter
org.springframework.security.web.access.intercept.FilterSecurityInterceptor
```

代码底层流程：重点看三个过滤器：
FilterSecurityInterceptor：是一个方法级的权限过滤器, 基本位于过滤链的最底部

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-30-29.png)

super.beforeInvocation(fi) 表示查看之前的 filter 是否通过。
fi.getChain().doFilter(fi.getRequest(), fi.getResponse());表示真正的调用后台的服务。



ExceptionTranslationFilter：是个异常过滤器，用来处理在认证授权过程中抛出的异常

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-32-01.png)



UsernamePasswordAuthenticationFilter ：对/login 的 POST 请求做拦截，校验表单中用户名，密码

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-33-57.png)



2.6 UserDetailsService 接口讲解
当什么也没有配置的时候，账号和密码是由 Spring Security 定义生成的。而在实际项目中账号和密码都是从数据库中查询出来的。 所以我们要通过自定义逻辑控制认证逻辑。如果需要自定义逻辑时，只需要实现 UserDetailsService 接口即可。接口定义如下

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-35-50.png)



- 返回值 UserDetails
  这个类是系统默认的用户“主体”
- ![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-36-23.png)

```java
public interface UserDetails extends Serializable {
    //获取用户登录的权限
    Collection<? extends GrantedAuthority> getAuthorities();
	//获取密码
    String getPassword();
	//获取用户名
    String getUsername();
	//判断账户是否过期
    boolean isAccountNonExpired();
	//判断账户是否被锁定
    boolean isAccountNonLocked();
	//判断凭证是否过期
    boolean isCredentialsNonExpired();
	//判断当前用户是否可用
    boolean isEnabled();
}
```

以下是 UserDetails 实现类

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-36-57.png)



- 方法参数 username
  表示用户名。此值是客户端表单传递过来的数据。默认情况下必须叫 username，否则无
  法接收。



PasswordEncoder 接口讲解

```java
public interface PasswordEncoder {
    // 表示把参数按照特定的解析规则进行解析
    String encode(CharSequence var1);
    
// 表示验证从存储中获取的编码密码与编码后提交的原始密码是否匹配。如果密码匹配，则返回 true；如果不匹配，则返回 false。第一个参数表示需要被解析的密码。第二个参数表示存储的密码
    
    boolean matches(CharSequence var1, String var2);
// 表示如果解析的密码能够再次进行解析且达到更安全的结果则返回 true，否则返回false。默认返回 false
    default boolean upgradeEncoding(String encodedPassword) {
        return false;
    }
}
```

接口实现类

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-47-11.png)



BCryptPasswordEncoder 是 Spring Security 官方推荐的密码解析器，平时多使用这个解析器。BCryptPasswordEncoder 是对 bcrypt 强散列方法的具体实现。是基于 Hash 算法实现的单向加密。可以通过 strength 控制加密强度，默认 10.

测试一把

```java
 @Test
    public void test01(){
        //创建密码解析器
        BCryptPasswordEncoder bCryptPasswordEncoder = new BCryptPasswordEncoder();
        //对密码加密
        String encode = bCryptPasswordEncoder.encode("zhuangkang");
        System.out.println("加密后的数据-->"+encode);
        //判断是否相同 加密前 加密后
        boolean result = bCryptPasswordEncoder.matches("zhuangkang", encode);
        System.out.println("比较结果:"+result);
    }
```

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-53-53.png)





修改用户名密码 三种方式

配置文件 

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_14-57-10.png)

配置类

```java
package com.zhuang.springsecurity.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @Classname SecurityConfig
 * @Description 添加一个配置类
 * @Date 2021/4/8 14:01
 * @Created by dell
 */
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        String password=passwordEncoder.encode("123");
        auth.inMemoryAuthentication().withUser("tom").password(password).roles("admin");
    }
}
```

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_15-30-44.png)



自己写个配置类

```java
package com.zhuang.springsecurity.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @Classname SecurityConfigTest
 * @Description 自己写的配置类
 * @Date 2021/4/8 21:20
 * @Created by dell
 */
@Configuration
public class SecurityConfigTest extends WebSecurityConfigurerAdapter {
    @Autowired
    private  UserDetailsService userDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }
    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

Service

```java
package com.zhuang.springsecurity.service;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @Classname MyUserDetailsService
 * @Description 自己的业务层
 * @Date 2021/4/8 21:22
 * @Created by dell
 */
@Service("userDetailsService")
public class MyUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        List<GrantedAuthority> auths = AuthorityUtils.commaSeparatedStringToAuthorityList("role");
        return new User("jack",new BCryptPasswordEncoder().encode("123"),auths);
    }
}
```

测试 成功 

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-08_21-27-47.png)



用mybatis-plus查询数据库 

业务层

```java
package com.zhuang.springsecurity.service;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.zhuang.springsecurity.entity.Users;
import com.zhuang.springsecurity.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @Classname MyUserDetailsService
 * @Description 自己的业务层
 * @Date 2021/4/8 21:22
 * @Created by dell
 */
@Service("userDetailsService")
public class MyUserDetailsService implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        //调用userMapper方法 根据用户名查询数据
        QueryWrapper<Users> wrapper=new QueryWrapper();

        wrapper.eq("username",username);
        Users users = userMapper.selectOne(wrapper);
        //判断
        if (users == null){
            throw new UsernameNotFoundException("用户名不存在！！");
        }
        System.out.println(users);

        List<GrantedAuthority> auths = AuthorityUtils.commaSeparatedStringToAuthorityList("admins");
     //   return new User("merry",new BCryptPasswordEncoder().encode("789"),auths);
        return new User(users.getUsername(),new BCryptPasswordEncoder().encode(users.getPassword()), auths);
    }
}
```



dao层

```java
package com.zhuang.springsecurity.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.zhuang.springsecurity.entity.Users;
import org.springframework.stereotype.Repository;

/**
 * @Classname UserMapper
 * @Description 继承BaseMapper
 * @Date 2021/4/8 22:17
 * @Created by dell
 */
@Repository
public interface UserMapper extends BaseMapper<Users> {
}
```



主启动类

```java
package com.zhuang.springsecurity;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.zhuang.springsecurity.mapper")
public class SpringSecurityApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringSecurityApplication.class, args);
    }

}
```



配置类

```java
package com.zhuang.springsecurity.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @Classname SecurityConfigTest
 * @Description 自己写的配置类
 * @Date 2021/4/8 21:20
 * @Created by dell
 */
@Configuration
public class SecurityConfigTest extends WebSecurityConfigurerAdapter {
    @Autowired
    private  UserDetailsService userDetailsService;

    @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }
    @Bean
    PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
```



3.4 基于角色或权限进行访问控制
3.4.1 hasAuthority 方法
如果当前的主体具有指定的权限，则返回 true,否则返回 false





- 修改配置类

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-17-01.png)



- 添加一个控制器

```java
@GetMapping("/find")
@ResponseBody
public String find(){
return "find";
}
```

- 给用户登录主体赋予权限

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-17-47.png)

测试成功！

3.4.2 hasAnyAuthority 方法
如果当前的主体有任何提供的角色（给定的作为一个逗号分隔的字符串列表）的话，返回
true.

.4.3 hasRole 方法
如果用户具备给定角色就允许访问,否则出现 403。
如果当前主体具有指定的角色，则返回 true。
底层源码：

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-18-33.png)

给用户添加角色：

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-18-59.png)

修改配置文件：
注意配置文件中不需要添加”ROLE_“，因为上述的底层代码会自动添加与之进行匹配。

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-19-30.png)



3.4.4 hasAnyRole
表示用户具备任何一个条件都可以访问。
给用户添加角色：

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-20-32.png)



修改配置文件：

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-20-50.png)



3.7 注解使用
3.7.1 @Secured
判断是否具有角色，另外需要注意的是这里匹配的字符串需要添加前缀“ROLE_“。
使用注解先要开启注解功能！

```java
@SpringBootApplication
@EnableGlobalMethodSecurity(securedEnabled=true)
public class DemosecurityApplication {
public static void main(String[] args) {
SpringApplication.run(DemosecurityApplication.class, args);
}
}
```

在控制器方法上添加注解

```java
// 测试注解：
@RequestMapping("testSecured")
@ResponseBody
@Secured({"ROLE_normal","ROLE_admin"})
public String helloUser() {
return "hello,user";
}
@Secured({"ROLE_normal","ROLE_ 管理员"})
```

将上述的角色改为 @Secured({"ROLE_normal","ROLE_ 管理员"})即可访问

![](C:\Users\dell\Desktop\Notes\SpringSecurity\img\Snipaste_2021-04-09_09-24-20.png)

3.7.2 @PreAuthorize
先开启注解功能：
@EnableGlobalMethodSecurity(prePostEnabled = true)
@PreAuthorize：注解适合进入方法前的权限验证， @PreAuthorize 可以将登录用
户的 roles/permissions 参数传到方法中。



```java
@RequestMapping("/preAuthorize")
@ResponseBody
//@PreAuthorize("hasRole('ROLE_ 管理员 ')")
@PreAuthorize("hasAnyAuthority('menu:system')")
public String preAuthorize(){
System.out.println("preAuthorize");
return "preAuthorize";
}
```

3.7.3 @PostAuthorize
先开启注解功能：
@EnableGlobalMethodSecurity(prePostEnabled = true)
@PostAuthorize 注解使用并不多，在方法执行后再进行权限验证，适合验证带有返回值
的权限

```java
@RequestMapping("/testPostAuthorize")
@ResponseBody
@PostAuthorize("hasAnyAuthority('menu:system')")
public String preAuthorize(){
System.out.println("test--PostAuthorize");
return "PostAuthorize";
}
```

3.7.4 @PostFilter
@PostFilter ：权限验证之后对数据进行过滤 留下用户名是 admin1 的数据
表达式中的 filterObject 引用的是方法返回值 List 中的某一个元素

```java
@RequestMapping("getAll")
@PreAuthorize("hasRole('ROLE_ 管理员')")
@PostFilter("filterObject.username == 'admin1'")
@ResponseBody
public List<UserInfo> getAllUser(){
ArrayList<UserInfo> list = new ArrayList<>();
list.add(new UserInfo(1l,"admin1","6666"));
list.add(new UserInfo(2l,"admin2","888"));
return list;
}
```



3.10 CSRF
3.10.1 CSRF 理解
造 跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click
attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已
登录的 Web 应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，XSS
利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。
跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个
自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买
商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。
这利用了 web 中用户身份验证的一个漏洞： 简单的身份验证只能保证请求发自某个用户的
浏览器，却不能保证请求本身是用户自愿发出的。
从 Spring Security 4.0 开始，默认情况下会启用 CSRF 保护，以防止 CSRF 攻击应用
程序，Spring Security CSRF 会针对 PATCH，POST，PUT 和 DELETE 方法进行防护。



在登录页面添加一个隐藏域：

```html
<input
type="hidden"th:if="${_csrf}!=null"th:value="${_csrf.token}"name="_csrf
"/>
```

关闭安全配置的类中的 csrf

```java
// http.csrf().disable();
```



































