---
title: "Sprint_guide"
date: 2021-03-07T21:03:15+09:00
draft: false
toc: false
images:
tags:
  - spring
---
学习spring的基本知识找不到好的课程所以跟着官方[Guides](https://spring.io/guides#getting-started-guides)学习

1.[建立一个resful服务](https://spring.io/guides/gs/rest-service/)

* 为了学习基础知识，不使用Github上的工程文件，使用`spring initializr`来建立工程的框架 <https://start.spring.io/>
	* 指定各项参数：
    	* Project: 选择使用Maven还是Gradle来管理工程（和依赖）。个人一般使用Maven所以使用Maven
        * Spring Boot 一般选择使用默认的版本
        * Project Metadata 
        	* Group 所在组织名，一般是网址的逆序
            * Artifact 工程的名字，会用于最后生成的jar或者war的命名上
            * 其余自动生成
        * Packaging 选择最后将工程打包成哪种格式的包
        * JAVA 选择使用的JAVA版本
        * Dependencies 选择使用的依赖
        
    * 指定好各项参数后在下载之前先点击`EXPLORE`来预览一下生成的`POM.xml`文件的内容是否正确

* 使用趁手的编译器来建立工程。我使用的是IDEA IntelliJ，在公司也使用同种编译器体验不错。
	* `Create New Project` -> `Maven` -> `Groupd`和`ArtifactId`与之前`spring initializr`的一致即可，版本因为是demo所以不用太在意如果需要版本方面的知识可以参考[语义化版本](https://semver.org/lang/zh-CN/) -> 接下来选择工程位置即可
    
* 然后将`spring initializr`上下载来的压缩包解压复制里面的POM文件到工程的POM文件中。之后一般IDEA会开始自动下载依赖与构建工程。

* 从`spring initializr`下载的压缩包里有个`mvnw.cmd`文件，里面存储着简易的Maven执行命令脚本（比起`mvn`命令更简单）。
	* 首先为了可以在IDEA的`Terminal`中可以运行`./mvnw `命令需要安装Maven的运行环境。我是Mac OS所以直接`brew install mvn`。
    * 然后在工程根目录下（或者直接在IDEA的`Terminal`中）运行`mvn -N io.takari:maven:wrapper`即可在工程根目录中安装[maven-wrapper](https://github.com/takari/maven-wrapper)。
    * 然后运行`./mvnw spring-boot:run`即可编译打包运行工程一条龙。
    
* 教程中值得注意的点：`This application uses the Jackson JSON library to automatically marshal instances of type Greeting into JSON. Jackson is included by default by the web starter.`
	* 意思是Jackson这个可以将JAVA类直接转换成JSON的库已经包含在了`Spring Web` 中了。
    
* 其他内容都写在教程中了
    
2.[访问Restful服务](https://spring.io/guides/gs/consuming-rest/)

* 根据教程即可

3.[使用Maven建立Java工程](https://spring.io/guides/gs/maven/)

* 根据教程即可

4.[上传文件](https://spring.io/guides/gs/uploading-files/)

* 仅凭教程中的代码不足以运行工程，需要根据教程的Github代码来天剑文件存储和例外抛出的类。
	* [教程的示例工程文件](https://github.com/spring-guides/gs-uploading-files/tree/master/complete/src)
