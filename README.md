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


remarque: monshell : ne pas utiliser system
./monshell
$> ls -a -l
$> ls -al		--> ls -al -i: commande -option -option2(secondaire)


exec : 
system exec


strtop: split

