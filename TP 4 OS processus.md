# Exercice 1



#Exercice 2



#Exercice 3






































































 - exercice 2

#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/wait.h>
#include <stdlib.h>
#include <time.h>

int main(int argc, char *argv[]) {
	time_t t;
	srand((unsigned) time(&t)); //init the random number generator

	//create pipes
	int p_pf1[2], p_pf2[2], p_f1p[2], p_f2p[2];
	if(pipe(p_pf1)==-1)
		exit(1);
	if(pipe(p_pf2)==-1)
		exit(1);
	if(pipe(p_f1p)==-1)
		exit(1);
	if(pipe(p_f2p)==-1)
		exit(1);

	//create sons
	int f1 = 1, f2 = 1;
	f1 = fork();
	if(f1>0)
		f2 = fork();

	if(f1>0 && f2>0) {
		//father
		close(p_pf1[0]);
		close(p_pf2[0]);
		close(p_f2p[1]);
		close(p_f1p[1]);

		//generate random numbers
		int buf[100];
		for(int i=0;i<100;i++) {
			buf[i] = rand()%200-100;
			if(buf[i]>=0)
				write(p_pf1[1], buf+i, sizeof(int)); //send to son 1
			else
				write(p_pf2[1], buf+i, sizeof(int)); //send to son 2
			
			usleep(1000*200); //sleep 200 ms
		}
		//close pipes
		close(p_pf1[1]);
		close(p_pf2[1]);

		//read sons responses
		int rep_f1, rep_f2;
		read(p_f1p[0], &rep_f1, sizeof(int));
		read(p_f2p[0], &rep_f2, sizeof(int));
		printf("Réponse du premier fils: %d\n", rep_f1);
		printf("Réponse du second fils: %d\n", rep_f2);

		//wait for the sons to terminate
		wait(NULL);
		wait(NULL);
	} else if(f1==0) {
		//first son (positive numbers)
		close(p_pf1[1]);
		close(p_pf2[0]);
		close(p_pf2[1]);
		close(p_f2p[0]);
		close(p_f2p[1]);
		close(p_f1p[0]);

		//read numbers
		int buf, total = 0;
		while(read(p_pf1[0], &buf, sizeof(int))>0)
			total += buf;
		
		//write result
		write(p_f1p[1], &total, sizeof(int));
	} else {
		//second son (negative numbers)
		close(p_pf2[1]);
		close(p_pf1[0]);
		close(p_pf1[1]);
		close(p_f1p[0]);
		close(p_f1p[1]);
		close(p_f2p[0]);

		//read numbers
		int buf, total = 0;
		while(read(p_pf2[0], &buf, sizeof(int))>0)
			total += buf;
		
		//write result
		write(p_f2p[1], &total, sizeof(int));
	}
	return 0;
}















 exercice 1

Question subsidiaire: que se passe-t-il si le processus père envoie le signal SIGSTOP au lieu de SIGUSR1? La situation est-elle différente avec le signal SIGINT?
	-Si il envoie SIGSTOP, l'enfant est mis en pause, et ne se termine donc jamais. Quand le père appelle wait(), il se met en pause tant que son fils n'a pas terminé son job; le père reste donc indéfiniment en pause.
	-Si il envoie SIGINT, l'enfant est tué. le signal SIGUSR2 n'est pas reçu par l'enfant, et quand le père appelle wait(), cela n'a aucun effet car il n'y a plus d'enfant. le programme se termine normalement.

Question subsidiaire: comment améliorer le programme pour le processus père ne soit pas interrompu lorsque l’on tape un CTRL-C?
	->voir ligne 15 du programme exercice_1.c: signal(SIGINT, SIG_IGN);
	On ignore les signaux SIGINT















- exercice 1

#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/wait.h>
#include <stdlib.h>

void son_signals(int sig);
int last_sig = -1; //the last signal received by the son

int main(int argc, char *argv[]) {
	int p = fork();
	//ignore CTRL-C (SIGINT signal)
	signal(SIGINT, SIG_IGN);

	if(p==0) { //son
		//define signal handlers
		signal(SIGUSR1, son_signals);
		signal(SIGUSR2, son_signals);

		//wait for signals
		while(last_sig!=SIGUSR2)
			pause();

	} else { //father
		
		sleep(5);
		kill(p, SIGUSR1); //send SIGUSR1
		sleep(5);
		kill(p, SIGUSR2); //send SIGUSR2
		wait(NULL); //wait for the child to terminate
	}
	return 0;
}

void son_signals(int sig) {
	last_sig = sig;
	printf("Le fils a reçu le signal %d !\n", sig);
}







//Mathieu Vandenneucker - exercice 3

#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/wait.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

//configuration
#define BUFFER_SIZE 300			//size of the buffer
#define NB_PRODUCERS 12			//amount of producers
#define NB_CONSUMERS 20			//amount of consumers
#define NB_ITEMS 1000 * 100		//amount of items to produce

//globals variables
int buffer[BUFFER_SIZE];
int buf_size = 0;
int produced_items = 0;
int computed = 0;

pthread_mutex_t buf_lock = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond_write = PTHREAD_COND_INITIALIZER; //is the buffer full ?
pthread_cond_t cond_read = PTHREAD_COND_INITIALIZER; //is the buffer empty ?

void *consumer(void *arg) {
	int item;
	while(1) {
		pthread_mutex_lock(&buf_lock);
		while(buf_size==0) {
			if(produced_items==NB_ITEMS) {
				//nothing more to read !
				pthread_cond_broadcast(&cond_read); //release the other consumers
				pthread_mutex_unlock(&buf_lock);
				break;
			}
			pthread_cond_wait(&cond_read, &buf_lock);
		}
		if(produced_items==NB_ITEMS && buf_size==0) //nothing more to read !
			break;
		
		item = buffer[--buf_size];
		pthread_cond_signal(&cond_write);
		pthread_mutex_unlock(&buf_lock);
		computed += item;
		//usleep(1000);
	}
	pthread_exit(NULL);
}

void *producer(void *arg) {
	int item;
	while(1) {
		pthread_mutex_lock(&buf_lock);
		while(buf_size==BUFFER_SIZE)
			pthread_cond_wait(&cond_write, &buf_lock);

		if(produced_items==NB_ITEMS) { //nothing more to produce !
			pthread_cond_broadcast(&cond_write); //release the other producers
			pthread_mutex_unlock(&buf_lock);
			break;
		}

		/*
			since I don't know what I'm producing, I decide to consider I don't want to produce too
			much data, so I produce it there (in the critical zone), because I need to check the number
			of produced items to know if I need to produce anything or not (and this number can change
			between the produce-time and when I'm in critical zone)
		*/
		item = rand() % 256;

		produced_items++;
		buffer[buf_size++] = item;
		pthread_cond_signal(&cond_read);
		pthread_mutex_unlock(&buf_lock);
		//usleep(1000);
	}
	pthread_exit(NULL);
}

int main(int argc, char *argv[]) {
	time_t t;
	srand((unsigned) time(&t)); //init the random number generator

	//create consumers and producers
	pthread_t consumers[NB_CONSUMERS], producers[NB_PRODUCERS];
	pthread_attr_t consumers_attr, producers_attr;

	//edit schedule policy for the threads
	pthread_attr_init(&consumers_attr);
	pthread_attr_setschedpolicy(&consumers_attr, SCHED_FIFO);
	pthread_attr_init(&producers_attr);
	pthread_attr_setschedpolicy(&producers_attr, SCHED_RR);

	for(int i=0;i<NB_CONSUMERS;i++)
		pthread_create(consumers+i, &consumers_attr, consumer, NULL);
	for(int i=0;i<NB_PRODUCERS;i++)
		pthread_create(producers+i, &producers_attr, producer, NULL);

	//wait the threads to terminate
	for(int i=0;i<NB_CONSUMERS;i++)
		pthread_join(consumers[i], NULL);
	for(int i=0;i<NB_PRODUCERS;i++)
		pthread_join(producers[i], NULL);

	//pourquoi ne pas calculer quelque chose après tout !
	printf("Valeur calculée: %d\n", computed);

	return 0;
}










