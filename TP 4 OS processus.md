# Exercice 1



#Exercice 2



#Exercice 3






































































 - exercice 2

```c
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

```
















