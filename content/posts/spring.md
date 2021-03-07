---
title: "Spring"
date: 2021-03-07T20:57:51+09:00
draft: true
toc: false
images:
tags:
  - spring
---
# Building REST services with Spring

## 建立雇员类(Employee.java)
- 使用JPA的`@Entity`注解来进行数据存储的准备。
- `id, name, role`都是领域对象模型。其中`id`被指定为主键并且自动由JPA provider生成。
- 当需要一个新的员工实例时会创建一个构造函数，但是此时还没有创建`id`。

## 使用JPA时建立雇员仓库(EmployeeRepository)来进行与数据库的交互
- 建立一个接口继承`JpaRepository`类。domain雷星设置为`Employee`，`id`类型设置为`long`。
- 仓库式的方法(repository solution)可以回避基于特定语言的数据存储方式。
- Spring Boot的入口是一个`public static void main`类型的，并且在需要Spring Boot行使其作用的地方使用`@SpringBootApplication`注释。
	- `@SpringBootApplication`是一个元注释，用来`component scanning, autoconfiguration, and property support`。本质就是用来启动一个容器并且使服务开始运行。

## 建立一个数据交互类LoadDatabase.java
### 程序做的事：
- 当Application的内容加载时Spring Boot会运行所有的`CommandLineRunner` bean。
- 这个运行类会请求一个`EmployeeRepository`的拷贝。
- 建立了两个实体并且存储了它们。

## HTTP平台
- 为了能从网络层获取数据，现在将目光转向Spring MVC。建立控制器类`EmployeeController.java`。
### 代码解说
- `@RestController`表示函数返回的结果直接写入响应而不是渲染模板。
- `EmployeeRepository`在controller的构造函数中被注入。
- 对应HTTP的各种请求方式，存在各种操作的路由。(GET, POST, PUT, and DELETE)
- `EmployeeNotFoundException`用来表示查找失败的意外。
	- 在抛出`EmployeeNotFoundException`例外之后添加Spring MVC的设置(advice)来生成HTTP 404响应。

### 意外抛出的解说
- `@ResponseBody`表示这个advice直接写入响应体。
- `@ExceptionHandler`指定这个advice只有抛出`EmployeeNotFoundException`时才会被调用。
- `@ResponseStatus`指定了抛出一个`HttpStatus.NOT_FOUND`。
- advice的函数体用来生成错误信息。这里直接调用了例外的内容。

## 上述达到RESTful的标准了吗？没有
### 缺少什么：
- 类似`/employees/3`的URL不符合风格。
- 很少使用GET，POST等。
- 所有的CRUD都分别映射好。

其实上述都是**RPC**（Remote Procedure Call，远程过程调用）。因为无法了解怎么与服务进行交互。还需要额外编写文档。

### 这里导入Spring HATEOAS，一个可以辅助编写多媒体驱动输出的Sping项目。
- 简而言之关键是需要添加对相关操作的链接（相关资源的路径）
	- `HAL`(Hypertext Application Language)形式是资源(Resources)+链接(Links)
		- 链接一般在`_links`中（JSON格式）
		- 资源中的列表一般在`_embedded`中（如雇员列表）

#### 简化的链接生成方式（手动使用`linkTo`不赘述）
- 使用HATEOAS的`RepresentationModelAssembler<转换前, 转换后>`，实现这个接口
	- 重写它的方法`toModel()`来实现将`Employee`转换为`EntityModel<Employee>`。
	- 通过`@Component`注解，应用开始运行时就可以创建这个assembler。
- 将上述assembler注入控制类中。

## 改进API
- 为了保证应用的弹性，需要使API具有即使规格变更也不能影响客户端的特性
	- 先导原则：不要删除数据库的列，在RESTful中同样适用。
	- 可以随时加入列
- 例：将`name`分解为`firstName`和`lastName`
	- 数据库新添加两列而不删除原来的
	- 代码解说
		- `name`替换为`firstName`和`lastName`
		- 获取名字的getter中的方法变为将`firstName`和`lastName`进行组合
		- 名字的setter解析输入适当分配
		- 生成初始数据的部分也适当修改
- 适当地变更响应
	- 代码解说 
		- 同样使用assembler来生成响应体
		- 用`ResponseEntity`来生成**HTTP 201 Created**状态信息，一般会包含一个**Location**的头，直接用资源自己URI来赋值即可。

## 为REST API创建链接
- 主要目的是为了将服务器逻辑与客户端的解析解耦合。
	- 比如订单状态
		- 处理中→完成/取消
		- 在payload中添加可进行的操作信息容易损坏服务器与客户端的联动并且后续维护成本大
		- 如果客户端可以解析HAL则可以自动显示可以进行的后续操作
- 使用的`HATEOAS(Hypermedia as the Engine of Application State)`
	- 根据当前状态动态地改变链接列表里的内容
	- 用链接作为应用状态管理的引擎

- 具体代码见[Spring Boot Guide](https://spring.io/guides/tutorials/rest/)
		


### 用词
- **domain object**: A domain class represents a table column and it allows you to handle the column value as a Java object. 

