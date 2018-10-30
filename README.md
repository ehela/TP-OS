# COURS D'OS

Notes et autres 




## correction TP1


Correction
TP 1.1

Afficher ligne num n :
head -50 fichier | tail -1
tail


Question 7:
grep passwd /etc/*
- chercher passwd dans tout ce qui se trouve dans le ... etc.
- on obtient des erreurs , pcq pas le droit.
Comment supprimer ces erreurs ?
uitliser l'option -s

rem:
stdin 0
stdout 1  -- Associés à l'écran
stderr 2  --

Pour supprimer erreurs, on fait redirection: 
grep passwd /etc/* >
OU BIEN POUR LA SORTIE STANDARD (TROU NOIR): redirection des toutes les sorties standard vers trou noir
grep passwd /etc/* 2> /dev/null
grep passwd /etc/* 2> fichier


Trouver fichier:
find path [aA]*
find path -iname a*
find a* -o A*

Remarque: commande find: compliqué mais puissant :)


Trouver fichiers qui commencent par -o et supprimer:
find -delete *.o    ATTENTION ça supprime tout ce qui est dans le répertoire
rm -i $(find path -name *.o)

find path -name "*.o" -type f -exec rm {} \*        REMARQUE : \ = signifier que commande finie
find 			      -delete
find ...








## correction TP3


Exercice:
processus affiche nbr dans l'ordre(père, fils):
crée un fils (il termine), puis l'autre fils.
Exercice 4:
Trouver façon élégante decréer lesfils 
montrer qu'il y a des relations entre des processus.
A la fin, il faut le bon nbr de processus crées et aucun processus zombie !! (bien mettre les wait)
père:crée fils1, fils2, fils3
Fils1: il et libre, il fait ce qu'il veut
pas mettre wait avant de crée fils2






remarque:
int execvp (const char *file ...);
le premier arg = nom fichier, si on veut exécuter commande ls, on la passe 2X



remarque: trouver chaine , analyser chaine
(1: commande, 2: argument)
-Par ex :chq option séparée par 1 seul espace (simplifier le pb)


remarque: monshell : ne pas utiliser system !!! ça comporte des failles très importantes !!
./monshell
$> ls -a -l
$> ls -al		--> ls -al -i: commande -option -option2(secondaire)


exec : 
system exec


strtop: split




## TP gestion de fichiers

Unix: trois types de fichiers dispo
-Ordinaire (txt, audio, ...)
-Répertoire (contient d'autres fichiers)
-Spéciaux (pointe sur autre fichier-Raccourcis, liens symbolique <> liens durs(qdplus de lien, supp le fichier), pipe (comminuqer avec d'autres processus), socket (moyen de comm entre plrs processu- a travers des réseaux, autre machine), fichier matériel (entreee-sortie : stdout))

Un fichier se caractérise par un "inode" (métadonné: taille, proprio, groupe, droits d'accès, date création)
rem: qd création de fichier,  l'OS crée un lien vers inode. /!\ pas de noms de fichiers: dans le répertoire (stocke le nom des fichiers)

Liens: a la cration de fichiers
-liens dur
-liens symbolique
Si aucune spécification: lien dur
ln        lien dur
ln -s     lien symbolique

...
Ex: 
touch foo (crée fichier "foo")
ls -i
ls -ia
ls -il (lien vers le fichier)

ln -s foo foo2 (make links: src leinsymbolique(foo2)vers src)
ls -lh

(et qd modif foo2 par ex)
cat foo (affiche foo)
(supprime foo)
rm foo  (foo2 marche)

less foo2 (ne marche plus car fait ref à foo)(affiche foo2)

rm foo2 (on supprime juste le lien symbolique)

(si on veut supp un fichier, il faut supp ts les liens qui pointent vers inode)
...


stat appel systeme
int stat ()
int fstat ()
int lstat


descripteur de ficheri (notion importante)
table de descripteur de fichiers
En c, plsr manieres de manip fichier (interagir avec noyau linux )- descripteur fichiers représente chq fichier
rem= descripteur != pointeur(== wrapper)


<i>open</i>
emprunte le chemin (pathname, flag, mode)      
-flag: mode avec lequel l'ouvrir
-mode: option secondaire


open("test", O_create | O_rdonly)   crée si n'existe pas, sinon rdonly:: lecture seule
open("test", O_create | O_rdonly, 0711)  
(mode) --> ici : 0 = sticky bit) A voir
7: utilisateur
1: groupe 
1: mode
remarque: permission, super_user, modif sous identitéé de super_user
rem: _rdwr : si n'est pas créé


EXEMPLE: 
int fd;
fd = open (...)   // fd >= 3 pcq stdr = 2 (y a des fichiers déjà ouverts)


READ: 

read (int fd, void *buf, size_t count);
fd: descrieur de fichiers
buf: contenu à copier 
size: nbr d'octets qu'on veut lire


DUP-DUP 2
duplication


table de descripteur (tableau):
0 strdin
1 stdout
2 strder
3 dummy
4
.
.


fd = open ("./dummy", O_CREAT);
fd2 = dup (fd)

rem : on utilise le dup pour faire la redirection

pid = fork()

(le fils hérite des fichiers ouverts par le père --> pere et fils peuvent communiquer !!mais attention, fichier ouverts en mm temps )



### TP


EX 1: 
cat : afficher contenu des fichiers

sol: 3-4 lignes (facile)


Ex3 :
je vs laisse reflechir un peu

EX4:


ex5: DUP-DUP2
redirection:
1) lancer resultat, stocker dans fichiers


Déclaration de répertoire 

EX6
rem comm:
pstree (voir relation entre diff processus system 
par ex: diff proc créés par ...)
utiliser fcts haut niveau

CE TP A RENDRE LA SEM PROCH (9)
