#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
// Initialize two mutexes (representing resources)
pthread_mutex_t lock1;
pthread_mutex_t lock2;
// Thread function for acquiring resources in the correct order
void* thread1_func(void* arg) {
    // Thread 1 acquires resources in the correct order: lock1 (R1) -> lock2 (R2)
    printf("Thread 1: Trying to acquire lock1...\n");
    pthread_mutex_lock(&lock1); // Acquire Resource 1
    printf("Thread 1: Acquired lock1, waiting for lock2...\n");
    sleep(1); // Simulate some work with lock1
    pthread_mutex_lock(&lock2); // Acquire Resource 2
    printf("Thread 1: Acquired lock2\n");
    // Critical section
    printf("Thread 1: In critical section\n");
    // Release resources
    pthread_mutex_unlock(&lock2);
    pthread_mutex_unlock(&lock1);
    return NULL;
}
void* thread2_func(void* arg) {
    // Thread 2 acquires resources in the correct order: lock1 (R1) -> lock2 (R2)
    printf("Thread 2: Trying to acquire lock1...\n");
    pthread_mutex_lock(&lock1); // Acquire Resource 1
    printf("Thread 2: Acquired lock1, waiting for lock2...\n");
    sleep(1); // Simulate some work with lock1
    pthread_mutex_lock(&lock2); // Acquire Resource 2
    printf("Thread 2: Acquired lock2\n");
    // Critical section
    printf("Thread 2: In critical section\n");
    // Release resources
    pthread_mutex_unlock(&lock2);
    pthread_mutex_unlock(&lock1);
    return NULL;
}
int main() {
    pthread_t thread1, thread2;
    // Initialize the mutex locks (representing resources)
    pthread_mutex_init(&lock1, NULL);
    pthread_mutex_init(&lock2, NULL);
    // Create two threads
    pthread_create(&thread1, NULL, thread1_func, NULL);
    pthread_create(&thread2, NULL, thread2_func, NULL);
    // Wait for both threads to finish
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
    // Destroy the mutex locks
    pthread_mutex_destroy(&lock1);
    pthread_mutex_destroy(&lock2);
    return 0;
}
