# OS-Assignment-5

Program Description

This assignment is exactly like the previous one, however, with the inclusion of
semaphores, we are able to mitigate race condition to ensure process synchronization. The
programs work the same way. The parent process will create our shared memory segment for
the processes to access and execute the child programs, which there are three of. The parent
initializes the shared memory segment in its “shm_init” function using “shmget”. It uses “shmat”
to attach the program to the shared memory, giving us access. Lastly, it copies the data stored
in our struct, using “memcpy”, into the shared memory segment for child processes to read and
write. The children will read the data from the shared memory segment and sell the seats. They
will then decrement the number of seats left. Previously, we had an issue where the child
processes were accessing the critical section of our program, the selling of seats, without
waiting for each other to go in and out one by one. This created unpredictable outputs where
children were selling seats simultaneously and then altering the data in the shared memory
segment. When we were to print the remaining seats, the outcome was incorrect due to race
condition. In this assignment however, with the implementation of semaphores, we were able to
synchronize the children such that only one child process may enter the critical section at a
time.

The parent begins by instantiating our semaphore with the function “sem_open”. The first
parameter will be the name given to our semaphore. The second parameter will create the
semaphore unless it is already created. The third parameter will give it read and write
permission and the last parameter initializes the semaphore to 1. This is assigned to the “sem”
pointer variable. Once a child executes it will also use “sem_open” to gain access to the same
semaphore created in the parent. Now comes the most important part of the synchronization
process. We must use the functions “sem_wait” and “sem_post” to wrap the critical section of
our code so that only one child process may access it at a given time. Remember that the
critical section is when the children are selling seats. But we cannot simply wrap the “sell_seats”
function in the main. We must go into the “sell_seats” function and go inside of the while loop
and wrap the “if, else” statement. We do this because this is the precise moment when the child
processes are selling the seats. The function “sem_wait” will decrement the initial value set to
the semaphore, 1, to 0, which disallows entry to this portion of the code for the other children.
When the child process that gained entry to the critical section is finished it will execute
“sem_post”, which increments the value of the semaphore from 0 back to 1. This allows entry for
the next child process waiting in line. This process will continue until all seats are sold and the
child processes will terminate. Because of the addition of semaphores, the output will now show
a seat being sold by one child at a time. We see that the number of seats decrements one by
one until we reach zero. Back in the parent process, we must call two more functions:
“sem_unlink” and “sem_close”. “Sem_unlink” will remove the semaphore name that we gave it
earlier in the parent process using “sem_open”. “Sem_close” will close the semaphore and free
any memory that was previously allocated for its creation.
