# Reflection 1.2

The reason why "Adhis's Computer: hey hey" prints first is due to the sequencing and behavior of task execution in the Rust code. Initially, the "hey hey" message is printed directly in the ```main()``` function, which executes operations synchronously on the main thread. Following this, a task is spawned using ```spawner.spawn``` that includes asynchronous operations like waiting for a timer and printing additional messages. However, this task is simply enqueued and doesn't execute immediately because the executor that processes these tasks hasn’t started yet. The executor begins its operation only after the spawner is dropped and the ```executor.run()``` command is executed. Therefore, the direct print statement executes before any tasks in the queue have started, resulting in "hey hey" being printed first, followed by the other messages as the executor processes the asynchronous task.

# Reflection 1.3

- with ```drop(spawner)```
![Image 07-05-24 at 23 20](https://github.com/tvadhisti/advprog-mdoule10-1/assets/127074983/cfa7c4ea-cc9f-41f6-b1b5-de2e2f7c3f5d)

- without ```drop(spawner)```
![Image 07-05-24 at 23 21](https://github.com/tvadhisti/advprog-mdoule10-1/assets/127074983/8100c3d7-c995-45e5-8592-882d745ddace)

the difference comes down to whether or not the ```drop(spawner)``` is used:

1. With ```drop(spawner)```: This line tells the program that no more tasks will be added. As a result, after the program finishes running all existing tasks, it stops and closes because it knows there’s nothing left to do.
2. Without ```drop(spawner)```: Without this line, the program doesn't know that it should stop looking for new tasks. So, even after it finishes all the tasks, it keeps running, waiting for more tasks that aren’t actually coming. This is why the program doesn’t end on its own in the second case.
  
In short, using drop(spawner) helps the program know when it can safely shut down because all work is done.
