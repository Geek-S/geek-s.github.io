---
title: "Flatten Maven Plugin"
date: 2022-01-11T21:08:28+08:00
draft: false
tags: ["java", "maven"]
categories: ["Java Foundation"]

showShare: false
---

PS：Version：1.2.2

# 场景

在进行多模块开发时，会在父级pom中定义统一版本号，如果直接进行打包，会直接将变量带入pom/jar中，导致无法正常使用，使用flatten-maven-plugin，会在打包前执行[flatten:flatten](https://www.mojohaus.org/flatten-maven-plugin/flatten-mojo.html)，会将变量替换成实际值

示例

```
<build>
  <plugins>
    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>flatten-maven-plugin</artifactId>
      <version>${maven-flatten-version}</version>
      <configuration>
        <!-- 打包使用生成的pom -->
        <updatePomFile>true</updatePomFile>
        <!-- 仅处理revision、sha1、changelist变量 -->
        <flattenMode>resolveCiFriendliesOnly</flattenMode>
        <!-- 为依赖添加版本号 -->
        <pomElements>
          <dependencies>expand</dependencies>
        </pomElements>
      </configuration>
      <executions>
        <execution>
          <id>flatten</id>
          <phase>process-resources</phase>
          <goals>
            <goal>flatten</goal>
          </goals>
        </execution>
        <execution>
          <id>flatten.clean</id>
          <phase>clean</phase>
          <goals>
            <goal>clean</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

# Goals

## flatten:clean

删除flatten:flatten生成的文件

## flatten:flatten

生成新的pom文件，并将项目的pom指向该文件

### 可选参数

#### embedBuildProfileDependencies

如果为true，则除根据os和jdk来激活的profile外的将被移除，如果是被激活的profile的dependencies将会直接放入pom中（默认false）

#### flattenDependencyMode

direct 仅处理直接依赖的版本（默认）

all	处理直接和传递依赖，将传递依赖转化成直接依赖（配置flattenMode后，不再生效）

可通过flatten.dependency.mode属性配置

#### flattenMode

ossrh 保留所有[OSS Repository-Hosting](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide)需要的可选节点

bom 除ossrh外，保留dependencyManagement和properties节点，

defaults 移除所有可选的pom节点，除repositories外

resolveCiFriendliesOnly 仅处理revision、sha1、changelist变量

可通过flatten.mode属性配置

#### flattenedPomFilename

生成pom文件名称（默认.flattened-pom.xml），可通过flattenedPomFilename变量配置

#### outputDirectory

生成pom文件目录（默认${project.basedir}）

#### pomElements

定义如何处理其他的pom节点，如果可以的话，尽量使用flattenMode，此参数仅为要求极度灵活配置使用

#### updatePomFile

是否将生成的pom设置为当前项目的pom，，可通过updatePomFile属性配置

# 参考

- [flatten-maven-plugin](https://www.mojohaus.org/flatten-maven-plugin)
