# HPC - Programmation parallèle sur MPI

## Tutoriel N°1

* Ce test pourrait aussi être émulé sur un PC en tant que super-calculateur à un seul noeud, ordonnant par conséquent aux différents cœurs de votre processeur d'effectuer vos opérations de calcul.

* Vous allez ensuite convertir la logique de ce code en version MPI, l'exécuter sur un cœur du processeur, puis mettre progressivement en ligne les cœurs restants.

* Ainsi, vous apprendrez à écrire et à exécuter un simple code ```C``` pour calculer ```π```. 
Ce code effectuera une intégration numérique (avec ```300 000 itérations```) sur une représentation de la fonction ```x``` de ```π```. Voici le code :

Ci-dessous l'implémentation des fonctions de base sur les listes chaînées.

### Bout de code :
```c
#include <math.h> // math library
#include <stdio.h>// Standard Input/Output library
int main(void) {
  long num_rects = 300000; long i;
  double x,height,width,area; double sum;
  width = 1.0/(double)num_rects; sum = 0;
  for(i = 0; i < num_rects; i++) {
    x = (i+0.5) * width;
    height = 4/(1.0 + x*x);
  sum += height; }
  area = width * sum; //1000000000;
  // taille d'un segment
  // x: distance au centre du ième segment
  // Somme des segments individuels
  // Approximation de la valeur de Pi avec l'équation finale
  printf("n");
  printf(" Calculated Pi = %.16fn", area);
  printf(" M_PI = %.16fn", M_PI);
  printf("Relative error = %.16fn", fabs(area - M_PI)); return 0;
}
```

Le code a été écrit à l'aide de l'éditeur vim dans l'environnement terminal Linux comme suit: 
* Commencez par changer le répertoire dans le dossier Desktop en entrant la commande ```cd Desktop```
* Créez maintenant un fichier nommé ```Pi.c``` en entrant la commande ```vim Pi.c```
* Appuyez sur ```i``` pour lancer la modification.
* Tapez ou copiez le code comme indiqué précédemment.
* Enregistrez le fichier en entrant ```Esc:wq```, qui vous ramène à l’invite de commande ($)


### Compilation et exécution
* Compiler le code à l'aide de la commande :  
``` mpicc Pi.c -o Pi ```  
Cette procédure génère un nom de fichier exécutable Pi situé dans le dossier Desktop.
Enfin, exécutez le code à l'aide de la commande  
``` time mpiexec Pi ```  
ou  
``` time mpiexec ./Pi ```  

* N'hésitez pas à modifier le code en modifiant la valeur de *** num_rects *** (actuellement égale à 300 000) et
observez la valeur modifiée du calcul de Pi. Le temps de calcul du code devrait changer en conséquence. Notez que ce calcul a été exécuté sur un seul coeur du processeur.
 
