# Gradle 

notes coming from O'Reilly course and day-to-day activities

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

## define the task
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

#### dependencies on tasks
```
task hi(dependsOn: hello) << {  // 'dependsOn' follows before the name of the main taks
  println 'Hello world!'    
}
```
