
# TP 3 OS

#Exercice 1

Étant donnée le listing du programme suivant :
...c
#include < stdio.h >
#include < ctype.h >
#include < stdlib.h >
#define SIZE 5
int getint ( int ∗ ) ;  // On déclare getint()
int main ( ) {
  int n , array [ SIZE ] ; // definition de variable
  // n augmente jusque 4 sauf si il a atteint la fin du fichier
  
  for ( n = 0 ; n < SIZE && getint (& array [ n ] ) != EOF; n++) ;
    return 0 ;
 }

int getint ( int ∗pn ) // pn est un pointeur
{
  int c , sign ;
  while ( isspace ( c = getchar ( ) ));
  if ( ! isdigit ( c ) && c != EOF && c != ’+ ’ && c != ’ − ’) {
    ungetc ( c , stdin ) ;
    return 0 ;
  }
  sign = ( c == ’ − ’) ? −1 : 1 ;
  if ( c == ’+ ’ | | c == ’ − ’)
    c = getchar ( ) ;
  for (∗pn = 0 ; isdigit ( c ) ; c = getchar ( ) )
  *pn = 10 ∗ ∗pn + ( c − ’ 0 ’ ) ;
  ∗pn ∗= si g n ;
  if ( c != EOF)
    ungetc ( c , stdin ) ;
  return c ;
}


...

Répondez aux questions suivantes :
###1. Que fait ce programme ?

GDB: GNU debugger
EOf: End-Of-File

int getchar(void) : The C library function int getchar(void) gets a character (an unsigned char) from stdin. This is equivalent to getc with stdin as its argument.

isspace (int): vérifie si le caractère est un espace 

ungetc(int char, FILE *stream): The C library function int ungetc(int char, FILE *stream) pushes the character char (an unsigned char) onto the specified stream so that the this is available for the next read operation.

int isdigit (int c): The C library function void isdigit(int c) checks if the passed character is a decimal digit character. Decimal digits are (numbers) − 0 1 2 3 4 5 6 7 8 9.

stdin :Standard input stream. The standard input stream is the default source of data for applications. In most systems, it is usually directed by default to the keyboard. stdin can be used as an argument for any function that expects an input stream (FILE*) as one of its parameters, like fgets or fscanf.


Tout d'abord, une première fonction "getint()" est déclarée
getint()
    On rentre dans une boucle WHILE qui demande à l'utilisateur de rentrer un caractère qui n'est pas un espace. Tant que l'utilisateur rentre un espace, alors, la boucle ne s'arrete pas.
    IF: Ensuite, on regarde si le caractère n'est pas un nombre ET que ce n'est pas la fin d'un fichier ET qu'il est différent du caractère "+" ET du caractère "-". 
    	Si ces conditions sont respectées, alors on fait ungetc (c, stdin) qui pousse c dans le truc que stdin a entré.
	ET on return 0;
		
    SINON: on met la variable "sign" à -1 si la variable c est un "-", ou bien la valeur 1
    IF: la variable c est "+" ou "-", alors on doit rentrer un autre caractère pour la variable c: c = getchar()
    FOR: 
        initialiser pointeur *pn --> jusqu'a ce que le nombre entré soit un "digit"/nombre (= CONDITION DE SORTIE)
	
    Dans la boucle, *pn = 10 * *pn + (c - '0') et ensuite, on multiplie *pn par sign ( qui vaut soit 1, soit -1)
	
    IF: si la variable c n'est pas la fin du fichier, alors on fait unget(c- '0')
    
    ON RETURN 0 à la fin
    

Ensuite on lance la fonction main qui lance le programme:
  On définit un tableau ayant une taille limitée, puis on initialise une boucle for ayant "n" comme variable compteur


#remarque ligne 62: Je ne suis pas sure que ce soit obligatoirement un simple caractère, il faudrait voir si ça pourrait être une "string"
#remarque ligne 63: en fait je pense plus que le caractère doit être différent du "NULL" ou bien de "\0"
#remarque ligne 64 : Est-ce que "stdin" signifie qu'on peut rentrer un autre caractère(/chaine de caractère) ou bien ?


2. En utilisant le gdb, mettez en évidence le passage par référence de chaque élément
du tableau.

* dans le fichier.c, ecrire : gcc -g programme.c -o programme
avec , programme, le nom du programme a exécuter .S'il y a plusieurs fichiers ".c", il faut aussi les mettre

*(gdb)kill 		//terminer le programme en cours
* (gdb)run		// redémarre l'exécution en cours



3. Essayez de comprendre les rapports de la commande strace

Permet de tracer un appel système généré par le processus
Remarque :> strace -o trace.txt ./<  >
crée un fichier nommé trace.txt dans lequel sont notés tous les appels systèmes


#Exercice 2

On veut créer un programme de dictionnaire qui permet de stocker des mots et ainsi
que leurs définitions. Il est à noter que le programme devra utiliser une liste chaînée afin
qu’on peut stocker invariablement plusieurs mots. De ce fait, vous devez implémenter au
moins ces fonctions suivantes :
— déclarer une structure de données d’un élément de dictionnaires.
— écrire une fonction permettant de tester si un dictionnaire est vide. La fonctionne
retourne un nombre positif si le dictionnaire est vide sinon zéro dans le
cas contraire.
— écrire une fonction permettant d’ajouter un mot à la tête de liste.
— écrire une fonction permettant d’ajouter un mot à la queue d’une liste.
— écrire une fonction permettant d’ajouter un mot en donnant l’indice d’insertion.
— écrire une fonction permettant de supprimer un mot à la tête de liste.
— écrire une fonction permettant de supprimer un mot à la queue de liste.
— écrire une fonction permettant de supprimer un mot en donnant son indice.
— écrire une fonction permettant de supprimer un mot en donnant le mot à supprimer.
— écrire une fonction permettant de rechercher un mot.
— écrire une fonction pour copier un dictionnaire. La fonction retourne un nouveau
dictionnaire.
— écrire une fonction affichant tous les mots entrés par page de 10.
— écrire une fonction affichant un menu pour faire appels à des fonctions
— écrire une fonction permettant de trier le dictionnaire par order croissant et décroissant.
en haut.
— écrire un makefile qui permet de compiler ce programme.
Lorsque le programme se termine, vous devez prendre soin de libérer toutes les zones
de mémoires utilisés, si votre programme n’effectue pas proprement ce nettoyage, cela
entraîne un point de moins sur la note attribuée à cet exercice.

...c


ce que le prof a mis au tableau: :o

struct block {
    char data[255];
    struct block *next;
}

struct word {
    char key[25];
    struct block *definition
    struct word *next
}

str. block *ptr;
ptr = (st??? bl??? ?) malloc(sizeof());
(*ptr).a = 3;
(*ptr)data ="boujour"
    strcpy (dest, source)
    strnpy( , ,j)
    dest[5] = '/0'




...









...c

// fichier.h --> Prototypes
# define <stdio.h>
# define <stlib.h>
# define size_def 1000
/* create the differents structures*/
struct Dict
{
	char data[size_def];
	struct Dict *p_Dnext;
};

struct Woord
{
	char key[30];
	struct Dict *p_definition;
	struct Word *p_Wnext
};


/*OU BIEN J'EN AI MARRE JE FAIS CETTE STRUCTURE TANT PIS SI  C'EST PAS BON
pcq avec l'uatre, y faudrait prendre en comptre que mot est différent de la déf
Ici, pas de distinction*/
struct Word 
{
	char data[size_def];
	struct word *p_Wnext;
	{
		
	};
}


/* TOUTES LES AUTRES STRUCTURE SONT A PARTIR DE CA*/


int add_head (struct Word *p_Whead, char* mot)
int add_tail (struct Word *p_Whead, char* mot )




// fichier .c

/*écrire une fonction permettant d’ajouter un mot à la tête de liste. 
*p_head: pointeur de début de liste
mot: mot à ajouter*/
int add_head (struct Word *p_Whead, char* mot){
	// *p_Wnext= *p_Whead
	//p_whead --> *p_Wnext
	// struct Word *p_Wnext = malloc(sizeof(*p_Wnext))

	// Word word = mot


}


//OK??  écrire une fonction permettant de tester si un dictionnaire est vide. RETURN un nombre positif IF vide, ELSE zéro  
int if_empty(struct Word *p_Whead ){
	if p_Whead == null;
		return 1;
	return 0
}


// écrire une fonction permettant d’ajouter un mot à la queue d’une liste.
int add_tail (struct Word *p_Whead, char* mot ){
	//char* p_Wnext

	struct Word *p_Wnew = malloc(sizeof(*p_Wnew));

	if p_Wnew != NULL{ // si l'alloc a marché
		p_Wnew = NULL;

		if p_Whead == null{
			p_Whead = p_Wnew;
			struct word data = mot;
		}
		else {
			struct 
			{
				
			};
		}
	}

	/* if p_Whead == NULL{
		*p_Wnext = *p_Whead
		*p_Whead pointe sur p_Whead
	}*/

	// p_new =p_Whead 

	// while (*p_new != NULL) --> Element suivant

		//

	return 0;
}	


// écrire une fonction permettant d’ajouter un mot en donnant l’indice d’insertion.
int insert_word (struct Word *p_Whead, char* mot, int index){
	// int i;

	//for i = 0, i > index, i ++{
	//Add word


}



// écrire une fonction permettant de supprimer un mot à la tête de liste.
int del_word (struct Word *p_Whead){
	//if *p_Whead == NULL , alors on ne supprime rien/ OU BIEN ERROR
		//return 0;
}


// écrire une fonction permettant de supprimer un mot à la queue de liste.

int delete_tail (){
	
	while (pointeur != NULL){
		pointeur = p_next
		// 
	}
}


// écrire une fonction permettant de supprimer un mot en donnant son indice.

// écrire une fonction permettant de supprimer un mot en donnant le mot à supprimer.

// écrire une fonction permettant de rechercher un mot.

// écrire une fonction pour copier un dictionnaire. La fonction retourne un nouveau dictionnaire.

// écrire une fonction affichant tous les mots entrés par page de 10.

// écrire une fonction affichant un menu pour faire appels à des fonctions

// écrire une fonction permettant de trier le dictionnaire par order croissant et décroissant. en haut.

// écrire un makefile qui permet de compiler ce programme.







// main.c

int main (){
	struct Word *p_Whead = NULL;
	return 0;
}








































__________________________



# Liste chainée



On veut créer un programme de dictionnaire qui permet de stocker des mots et ainsi que leurs définitions. Il est à noter que le programme devra utiliser une liste chaînée afin qu’on peut stocker invariablement plusieurs mots. De ce fait, vous devez implémenter au moins ces fonctions suivantes : 

Listes chainées
---------------
-> semblables aux tableaux

/!\ l'accès se fait par pointeur et non par index(=char[] je suppose)
le pointeur suivant du dernier élément pointe vers NULL


Pour accéder à un élément:
--------------------------
1) parcourir la tête
2) le 2e pointeur sert à se déplacer vers le prochain élément

/!\ le déplacement se fait dans une seule direction

Pour se déplacer dans les 2 directions --> listes doublement chainées

Pour définir un élément de la liste:
------------------------------------
* on utilise struct

Sauvegarde d'élément:
---------------------
* pour avoir le controle de la liste
* le premier élément, le dernier et le nombre d'éléments
* avec des variables ou des structures

```c
typedef struct ListeRepere {
	Element *debut; // adresse du premier élément de la liste
	Element *fin; // adresse du dernier élément de la  liste
	int taille; // nombre d'éléments
} Liste;
```

/!\ peu importe la position dans la liste pointeurs début et fin pointent toujours vers le 1er et dernier élément.
/!\ taille reste aussi inchangé

Insertion et suppression sur les listes chainées
------------------------------------------------
Avant toute opération, il faut initialiser. Le pointeur début et fin sont initialisés avec le pointeur NULL et la taille avec 0.

```c
void initialisation (Liste *liste) {
	liste ->debut = NULL;
	liste -> fin = NULL;
	taille = 0;
}
```

ajouter un élément dans la liste
--------------------------------
* déclarer les éléments à insérer
* allocation de mémoire
* remplir le contenu du champs de données
* maj des pointeurs vers élément 1 et dernier (slmt si nécessaire)
* la taille de la liste est maj !

insérer dans une liste vide
---------------------------
* (déclarer élément à insérer)
* allocation de mémoire
* remplir le champs de données avec les nouveaux éléments
* le pointeur après l'élément ajouter pointera vers NULL
* pointeur debut et fin pointent vers l'élément ajouté (puisque 1 seul élément)
* taille maj

```c
int ins_dans_liste_vide (Liste * liste, char *donnee){ 

	Element *nouveau_element; // déclarer élément à insérer

	// allocation de mémoire
	if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
		return -1; // échec
	if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) 
      == NULL) 
		return -1; // échec
	
	// champs des données avec nouveau élément
  	strcpy (nouveau_element->donnee, donnee); 

	// pointeur après l'élément ajouté vers NULL
	nouveau_element->suivant = NULL; 

	// debut et fin pointent vers nouvel élément
	liste->debut = nouveau_element; 
	liste->fin = nouveau_element; 

	// maj taille
  	liste->taille++; 

  	return 0; // réussite
} 
```

insertion en début de liste 
---------------------------
* allocation de la mémoire pour le nouvel élément
* remplir le champ de données du nouvel élément
* le pointeur suivant du nouvel élément pointe vers le 1er élément
* le pointeur debut pointe vers le nouvel élément
* le pointeur fin ne change pas
* la taille est incrémentée

```c
int ins_debut_liste (Liste * liste, char *donnee){ 

	// déclarer nouveau élément à insérer
	Element *nouveau_element; 

	// allocation de mémoire
	if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
		return -1; //echec
	if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) == NULL) 
		return -1; //echec

	// remmplir champs de données
	strcpy (nouveau_element->donnee, donnee); 
	
	// pointeur suivant du nouvel élément pointe vers le premier élément
	nouveau_element->suivant = liste->debut;

	// pointeur debut pointe vers le nouvel élément 
	liste->debut = nouveau_element; 

	// maj taille
	liste->taille++; 

	return 0; // réussite
}
```

insérer élément en fin de liste
-------------------------------
* allocation de la mémoire pour le nouvel élément
* remplir le champ de données du nouvel élément
* le pointeur suivant du dernier élément pointe vers le nouvel élément
* le pointeur fin pointe vers le nouvel élément
* le pointeur debut ne change pas
* la taille est incrémentée

insérer ailleurs dans la liste
------------------------------
* allocation de la mémoire pour le nouvel élément
* remplir le champ de données du nouvel élément
* choisir une position dans la liste (l'insertion se fera après la position choisie)
* le pointeur suivant du nouvel élément pointe vers l'adresse sur laquelle pointe le pointeur suivant d'élément courant. (cette explication est un peu tordue, mais l'image vous éclaira ;-)
* le pointeur suivant du l'élément courant pointe vers le nouvel élément
* les pointeurs debut et fin ne change pas
* la taille est incrémentée d'une unité

```c
int ins_liste (Liste * liste, char *donnee, int pos){ // pos = position dans la liste
	if (liste->taille < 2) 
		return -1; // echec
	if (pos < 1 || pos >= liste->taille) 
		return -1; // echec

	Element *courant; 
	Element *nouveau_element; 
	int i; 

	// allocation de mémoire
	if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
		return -1; // échec
	if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) == NULL) 
		return -1; // échec

	// ajout dans le champs des données
	courant = liste->debut; 
	for (i = 1; i < pos; ++i) 
		courant = courant->suivant; 
	if (courant->suivant == NULL) 
		return -1; 
	strcpy (nouveau_element->donnee, donnee); 

	// le pointeur suivant du nouvel élément pointe vers l'adresse sur laquelle pointe le pointeur suivant d'élément courant. 
	nouveau_element->suivant = courant->suivant; 

	// le pointeur suivant du l'élément courant pointe vers le nouvel élément
	courant->suivant = nouveau_element; 

	// maj de la taille
	liste->taille++; 

	return 0; // réussite
} 
```


Suppression d'un élément dans la liste
--------------------------------------
* utilisation d'un pointeur temporaire pour sauvegarder l'adresse d'éléments à supprimer
* l'élément à supprimer se trouve après l'élément courant
* Faire pointer le pointeur suivant de l'élément courant vers l'adresse du pointeur suivant de l'élément à supprimer
* libérer la mémoire occupée par l'élément supprimé
* mettre à jour la taille de la liste

Pour supprimer un élément dans la liste il y a plusieurs situations :
1. Suppression au début de la liste
2. Suppression ailleurs dans la liste

voir plus d'infos sur: https://www.commentcamarche.com/faq/7444-liste-simplement-chainee#operations-sur-les-listes-chainees

Affichage de la liste
---------------------
* Il faut se positionner au début de la liste (le pointeur debut le permettra). 
* Ensuite en utilisant le pointeur suivant de chaque élément la liste est parcourue du 1er vers le dernier élément. (2)
* La condition d'arrêt est donnée par le pointeur suivant du dernier élément qui vaut NULL. (2)

```c
void affiche (Liste * liste){ 

	Element *courant; 

	// positionnement en début de liste
	courant = liste->debut; 

	// (2)
	while (courant != NULL){ 
		printf ("%p - %s\n", courant, courant->donnee); 
		courant = courant->suivant; 
	} 
} 
```

Supprimer une liste entière
---------------------------
```c 
void detruire (Liste * liste){ 
  while (liste->taille > 0) 
    supp_debut (liste); 
} 
```


* déclarer une structure de données d’un élément de dictionnaires.

```c 
struct block {
    char data[255];
    struct block *next;
}

struct word {
    char key[25];
    struct block *definition
    struct word *next
}
```

```c

typedef struct ListeRepere {
	Element *debut; // adresse du premier élément de la liste
	Element *fin; // adresse du dernier élément de la  liste
	int taille; // nombre d'éléments
} Liste;

```

* écrire une fonction permettant de tester si un dictionnaire est vide. La fonction retourne un nombre positif si le dictionnaire est vide sinon zéro dans le cas contraire.

* écrire une fonction permettant d’ajouter un mot à la tête de liste.


```c
int ins_debut_liste (Liste * liste, char *donnee){ 

	// déclarer nouveau élément à insérer
	Element *nouveau_element; 

	// allocation de mémoire
	if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
		return -1; //echec
	if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) == NULL) 
		return -1; //echec

	// remmplir champs de données
	strcpy (nouveau_element->donnee, donnee); 
	
	// pointeur suivant du nouvel élément pointe vers le premier élément
	nouveau_element->suivant = liste->debut;

	// pointeur debut pointe vers le nouvel élément 
	liste->debut = nouveau_element; 

	// maj taille
	liste->taille++; 

	return 0; // réussite
}
```

* écrire une fonction permettant d’ajouter un mot à la queue d’une liste.
```c
int ins_fin_liste (Liste * liste, Element * courant, char *donnee){ 
  Element *nouveau_element; 
  if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
    return -1; 
  if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) 
      == NULL) 
    return -1; 
  strcpy (nouveau_element->donnee, donnee); 

  courant->suivant = nouveau_element; 
  nouveau_element->suivant = NULL; 

  liste->fin = nouveau_element; 

  liste->taille++; 
  return 0; 
} 


```
* écrire une fonction permettant d’ajouter un mot en donnant l’indice d’insertion.

```c
int ins_liste (Liste * liste, char *donnee, int pos){ // pos = position dans la liste
	if (liste->taille < 2) 
		return -1; // echec
	if (pos < 1 || pos >= liste->taille) 
		return -1; // echec

	Element *courant; 
	Element *nouveau_element; 
	int i; 

	// allocation de mémoire
	if ((nouveau_element = (Element *) malloc (sizeof (Element))) == NULL) 
		return -1; // échec
	if ((nouveau_element->donnee = (char *) malloc (50 * sizeof (char))) == NULL) 
		return -1; // échec

	// ajout dans le champs des données
	courant = liste->debut; 
	for (i = 1; i < pos; ++i) 
		courant = courant->suivant; 
	if (courant->suivant == NULL) 
		return -1; 
	strcpy (nouveau_element->donnee, donnee); 

	// le pointeur suivant du nouvel élément pointe vers l'adresse sur laquelle pointe le pointeur suivant d'élément courant. 
	nouveau_element->suivant = courant->suivant; 

	// le pointeur suivant du l'élément courant pointe vers le nouvel élément
	courant->suivant = nouveau_element; 

	// maj de la taille
	liste->taille++; 

	return 0; // réussite
} 
```

* écrire une fonction permettant de supprimer un mot à la tête de liste.

* écrire une fonction permettant de supprimer un mot à la queue de liste.

```c 
int supp_debut (Liste * liste){ 
    if (liste->taille == 0) 
        return -1; 
    Element *supp_element; 
    supp_element = liste->debut; 
    liste->debut = liste->debut->suivant; 
    if (liste->taille == 1) 
        liste->fin = NULL; 
    free (supp_element->donnee); 
    free (supp_element); 
    liste->taille--; 
    return 0; 
} 
``.

* écrire une fonction permettant de supprimer un mot en donnant son indice.

```c 
int supp_dans_liste (Liste * liste, int pos){ 
  if (liste->taille <= 1 || pos < 1 || pos >= liste->taille) 
    return -1; 
  int i; 
  Element *courant; 
  Element *supp_element; 
  courant = liste->debut; 

  for (i = 1; i < pos; ++i) 
    courant = courant->suivant; 

  supp_element = courant->suivant; 
  courant->suivant = courant->suivant->suivant; 
  if(courant->suivant == NULL) 
          liste->fin = courant; 
  free (supp_element->donnee); 
  free (supp_element); 
  liste->taille--; 
  return 0; 
} 

```





























https://hackmd.io/dYuccB7DR2q5uyO7FKZG-A?edit
