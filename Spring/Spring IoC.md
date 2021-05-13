# Spring IoC

**IoC，是 Inversion of Control 的缩写，即控制反转。**

- 上层模块不应该依赖于下层模块，它们共同依赖于一个抽象
- 抽象不能依赖于具体实现，具体实现依赖于抽象

***注：又称为依赖倒置原则。这是设计模式六大原则之一。***

## 依赖注入

Dependence Injection （DI）依赖注入是IoC的最常见形式。容器全权负责组件的装配，它把符合依赖关系的对象通过JavaBean属性或构造函数传递给需要的对象。

- **谁依赖于谁：**当然是应用程序依赖于 IoC 容器；
- **为什么需要依赖：**应用程序需要 IoC 容器来提供对象需要的外部资源；
- **谁注入谁：**很明显是 IoC 容器注入应用程序某个对象，应用程序依赖的对象；
- **注入了什么**：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）

由IoC容器管理的组成应用程序的对象成为Bean，是一种Java写成的可重用组件。

- 类必须是具体的和公共的
- 具有无参构造器
- 提供getter/setter方法来访问其对象

## IoC容器

Spring中有两种IoC容器

- `BeanFactory`: Spring 实例化、配置和管理对象的最基本接口
- `ApplicationContext`：BeanFactory的子接口。支持更丰富的功能

`ApplicationContext`实现：

- **ClassPathXmlApplicationContext**：从classpath获取配置文件

``` java
BeanFactory beanFactory = new ClassPathXmlApplicationContext("classpath.xml");
```

- **FileSystemXmlApplicationContext**:从文件系统获取配置文件

```java
BeanFactory beanFactory = new FileSystemXmlApplicationContext("fileSystemConfig.xml");
```

### IoC容器的工作步骤

- 配置元数据：需要配置一些元数据来告诉 Spring，你希望容器如何工作，具体来说，就是如何去初始化、配置、管理 JavaBean 对象。
- 实例化容器：由 IoC 容器解析配置的元数据。IoC 容器的 Bean Reader 读取并解析配置文件，根据定义生成 BeanDefinition 配置元数据对象，IoC 容器根据 BeanDefinition 进行实例化、配置及组装 Bean。
- 使用容器：由客户端实例化容器，获取需要的 Bean。

#### 配置元数据

- **基于xml配置**：Spring 的传统配置方式。在 `<beans>` 标签中配置元数据内容。

  缺点是当 JavaBean 过多时，产生的配置文件较杂乱。

- **基于注解配置**：Spring2.5 引入。可以大大简化配置。

- **基于 Java 配置**：可以使用 Java 类来定义 JavaBean 。

  为了使用这个新特性，需要用到 `@Configuration` 、`@Bean` 、`@Import` 和 `@DependsOn` 注解。

### Bean概述

​	一个 Spring 容器管理一个或多个 bean。 这些 bean 根据你配置的元数据（比如 xml 形式）来创建。 Spring IoC 容器本身，并不能识别你配置的元数据。为此，要将这些配置信息转为 Spring 能识别的格式——BeanDefinition 对象。

#### 命名Bean

​	指定 id 和 name 属性不是必须的。 Spring 中，并非一定要指定 id 和 name 属性。实际上，Spring 会自动为其分配一个特殊名。 如果你需要引用声明的 bean，这时你才需要一个标识。官方推荐驼峰命名法来命名。

#### 别名

```xml
<alias name="subsystemA-dataSource" alias="subsystemB-dataSource"/>
<alias name="subsystemA-dataSource" alias="myApp-dataSource" />
```

#### 实例化Bean

**构造器方式**

``` xml
<bean id="exampleBean" class="examples.ExampleBean"/>
```

**静态工厂方法**

- 需要在bean的class属性中指定拥有该工厂方法的类

```xml
<bean id="car" class="com.itdoc.spring.factory.StaticFactory" factory-method="getCar">
        <constructor-arg value="Maserati"/>
    </bean>
```

