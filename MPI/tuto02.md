# HPC - Programmation parallèle sur MPI

## Tutoriel N°2

### Objectifs
* Découvrir les bases de MPI
* Comprendre les fonctions/routines de base MPI
* Exploiter les différents blocs de processeurs de votre PC pour observer l’exécution de code
* Tester le programme avec différents nombres de processus et pouvoir en observer le comportement

Ce premier programme utilise MPI et à chaque processus MPI affiche à l’écran:
```Hello world du processus i de n ```
en utilisant le rang dans ```MPI_COMM_WORLD``` pour ```i``` et la taille de ```MPI_COMM_WORLD``` pour ```n```.
Notez l'ordre d'affichage des résultats. Un autre tutoriel avec la stratégie maître/esclaves montrera comment commander la sortie pour afficher les processus sur le bon ordre.

### Code C

```c
#include <stdio.h>
#include "mpi.h"
  int main( argc, argv ){
    int  argc;
    char **argv;
    int rank, size;
    MPI_Init( &argc, &argv );
    MPI_Comm_size( MPI_COMM_WORLD, &size );
    MPI_Comm_rank( MPI_COMM_WORLD, &rank );
    printf( "Hello world from process %d of %d\n", rank, size );
    MPI_Finalize();
  return 0;
}
```

Ce code est à exécuter plusieurs fois afin d'observer l’ordre de séquence des processus.

### Compilation et exécution
Compiler le code à l'aide de la commande :  
``` mpicc hello.c -o hello ```  
Cette procédure génère un nom de fichier exécutable hello situé dans le dossier Desktop.
Enfin, exécutez le code à l'aide de la commande  
``` time mpiexec hello ```  
ou  
``` time mpiexec ./hello ```  
