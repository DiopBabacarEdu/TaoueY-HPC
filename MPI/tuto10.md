# HPC - Programmation parallèle sur MPI

## Tutoriel N°10

Ce programme permet de recevoir correctement les messages de tous les processus d’envoi.
Le programme est organisé pour que tous les processus, à l'exception du processus 0, envoient 100 messages au processus 0.
Le processus 0 imprime les messages au fur et à mesure de leur réception.  

Vous aller utiliser les routines MPI suivantes:
```MPI_Waitsome
MPI_Irecv
MPI_Cancel
```

### Bout de code :
```c
#define large 128
#include "mpi.h"
#include <stdio.h>
int main(argc, argv)
int argc;
char **argv;
{
    int rank, size, i, sbuf = 1, cnt;
    MPI_Init( &argc, &argv );
    MPI_Comm_rank( MPI_COMM_WORLD, &rank ); 
    MPI_Comm_size( MPI_COMM_WORLD, &size );
    if (rank == 0) {
        MPI_Request requests[large]; 
        MPI_Status statuses[large];
        int         indices[large];
        int         buf[large];
        int         j, ndone;
        cnt = (size-1)*100;
        for (i=1; i<size; i++) MPI_Irecv( buf+i, 1, MPI_INT, i, MPI_ANY_TAG, MPI_COMM_WORLD, &requests[i-1] );
        while(cnt > 0) {
            MPI_Waitsome( size-1, requests, &ndone, indices, statuses );
            for (i=0; i<ndone; i++) {
                j = indices[i];
                printf( "Message du processus %d avec le tag %d\n",statuses[i].MPI_SOURCE, statuses[i].MPI_TAG );
                MPI_Irecv( buf+j, 1, MPI_INT, j+1, MPI_ANY_TAG, MPI_COMM_WORLD, &requests[j] );
            }
            cnt -= ndone;
        }
        /* Annulation des réception en cours  */
        for (i=0; i<size-1; i++)
        MPI_Cancel( &requests[i] );
    }
    else {
        for (i=0; i<100; i++) MPI_Send( &sbuf, 1, MPI_INT, 0, i, MPI_COMM_WORLD );
    }
    MPI_Finalize();
    return 0; 
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez un fichier nommé ```moreReceive.c``` en entrant la commande ```vim moreReceive.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc moreReceive.c -o moreReceive ```  
Cette procédure génère un nom de fichier exécutable ```moreReceive``` situé dans le dossier parent.
* Enfin, exécutez le code à l'aide de la commande  
``` mpiexec ./moreReceive ```  ou  ``` time mpiexec ./moreReceive ```  ou ``` time mpiexec -np 10 ./moreReceive ```
