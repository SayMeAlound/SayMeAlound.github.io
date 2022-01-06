SpringBoot复习
## SpringBoot复习

#### 起步依赖

- 起步依赖本质上市一个Maven项目对象模型(Project Object Model,POM),定义对其他库的依赖,这些东西加在一起即支持某项功能.

  简单的说,起步依赖就是具备某种功能的坐标打包在一起,并提供一些默认的功能.

- 自动配置

  SpringBoot的自动配置时一个运行时(更准确地说,是应用程序启动时)的过程,考虑了众多因素,才决定Spring配置应该用哪个,不该用哪个,该过程是Spring自动完成的



起步依赖

```shell
<parent> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version> 
</parent> 
```

## SpringBoot原理分析

### 查看spring-boot-starter-parent

​	按住Ctrl点击pom.xml中的**spring-boot-starter-parent**，跳转到了spring-boot-starter-parent的pom.xml，xml配

置如下（只摘抄了部分重点配置）：

```shell
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

​	按住Ctrl点击pom.xml中的**spring-boot-starter-dependencies**，跳转到了spring-boot-starter-dependencies的
pom.xml，xml配置如下（只摘抄了部分重点配置）：

```java
<properties>
    <activemq.version>5.15.3</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.63</appengine-sdk.version>
    <artemis.version>2.4.0</artemis.version>
    <aspectj.version>1.8.13</aspectj.version>
    <assertj.version>3.9.1</assertj.version>
    <atomikos.version>4.0.6</atomikos.version>
    <bitronix.version>2.1.4</bitronix.version>
    
	<build-helper-maven-plugin.version>3.0.0</build-helper-maven-plugin.version>
	<byte-buddy.version>1.7.11</byte-buddy.version>
... ... ...
</properties>
<dependencyManagement>
	<dependencies>
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot</artifactId>
            <version>2.0.1.RELEASE</version>
		</dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.0.1.RELEASE</version>
        </dependency>
... ... ...
	</dependencies>
</dependencyManagement>
<build>
	<pluginManagement>
		<plugins>
			<plugin>
                <groupId>org.jetbrains.kotlin</groupId>
                <artifactId>kotlin-maven-plugin</artifactId>
                <version>${kotlin.version}</version>
			</plugin>
            <plugin>
                <groupId>org.jooq</groupId>
                <artifactId>jooq-codegen-maven</artifactId>
                <version>${jooq.version}</version>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.0.1.RELEASE</version>
            </plugin>
        	... ... ...
        </plugins>
	</pluginManagement>
</build>
```

​	从上面的spring-boot-starter-dependencies的pom.xml中我们可以发现，一部分坐标的版本、依赖管理、插件管
理已经定义好，所以我们的SpringBoot工程继承spring-boot-starter-parent后已经具备版本锁定等配置了。所以
起步依赖的作用就是进行依赖的传递。

#### 分析spring-boot-starter-web

​	按住Ctrl点击pom.xml中的spring-boot-starter-web，跳转到了spring-boot-starter-web的pom.xml，xml配置如
下（只摘抄了部分重点配置）：

```sh
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
http://maven.apache.org/xsd/maven-4.0.0.xsd"
xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starters</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.0.1.RELEASE</version>
    <name>Spring Boot Web Starter</name>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>2.0.1.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-json</artifactId>
            <version>2.0.1.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <version>2.0.1.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.0.9.Final</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.0.5.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.5.RELEASE</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>
</project>
```

​	从上面的spring-boot-starter-web的pom.xml中我们可以发现，spring-boot-starter-web就是将web开发要使用的
spring-web、spring-webmvc等坐标进行了“打包”，这样我们的工程只要引入spring-boot-starter-web起步依赖的
坐标就可以进行web开发了，同样体现了依赖传递的作用。

#### 自动配置原理解析

​	按住Ctrl点击查看启动类MySpringBootApplication上的注解@SpringBootApplication

```java
@SpringBootApplication
public class MySpringBootApplication {
	public static void main(String[] args) {
		SpringApplication.run(MySpringBootApplication.class);
	}
}
```

注解@SpringBootApplication的源码

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
        @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
        @Filter(type = FilterType.CUSTOM, classes =
AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    /**
    * Exclude specific auto-configuration classes such that they will never be
    applied.
    * @return the classes to exclude
    */
    @AliasFor(annotation = EnableAutoConfiguration.class)
    Class<?>[] exclude() default {};
    ... ... ...
}
```

其中，
@SpringBootConfiguration：等同与@Configuration，既标注该类是Spring的一个配置类
@EnableAutoConfiguration：SpringBoot自动配置功能开启

​	按住Ctrl点击查看注解@EnableAutoConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
	... ... ...
}
```

​	其中，@Import(AutoConfigurationImportSelector.class) 导入了AutoConfigurationImportSelector类
按住Ctrl点击查看AutoConfigurationImportSelector源码

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
        ... ... ...
        List<String> configurations = getCandidateConfigurations(annotationMetadata,
        attributes);
        configurations = removeDuplicates(configurations);
        Set<String> exclusions = getExclusions(annotationMetadata, attributes);
        checkExcludedClasses(configurations, exclusions);
        configurations.removeAll(exclusions);
        configurations = filter(configurations, autoConfigurationMetadata);
        fireAutoConfigurationImportEvents(configurations, exclusions);
        return StringUtils.toStringArray(configurations);
}
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata,
		AnnotationAttributes attributes) {
	List<String> configurations = SpringFactoriesLoader.loadFactoryNames(
		getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader());
	return configurations;
}
```

​	其中，SpringFactoriesLoader.loadFactoryNames 方法的作用就是从**META-INF/spring.factories**文件中读取指定
类对应的类名称列表