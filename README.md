<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project 2: Wait Queue</title>
</head>
<body>
    <h1>Project 2: Wait Queue</h1>
    <p>
        A <strong>Wait Queue</strong> is a synchronization mechanism in the Linux kernel used to put processes to sleep while waiting for a specific condition to be met. Once the condition is satisfied, the processes are awakened. It is commonly used to avoid busy-waiting, thereby improving system efficiency.
    </p>

    <h2>Objective</h2>
    <p>
        Implement a custom wait queue-like functionality in kernel space, allowing user applications to operate through system calls. Ensure threads exit the wait queue in <strong>FIFO (First In, First Out)</strong> order.
    </p>

    <h2>Sample Output</h2>
    <pre>
enter wait queue thread_id: 0
enter wait queue thread_id: 1
...
start clean queue ...
exit wait queue thread_id: 0
exit wait queue thread_id: 1
...
    </pre>

    <h2>Sample Code</h2>
    <h3>User Space</h3>
    <pre>
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;unistd.h&gt;
#include &lt;pthread.h&gt;
#include &lt;sys/syscall.h&gt;

#define NUM_THREADS 10

void *enter_wait_queue(void *thread_id) {
    fprintf(stderr, "enter wait queue thread_id: %d\n", *(int *)thread_id);
    // syscall here
    fprintf(stderr, "exit wait queue thread_id: %d\n", *(int *)thread_id);
}

void *clean_wait_queue() {
    // syscall here
}

int main() {
    void *ret;
    pthread_t id[NUM_THREADS];
    int thread_args[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; i++) {
        thread_args[i] = i;
        pthread_create(&id[i], NULL, enter_wait_queue, (void *)&thread_args[i]);
    }

    sleep(1);
    fprintf(stderr, "start clean queue ...\n");
    clean_wait_queue();

    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(id[i], &ret);
    }

    return 0;
}
    </pre>

    <h3>Kernel Space</h3>
    <pre>
static int enter_wait_queue(void) {
    return 0;
}

static int clean_wait_queue(void) {
    return 0;
}

SYSCALL_DEFINE1(call_wait_queue, int, id) {
    switch (id) {
        case 1:
            enter_wait_queue();
            break;
        case 2:
            clean_wait_queue();
            break;
    }
    return 0;
}
    </pre>

    <h2>Requirements</h2>
    <ul>
        <li>Implement a system call <code>call_my_wait_queue(int id)</code>.</li>
        <li>Operations:
            <ol>
                <li>Add a thread to the wait queue to wait (return <code>1</code> on success, <code>0</code> on error).</li>
                <li>Remove threads from the wait queue (FIFO order, return <code>1</code> on success, <code>0</code> on error).</li>
            </ol>
        </li>
        <li>Use kernel-provided wait queue-related functions.</li>
        <li>Declare a <code>my_wait_queue</code> in kernel space.</li>
    </ul>

    <h2>Submission</h2>
    <ul>
        <li><strong>Due date:</strong> 23:55, 29th Dec.</li>
        <li>Demo will be held from 30th Dec.</li>
        <li>Fill out the <a href="https://docs.google.com/spreadsheets/d/1WKJkC0sjSVLbHsdyI69zoKQZTe9ZDI7EsGTQ1cM1aTI/edit?gid=1423695453#gid=1423695453">demo time form</a> before 29th Dec.</li>
        <li>Submit the report in <a href="https://hackmd.io/">HackMD</a> format.</li>
        <li>Include:
            <ul>
                <li>Team members' names and student IDs.</li>
                <li>Source code and execution results.</li>
            </ul>
        </li>
        <li>Submit the HackMD URL to the course system.</li>
        <li><strong>Late submissions will not be accepted.</strong></li>
    </ul>

    <h2>References</h2>
    <ul>
        <li><a href="https://elixir.bootlin.com/linux/v3.9/source/include/linux/wait.h#L1">Linux Kernel Wait Queue Documentation</a></li>
    </ul>
</body>
</html>
