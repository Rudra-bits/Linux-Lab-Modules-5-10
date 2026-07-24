# Question 2: Process Monitoring and Signal Handling

## Objective
To implement a C program using `fork()`, `waitpid()`, and signals (`SIGCHLD`, `SIGKILL`) where a parent process monitors a child worker process and terminates it if it becomes unresponsive.

## Code (`server_monitor.c`)
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <signal.h>

void handle_sigchld(int sig) {
    while (waitpid(-1, NULL, WNOHANG) > 0);
}

int main() {
    pid_t pid;
    signal(SIGCHLD, handle_sigchld);

    pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    } 
    else if (pid == 0) {
        printf("Child worker started with PID: %d\n", getpid());
        while(1) {
            sleep(1);
        }
        exit(0);
    } 
    else {
        printf("Parent monitoring child with PID: %d\n", pid);
        sleep(3);
        printf("Child is unresponsive. Terminating using signal...\n");
        kill(pid, SIGKILL);
        sleep(1);
        printf("Parent exiting safely.\n");
    }

    return 0;
}
How to Compile and Run
Bash
gcc server_monitor.c -o server_monitor
./server_monitor
Expected Output

Parent monitoring child with PID: [Child_PID]
Child worker started with PID: [Child_PID]
Child is unresponsive. Terminating using signal...
Parent exiting safely.
