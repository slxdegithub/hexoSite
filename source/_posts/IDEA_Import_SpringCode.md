---
title: IDEA导入Spring源码
date: 2021-12-03 20:44:34
categories: spring
tags: "spring"
---
# IDEA导入Spring源码

## 1.下载spring源码

 Spring源码现在在由github托管,git地址

>https://github.com/spring-projects/spring-framework/tree/5.1.x
>
>![/image-20211126191825760](image-20211126191825760.png)

![/image-20211126191900085](image-20211126191900085.png)

先把源码下载好，推荐5.0X或者5.1X

## 2.下载gradle

安装配置gradle环境变量

- To build you will need Git and JDK 8 update 60 or later. Be sure that your JAVA_HOME environment variable points to the jdk1.8.0 folder extracted from the JDK download.

所以安装前要确保javahome在jdk**1.8.0.60**以上版本 


在下载之前，先找到我们下载的源码，spring-framework\gradle\wrapper下面的gradle-wrapper.properties文件，

打开先瞅两眼！



![/image-20211126194001815](image-20211126194001815.png)

打开后可以看到 默认是去gradle仓库下载指定版本的，

所以接下来我们下载的时候最好下载适配版本，不然很容易出现各种奇奇怪怪的错误。

![/image-20211126194132893](image-20211126194132893.png)

gradle下载地址

>https://services.gradle.org/distributions/

配置gradle的环境变量

![/image-20211126192740437](image-20211126192740437.png)

![image-20211126192813164](image-20211126192813164.png)

下面这个是gradle的仓库位置，自己选地方放就行了，注意二级目录是.gradle不能改。



![image-20211126192852129](image-20211126192852129.png)

![image-20211126193015207](image-20211126193015207.png)

如果不改的话默认就会在C盘用户下面的创建一个.gradle

![image-20211126195925377](image-20211126195925377.png)



最后把path添加上

![image-20211126193245885](image-20211126193245885.png)

配好之后可以在cmd上输入 gradle -v检测是否配成功

![image-20211126193343526](image-20211126193343526.png)

其实这一步不做也可以，主要是为了之后使用方便，如果是单纯的构建源码可以省略这步。

## 3.构建源码

我们先选中下载好的源码，直接open打开即可。

先打开IDEA的Settings  --> Plugins 检查有没有下载好插件

![image-20211126193538024](image-20211126193538024.png)



插件安装好之后 在Settings找到Gradle

![image-20211126193820036](image-20211126193820036.png)



可以看到这里可以选择是用gradle-wrapper.properties指定的地址下载gradle，默认会先去你指定的仓库先找，找不到就去下载。也可以使用本地的gradle。建议使用本地gradle。

![image-20211126195251922](image-20211126195251922.png)

配置好之后 我们找到build.gradle文件，配置上国内镜像 下载速度会快很多

```properties
allprojects {
			repositories {
				maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
				maven { url 'https://maven.aliyun.com/repository/google' }
				maven { url 'https://maven.aliyun.com/repository/jcenter'}
			}
		}
```



![image-20211126195730078](image-20211126195730078.png)

然后等待构建完成即可



## 4.常见错误

这里列举一些遇到的坑

第一个

![image-20211126200402355](image-20211126200402355.png)

报找不到这个插件，反正网上试了各种办法都不行，然后换了个idea就没这个错误了。。。这个错误用的是IDEA2019.3.1报的。然后我用2018.2.3和2021.1都没这个问题。感兴趣的自己钻研。。。。。。。

附上版本适配图

![image-20211126200816658](image-20211126200816658.png)

![image-20211126200826599](image-20211126200826599.png)

第二个  

```java
Unable to find method 'org.gradle.api.artifacts.result.ComponentSelectionReason.getDescription()Ljava/lang/String;'.
Possible causes for this unexpected error include:<ul><li>Gradle's dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)
Re-download dependencies and sync project (requires network)</li><li>The state of a Gradle build process (daemon) may be corrupt. Stopping all Gradle daemons may solve this problem.
Stop Gradle build processes (requires restart)</li><li>Your project may be using a third-party plugin which is not compatible with the other plugins in the project or the version of Gradle requested by the project.</li></ul>In the case of corrupt Gradle processes, you can also try closing the IDE and then killing all Java processes.

```

![image-20211126200934973](image-20211126200934973.png)

这个问题出现的可能有两种，

1、gradle不适配，换几个试试。gradle得和spring源码还有IDEA都适配。巨坑

![image-20211126201658911](image-20211126201658911.png)



2、IDEA版本太低！就是这个问题搞了我三个小时！！！！我用2021.1的IDEA就解决了这个错误



第三个

![image-20211126201335041](image-20211126201335041.png)

这个错误。。，gradle版本太低了。和源码不适配，得往高了换。换了一般都能解决。



第四个

```java
Caused by: java.lang.NoSuchMethodError: org.gradle.api.artifacts.ProjectDependency.getConfiguration
```

如果 *build.gradle* 文件包含 **spring-boot-gradle-plugin，**升级其版本 或者其他插件版本低了



第五个 jar包找不到问题，百度很好解决。



