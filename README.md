A Wait Queue is a synchronization mechanism in the Linux kernel used to put processes to sleep while waiting for a specific condition to be met. Once the condition is satisfied, the processes are awakened. It is commonly used to avoid busy-waiting, thereby improving system efficiency.

Under normal circumstances, a function using a wait queue must know the name or the pointer of the wait queue. This is because a wait queue is represented as a variable (usually of type wait_queue_head_t ), and the function needs the address of this variable to perform operations.
In this lab, implement a custom wait queue-like functionality in kernel space, allowing user applications to operate through the system call.

Implement a system call call_my_wait_queue(int id) . This function takes an argument
id to determine which of the following two operations to perform:

1. Add a thread to the wait queue to wait

2. Remove threads from the wait queue, allowing them to exit
