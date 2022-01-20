---
title: "Code Format"
date: 2022-01-20T10:58:28+08:00
draft: false
tags: ["Java"]
categories: ["Java Foundation"]

showShare: false
---

在开发过程中，大家的开发工具、格式化的习惯等都有所不同，在进行协作时，如果将一个未进行格式化的代码文件进行格式化，将会有很大的变动，在Git的变动中，会不可读，而且很容易造成代码冲突，另外处于规范的角度，统一的代码格式化是很有必要的，之前都是基于默认的格式化（eclipse、idea），在切换工具时，会发现，差别很大，所以需要工具、环境无关的方案，本文就Spring format在《Idea + Gradle》使用的代码格式化来进行说明。

# Gradle

## 添加格式化插件

```
buildscript {
	repositories {
		mavenCentral()
	}
	dependencies {
		classpath("io.spring.javaformat:spring-javaformat-gradle-plugin:0.0.29")
	}
}

apply plugin: 'io.spring.javaformat'
```

插件会添加`format`和`checkFormat`任务到项目，当运行Gradle的`check`任务时，`checkFormat`任务会自动执行，如果想排除某些包，不进行检查，可以添加如下配置：

```
tasks.withType(io.spring.javaformat.gradle.CheckTask) {
    exclude "package/to/exclude"
}
```

## 添加Checkstyle

```
pply plugin: 'checkstyle'

checkstyle {
	toolVersion = "8.45.1"
}

dependencies {
	checkstyle("io.spring.javaformat:spring-javaformat-checkstyle:0.0.29")
}
```

添加checktyle配置文件（默认目录config/checkstyle/checkstyle.xml）

```
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
   		"-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
   		"https://checkstyle.org/dtds/configuration_1_3.dtd">
<module name="com.puppycrawl.tools.checkstyle.Checker">
	<module name="io.spring.javaformat.checkstyle.SpringChecks" />
</module>
```

# Java 8

默认formatter需要Java 11，在项目根目录添加`.springjavaformatconfig`文件：

```
java-baseline=8
```

# 空格缩进

默认的缩进格式为`tabs`（强烈建议），如果需要使用空格进行缩进，在项目根目录添加`.springjavaformatconfig`文件：

```
indentation-style=spaces
```

# Tips

## 排除指定的检查

```
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
   		"-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
   		"https://checkstyle.org/dtds/configuration_1_3.dtd">
<module name="com.puppycrawl.tools.checkstyle.Checker">
	<module name="io.spring.javaformat.checkstyle.SpringChecks">
		<property name="excludes" value="io.spring.javaformat.checkstyle.check.SpringAvoidStaticImportCheck" />
	</module>
</module>
```

## 代码块禁用格式化

```
// @formatter:off

... code not be formatted

// @formatter:on
```

# 参考

- [spring-javaformat](https://github.com/spring-io/spring-javaformat)
- [google-java-format](https://github.com/google/google-java-format)
- [vjtools-formatter](https://github.com/vipshop/vjtools/tree/master/standard/formatter)
