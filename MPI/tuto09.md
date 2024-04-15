# HPC - Programmation parallèle sur MPI

## Tutoriel N°9

Ce TP est un programme pour tester la justesse de la mise en œuvre du passage du message. 
Pour ce faire, tous les processus, à l'exception du processus 0, envoient 100 messages au processus 0. 
Le processus 0 imprime les messages dès leur réception, à l'aide de ```MPI_ANY_SOURCE``` et de ```MPI_ANY_TAG``` dans ```MPI_Recv```.

### Bout de code :
```c
/******************************************************************************
MPI C Compiler
@authors : Babacar Diop
@organisation : CINERI
@Website: www.cineri.sn 
@contact : babacar.diop@cineri.sn
@Github: DiopBabacarEdu
This code has been proposed as tutorial part of the CINERI users for HPC
*******************************************************************************/

#include "mpi.h"
#include <stdio.h>
int main(argc, argv)
int argc;
char **argv;
{
    int rank, size, i, buf[1];
    MPI_Status status;
    MPI_Init( &argc, &argv );
    MPI_Comm_rank( MPI_COMM_WORLD, &rank );
    MPI_Comm_size( MPI_COMM_WORLD, &size );
    if (rank == 0) {
        for (i=0; i<100*(size-1); i++) {
            MPI_Recv( buf, 1, MPI_INT, MPI_ANY_SOURCE, MPI_ANY_TAG, MPI_COMM_WORLD, &status );
            printf( "Message du processus %d avec le tag %d\n", status.MPI_SOURCE, status.MPI_TAG );
        } 
    }
    else {
        for (i=0; i<100; i++)
            MPI_Send( buf, 1, MPI_INT, 0, i, MPI_COMM_WORLD );
    }
    MPI_Finalize();
    return 0; 
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez maintenant un fichier nommé ```mpiAny.c``` en entrant la commande ```vim mpiAny.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc mpiAny.c -o mpiAny ```  
Cette procédure génère un nom de fichier exécutable mpiAny situé dans le dossier Desktop.
* Enfin, exécutez le code à l'aide de la commande  
``` mpiexec ./mpiAny ```  ou  ``` time mpiexec ./mpiAny ```  ou ``` time -np 20 mpiexec ./mpiAny ``` 
