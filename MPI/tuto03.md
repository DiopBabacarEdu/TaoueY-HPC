# HPC - Programmation parallèle sur MPI

## Tutoriel N°3
### À la découverte des bases de MPI


### Objectifs
Les objectifs de ce tutoriel sont déclinées comme suit :
* Découvrir un programme de base utilisant la bibliothèque MPI
* Comprendre les fonctions MPI de base pour la communication entre processus
* Tester le programme sur plusieurs processus et de pouvoir en observer le comportement

Il est généralement nécessaire qu'un processus obtienne des données de l'utilisateur, soit en lisant les arguments du terminal ou de la ligne de commande, puis en distribuant ces informations à tous les autres processeurs.  
**Ce programme lit une valeur entière à partir du terminal et la distribue à tous les processus MPI.**  
Chaque processus affiche son rang et la valeur qu’il a reçue. Les valeurs sont lues jusqu'à ce qu'un entier négatif soit donné en entrée.

### Code C

```c
#include <stdio.h>
#include "mpi.h"
int main( argc, argv ){
  int argc;
  char **argv;
  int rank, value;
  MPI_Init( &argc, &argv );
  MPI_Comm_rank( MPI_COMM_WORLD, &rank );
  do {
       if (rank == 0)
            scanf( "%d", &value );
            MPI_Bcast( &value, 1, MPI_INT, 0, MPI_COMM_WORLD);
            printf( "Processus %d got %d\n", rank, value );
      }
  while (value >= 0);
  MPI_Finalize();
  return 0;
}
```

Ce code est à exécuter plusieurs fois afin d'observer l’ordre de séquence des processus.

### Compilation et exécution
Compiler et ex&écuter le code à l'aide des commandes :  
``` mpicc bcast.c -o bcast ```      
``` mpirun -np 4 bcast ```  
