//gcc -pthread -std=c99 as1.c


#include <pthread.h>
#include <stdio.h>
#include <time.h>
void *CalculateSum(void *);
pthread_mutex_t mutex1 = PTHREAD_MUTEX_INITIALIZER;
unsigned long int sum = 0;
int interval;
int tnum;
unsigned long int a = 0;
int threadno = 1;
int main(){

	
	int tempthread;
	int tempinterval;
	//input and validation
	printf("The number of threads: ");
	if (scanf("%d",&tempthread) != 1)
	{
		printf("Please enter valid value. \n");
		return 1;
	}else if(tempthread > 380)
	{
		printf("Threads number shall be lower than 380. \n");
		return 1;	
	}else if(tempthread < 1)
	{
		printf("Threads number shall be greater than 0. \n");
		return 1;	
	}
	else{
	tnum = tempthread;
	}
	printf("The value of interval: ");
	if (scanf("%d",&tempinterval) != 1)
	{
		printf("Please enter valid value. \n");
		return 1;
	}else if(tempinterval > 2000000000)
	{
		printf("Interval number shall lower than 2000000000. \n");
		return 1;	
	}else if(tempinterval < 1)
	{
		printf("Interval number shall be greater than 0. \n");
		return 1;	
	}
	else{
	interval = tempinterval;
	}
	unsigned long int b = tnum * interval;
	if(b > 10000000000)
	{
		printf("Threadnumber * interval is too large, should be lower than 10000000000. \n");
		return 1;
	}
	time_t starttime;
	time(&starttime);
	//create threads
	pthread_t thread_id[tnum];
	for(int i=0; i < tnum; i++)
        {
        	pthread_create( &thread_id[i], NULL, CalculateSum,NULL);
        }
	//execution
	for(int j=0; j < tnum; j++)
        {
        	pthread_join( thread_id[j], NULL);
        }
	//print the result
	time_t endtime;
	time(&endtime);
	double duration = difftime(endtime,starttime);
	printf("sum = %lu , b-1 = %lu \n", sum, a);
	printf("Execution time: %f  sec \n",duration);
	return 0;
}

void *CalculateSum(void *para)
{
	time_t curtime;
	time(&curtime);
	printf("Thread %d Start time: %s",threadno, ctime(&curtime));
	int threadend = threadno;
	threadno++;
	//lock the resource and do the calculation	
	pthread_mutex_lock( &mutex1 );
	

	for( int k = 0; k < interval; k++) {
	
		sum = sum + a;
		a++;
 	 
        }
	time_t curtime2;
	time(&curtime2);
	printf("Thread %d End   time: %s",threadend, ctime(&curtime2));   
	//unlock the resource
	pthread_mutex_unlock( &mutex1 );
}
