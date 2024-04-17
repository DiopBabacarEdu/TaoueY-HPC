# HPC - Programmation parallèle sur MPI

## Tutoriel N°4
### Diffusion de messages, broadcasting

### Objectifs
* Découvrir la fonction ```MPI_Bcast```
* Comprendre l’utilité des fonctions utilisées
* Augmenter le nombre de processus et pouvoir en observer le comportement

Dans ce tutoriel, vous allez communiquer différents types de données avec un seul appel de diffusion MPI (```MPI_Bcast```). Votre programme lit un entier et une valeur double précision à partir de
l'entrée standard (à partir du processus 0, comme auparavant), et les communique à tous les autres processus avec un appel ```MPI_Bcast```.
Tous les processus sont fermés lorsqu'un entier négatif est lu.  
Voici les fonctions MPI utilisées dans ce tutoriel:
```MPI_Address
   MPI_Type_struct
   MPI_Type_commit
   MPI_Type_free
   MPI_Bcast
```

### Bout de code :
```c
#include <stdio.h>
#include "mpi.h"
int main( argc, argv ){
  int argc;
  char **argv;
  int rank;
  struct {
    int a;
    double b;
  } value;
  MPI_Datatype mystruct;
  int blocklens[2]; MPI_Aint indices[2]; MPI_Datatype old_types[2];
  MPI_Init( &argc, &argv );
  MPI_Comm_rank( MPI_COMM_WORLD, &rank );
  /* Une valeur de chaque type */
  blocklens[0] = 1;
  blocklens[1] = 1;
  /* Les types de base */
  old_types[0] = MPI_INT;
  old_types[1] = MPI_DOUBLE;
  /* Les emplacements mémoire de chaque element */
  MPI_Address( &value.a, &indices[0] ); MPI_Address( &value.b, &indices[1] );
  /* Etablir les correspondances */ indices[1] = indices[1] - indices[0]; indices[0] = 0;
  MPI_Type_struct( 2, blocklens, indices, old_types, &mystruct ); MPI_Type_commit( &mystruct );
      do {
            if (rank == 0) {
                  printf( "Ce programme lit 2 nombres, un entier et un réél, et effectue un message brodcast à tous les processus.\n" );
                  fflush(stdout);
                  printf( "Donner un entier et un réél \n" );fflush(stdout);
                  scanf( "%d %lf", &value.a, &value.b );
            }
            MPI_Bcast( &value, 1, mystruct, 0, MPI_COMM_WORLD );
            printf( "Processus %d a reçu %d and %lf\n", rank, value.a,
            value.b );fflush(stdout);
      } while (value.a >= 0);
    /* Libérer la mémoire et finaliser */
    MPI_Type_free( &mystruct );
    MPI_Finalize();
    return 0;
}

```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez un fichier nommé ```bcast2.c``` en entrant la commande ```vim bcast2.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc bcast2.c -o bcast2 ```  
Cette procédure génère un nom de fichier exécutable Pi bcast2 dans le dossier parent.
Enfin, exécutez le code à l'aide de la commande  
``` time mpiexec bcast2 ```  
ou  
``` time mpiexec ./bcast2 ```  

