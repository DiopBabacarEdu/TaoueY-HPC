# HPC - Programmation parallèle sur MPI

## Tutoriel N°7

Dans l'exemple en anneau du tutoriel 6, nous supposons que le processus "suivant" est celui dont le rang est supérieur à notre rang. 
Autrement dit, le processus ```i``` envoie au processus ```i+1```. 
Cela n’est peut-être pas le meilleur choix du "prochain" processus, en particulier si vous utilisez un autre communicateur que ```MPI_COMM_WORLD```. 
MPI fournit des routines de topologie pour trouver un bon ordre des processus, en particulier pour des ordres linéaires simples tels que requis ici. 
L'affectation consiste à remplacer l'utilisation de "```rang + 1```" et "```rang-1```" (où ```rang``` fait référence au rang dans ```MPI_COMM_WORLD``` du processus appelant) 
par des valeurs calculées à l'aide de ```MPI_Cart_shift```.  
Voici les routines MPI dans ce TP:
```
MPI_Cart_create
MPI_Cart_shift
```


### Bout de code :
```c
#include <stdio.h>
#include "mpi.h"
int main( argc, argv )
int argc;
char **argv;
{
  int rank, value, size, false=0; int right_nbr, left_nbr; MPI_Comm ring_comm; MPI_Status status;
  MPI_Init( &argc, &argv );
  MPI_Comm_size( MPI_COMM_WORLD, &size );
  MPI_Cart_create( MPI_COMM_WORLD, 1, &size, &false, 1, &ring_comm );
  MPI_Cart_shift( ring_comm, 0, 1, &left_nbr, &right_nbr ); MPI_Comm_rank( ring_comm, &rank );
  MPI_Comm_size( ring_comm, &size );
      do {
       if (rank == 0) {
             scanf( "%d", &value );
             MPI_Send( &value, 1, MPI_INT, right_nbr, 0, ring_comm );
        }
        else {
             MPI_Recv( &value, 1, MPI_INT, left_nbr, 0, ring_comm, &status );
             MPI_Send( &value, 1, MPI_INT, right_nbr, 0, ring_comm );
        }
       printf( "Processus %d a reçu %d\n", rank, value );
      } while (value >= 0);

  MPI_Finalize();
  return 0;
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez maintenant un fichier nommé ```cartShift.c``` en entrant la commande ```vim cartShift.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc cartShift.c -o cartShift ```  
Cette procédure génère un nom de fichier exécutable ```cartShift``` situé dans le dossier parent.
* Enfin, exécutez le code à l'aide de la commande ``` time mpiexec cartShift ```   ou  ``` time mpiexec ./cartShift ```  ou avec ```time mpiexec -g -np 10 ./cartShift ```
