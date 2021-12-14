# Maven

## Maven的一些概念

```
Maven目录结构 -- 每一个Maven项目在磁盘中都是一个文件夹(以项目 -Hello 为例子)
	Hello/
		--/src
			--/main -- 放主程序Java代码和配置文件
				--/java -- 放程序包和包中的Java文件
				--/resources -- Java程序中要使用的配置文件
			--/test -- 放测试程序代码和文件(可以没有)
				--/java -- 测试程序包和包中的Java文件
				--/resources -- 测试Java程序中要使用的配置文件
		--/pom.xml -- Maven核心文件(Maven必须有)
```

```
坐标

<groupId>org.example</groupId> -- groupId 组织ID 一般为公司域名的倒写 1. 域名倒写 2. 域名倒写 + 项目名
<artifactId>Hello</artifactId>	-- artifactId 项目名称、模块名称 对应groupId中项目的子项目
<version>1.0-SNAPSHOT</version> -- version 项目版本号 项目还在开发中通常后跟SNAPSHOT version 使用三位数值标识(1.1.0)
坐标是一个唯一值
此三个组成了一个Maven项目的基本坐标 在众多Maven项目中可以唯一定位到某一个项目 同时也决定着将来项目在仓库中的路径及名称
```

```
打包

packaging -- 打包后压缩文件的扩展名 默认是jar web应用是war packaging可以不写 默认是jar
```

```
依赖
 
[Maven中央仓库](https://mvnrepository.com/repos/central)

dependencies 和 dependency

-- 添加依赖
<dependencies>
	<!-- 依赖 Java代码中 import -->
	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
    	<groupId>mysql</groupId>
    	<artifactId>mysql-connector-java</artifactId>
    	<version>5.1.9</version>
	</dependency>
</dependencies>
```

```
配置属性

<properties>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
</properties>
```

```
构建
 
build -- build表示与构建相关的配置 例如设置编译插件的JDK版本

<build>
	<!-- 配置插件 -->
	<plugins>
		<!-- 配置具体的插件 -->
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<!-- 插件的名称 -->
			<artifactId>maven-compiler-plugin</artifactId>
			<!-- 插件的版本 -->
			<version>3.8.1</version>
			<!-- 插件的信息 -->
			<configuration>
				<!-- 告诉Maven 代码在JDK1.8编译 -->
				<source>1.8</source>
				<!-- 告诉Maven 程序应运行在JDK1.8上 -->
				<target>1.8</target>
			</configuration>
		</plugin>
	</plugins>
</build>
```

```
继承

parent -- 在Maven中如果多个模块都需要声明相同的配置 可以用parent声明要继承的父工程的pom配置
```

```
聚合

modules -- 在Maven多模块的开发中 为了统一构建整个项目的所有模块 可以提供一个额外的模块 该模块的打包方式为pom
	    并且在其中使用modules聚合其他模块 这样通过本模块就可以一键自动识别模块间的依赖关系来构建所有模块
```

## Maven常用命令

```
Maven的生命周期 -- 指Maven构建项目的过程 清理 编译 测试 报告 打包 安装 部署

Maven的命令 -- Maven独立使用 通过命令 完成Maven的生命周期的执行
			Maven可以使用命令 完成项目的清理 编译 测试等等
			
		Maven常用命令
			1. mvn clean -- 清理(删除原来测试和编译的目录(target) 但已install到仓库里的包不会删除)
			2. mvn compile -- 编译主程序(会在当前目录下生成一个target文件夹 里面存放主程序编译生成的字节码文件)
			   mvn test-compile -- 编译测试文件
			3. mvn test -- 测试(会生成一个surefire-reports 保存测试结果)
			4. mvn package -- 打包主程序(会编译 编译测试 测试 并且按照pom.xml配置把主程序打包生成jar包或war包)
			5. mvn install -- 安装主程序(会把本工程打包 并按照本工程的坐标保存到本地库)
			6. mvn deploy -- 部署主程序(会把本工程打包 并按照本工程的坐标保存到本地库 并且还会保存到私服仓库 
			还会自动把项目部署到web容器中)
		注意 -- 执行以上的命令必须在命令行进入到pom.xml所在目录！
			
Maven的插件 -- Maven命令执行时 真正完成功能的是插件 插件就是一些jar文件 一些类

Junit单元测试 -- 测试的是类中的方法 每一个方法都是独立测试的 方法是测试的基本单位(单元)
			Maven借助单元测试 批量测试类中的大量方法是否符合预期
Junit使用
	1. 加入依赖 在pom.xml加入单元测试依赖
		<!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
	2. 在Maven项目中的src/test/java目录下创建测试程序
		推荐的创建类和方法的提示
			1. 测试类的名称 Test + 测试的类名
			2. 测试方法的名称 Test + 方法名称
		如
			@Test
			public void testMethod() {
				
			}
```

