# Advance C Programming - Module 2 Assignment

## Question 1 - Multithreading in C

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

int N;

// Function to check prime number
int isPrime(int num)
{
    int i;

    if (num < 2)
        return 0;

    for (i = 2; i * i <= num; i++)
    {
        if (num % i == 0)
            return 0;
    }

    return 1;
}

// Thread A
void *sumPrime(void *arg)
{
    int count = 0;
    int num = 2;
    int sum = 0;

    while (count < N)
    {
        if (isPrime(num))
        {
            sum += num;
            count++;
        }

        num++;
    }

    printf("Sum of first %d prime numbers = %d\n", N, sum);

    pthread_exit(NULL);
}

// Thread B
void *thread1(void *arg)
{
    int i;

    for (i = 0; i < 50; i++)
    {
        printf("Thread 1 running\n");
        sleep(2);
    }

    pthread_exit(NULL);
}

// Thread C
void *thread2(void *arg)
{
    int i;

    for (i = 0; i < 34; i++)
    {
        printf("Thread 2 running\n");
        sleep(3);
    }

    pthread_exit(NULL);
}

int main()
{
    pthread_t t1, t2, t3;

    printf("Enter N: ");
    scanf("%d", &N);

    // Create threads
    pthread_create(&t1, NULL, sumPrime, NULL);
    pthread_create(&t2, NULL, thread1, NULL);
    pthread_create(&t3, NULL, thread2, NULL);

    // Wait for threads to finish
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    pthread_join(t3, NULL);

    printf("Execution completed\n");

    return 0;
}
```

## SAMPLE OUTPUT:

Enter N: 3
Thread A started
Sum of first 3 prime numbers = 10
Thread 1 running
Thread 2 running
Thread 1 running
Thread 2 running
Thread 1 running
Thread 2 running
Thread 1 running
Thread 1 running
Thread 2 running
Thread 1 running
Thread 2 running
Thread 1 running
Thread 1 running

## Question - 2 SIGNAL HANDLING

## Question 2 - Signal Handling and Thread Functions

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
#include <signal.h>
#include <time.h>

int N;

// Signal Handler
void signalHandler(int sig)
{
    printf("\nSIGINT received. Program will continue running.\n");
}

// Function to check prime number
int isPrime(int num)
{
    int i;

    if (num < 2)
        return 0;

    for (i = 2; i * i <= num; i++)
    {
        if (num % i == 0)
            return 0;
    }

    return 1;
}

// Thread Function A
void *sumPrime(void *arg)
{
    int count = 0;
    int num = 2;
    int sum = 0;

    printf("Thread A started\n");

    while (count < N)
    {
        if (isPrime(num))
        {
            sum += num;
            count++;
        }

        num++;
    }

    printf("Sum of first %d prime numbers = %d\n", N, sum);

    pthread_exit(NULL);
}

// Thread Function B
void *thread1(void *arg)
{
    int i;

    printf("Thread B started\n");

    for (i = 0; i < 50; i++)
    {
        printf("Thread 1 running\n");
        sleep(2);
    }

    pthread_exit(NULL);
}

// Thread Function C
void *thread2(void *arg)
{
    int i;

    printf("Thread C started\n");

    for (i = 0; i < 34; i++)
    {
        printf("Thread 2 running\n");
        sleep(3);
    }

    pthread_exit(NULL);
}

int main()
{
    pthread_t t1, t2, t3;

    clock_t start, end;
    double time_taken;

    // Register signal handler
    signal(SIGINT, signalHandler);

    printf("Enter N: ");
    scanf("%d", &N);

    start = clock();

    // Create threads
    pthread_create(&t1, NULL, sumPrime, NULL);
    pthread_create(&t2, NULL, thread1, NULL);
    pthread_create(&t3, NULL, thread2, NULL);

    // Wait for threads
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    pthread_join(t3, NULL);

    end = clock();

    time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;

    printf("\nExecution completed\n");
    printf("Time taken = %f seconds\n", time_taken);

    return 0;
}
```
## SAMPLE OUTPUT:

Enter N: 5
Thread A started
Sum of first 5 prime numbers = 28
Thread B started
Thread 1 running
Thread C started
Thread 2 running

Execution completed
Time taken = 100.002314 seconds

# Question 3
# i) Child Process - `fork()`

# Child Process - `fork()`

## Introduction

`fork()` is a system call used in C programming to create a new process.  
The newly created process is called the **child process**, while the original process is called the **parent process**.

`fork()` is mainly used in Linux and Unix Operating Systems for multitasking and concurrent execution.

---

## Syntax

```c
pid_t fork();
```

Header file required:

```c
#include <unistd.h>
```

---

## How `fork()` Works

When `fork()` is executed:

- A child process is created
- The child process gets a copy of the parent process
- Both processes execute independently
- Execution continues from the next line after `fork()`

---

## Return Values

| Return Value | Description |
|---|---|
| `0` | Returned to child process |
| `> 0` | Returned to parent process (Child PID) |
| `< 0` | Process creation failed |

---

## Example Program

```c
#include <stdio.h>
#include <unistd.h>

int main()
{
    pid_t pid;

    pid = fork();

    if (pid == 0)
    {
        printf("This is Child Process\n");
    }
    else if (pid > 0)
    {
        printf("This is Parent Process\n");
    }
    else
    {
        printf("Fork Failed\n");
    }

    return 0;
}
```

---

## Sample Output

```text
This is Parent Process
This is Child Process
```

---

## Features of `fork()`

- Creates a new child process
- Parent and child execute separately
- Child process gets a copy of parent memory
- Supports concurrent execution

---

## Advantages

- Enables multitasking
- Improves process management
- Useful in parallel execution
- Widely used in Operating Systems

---

## Applications

- Shell programming
- Server applications
- Background process execution
- Process management systems

---

## Important Points

- Parent and child processes have different Process IDs (PID)
- Child process executes independently after creation
- `fork()` is available only in Unix/Linux systems
- Both parent and child continue execution from the same statement after `fork()`

## Conclusion

`fork()` is an important system call in C programming used for process creation in Linux and Unix systems. It helps in multitasking and allows multiple processes to run simultaneously.

# 2) Handling Common Signals in C

## Introduction

Signals are software interrupts used to notify a process that a specific event has occurred.

Signals are commonly used in Operating Systems for:

- Interrupt handling
- Process control
- Exception handling
- Communication between processes

In C programming, signals can be handled using the `signal()` function.

---

## Header File

```c
#include <signal.h>
```

---

## Syntax

```c
signal(signal_number, signal_handler);
```

Where:

- `signal_number` → Type of signal
- `signal_handler` → Function to handle the signal

---

## Common Signals

| Signal | Description |
|---|---|
| `SIGINT` | Interrupt signal (`Ctrl + C`) |
| `SIGKILL` | Forcefully terminates a process |
| `SIGSTOP` | Stops a process |
| `SIGTERM` | Requests process termination |
| `SIGSEGV` | Segmentation fault |
| `SIGFPE` | Arithmetic exception |
| `SIGALRM` | Alarm signal |

---

## Example Program

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void signalHandler(int sig)
{
    printf("\nSIGINT Signal Received\n");
}

int main()
{
    signal(SIGINT, signalHandler);

    while (1)
    {
        printf("Program Running...\n");
        sleep(1);
    }

    return 0;
}
```

---

## Working of the Program

1. `signal()` registers the signal handler
2. Program runs continuously
3. When user presses `Ctrl + C`
4. `SIGINT` signal is generated
5. `signalHandler()` function executes
6. Program handles the interrupt instead of terminating immediately

---

## Sample Output

```text
Program Running...
Program Running...
Program Running...

SIGINT Signal Received
```

---

## Advantages of Signal Handling

- Prevents abrupt program termination
- Helps in error handling
- Supports process communication
- Improves program reliability

---

## Applications

- Operating Systems
- Process management
- Interrupt handling
- Background services
- Server applications

---

## Important Points

- Signals are asynchronous events
- Some signals cannot be handled (`SIGKILL`)
- Signal handlers should be simple and fast
- Used heavily in Linux and Unix systems

---

## Conclusion

Signal handling is an important concept in C programming that allows programs to respond to interrupts and system events efficiently. It improves process control and system reliability.

# 3) Exploring Different Kernel Crashes

## Introduction

A kernel crash occurs when the Operating System kernel encounters a critical error and stops functioning properly.

The kernel is the core part of an Operating System responsible for:

- Memory management
- Process management
- Device handling
- System calls

When a severe error occurs inside the kernel, the system may:

- Freeze
- Restart automatically
- Display an error screen
- Become unresponsive

---

## Common Types of Kernel Crashes

| Crash Type | Description |
|---|---|
| Null Pointer Dereference | Accessing invalid memory location |
| Stack Overflow | Excessive function calls exhaust stack memory |
| Invalid Memory Access | Accessing restricted memory |
| Divide by Zero | Arithmetic exception inside kernel |
| Deadlock | Processes wait forever for resources |
| Infinite Loop | Kernel stuck in endless execution |
| Buffer Overflow | Writing beyond allocated memory |
| Driver Failure | Faulty device drivers crash kernel |

---

# 1. Null Pointer Dereference

## Description

Occurs when the kernel tries to access memory using a null pointer.

Example:

```c
int *ptr = NULL;
*ptr = 10;
```

## Effects

- Kernel panic
- Segmentation fault
- System crash

---

# 2. Stack Overflow

## Description

Occurs when excessive recursive function calls consume all stack memory.

## Example

```c
void test()
{
    test();
}
```

## Effects

- Program crash
- Kernel instability
- Memory corruption

---

# 3. Invalid Memory Access

## Description

Occurs when the kernel accesses unauthorized memory regions.

## Example

```c
char *ptr = (char *)0xFFFFFFFF;
printf("%c", *ptr);
```

## Effects

- Segmentation fault
- Kernel panic
- System freeze

---

# 4. Divide by Zero Exception

## Description

Occurs when division is performed using zero.

## Example

```c
int a = 10;
int b = 0;
int c = a / b;
```

## Effects

- Arithmetic exception
- Program termination
- Kernel interruption

---

# 5. Deadlock

## Description

Occurs when multiple processes wait indefinitely for resources.

## Example Scenario

- Process A waits for Resource B
- Process B waits for Resource A

## Effects

- System hangs
- Resource starvation
- Reduced performance

---

# 6. Infinite Loop

## Description

Occurs when the kernel executes endlessly without termination.

## Example

```c
while(1)
{
}
```

## Effects

- CPU usage becomes very high
- System becomes unresponsive
- Possible kernel freeze

---

# 7. Buffer Overflow

## Description

Occurs when data exceeds allocated buffer size.

## Example

```c
char arr[5];
strcpy(arr, "OperatingSystem");
```

## Effects

- Memory corruption
- Security vulnerabilities
- System crash

---

# 8. Driver Failure

## Description

Faulty or incompatible device drivers can crash the kernel.

## Causes

- Hardware incompatibility
- Corrupted drivers
- Invalid kernel modules

## Effects

- Blue screen
- Kernel panic
- Device malfunction

---

## Kernel Panic

### Definition

A kernel panic is a safety mechanism triggered when the Operating System detects a fatal internal error.

### Symptoms

- System freeze
- Black screen
- Automatic reboot
- Error messages displayed

---

## Prevention Methods

- Proper memory management
- Avoid null pointer access
- Use safe recursion limits
- Validate user inputs
- Update device drivers
- Use synchronization techniques

---

## Applications of Kernel Crash Analysis

- Operating System development
- Debugging system software
- Driver development
- Cybersecurity research
- Performance optimization

---

## Important Points

- Kernel crashes affect the entire system
- Most crashes occur due to memory errors
- Linux systems display "Kernel Panic"
- Windows systems display "Blue Screen of Death (BSOD)"

---

## Conclusion

Kernel crashes occur when critical errors happen inside the Operating System kernel. Understanding different types of crashes helps developers improve system stability, debugging, and security.

# 4) Time Complexity

## Introduction

Time Complexity is used to measure the amount of time an algorithm takes to execute as the input size increases.

It helps in:

- Analyzing algorithm efficiency
- Comparing different algorithms
- Predicting performance for large inputs

Time complexity is usually represented using **Big O Notation**.

---

## Big O Notation

Big O notation describes the worst-case execution time of an algorithm.

Example:

```text
O(1), O(n), O(log n), O(n²)
```

---

## Common Time Complexities

| Complexity | Name | Example |
|---|---|---|
| `O(1)` | Constant Time | Array access |
| `O(log n)` | Logarithmic Time | Binary Search |
| `O(n)` | Linear Time | Traversing array |
| `O(n log n)` | Linearithmic Time | Merge Sort |
| `O(n²)` | Quadratic Time | Nested loops |
| `O(2ⁿ)` | Exponential Time | Recursive Fibonacci |
| `O(n!)` | Factorial Time | Traveling Salesman |

---

# 1. Constant Time — `O(1)`

## Description

Execution time remains constant regardless of input size.

## Example

```c
int arr[5] = {1,2,3,4,5};
printf("%d", arr[2]);
```

## Complexity

```text
O(1)
```

---

# 2. Linear Time — `O(n)`

## Description

Execution time increases linearly with input size.

## Example

```c
for(int i = 0; i < n; i++)
{
    printf("%d ", i);
}
```

## Complexity

```text
O(n)
```

---

# 3. Quadratic Time — `O(n²)`

## Description

Occurs mainly in nested loops.

## Example

```c
for(int i = 0; i < n; i++)
{
    for(int j = 0; j < n; j++)
    {
        printf("%d %d\n", i, j);
    }
}
```

## Complexity

```text
O(n²)
```

---

# 4. Logarithmic Time — `O(log n)`

## Description

Input size reduces after each iteration.

## Example

```c
while(n > 1)
{
    n = n / 2;
}
```

## Complexity

```text
O(log n)
```

---

# 5. Linearithmic Time — `O(n log n)`

## Description

Common in efficient sorting algorithms.

## Example

- Merge Sort
- Quick Sort

## Complexity

```text
O(n log n)
```

---

# 6. Exponential Time — `O(2ⁿ)`

## Description

Execution time doubles with each additional input.

## Example

Recursive Fibonacci:

```c
int fib(int n)
{
    if(n <= 1)
        return n;

    return fib(n-1) + fib(n-2);
}
```

## Complexity

```text
O(2ⁿ)
```

---

## Time Complexity of Prime Number Checking

Example:

```c
for(i = 2; i * i <= n; i++)
{
    if(n % i == 0)
        return 0;
}
```

### Complexity

```text
O(√n)
```

Reason:

- Loop runs only up to square root of `n`

---

## Importance of Time Complexity

- Helps choose efficient algorithms
- Improves performance
- Reduces execution time
- Important for large-scale applications

---

## Applications

- Competitive programming
- Software optimization
- Operating Systems
- Data Structures and Algorithms
- Machine learning systems

---

## Important Points

- Lower complexity means better performance
- Big O represents worst-case scenario
- Time complexity ignores hardware speed
- Used to compare algorithms independently

---

## Conclusion

Time complexity is an important concept in programming used to measure algorithm efficiency. Understanding different complexities helps developers design faster and more optimized programs.

# 5) Locking Mechanism - Mutex and Spinlock

## Introduction

In multithreading, multiple threads may access shared resources simultaneously.  
This can lead to problems like:

- Race conditions
- Data inconsistency
- Unexpected outputs

Locking mechanisms are used to synchronize threads and protect shared resources.

Two common locking mechanisms are:

1. Mutex
2. Spinlock

---

# 1. Mutex

## Definition

A Mutex (Mutual Exclusion) is a locking mechanism that allows only one thread to access a shared resource at a time.

If one thread locks the mutex:

- Other threads must wait
- Only the owner thread can unlock it

---

## Header File

```c
#include <pthread.h>
```

---

## Mutex Functions

| Function | Description |
|---|---|
| `pthread_mutex_init()` | Initialize mutex |
| `pthread_mutex_lock()` | Lock mutex |
| `pthread_mutex_unlock()` | Unlock mutex |
| `pthread_mutex_destroy()` | Destroy mutex |

---

## Example Program using Mutex

```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t lock;
int counter = 0;

void *increment(void *arg)
{
    int i;

    for(i = 0; i < 1000; i++)
    {
        pthread_mutex_lock(&lock);

        counter++;

        pthread_mutex_unlock(&lock);
    }

    pthread_exit(NULL);
}

int main()
{
    pthread_t t1, t2;

    pthread_mutex_init(&lock, NULL);

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Counter = %d\n", counter);

    pthread_mutex_destroy(&lock);

    return 0;
}
```

---

## Advantages of Mutex

- Prevents race conditions
- Ensures data consistency
- Easy to implement
- Suitable for long waiting times

---

## Disadvantages of Mutex

- Thread switching overhead
- Slower than spinlocks in short waits

---

# 2. Spinlock

## Definition

A Spinlock is a locking mechanism where a thread continuously checks the lock until it becomes available.

Instead of sleeping, the thread keeps "spinning".

---

## Header File

```c
#include <pthread.h>
```

---

## Spinlock Functions

| Function | Description |
|---|---|
| `pthread_spin_init()` | Initialize spinlock |
| `pthread_spin_lock()` | Lock spinlock |
| `pthread_spin_unlock()` | Unlock spinlock |
| `pthread_spin_destroy()` | Destroy spinlock |

---

## Example Program using Spinlock

```c
#include <stdio.h>
#include <pthread.h>

pthread_spinlock_t spinlock;
int counter = 0;

void *increment(void *arg)
{
    int i;

    for(i = 0; i < 1000; i++)
    {
        pthread_spin_lock(&spinlock);

        counter++;

        pthread_spin_unlock(&spinlock);
    }

    pthread_exit(NULL);
}

int main()
{
    pthread_t t1, t2;

    pthread_spin_init(&spinlock, 0);

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Counter = %d\n", counter);

    pthread_spin_destroy(&spinlock);

    return 0;
}
```

---

# Difference Between Mutex and Spinlock

| Feature | Mutex | Spinlock |
|---|---|---|
| Waiting Method | Thread sleeps | Thread continuously checks |
| CPU Usage | Low | High |
| Speed | Slower | Faster for short waits |
| Context Switching | Yes | No |
| Best Use Case | Long waiting time | Short waiting time |

---

## Race Condition

### Definition

A race condition occurs when multiple threads access shared data simultaneously without proper synchronization.

### Example

```c
counter++;
```

If two threads execute this at the same time:

- Incorrect values may occur
- Data corruption may happen

Locks help avoid race conditions.

---

## Applications

- Operating Systems
- Database systems
- Multithreaded applications
- Parallel computing
- Device drivers

---

## Important Points

- Mutex is suitable for general synchronization
- Spinlock is useful for short critical sections
- Improper locking may cause deadlocks
- Synchronization improves thread safety

---

## Conclusion

Mutex and Spinlock are important synchronization mechanisms used in multithreaded programming. They help protect shared resources and prevent race conditions, ensuring safe and efficient execution of concurrent programs.
