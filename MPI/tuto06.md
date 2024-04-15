# HPC - Programmation parallèle sur MPI

## Tutoriel N°6
### Envoi et réception basiques de message sur MPI

### Objectifs
* Découvrir les fonctions ```MPI_Send```, ```MPI-Recv```
* Pouvoir envoyer et recevoir entre 2 processus avec les routines ```MPI_Send``` et ```MPI-Recv```
* Augmenter le nombre de processus et observer le comportement

Ce programme extrait les données du processus ```zéro``` et les envoie à tous les autres processus en anneau. C'est-à-dire que le processus ```i``` doit recevoir les données et les envoyer au processus
```i + 1```, jusqu'à ce que le dernier processus soit atteint.
Supposons que les données se composent d'un seul entier. Le processus zéro initie l’exécution en lisant les données de l’utilisateur.
Voici les routines MPI utilisées dans ce tutoriel :
```
MPI_Send
MPI_Recv
```
### Le code :
```c
#include <stdio.h>
#include "mpi.h"
int main(argc, argv)
int argc;
char **argv;
 
{
    int rank, value, size; MPI_Status status;
    MPI_Init(&argc, &argv );
    MPI_Comm_rank( MPI_COMM_WORLD, &rank ); MPI_Comm_size( MPI_COMM_WORLD, &size );
        do {
         if (rank == 0) {
                scanf( "%d", &value );
                MPI_Send( &value, 1, MPI_INT, rank + 1, 0, MPI_COMM_WORLD );
          }
          else {
              MPI_Recv( &value, 1, MPI_INT, rank - 1, 0, MPI_COMM_WORLD, &status );
              if (rank < size - 1) MPI_Send( &value, 1, MPI_INT, rank + 1, 0, MPI_COMM_WORLD );
            }
          printf( "Processus %d a reçu %d\n", rank, value );}
        while (value >= 0);
    MPI_Finalize();
    return 0;
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 

* Créez un fichier nommé ```sendRecev.c``` en entrant la commande ```vim sendRecev.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc sendRecev.c -o sendRecev ```  
Cette procédure génère un nom de fichier exécutable sendRecev situé dans le dossier Desktop.
Enfin, exécutez le code à l'aide de la commande  
``` time mpiexec sendRecev ```  
ou  
``` time mpiexec ./sendRecev ```  

