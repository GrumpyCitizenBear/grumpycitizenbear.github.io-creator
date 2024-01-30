---
title: "Maven包管理的一些见解和总结"
date: 2021-10-21T10:42:06+08:00
draft: false
summary: "Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，许多 Apache Jakarta 项目发文时使用 Maven，而且公司项目采用 Maven 的比例在持续增长。"
---

# Java的包管理与maven

## 一、什么是包？

### 1.如果要知道什么是包，那么首先要了解一下JVM的工作模式

其实很简单，只需要两步：
 - 执行一个类的字节码
 - 假如在这个过程中碰到了新的类，加载它

   1、2步循环执行，直到完成整个程序




### 2.同时我们还要知道在哪里可以找到类？

classpath类路径

类的权限定类名（目录层级）唯一确定了一个类




### 3.包就是把许多类放在一起打的压缩包

通常以.jar扩展名结尾，也可以手动改为zip




### 4.传递性依赖

通俗的说，就是你的程序依赖的类还依赖了别的类

产生的问题：
 - 如果classpath中有重名的类，会优先使用排在前面的类
 - 这样会导致程序在未来的某一天因为改变了classpath而出现bug

 引申出包管理➡️

## 二、什么是包管理

 你要使用一些第三方的类，需要告诉JVM去哪里找

 包管理的本质就是告诉JVM如何找到所需的第三方类库，以及成功解决其中的冲突问题

## 三、包管理的发展历史

### 1.Apache Ant 

 1. 手动下载jar包，放在一个目录中
 2. 写XML配置，指定编译的源代码目录、依赖的jar包、输出目录等
 3. 缺点
    - 每个人都要造自己的轮子
    - 第三方类库需要手动下载（如果依赖了10000个。。。。）
    - 没有解决包冲突问题

### 2.Maven

## 四、maven包管理

maven远不止包管理工具

### 1.maven的仓库
  - maven的中央仓库
    按照一定的约定储存包

  - maven的本地仓库
    默认位置 ～/.m2

### 2.maven的编号

  - groupid
  - artifactid
  - version

maven的包按照约定为所有的包编号，方便检索

### 3.maven的传递性依赖自动管理

#### 原则：决不允许出现同名不同版本号的jar包

#### 解决方案：1.选择离项目最近的 2.如果距离相同，选择版本号靠前的


### 4.会产生包冲突

```
举个例子
我的项目➡️A➡️B➡️C(2.0.0)
我的项目➡️D➡️C(1.0.0)

我的项目依赖了A，A依赖B，B依赖C的2.0.0版本
我的项目还依赖了D，D依赖了C的1.0.0版本

按照maven的解决方法，优于C（1.0.0）距离我的项目更近，所以会选择使用1.0.0版本的C

带来的问题：A引用的C（2.0.0）中的方法在C（1.0.0）版本中没有，系统就会报错
```

#### 依赖冲突的信号
1. AbstractMethodError
2. NoClassDefFoundError
3. ClassNotFoundException
4. LinkageError

### 5.解决包冲突

#### 查看冲突的方式
1. 在IDEA maven 图形化界面中查看
2. 命令行
```
mvn dependency:tree

可以重定向到文件中，方便查看

使用过滤器：
mvn dependency -Dincludes=
```
3. IDEA中的插件：maven helper

#### 解决方法

1. 直接在pom.xml中引用需要的版本
2. 强行解除依赖

```xml
<exclusions>
    <exclusion>
        <groupId>sample.ProjectC</groupId>
          <artifactId>Project-C</artifactId>
    <exclusion>
<exclusions>
```

## 五、maven的其他知识

### 1.scope

包的作用域，隔离第三方依赖

- compile：在main和test中都可见
- test：只在test中可见
- provided：只在编译时有效，运行时不起作用

### 2.pom.xml

全称：project object model

项目的说明书


