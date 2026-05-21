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

