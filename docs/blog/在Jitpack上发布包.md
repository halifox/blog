---
title: 在Jitpack上发布Android包
createTime: 2025/10/03 00:00:00
permalink: /blog/79h6phn8/
tags:
  - Jitpack
---
>相关文档: https://docs.jitpack.io/android/#create-your-release

# 添加maven-publish插件

build.gradle.kts

```kotlin
plugins {  
    id("maven-publish")
}

afterEvaluate {
    publishing {
        publications {
            create<MavenPublication>("maven") { //"release"
                from(components["release"])  //如果是纯Java\Kotlin项目则为"java" 例如ksp项目
                groupId = "com.github.jitpack"
                artifactId = "android-example"
                version = "1.0.1"
            }
        }
    }
}

```

# 发布 ANDROID 库

要在 JitPack 上发布您的 Android 库，您只需要在 Git 存储库中有一个可用的构建文件。

Android SDK 在构建环境中可用，并且 ANDROID\_HOME 变量在构建开始时已设置。默认情况下，构建使用 Java 8 运行，但可以使用 jitpack.yml 文件进行配置。

例如使用Java17

```yaml
# jitpack.yml
jdk:  
  - openjdk17
```

## 配置Gradle

要在 JitPack 上进行构建，您需要配置`maven-publish`Gradle 插件用于发布您的库，如中所述[Android 文档](https://developer.android.com/studio/build/maven-publish-plugin)。

## 检查发布并从本地安装依赖

检查您的库是否可以安装到 mavenLocal (`$HOME/.m2/repository`）：

> https://docs.gradle.org/current/kotlin-dsl/gradle/org.gradle.api.artifacts.dsl/-repository-handler/maven-local.html

存储库的位置确定如下（按优先顺序）：

1. 系统属性“maven.repo.local”的值（如果已设置）；
2. 元素的值该文件是否存在并且元素已设置；`~/.m2/settings.xml`
3. 元素的值of （其中是具有该名称的环境变量的值）如果该文件存在并且元素已设置；`$M2_HOME/conf/settings.xml``$M2_HOME`
4. 路径。`~/.m2/repository`

```groovy
./gradlew publish

./gradlew publishToMavenLocal

// or if you named your publication "release"
./gradlew publishReleasePublicationToMavenLocal
```

```groovy
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
		mavenLocal()//从本地检索包
    }
}
```

```groovy
dependencies {
    implementation("com.github.jitpack:android-example:1.0.1")//从本地检索包
}
```

## 创建您的依赖版本

如果上一步一切顺利，您的库就可以发布了！创建 GitHub 版本或添加 git 标签即可完成！

## 从jitpack引入依赖

您的库的用户需要将 jitpack.io 存储库添加到**settings.gradle**文件中：

```groovy
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

```groovy
dependencies {
    implementation("com.github.jitpack:android-example:1.0.1")
}
```

注意：不要在下面添加 jitpack.io 存储库`buildscript`

## 添加示例应用程序

如果您将示例应用程序添加到同一存储库，那么您的应用程序需要依赖于该库。要在 app/build.gradle 中执行此操作，请添加以下形式的依赖项：

```groovy
dependencies {
    implementation project(':library')
}
```

其中“library”是您的库模块的名称。

## 例子

* [示例](https://github.com/jitpack/android-example)
