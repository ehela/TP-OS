# TITRE 

```c 


#include <stdio.h>

int main (void){
  printf ("Hello");
  return 0;
}

```



# TP-OS

EXERCICE:

|5|3|0|9|8|7|
trier ça


* une fenêtre : add // en tete de liste
add (table *t, int v);
*afficher
printList(table *t);
*trier
sort (table *)
*Libération de mém.
freeList(table *);


...c
int add (table *t, int v){
  
  return 0;
}







































:
#Exercive 3:
Écrivez un programme qui se comporte comme le programme cat, c’est-à-dire qu’il
lit un ou plusieurs fichiers passés en argument et puis affiche leurs contenus sur l’écran.

```c
#include <stdio.h>
#include <sys/type.h>
#include <sys/stat.h>
#include <unistd.h> // pour close le fichier

void main (int file){
    int contenu;
    //contenu = open( file, | O_CREATE );
    //close(contenu);
    //A VOIR contenu read(int fd, void *buf, size)
    printf("%s", contenu);
}


```



##Exercice 2

```c
#include <unistd.h>

int copy (int src, int dst){
    int contenu;
    contenu read (int fd, void *buf, size_t


int main (int src, int dst){
    copy(src, dst);
}


```

## Exercice 3

Écrivez un programme qui ouvre un fichier texte dont le chemin est passé en argument
et puis crée un processus fils. Le processus père lit 5 caractères et les affiche sur l’écran.


2. Comment on peut résoudre le problème observé ?
Le fichier texte contient des messages suivants :

Réponse Sous-Question 1:
Le problème: on ne revient pas au début lorsqu'on lit le fichier pour la deuxième fois.
 Par exemple, après que le père ait lu une fois les 5 caractères, le fils commence au 5 ième caractère alors qu il aurait du commencer au premier caractère.

Réponse Sous- Question 2:
On peut résoudre ce problème en
-recréant un nouv


```c
#includde <>

int programme (int fd , void *buf, size_t count){
    int pid, contenu, p* = NULL ;
    char tabl[];
    p* fopen (fd, r); //ouverture fichier en mode lecture seule
    // if contenu == NULL, alors échoué
    if (p == NULL){



includde <>

int programme (int fd , void *buf, size_t count){
    int pid, contenu, p* = NULL ;
    char tabl[];
    p* fopen (fd, r); //ouverture fichier en mode lecture seule
    // if contenu == NULL, alors échoué
    if (p == NULL){
        fclose(p*);
        // impossible
        return 0;  //end
    }

    pid fork();
    if (pid == -1){
        perror("error, error");
    }
    else if(pid == 0){




 }

    pid fork();
    if (pid == -1){
        perror("error, error");
    }
    else if(pid == 0){


        contenu = read (int fd, void *buf, size_t count)
        if (contenu == NULL){
            //do nothing
        for (i = 0; i == 10, i ++){
            to_read = getc (fd) // lire le caractère
            printf ("%s", to_read);
        // lire fichier 10
        exit();
    }
    else



    else
        // do nothing

        contenu = read (int fd, void *buf, size_t count)
        if (contenu == NULL){
            //do nothing
        for (i = 0; i == 5, i ++){
            to_read = getc (fd) // lire le caractère
            printf ("%s",to_read);

        wait();



    fclose();
    return 0;


```


## Exercice 4

## Exercice 4

// fichier write
// ecrire sur un fichier

```c

#include <stdio.h>
#include <unistd.h>
#include

int entrelacer (src1, src2, dest){
    // write
    int i;
    // créer fichier (create)
    while (i != EOF)
    // fputs () // écrire une chaine de caractères dans le fichier








int printList(table *t){
  if t != NULL
  pointeur = t;
  while (pointeur != NULL){
    printf("%d\n", table n)
  }
  return 0;
}

int sort (table *){
  int valuemin = 0, value;
  
  while (pointeur != NULL){
    value = table.n;
    if (value < valuemin){
      valuemin= value;
    tmp = (table*) malloc(sizeof(table));
    (*tmp).next
    }
  }
  
  return0;
}

int freeList(table *){
  return 0;
  
}

int main (void){
  
  return 0;
}

...

code blocks 


