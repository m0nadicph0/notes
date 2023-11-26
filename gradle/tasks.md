# Tasks

Writing tasks

```
tasks.register('first') {
  doLast {
    println "First task"
  }
}
```

specifying task dependencies

```
tasks.register('first') {
	doLast {
		println "First task"
	}
}

tasks.register('second') {
	dependsOn tasks.first
	doLast {
		println "Second task"
	}
}
```
dynamic tasks and accessing tasks

```
4.times {
	tasks.register("task$it") {
		doLast {
			println "executing $it"
		}
	}
}

tasks.named('task0') {
	dependsOn('task1', 'task2')
}
```


