# Gradle

## Task
- A task is an object
- A task has an API
- A task has a list of activities
- Tasks form a Directed Ayclic Graph
- A gradle build is a program

Create a file `build.gradle` in an empty directory

```
$ task helloWorld
```

 Now execute the command

```
$ gradle tasks --all
...
Other tasks
-----------
...
helloWorld
...
BUILD SUCCESSFUL in 723ms
1 actionable task: 1 executed
```

The helloWorld task does nothing, now configure the task

```
task helloWorld

helloWorld {
    doLast {
        println "Hello, world !"
    }
}
```

Save and run gradle build

```
$ gradle helloWorld

> Task :helloWorld
Hello, world !

BUILD SUCCESSFUL in 836ms
1 actionable task: 1 executed
```
Build file with two tasks

```
task hello
task world

world {
    doLast {
        println "world!"
    }
}

hello {
    doLast {
        println "Hello, "
    }
}
```

Execute the tasks

```
$ gradle -q hello
Hello,
$ gradle -q world
world!
```

Adding dependency

```
task hello
task world

world {
    dependsOn << hello
    doLast {
        println "world!"
    }
}

hello {
    doLast {
        print "Hello, "
    }
}
```

```
$ gradle -q world
Hello, world!
```

Alternative way of adding dependencies 

```
task hello
task world
task helloWorld

helloWorld {
    dependsOn << [world, hello]
}

world {
    doLast {
        println "world!"
    }
}

hello {
    doLast {
        print "Hello, "
    }
}
```

Notice the short-hand notation for long task names

```
$ gradle -q hW
Hello, world!
```
## Java Plugin

```
plugins {
	id 'java'
}

task caesar(type: JavaExec) {
	mainClass = 'org.monadic.poetry.Poetry'
	classpath = sourceSets.main.runtimeClasspath
}
```

