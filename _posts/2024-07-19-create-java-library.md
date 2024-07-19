---
title: "[Java] 자바 라이브러리 생성 방법"
description: 
author: 이원모
date: 2024-07-19
categories: [Language, Java]
tags: [library, jar, java, 자바]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

Java 라이브러리는 여러 프로젝트에서 공통으로 사용될 수 있는 유용한 함수를 모아놓은 코드의 집합입니다. 이번 포스트에서는 Java 라이브러리를 만드는 다양한 방법을 단계별로 알아보겠습니다. 여기서는 Gradle, Maven, Ant, 그리고 순수 자바 프로젝트를 사용하여 라이브러리를 만드는 방법을 다룹니다.

## Gradle 활용
---
Gradle은 프로젝트 빌드, 의존성 관리, 배포 등을 자동화하는 강력한 도구입니다. 먼저 Gradle을 사용하여 라이브러리를 만드는 방법을 알아보겠습니다.

<br>

1\. 프로젝트 디렉토리 생성 및 초기화

먼저 Gradle 프로젝트 디렉토리를 생성하고 초기화합니다.

```bash
mkdir utility-library
cd utility-library
gradle init
```

<br>

2\. build.gradle 파일 설정

프로젝트 루트 디렉토리에 있는 build.gradle 파일을 수정하여 필요한 설정을 추가합니다.

```groovy
plugins {
    id 'java'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    // 필요한 의존성을 여기에 추가할 수 있습니다.
}

jar {
    archiveBaseName = 'utility-library'
    archiveVersion = version
    from sourceSets.main.output
    manifest {
        attributes(
            'Implementation-Title': 'Utility Library',
            'Implementation-Version': version
        )
    }
}
```

<br>

3\. 디렉토리 구조 설정 및 유틸리티 함수 작성

프로젝트 디렉토리 구조를 설정하고 유틸리티 함수를 작성합니다.

```text
utility-library/
├── build.gradle
└── src
    └── main
        ├── java
        │   └── com
        │       └── example
        │           └── UtilityFunctions.java
        └── resources
```

```java
package com.example;

public class UtilityFunctions {

    public static int add(int a, int b) {
        return a + b;
    }

    public static String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>

4\. JAR 파일 생성

터미널에서 다음 명령어를 실행하여 JAR 파일을 생성합니다.

```bash
./gradlew clean build
```

이 명령어가 완료되면 build/libs 디렉토리에 utility-library-1.0.0.jar 파일이 생성됩니다.

<br>

## Maven 활용
---
Maven은 널리 사용되는 프로젝트 관리 및 이해 도구로, 프로젝트의 빌드, 보고서 생성, 문서화를 관리할 수 있습니다.

<br>

1\. 프로젝트 디렉토리 생성 및 초기화

먼저 Maven 프로젝트 디렉토리를 생성하고 초기화합니다.

```bash
mkdir utility-library
cd utility-library
mvn archetype:generate -DgroupId=com.example -DartifactId=utility-library -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

<br>

2\. pom.xml 파일 설정

프로젝트 루트 디렉토리에 있는 pom.xml 파일을 수정하여 필요한 설정을 추가합니다.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>utility-library</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    <dependencies>
        <!-- 필요한 의존성을 여기에 추가 -->
    </dependencies>
</project>
```

<br>

3\. 디렉토리 구조 설정 및 유틸리티 함수 작성

프로젝트 디렉토리 구조를 설정하고 유틸리티 함수를 작성합니다.

```text
utility-library/
├── pom.xml
└── src
    └── main
        ├── java
        │   └── com
        │       └── example
        │           └── UtilityFunctions.java
        └── resources
```

```java
package com.example;

public class UtilityFunctions {

    public static int add(int a, int b) {
        return a + b;
    }

    public static String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>

4\. JAR 파일 생성

터미널에서 다음 명령어를 실행하여 JAR 파일을 생성합니다.

```bash
mvn clean package
```

이 명령어가 완료되면 target 디렉토리에 utility-library-1.0.0.jar 파일이 생성됩니다.

<br>

## Ant 활용
---
Ant는 Java 라이브러리 및 애플리케이션을 빌드하고 배포하는 도구입니다.

<br>

1\. 프로젝트 디렉토리 생성 및 초기화

먼저 프로젝트 디렉토리를 생성하고 초기화합니다.

```bash
mkdir utility-library
cd utility-library
```

<br>

2\. build.xml 파일 설정

프로젝트 루트 디렉토리에 build.xml 파일을 생성하고 설정을 추가합니다.

```xml
<project name="utility-library" default="jar" basedir=".">
    <property name="src" location="src"/>
    <property name="build" location="build"/>
    <property name="dist" location="dist"/>

    <target name="clean">
        <delete dir="${build}"/>
        <delete dir="${dist}"/>
    </target>

    <target name="compile">
        <mkdir dir="${build}"/>
        <javac srcdir="${src}" destdir="${build}"/>
    </target>

    <target name="jar" depends="compile">
        <mkdir dir="${dist}"/>
        <jar destfile="${dist}/utility-library.jar" basedir="${build}"/>
    </target>
</project>
```

<br>

3\. 디렉토리 구조 설정 및 유틸리티 함수 작성

프로젝트 디렉토리 구조를 설정하고 유틸리티 함수를 작성합니다.

```text
utility-library/
├── build.xml
└── src
    └── com
        └── example
            └── UtilityFunctions.java
```

```java
package com.example;

public class UtilityFunctions {

    public static int add(int a, int b) {
        return a + b;
    }

    public static String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>

4\. JAR 파일 생성

터미널에서 다음 명령어를 실행하여 JAR 파일을 생성합니다.

```bash
ant jar
```

이 명령어가 완료되면 dist 디렉토리에 utility-library.jar 파일이 생성됩니다.

<br>

## 순수 자바 프로젝트 활용
---
Gradle, Maven, Ant와 같은 빌드 도구를 사용하지 않고, 순수 자바 프로젝트로도 JAR 파일을 만들 수 있습니다.

<br>

1\. 프로젝트 디렉토리 구조 설정

프로젝트 디렉토리를 설정합니다.

```text
utility-library/
├── src
│   └── com
│       └── example
│           └── UtilityFunctions.java
└── manifest.txt
```

<br>

2\. 유틸리티 함수 작성

src/com/example/UtilityFunctions.java 파일을 생성하고 유틸리티 함수를 작성합니다.

```java
package com.example;

public class UtilityFunctions {

    public static int add(int a, int b) {
        return a + b;
    }

    public static String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>

3\. 컴파일

javac 명령어를 사용하여 Java 파일을 컴파일합니다. 컴파일된 클래스 파일은 bin 디렉토리에 저장합니다.

```bash
mkdir bin
javac -d bin src/com/example/UtilityFunctions.java
```

<br>

4\. manifest.txt 파일 작성

프로젝트 루트 디렉토리에 manifest.txt 파일을 생성하고 다음 내용을 추가합니다.

```text
Manifest-Version: 1.0
Created-By: 1.8.0_242 (Oracle Corporation)
```

<br>

5\. JAR 파일 생성

jar 명령어를 사용하여 JAR 파일을 생성합니다.

```bash
jar cfm utility-library.jar manifest.txt -C bin .
```

이 명령어는 manifest.txt 파일을 포함하여 bin 디렉토리의 모든 클래스 파일을 utility-library.jar 파일로 패키징합니다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI