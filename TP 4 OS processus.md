# Exercice 1



# TP3 Gestion de processus - Système d’exploitation


## 1) Processus, fork et wait

### Exercice 1
Écrivez un programme qui crée un fils. Le père doit afficher "je suis père" et le pid de son processus fils, et le fils doit afficher "je suis fils" son pid et son ppid.

```c 
#include <unistd.h>
#include <error.h>
#include <stdio.h>
#include <sys/wait.h>
#include <sys/types.h>


int main (int argc, char *argv) {
    int pid;
    pid = fork();  // créer processus fils
    switch(pid) {
        case -1: perror("cannot create a process");
                 break;
        case 0: printf("je suis fils et mon pid: %d et mon ppid: %d\n", getpid(), getppid());
                exit(0);
                break;
        default: printf("je suis le père du processus fils pid: %d\n", pid);
    }
    wait(NULL);
    return 0;
}
```

### Exercice 2
Écrivez un programme qui crée 4 processus fils choisissant chacun un nombre aléatoire. Pour chaque fils, affichez son PID ainsi que le numéro choisi. À la fin de votre programme le père doit afficher un message pour chacun de ses fils : le message doit comporter son PID, le PID de fils et le numéro choisi par son fils. Consultez le manuel en ligne de l’appel-système wait().


```c 
#include <unistd.h>
#include <error.h>
#include <stdio.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h> 

int main(void) {
    int pid, i, status;
    for (i=0; i <= 4; i++) { 
        pid = fork ();
        switch(pid) {
            case -1:
                perror("cannot create a process");
                break; // signifie terminer le switch
            case 0: 
                printf("je suis fils et mon pid: %d et mon ppid: %d\n", getpid(), getppid());
                wait(&status); //suspend l'exécution du processus appelant jusqu'à ce que l'un de ses fils se termine; 
                exit(i); // termine normalement le processus et la valeur status & 0377 est renvoyée au processus parent
                break; // facultatif puisque exit
            default: 
                printf("je suis le père du processus fils pid: %d\n", pid);
        }
    }    
    
    
    return 0;
}
```

```c 

 //man rand nombre aléatoire + exit(spécifier la valeur donnée ici)

 //créer un programme qui créer 4 fils (1, 2, 3, 4)
 //chaque processus va envoyer un numéro avec wait je crois
 //tester si on arrive à récupérer la valeur
 //changer 1,2,3,4 en un nombre random avec rand
 
 // passer pointeurt ? (null) ? -> http://www.lifl.fr/~marquet/ens/sem/processus1/processus1006.html (ça parle de pointeur null et de pid avec wait)
 //exit c'est le fils qui a envoyé quelque chose
 //attention le random peut générer le même numéro pour les 4 processus
 // il faut mettre un pointeur à wait (sauf si le prof dit de la merde c'est pas moi qui l'ai dit c'est à côté)
 //si pas de dessin cet exercice est faux
 //wait (&statut)
#include <unistd.h>
#include <error.h>
#include <stdio.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <stdlib.h>
#include <time.h>

int main (int argc, char *argv) {
    int i, pid, status; //mon code c'est de la merde ...meuuuuh nn tkt c super ton code ;)
    for (i=0, i <= 4, i++): {
        switch(pid) {
        case -1: perror("cannot create a process");
                 break;
        case 0: printf("je suis fils et mon pid: %d et mon ppid: %d\n", getpid(), getppid());
                wait() // faut waiter qqch ? suis pas sure de ça // i don't know mais le wait renvoie un pid je crois
                exit(rand()); //je crois ils ont aussi exit (i) 
                break;
        default: printf("je suis le père du processus fils pid: %d\n", pid);
        }
        
    }
    wait(pid,&status, 0) // c'est surement de la merde
    return 0;
}


```

```c 
//utiliser l'id du processus 
#include <stdio.h>
#include <stdlib.h>
#include <time.h> 

int main(void){
    srand(time(NULL));
    int random = rand();
    printf("%d", random);
	return 0;
}

```
### Exercice 3
Écrivez un programme C qui crée deux fils, l’un affiche les entiers de 1 à 25, l’autre de 26 à 50. Si l’affichage des nombres n’est pas dans l’ordre, veuillez modifier votre programme pour que ce soit le cas.





```c 
#include <unistd.h>
#include <error.h>
#include <stdio.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <stdlib.h>
#include <time.h>


int main (int argc, char *argv){
    int i, pid, number_t = 25, number_h= 0;     //initialiser i, pid
    for (i= 0; i< 2;i++ ){
        pid = fork ();
        switch (pid){
        case -1: perror("cannot create a process");
                 break;
        case 0: if (i %2 == 1){        // processus fils
                    number_t = 26;
                    number_h = 25;
                    // Est ce qu'on sait faire un wait qui attend l'autre processus fils ?
                }// fils
                
                for (number_h; number_h <= number_t; number_h++){
                    printf("%d,", number_h);
                }
                // 
                // 
                break;
        default: wait(NULL);//père
        }


    }
    exit(status);
    return 0;
}

```



### Exercice 4
Écrivez un programme C qui crée une famille de processus comme dans le graphe: 
Le programme doit gérer correctement la terminaison de tous les processus fils créés.

```c 

int main (void){
    int i,j, k, pid;
    for (i=0; i = 3; i++){
        pid = fork();
        if (pid == -1){
            perror("cannot create a process");
        }
        if else (pid == 0){         //I'm your son
            if (i == 0){
            
            for (j=0, j=2)
                // 
                // 
            }
        
        }
        else{         //I'm your Father Luke
            wait (&status)
        }
    }
    
    return 0;
}

```



## 2) Famille d’exec
### Exercice 1
Écrivez un programme C monshell qui se comporte comme le shell, c’est-à-dire
une fois lancé, il affiche une invite, prend le nom d’une commande, l’exécute, affiche le résultat d’exécution et puis ré-affiche une invite. Pour complexifier un peu le problème, monshell peut exécuter la commande avec des arguments mais sans redirection. Le programme se termine quand l’utilisateur saisit le mot clé quit.

### Exercice 2 - Path et variables d’environnement
1. Écrivez un programme affichez qui affiche une chaîne de caractères passée en argument.
2. Écrivez un programme qui crée un processus fils qui exécute un autre programme affichez avec l’argument bonjour.
Pour vous aider, demandez vous :
* Que donne la commande shell which ls ?
* Que donne la commande shell echo $PATH ?















#  C'est Maï :) 
 
//ahah 


```c 


```





































































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
















