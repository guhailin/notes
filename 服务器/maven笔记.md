###maven创建
两种创建方式
1. mvn archetype:create -DgroupId=  -DartifactId -DpackageName  （弃用）
2. mvn archetype:generate

archetype称为插件，crate、generate称为目标

###简单的项目对象模型（Project Object Model）

mvn help:effective-pom
查看全部隐藏的pom设置

###插件和目标
一个maven插件是一个单个或者多个目标的集合。
mvn pluginId:goalId -Dkey=value

###生命周期

当maven经过以package为结尾的默认生命周期的时候，下面的目标按顺序被执行：
**resources:resources**
    Resources插件的resources目标绑定到了resources阶段。这个目标复制src/main/resources下的所有资源和其他任何配置的资源目录，到输出目录。
**compiler:compile**
    Compiler插件的compile目标绑定到了compile阶段。这个目标编译src/main/java下的所有源代码和其他任何配置的资源目录，到输出目录。
**surefire:test**
    Surefire插件的test目标绑定到了test阶段。这个目标运行所有的测试并且创建那些捕捉详细测试结果的输出文件。默认情况下，如果有测试失败，这个目标会终止。
**jar:jar**
    Jar插件的jar目标绑定到了package阶段。这个目标把输出目录打包成JAR文件。
总结得来说，当我们运行mvn package，maven运行到打包为止的所有阶段，在maven沿着生命周期一步步向前的过程中，它运行绑定在每个阶段上的所有目标。你也可以像下面这样显式的指定一系列插件目标，以得到同样的结果：

```
mvn resources:resources \
    compile:compile \
    resources:testResources \
    compile:testCompile \
    surefire:test \
    jar:jar
```

###maven坐标
maven项目坐标：groupId，artifactId，version和packaging。
maven坐标通常用冒号来作为分隔符来书写，groupId:artifactId:packaging:version。
**groupId**
    团体，公司，小组，组织，项目，或者其他团体。团体标识的约定是，它以创建这个项目的组织名称的逆向域名(reverse domain name)开头。
**artifactId**
    在groupId下的表示一个单独项目的唯一标识符。
**version**
    一个项目的特定版本。发布的项目有一个固定的版本表示表示来指向该项目的某一个特定的版本。而正在开发中的项目可以用一个特殊的标识，这种标识给版本加上一个"SNAPSHOT"的标记。
项目的打包格式也是maven坐标的重要组成部分，但是它不是项目唯一标识符的一部分。一个项目的groupId:artifactId:version使之成为一个独一无二的项目：你不能同时拥有同样的groupId,artifactdId和version标识的项目。
**packaging**
    项目的类型，默认是jar，描述了项目打包后的输出。类型为jar的项目产生一个JAR文件，类型为war的项目产生一个web应用。

###maven依赖管理

在maven中一个依赖不仅仅是一个JAR。它是一个POM文件，这个POM可能也声明了对其他构件的依赖。这些依赖的依赖叫做传递性依赖，maven仓库不仅仅存储二进制文件，也存储了这些文件的元数据(metadata)，才使传递性依赖成为可能。
maven也提供了不同的依赖范围(dependency scope)。当一个依赖的范围是test的时候，说明它在Compiler插件运行compile目标的时候是不可用的。它只有在运行compiler:testCompile和surefire:test目标的时候才会被加入到calsspath中。
当为项目创建JAR文件的时候，它的依赖不会被捆绑在生成的构件中，他们只是用来编译。当用maven来创建WAR或者EAR，你可以配置maven让它在生成的构件中捆绑依赖。provided范围告诉maven一个依赖在编译的时候需要，但是它不应该被捆绑在构建的输出中。当你开发web应用的时候provided范围变得十分有用，你需要通过Servlet API来编译你的代码，但是你不希望Servlet api的JAR文件包含在你web应用的WEB-INF/lib目录中。

###站点生成和报告###

另外一个maven的重要特征是，它能生成文档和报告。
```
mvn site
```
这将会运行site生命周期阶段。它不像默认生命周期那样，管理代码生成，操作资源，编译，打包等等。Site生命周期只关心处理在src/site目录下的site内容，还有生成报告。在这个命令运行过之后，你将会在target/site目录下看到一个项目web站点。载入target/site/index.html你会看到项目站点的基本外貌。