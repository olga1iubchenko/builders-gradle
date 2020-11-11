# Build the project with Gradle

**What you’ll build**

You’ll generate a Java application with a few modules that follows Gradle’s conventions.

**What you’ll need**

A text editor or IDE - for example [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)

A Java Development Kit (JDK), version 8 or higher

The latest [Gradle distribution](Gradle distribution)

**Create a project folder**
Gradle comes with a built-in task, called init, that initializes a new Gradle project in an empty folder. The init task uses the (also built-in) wrapper task to create a Gradle wrapper script, gradlew.

The first step is to create a folder for the new project and change directory into it.

**Run the init task**
From inside the new project directory, run the init task using the following command in a terminal: gradle init. 
When prompted, select the 2: application project type and 3: Java as implementation language.
Next you can choose the DSL for writing buildscripts - 1 : Groovy or 2: Kotlin. For the other questions, press enter to use the default values.

```sh
$ gradle init

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4]

Project name (default: demo):
Source package (default: demo):


BUILD SUCCESSFUL
2 actionable tasks: 2 executed
```

The init task generates the new project with the following structure:
```sh
├── gradle 
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew 
├── gradlew.bat 
├── settings.gradle 
└── admin
    ├── build.gradle 
    └── src
        ├── main
        │   └── java 
        │       └── ...
        │           
        └── test
            └── java 
                └── ..
 └── services
    ├── build.gradle 
    └── src
        ├── main
        │   └── java 
        │       └── ...
        │           
        └── test
            └── java 
                └── .. 
 └── utils
    ├── build.gradle 
    └── src
        ├── main
        │   └── java 
        │       └── ...
        │           
        └── test
            └── java 
                └── ..   
                
  └── web
    ├── build.gradle 
    └── src
        ├── main
        │   └── java 
        │       └── ...
        │           
        └── test
            └── java 
                └── ..     
 ```               
**Review the project files**
The settings.gradle(.kts) file has two active line:

```sh
settings.gradle
rootProject.name = 'builders'
include 'admin'
include 'services'
include 'utils'
include 'web'
 ``` 
* rootProject.name assigns a name to the build, which overrides the default behavior of naming the build after the directory it’s in. 
It’s recommended to set a fixed name as the folder might change if the project is shared - e.g. as root of a Git repository.

* include defines that the build consists of one subproject called app that contains the actual code and build logic. 

Our build contains 4 subprojects that represents the Java application we are building. It is configured in the app/build.gradle(.kts) file:

```sh
build.gradle
apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'war'


repositories {
    mavenLocal()
    mavenCentral()
}

    dependencies {
        testCompile 'junit:junit:4.12'
        compile group: 'javax.servlet', name: 'javax.servlet-api', version: '3.0.1'
    }

    dependencies {
        compile project(':admin')
        compile project(':web')
    }

jar {
    baseName = 'admin-jar'
    version =  '0.0.1'
}
war {
    from 'web'
    webInf { from 'webapp/WEB-INF' }
    classpath fileTree('libs')
    webXml = file('/web/src/main/resources/web_config.properties')
}

 ``` 
 **Build the project**
 Run:
 ```sh
 gradle build
 ``` 
 Also you can run each module separately using 
  ```sh
 gradle :<module_name>:build
 ``` 
 
 
 
