OpenBLAS est une bibliothèque open source optimisée pour l'algèbre linéaire de base (BLAS) et les fonctions linéaires (LAPACK). 
Elle fournit des implémentations rapides et efficaces des opérations de base de l'algèbre linéaire, telles que les multiplications de matrices, 
les résolutions de systèmes linéaires et les décompositions de valeurs singulières.

OpenBLAS est souvent utilisée dans des applications scientifiques et de calcul numérique nécessitant des performances élevées, 
telles que l'apprentissage automatique, la modélisation mathématique, la simulation, etc. Elle peut être intégrée dans de nombreux langages de programmation, 
y compris C, C++, Fortran et Python, et est compatible avec une variété de systèmes d'exploitation.

Voici un exemple simple d'utilisation d'OpenBLAS en C pour effectuer une multiplication de matrices :

#include <stdio.h>
#include <stdlib.h>
#include <cblas.h>

#define N 3

int main() {
    // Déclaration des matrices
    double A[N*N] = {1.0, 2.0, 3.0,
                     4.0, 5.0, 6.0,
                     7.0, 8.0, 9.0};
    double B[N*N] = {9.0, 8.0, 7.0,
                     6.0, 5.0, 4.0,
                     3.0, 2.0, 1.0};
    double C[N*N] = {0.0};

    // Multiplication des matrices
    cblas_dgemm(CblasRowMajor, CblasNoTrans, CblasNoTrans, N, N, N, 1.0, A, N, B, N, 0.0, C, N);

    // Affichage du résultat
    printf("Résultat de la multiplication de matrices :\n");
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            printf("%f ", C[i*N + j]);
        }
        printf("\n");
    }

    return 0;
}

Dans cet exemple :

Nous définissons trois matrices A, B, et C de taille 3x3.
Nous utilisons la fonction **cblas_dgemm** d'OpenBLAS pour effectuer la multiplication des matrices A et B, stockant le résultat dans la matrice C.
Enfin, nous affichons le résultat de la multiplication de matrices.

gcc -o example example.c -lopenblas

