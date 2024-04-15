# HPC - Programmation parallèle sur MPI

## Tutoriel N°11
### SIMD - Maître-Esclave 

Dans ce tutoriel, vous allez construire un système ***maître/esclave*** d’entrée/sortie. 
Cela vous permettra d'organiser relativement facilement différents types d'entrées et de sorties de votre programme
Les processus de ```MPI_COMM_WORLD``` sont divisés en deux ensembles: le maître (qui exécute toutes les E/S) et les esclaves (qui effectuent toutes leurs E/S en contactant le maître). 
Les esclaves feront aussi tout autre calcul qu'ils pourraient désirer; par exemple implémenter l'itération Jacobi.
Dans un premier temps, les processus sont divisés en deux communicateurs, l'un étant le maître et l'autre les esclaves. 
Le maître doit accepter les messages des esclaves (de type ```MPI_CHAR```) et les imprimer par ordre de ```rang``` (c'est-à-dire d'abord l'esclave 0, puis l'esclave 1, etc.). 
Les esclaves doivent chacun envoyer 2 messages au maître.

```Hello from slave i ```  
```Goodbye from slave i ```  
(avec ```i``` la valeur appropriée pour chaque esclave). 

On suppose une longueur maximale de message de 256 caractères.
Les routines MPI utilisées sont:
```
MPI_Comm_split
MPI_Send
MPI_Recv
```

### Bout de code :
```c
/******************************************************************************
MPI C Compiler.
@authors : Babacar Diop
@organisation : CINER
@Website: www.cineri.sn 
@contact : babacar.diop@cineri.sn
@Github: DiopBabacarEdu
This code has been proposed as tutorial part of the CINERI users for HPC
*******************************************************************************/

#include <stdio.h>
#include "mpi.h"
int main( argc, argv )
int argc;
char **argv;
{
    int rank, size;
    MPI_Comm new_comm;
    MPI_Init( &argc, &argv );
    MPI_Comm_rank( MPI_COMM_WORLD, &rank );
    MPI_Comm_split( MPI_COMM_WORLD, rank == 0, 0, &new_comm );
    if (rank == 0) master_io( MPI_COMM_WORLD, new_comm );
    else slave_io( MPI_COMM_WORLD, new_comm );
    MPI_Finalize( );
    return 0; 
}

/* Voici le code du Maître*/
int master_io( master_comm, comm ) 
MPI_Comm comm;
{
    int        i,j, size;
    char       buf[256];
    MPI_Status status;
    MPI_Comm_size( master_comm, &size );
    for (j=1; j<=2; j++) {
        for (i=1; i<size; i++) {
            MPI_Recv( buf, 256, MPI_CHAR, i, 0, master_comm, &status );
            fputs( buf, stdout );
        } 
    }
}

/* voici le code des Esclaves*/
int slave_io( master_comm, comm )
MPI_Comm comm;
{
    char buf[256];
    int  rank;
    MPI_Comm_rank( comm, &rank );
    sprintf( buf, "Hello from slave %d\n", rank );
    MPI_Send( buf, strlen(buf) + 1, MPI_CHAR, 0, 0, master_comm );
    sprintf( buf, "Goodbye from slave %d\n", rank );
    MPI_Send( buf, strlen(buf) + 1, MPI_CHAR, 0, 0, master_comm );
    return 0; 
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez un fichier nommé ```masterSlave.c``` en entrant la commande ```vim masterSlave.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc masterSlave.c -o masterSlave ```  
Cette procédure génère un nom de fichier exécutable masterSlave situé dans le dossier parent.
Enfin, exécutez le code à l'aide de la commande  ``` time mpiexec -np 20 ./masterSlave ```, qui signifie que nous déployerons un maître et 19 esclaves.
