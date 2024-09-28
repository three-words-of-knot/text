# Maven学习笔记

## Maven是什么？

Maven的本质是一个项目管理工具

它将项目的开发和管理过程抽象成一个项目模型(POM->Project Object Model)

它实现了项目构建（跨平台的项目自动化构建）、依赖管理（方便管理的项目依赖资源，避免了版本冲突）两个主要功能

## 基础概念

**仓库：** 存储Jar包

- 本地仓库：自己电脑上存储资源的仓库，连接远程仓库获取资源
- 远程仓库：中央仓库和私服（中央仓库是全球贡献的，私服是局部共享的）

**坐标**：用以描述仓库的资源位置

- 组织名(group)
- 项目名称(artifactld)
- 版本号(version)
- 包名称(package)

## 配置备忘录

配置Maven的仓库位置需要自己手动进行（一般来说一次就行），（在你修改完环境变量之后）需要修改的地方是 conf 文件夹下的 setting.xml文件，可以在其中修改涉及到的镜像仓库位置、默认下载位置等配置，**注意**，这是全局修改，想要用户修改的话，请在修改的仓库文件夹里再复制一份setting.xml 做自己的修改

## 项目构建命令

```mvn
mvn test//测试
mvn clean //清除
mvn compile //编译
mvn package //打包(只打包源程序，且会先编译再测试最后打包)
mvn install //安装到本地仓库（打包源程序干的他都干，之后需要它来将其放到本地仓库）
```

## 创建目录结构的插件

可以使用命令行直接创建（会在仓库里下载其他插件）

## 依赖管理

1. 首先，依赖会传递（我依赖你，你依赖的我也依赖）
2. 直接依赖是当前的项目的依赖，间接依赖是当前项目依赖的依赖……
3. 生效优先级（层级越浅越高。声明越早越高）

- 可选依赖

```xml
    <dependencies>
        <dependency>
        <groupID>log4j</groupID>
        <artifactID>log4j</artifactID>
        <version>4.12</version>
        <optional>true</optional>
        <!-- 添加这个代表不向上级显示自己使用了这个依赖 -->
        </dependency>    
    </dependencies>
```

- 排除依赖：上级主动断开依赖

```xml
    <dependencies>
        <dependency>
        <groupID>log4j</groupID>
        <artifactID>log4j</artifactID>
        <version>4.12</version>
        <exclusions>
            <exclusion>
            <groupID>junit</groupID>
            <artifactID>junit</artifactID>
            <!-- 不写版本因为，不论版本都不用 -->
            </exclusion>
        </exclusions>
        </dependency>    
    </dependencies>
```

- 依赖范围
  - ```<scope></scope>```词条表示依赖范围设置
  - compile 意味着主程序、测试代码、打包时都有效
  - test 意味着只参与测试代码
  - provided 意味着只参与主代码和测试代码
  - runtime 只参与打包
- 依赖范围传递性
  - 直接依赖范围是 compile时，间接依赖中的资源依赖范围是什么就是什么
  - 直接依赖的依赖范围是 runtime的时候，间接依赖的原有范围是compile、runtime时，现在依赖范围是runtime，其余情况下不变（仍是直接依赖原有范围）
  - 直接以来的依赖范围是test或provided时，间接依赖不会被识别

## 生命周期

1. clean：清理工作，移除上次构建成的文件
2. default：编译、编译测试代码、测试、打包、安装、部署
3. site：生成项目的站点文件、将其部署到特定服务器上

根据生命周期的不同，使用的插件也不一样，插件的配置需要根据生命周期确定自己的执行时间以及执行方式

构建过程事实上是依靠插件进行的。
