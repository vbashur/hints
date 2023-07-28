# Gradle 

notes coming from linkedin course and day-to-day activities

List configurations for the project

```
./gradlew projects --info
```

List of the tasks
```
./gradlew tasks
```

List dependencies
```
./gradlew :api:dependencies
```

Why is the dependency part of tree
```
./gradlew dependencyInsight
```

## gradle wrapper
The Wrapper standardizes a Gradle version compatible with the build, downloads the distribution for it and uses it to execute the build.

The Wrapper files are checked into version control and allow to execute the build without having to install the Gradle runtime. 

## file structure
`build.gradle` - resides in every project
`settings.gradle` - resides in root directory of the project hierarchy, declares participating projects, can change the defaults
`gradle.properties` - resides in root directory of the project, preconfigures runtime behavior, externalize custom key-value pairs



## define the tasks
below is the exapmle of the __ad-hoc__ task
```
task hello {                // here we have the name of the task
  doLast {                  // 'doLast' defines an action to be perfromed by the task
    println 'Hello world!'
  }
}


task hi << {                // '<<' defines an action to be perfromed by the task (synonym for 'doLast')
  println 'Hello world!'    // '<<' means that entire body of the task is the executable code
}
```

below is the exapmle of the __typed__ task
```
task copyFiles(type: Copy) {   // explicit type 'Copy'
  from "sourceFiles"           // 'from' and 'into' are methods provided by Copy API
  into "target"
}
```

#### dependencies on tasks
```
task hi(dependsOn: hello) << {  // 'dependsOn' follows before the name of the main taks
  println 'Hello world!'    
}
```
another option 
```
task createZip(type: Zip) {
  from "build/docs"
  archiveFileName = "docs.zip"
  destinationDirectory = file("target")
  dependsOn copyFiles
}
```

#### tasks structure
Gradle builds Directed Acyclic Graph (DAG) from the tasks

Example of the custom task that creates .tgz file with renamed txt files:
```
apply plugin: 'base'

task createTar(type: Tar) {
  from "files"
  include '*.txt'
  into 'text'
  rename '(.+).txt', '$1.text'
  compression = Compression.GZIP
  
  doFirst {
    println 'Creating TAR file'
  }
}

task createArchive {
  dependsOn createTar
}

```

### gralde plugins
* script plugin - split logic and make it more maintainable
* binary plugin - implemented as classes; bundled as JAR files

### domain objects
Project (org.gradle.api.project) - represents a software component and provides API access to object hierarchy

Task (org.gradle.api.Task) - represents unit of work with potential dependencies

Action (org.gradle.api.Action) - actual work performed during execution phase

Plugin (org.gradle.api.plugin) - provides reusable logic

### configuration
configuration defines a scope where a library is available, 

`testImplementation' for  example we need test library available at compile and run time, e.g. jupiter-api
`testRuntime' for  example we need test library available at run time only, e.g jupiter-engine


