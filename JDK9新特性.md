# JDK9 新增了什么

## JDK9重点改变

### Java 模块化

一种新的java编程组件，模块化——由一个名称来自我描述一个代码或者数据的集合:

- 引入一种新的可选阶段```link time```介于编译时间和运行时间，在此期间，可以将一组模块组装并优化成自定义运行时映像;在java中可以查看```jlink```工具。

- 添加可配置项到```javac```,```jlink```和```java```可以指定模块的路径。这个来定位模块的定义。

- 引入模块化JAR文件,这些文件在根目录有一个```module-info.class```文件。

- 引入JMOD格式，一种类似于JAR的打包格式。可以将本地代码和配置文件打包;```jmod```工具。

JDK本身就被分成一系列的模块，这里改变:

- 能够将JDK的模块组合成各种配置,包括:
	- 与JRE和JDK相对应的配置。
	- 与Java SE 8中定义的每个紧凑配置文件的内容大致相当。
	- 自定义配置，仅包含指定的模块集及其所需模块
- 重新构造JDK和JRE运行时映像，以适应模块并提高性能、安全性和可维护性。

- 定义一个新的URI方案，用于命名模块、类和存储在运行时映像中的资源，而不需要揭示映像的内部结构或格式。

- Removes the endorsed-standards override mechanism and the extension mechanism。

- 删除```rt.jar```和```tools.jar```。

- 默认大多数JDK内部API难见但是留下关键字。广泛使用的内部API可访问，直到所有或大部分功能都支持替换。
运行```jdeps -jdkinternals```确定您的代码是否使用内部JDK api。

### JEP223: 新```Version-String```方案

(标志Java中的版本号的字符串)

提供一个简化的```Version-string```标准以更清晰的区分主要的，小的，安全的，补丁更新版本。

新的```version-string```标准如:

<pre>
$MAJOR.$MINOR.$SECURITY.$PATCH
</pre>

## 新的JDK9安装方式

JDK 9增强Microsoft Windows和macOS平台的安装程序。

### Windows安装程序

使用安装程序UI来启用或禁止网站发布(是否让JAVA运行在网页上)。

在欢迎页提供选项来禁止或开启网站发布。选择```Custom Setup```点击```Install```然后选择```Enable Java content in the Browser```选择框来开启。

### macOS安装程序

**CPU 版本可用**

在卸载当前CPU版本后，提供下一个CPU可用性的通知。

**用户体验**

更新JRE以提高用户体验。

## 新的工具

JDK9增强的工具

### jshell:Java Shell(Read-Eval-Print Loop)

添加 Read-Eval-Print Loop(REPL)方法

```jshell```交互式工具提供命令行接口用于评估Java里的声明、 语句和表达式。它促进了对编码选项的原型和探索的直接结果和反馈。即时反馈与从表达开始的能力相结合对于教育是有用的——无论是学习Java语言还是仅仅学习新的API或语言特性。

查看```jshell```工具。

JShell API开启应用使用REPL功能.查看```jdk.jshell```包。

### JEP228: 添加更多诊断命令

定义更多诊断命令来提高Hotspot里的issues和JDK的诊断能力。

查看```jcmd```。

### JEP231: 删除启动时间JRE(Launch-Time)版本选择

移除在启动时间请求一个并没有正在运行的JRE的能力。

现代应用程序通常通过Java Web Start(使用JNLP文件)、原生操作系统打包系统或使用安装程序来部署。这些技术有自己的方法来管理所需的JREs，这些方法是根据需要查找或下载和更新所需的JRE。这使得启动时JRE版本的选择过时了。

### JEP238: Multi-Release JAR Files(应该指JAR可以同时有多个发布版本)

扩展JAR文件格式，以启用多个、Java发布的类文件版本，以便在单个归档中共存。

一个多版本JAR(MRJAR)包含针对特定Java平台版本的类和资源的额外的、版本化的目录。使用```jar```工具的```——release```选项指定版本目录。

### JEP240: 删除JVM TI hprof 代理

从JDK删除```hprof```代理。这个代理是作为JVM工具接口的演示代码编写的，而不是用来作为生产工具的。

hprof代理的有用特性已经被更好的替代方案所取代。

> 注意: 虽然hprof代理已经被删除，但仍然可以使用jmap或其他诊断工具在hprof格式中创建堆转储。参见Java平台中的诊断工具，标准版故障排除指南。

### JEP241:删除jhat工具

从JDK中删除```jhat```工具

```jhat```工具是在JDK 6中添加的试验的不严密的工具。它已经过时了;高级堆的```visualizer```和分析器已经使用很多年了。

### JEP245:校验JVM命令行标记参数

校验所有的数字JVM命令行标志参数来避免失败，并在发现错误消息时显示适当的错误消息。

需要用户指定的数值的参数，已经实现了范围和可选的约束检查。

查看[这里](http://docs.oracle.com/javase/9/tools/java.htm#JSWOR-GUID-9569449C-525F-4474-972C-4C1F63D5C357)

### JEP247:编译旧平台版本

增强```javac```可以编译Java程序来运行在老版本的平台上。

使用```-source```或者```-target```选项。编译程序可能会意外地使用在给定目标平台上不支持的api。```--release```选项将防止api的意外使用。

### JEP282:jlink: JAVA Linker

```JEP220```:将一组模块及其依赖项优化为自定义运行时映像。

```jlink```工具定义了一个插入式机制用于在组装过程中进行转换和优化，以及生成替代图像格式。它可以为单个程序创建一个自定义运行时```custom runtime optimized```。```JEP261```定义链接时间是编译时和运行时之间的一个可选阶段。链接时间需要一个连接工具来组装和优化一组模块及其传递依赖关系来创建运行时图像或可执行文件。

## 在JDK 9中有什么新的安全性

### JEP219:数据传输层安全(DTLS)

允许JSSE API和SunJSSE支持DTLS Version 1.0和DTLS Version 1.2协议。

查看[DTLS](http://docs.oracle.com/javase/9/security/java-secure-socket-extension-jsse-reference-guide.htm#JSSEC-GUID-D0F8B9C3-721B-43A4-B2CE-7512B175F76D)