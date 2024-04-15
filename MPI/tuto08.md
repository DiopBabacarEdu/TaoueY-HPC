# HPC - Programmation parallèle sur MPI

## Tutoriel N°8

Ce tutoriel présente un programme simple pour calculer la valeur de pi : la méthode évalue l'intégrale de $\dfrac{4}{1+x^{2}}$  entre ```0``` et ```1```. 
La méthode est simple: l'intégrale est approximée par la somme de ```n``` intervalles; l'approximation de l'intégrale dans chaque intervalle est égale à : $\dfrac{(\dfrac{1}{n}*4)}{1+x^{2}}$.   
Le processus maître (```rang 0```) demande à l'utilisateur le nombre d'intervalles; le maître communique ce nombre à tous les autres processus. 
Chaque processus additionne ensuite chaque ième intervalle (```x=rang/n, rang/n+taille/n, ...```). 
Enfin, les sommes calculées par chaque processus sont additionnées à l'aide d'une réduction avec la fonction ```MPI_Reduce```.  
Voici les routines MPI utilisées dans ce tutoriel :
```
MPI_Bcast 
MPI_Reduce
```
### Le code :
```c
#include <stdio.h>
#include "mpi.h"
#include <math.h>
int main(argc,argv)
int argc;
char *argv[];
{
    int done = 0, n, myid, numprocs, i;
    double PI25DT = 3.141592653589793238462643;
    double mypi, pi, h, sum, x;
    MPI_Init(&argc,&argv);
    MPI_Comm_size(MPI_COMM_WORLD,&numprocs); MPI_Comm_rank(MPI_COMM_WORLD,&myid);
    while (!done)
    {
        if (myid == 0) {
            printf("Entrer le nombre d’intervalles: (0 pour quitter)");
            
        }
        scanf("%d",&n);
        MPI_Bcast(&n, 1, MPI_INT, 0, MPI_COMM_WORLD);
        if (n == 0) break;
        h   = 1.0 / (double) n;
        sum = 0.0;
        for (i = myid + 1; i <= n; i += numprocs) {
            x = h * ((double)i - 0.5);
            sum += 4.0 / (1.0 + x*x);
        }
        mypi = h * sum;
        MPI_Reduce(&mypi, &pi, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
        if (myid == 0)
        printf("Valeur Pi = %.16f, Erreur = %.16f\n",pi, fabs(pi- PI25DT)); 
    }
    MPI_Finalize();
    return 0; 
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Créez  un fichier nommé ```reduce.c``` en entrant la commande ```vim reduce.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc reduce.c -o reduce ```  
Cette procédure génère un nom de fichier exécutable reduce situé dans le dossier parent.
* Enfin, exécutez le code à l'aide de la commande  
``` time mpiexec reduce ```  ou  ``` time mpiexec ./reduce ```  
