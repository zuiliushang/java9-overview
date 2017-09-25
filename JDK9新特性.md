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

### JEP244:TLS应用层协议协商扩展

允许客户端或者服务器在传输层安全(TLS)连接协商使用应用协议。在应用层协议协定(ALPN)中,客户端发送一系列支持的应用协议作为ServerHello信息。在TLS握手时应用协议协商可以被完成。即使没有网络传输。

...略

## JDK9中新的发布

增强JDK9发布功能。

### 反对Java插件法

在Oracle的JDK 9构建中，不赞成Java插件和相关的applet技术。虽然在JDK 9中仍然可用，但这些技术将被考虑在未来的版本中从Oracle JDK和JRE中删除。

在web页面中嵌入的applet和JavaFX应用程序需要运行Java插件。考虑将这些类型的应用程序重写为Java Web Start或自包含应用程序。

### 加强JAVA控制面板

在Java控制面板中改进分组和显示选项。信息更容易定位，搜索字段可用，模态对话框不再使用。注意，一些选项的位置已经从以前版本的Java控制面板更改。

### JEP275:Java模块化应用打包

将项目Jigsaw的特性集成到Java封装器中，包括模块感知和自定义运行时创建。

利用jlink工具创建更小的包。

仅创建使用JDK9运行的应用。不能将应用程序打包为早期版本的JRE。

### JEP289:反对Applet API

随着web浏览器供应商删除对Java浏览器插件的支持，Applet API变得越来越不实用。尽管在JDK 9中仍然可用，Applet类将在未来的版本中被考虑删除。考虑将applet重写为Java Web Start或自包含应用程序。

## JDK9 Java语言的新特性

一点小改动

### JEP213:Milling Project Coin

一些细微的变化:

- 允许@SafeVargs在私有实例方法中使用。

- 允许有效的final变量作为资源在try-with-resources语句中使用。

- 如果推断类型的参数类型是可指示的，则允许使用匿名类。

- 完成删除,从JAVA SE8开始,从一组合法的标识符名称的下划线。

- 添加私有接口方法支持。

## JDK9新的javadoc

javadoc增强包括:简化的Doclet API,Javadoc查找,支持生成HTML5输出,支持模块系统中的文档注释。

## JDK9 JVM新的特性

JDK9中JVM的增强:

### JEP165:编译控制

提供一种方式来控制JVM编译(编译指定选项)。控制级别是运行时可管理的和有方法的。编译器控制，并且向后兼容，与编译命令。

### JEP197:段落代码缓存

将代码缓存划分为不同的段，每个段包含特定类型的编译代码，以提高性能并启用未来扩展。

### JEP276:语言定义对象模型的动态链接

在运行时动态链接高等级对象操作,例如读写资源，调用方法，适当的目标方法执行。它将这些操作与基于传递的实际值类型的目标方法句柄链接起来。这些对象操作被表示为invokedynamic。

```java.lang.invoke```为invokedynamic调用标记的动态链接提供了一个低级API，它没有提供一种方法来表达更高级别的对象或实现它们的方法。

通过```jdk.dynalink```包,您可以实现编程语言，其表达式包含动态类型(不能静态确定的类型)，以及在这些动态类型上的操作被表示为invokedynamic call sites(因为语言的对象模型或类型系统与JVM不太匹配)。

## JDK9 JVM调优新特性

JDK9增强的JVM调优

### 改进G1的可用性、确定性和性能

增强垃圾回收器(G1)，自动确定几个重要的内存回收设置。以前这些设置必须手动设置以获得最优结果。此外，还解决了G1垃圾收集器的可用性、确定性和性能问题。

### JEP158:统一JVM日志

引入一个通用日志系统来管理JVM所有组件。

可以在Java中-Xloggc java选项查看

### JEP214:删除JDK 8中反对的GC组合

删除JD8中已经弃用的GC组合。

以下GC组合不再存在:

- DefNew + CMS

- ParNew + SerialOld

- Incremental CMS

并发标记扫描(CMS)的“前台”模式也已被删除。已删除下列命令行标志:

- -Xincgc
- -XX:+CMSIncrementalMode
- -XX:+UseCMSCompactAtFullCollection
- -XX:+CMSFullGCsBeforeCompaction
- -XX:+UseCMSCollectionPassing

命令行标志- xx:+ UseParNewGC不再有效果。ParNew只能用于CMS,CMS需要ParNew。因此，- xx:+ UseParNewGC标志被弃用，很可能在以后的版本中删除。

### JEP248:使用G1作为默认的垃圾收集器

### JEP271:统一的GC日志记录

使用JEP 158中引入的统一JVM日志框架重新实现垃圾收集(GC)日志。GC日志记录以与当前GC日志记录格式一致的方式重新实现;然而，新旧格式之间存在一些差异。

### JEP291:弃用并发标记扫描(CMS)垃圾收集器

不使用并发的标记扫描(CMS)垃圾收集器。在命令行上使用- xx:+ UseConcMarkSweepGC选项时发出警告消息。garbage - first(G1)垃圾回收器是为了取代CMS的大多数用途。

## JDK9新的重要库

### JEP102:Process API更新

增强控制和管理操作系统进程的API

```ProcessHandle```类提供process的native process ID,arguments,command,start time,累积CPU时间,user,父process和衍生代。这个类同样可以监控进程的生命周期和结束进程。有```ProcessHandle.onExit```方法，CompletableFuture类的异步机制可以在进程退出时执行操作。

### JEP193:Variable处理

在对象字段(field)和数组元素定义```java.util.concurrent.atomic```和```sun.misc.Unsafe```操作的等价方法。

定义一套标准的防护操作,由```VarHandle```静态方法组成用来启用内存排序细粒度的的控制。这是替代 sun.misc.Unsafe，提供一个非标准的防护操作集。

### JEP254:紧凑型字符串

对字符串采用更空间有效的内部表示。以前，字符串类在char数组中存储字符，每个字符使用两个字节(16位)。字符串类的新内部表示是一个字节数组，加上一个挂起标记字段。

这纯粹是一个实现变更，没有对现有公共接口的更改。

查看```CompactStrings ```[Java选项](http://docs.oracle.com/javase/9/tools/java.htm#JSWOR624)

### JEP264:平台日志API和服务

定义一个最小的日志API，平台类可以使用它来记录消息，并为这些消息的使用者提供一个服务接口。一个库或应用程序可以提供此服务的实现，以将平台日志消息路由到其选择的日志框架。如果没有提供实现，则基于```java.util.logging```的默认实现。使用日志API。

### JEP266:更多并发性更新

在JEP 155的JDK 8中引入了更多的并发性更新:并发更新，包括可互操作的发布-订阅框架和对CompletableFuture API的增强。

### JEP268:XML分类

添加一个标准的XML目录API，支持组织提高结构化信息标准(OASIS)XML目录版本1.1标准。API定义了目录和目录解析器抽象，这些抽象可以作为带有JAXP处理器的固有或外部解析器，接受解析器。

使用内部的分类API的现有库或应用程序需要迁移到新的API以利用新特性。

### JEP269:集合便利的工厂方法

更加容易创建少量的元素的集合的实例和映射。新的静态工厂方法加入```List```,```Set```和```Map```接口使创建这些集合的不可变实例变得更加简单。

例如:
``` Set<String> alphabet = Set.of("a","b","c");```

### JEP274:加强方法处理

加强```java.lang.invoke```包下的```MethodHandle```,```MethodHandles```和```MethodHandles.Lookup```类更容易使用和更好的编译器优化。

添加包括:

- 在```java.lang.invoke```包下的```MethodHandle```类中提供一个新的```MethodHandle```来循环```try/finally```代码块。

- 加强```MethodHandle```和```MethodHandles```类用于参数处理的新方法处理组合器。

- 为接口方法实现新的查找，并在```MethodHandles.Lookup```类中新增可选择的父构造器。

### JEP277:删除加强

修改```@Deprecated```注解来提供关于规范中API的状态和预期配置的更好信息。添加了两个新元素:

- ```@Deprecated(forRemoval=true)```表示这个API在未来将会被移除。
- ```@Deprecated(since="version")```包含Java SE版本字符串，该字符串表示API元素何时被弃用。

例如:```@Deprecated(since="9", forRemoval=true)```

可以使用```jdeprscan```工具来扫描。

### JEP285:循环等待提示

定义一个API，该API允许Java代码提示自旋循环正在执行。一个自旋循环反复地检查一个条件是否为真，比如当一个锁可以被获取时，在此之后，可以安全地执行一些计算，然后释放锁。这个API纯粹是一个提示，不带有任何语义行为需求。查看[Thread.onSpinWait](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.html#onSpinWait--)方法。