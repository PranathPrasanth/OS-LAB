#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>

#define BUFFER_SIZE 5

int buffer[BUFFER_SIZE];
int in = 0, out = 0;

int empty = BUFFER_SIZE;
int full=0;
int mutex=1;

void wait(int *sem)
{
	while(*sem <= 0){}
	(*sem)--;
}

void signal(int *sem)
{
	(*sem)++;
}

void* producer(void* arg)
{
	int item;
	while(1)
	{
		item = rand() % 100;
		
		wait(&empty);
		wait(&mutex);
		
		buffer[in] = item;
		printf("Produced: %d\n", item);
		in = (in + 1)% BUFFER_SIZE;
		
		signal(&mutex);
		signal(&full);
		
		sleep(rand() % 20);
	}
	return NULL;
}

void* consumer(void* arg)
{
	int item;
	while(1)
	{
		wait(&full);
		wait(&mutex);
		
		item = buffer[out];
		printf("Consumed: %d\n", item);
		out = (out + 1) % BUFFER_SIZE;
		
		signal(&mutex);
		signal(&empty);
		
		sleep(rand() % 2);
	}
	return NULL;
}

int main()
{
	pthread_t prod, cons;
	
	pthread_create(&prod, NULL, producer, NULL);
	pthread_create(&cons, NULL, consumer, NULL);
	
	pthread_join(prod, NULL);
	pthread_join(cons, NULL);
	
	return 0;
}

