# HPC - Didacticiel d'utilisation de HDF5/C sur Taouey 

## Comment soumettre un job HDF5 avec SLURM
Afin de soumettre des tâches R à SLURM, nous aurons besoin principalement de deux code sources :

* (1) un code source HDF5/C qui décrit la solution à votre problème et,
* (2) un script SLURM qui spécifie les ressources nécessaires, définit l'environnement et les commandes à exécuter.

# Basiques HDF5
Supposons qu’on ait un exemple **hello_hdf5.c** qui crée un fichier HDF5, écrit un ensemble de données dans ce fichier, puis lit les données à partir du fichier :

```C
#include <stdio.h>
#include "hdf5.h"

#define FILENAME "hello.h5"
#define DATASETNAME "dataset"

int main() {
    hid_t file_id, dataset_id, dataspace_id;
    herr_t status;
    int data[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int data_read[10];

    // Création du fichier HDF5
    file_id = H5Fcreate(FILENAME, H5F_ACC_TRUNC, H5P_DEFAULT, H5P_DEFAULT);
    if (file_id < 0) {
        printf("Erreur lors de la création du fichier HDF5.\n");
        return -1;
    }

    // Création de l'espace de données
    dataspace_id = H5Screate(H5S_SCALAR);

    // Création de l'ensemble de données
    dataset_id = H5Dcreate2(file_id, DATASETNAME, H5T_NATIVE_INT, dataspace_id, H5P_DEFAULT, H5P_DEFAULT, H5P_DEFAULT);
    if (dataset_id < 0) {
        printf("Erreur lors de la création de l'ensemble de données.\n");
        return -1;
    }

    // Écriture des données dans l'ensemble de données
    status = H5Dwrite(dataset_id, H5T_NATIVE_INT, H5S_ALL, H5S_ALL, H5P_DEFAULT, data);
    if (status < 0) {
        printf("Erreur lors de l'écriture des données.\n");
        return -1;
    }

    // Fermeture de l'ensemble de données, de l'espace de données et du fichier
    status = H5Dclose(dataset_id);
    status = H5Sclose(dataspace_id);
    status = H5Fclose(file_id);

    // Ouverture du fichier HDF5 en lecture seule
    file_id = H5Fopen(FILENAME, H5F_ACC_RDONLY, H5P_DEFAULT);
    if (file_id < 0) {
        printf("Erreur lors de l'ouverture du fichier HDF5.\n");
        return -1;
    }

    // Ouverture de l'ensemble de données
    dataset_id = H5Dopen2(file_id, DATASETNAME, H5P_DEFAULT);
    if (dataset_id < 0) {
        printf("Erreur lors de l'ouverture de l'ensemble de données.\n");
        return -1;
    }

    // Lecture des données depuis l'ensemble de données
    status = H5Dread(dataset_id, H5T_NATIVE_INT, H5S_ALL, H5S_ALL, H5P_DEFAULT, data_read);
    if (status < 0) {
        printf("Erreur lors de la lecture des données.\n");
        return -1;
    }

    // Affichage des données lues
    printf("Données lues depuis le fichier HDF5 :\n");
    for (int i = 0; i < 10; i++) {
        printf("%d ", data_read[i]);
    }
    printf("\n");

    // Fermeture de l'ensemble de données et du fichier
    status = H5Dclose(dataset_id);
    status = H5Fclose(file_id);

    return 0;
}

```
Sauvegardez ce code dans un fichier nommé **hello_hdf5.c**.

## Écriture du script SLURM 
Maintenant, vous devez créer un script SLURM **hello_hdf5.sh** pour soumettre votre travail. Voici un exemple de script SLURM :

``` C
#!/bin/bash
#SBATCH --job-name=hello_world
#SBATCH --output=hello_world.out
#SBATCH --error=hello_world.err
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=1:00:00

# Chargement du module HDF5
module purge
module load hdf5/1.14.3/gcc-4.8.5

# Exécution du script HDF5
gcc -o hello_hdf5 hello_hdf5.c -lhdf5
```

## Exécution 
Pour exécuter le code R, soumettez simplement le travail à SLURM avec la commande suivante :

```C
$ sbatch ./hello.sh

```
# Suivi des job R sur SLURM
Afin de suivre l'état d'avancement de votre job une fois soumis, se reférer au didacticiel sur SLURM.  
cat("[Didacticiel sur SLURM](https://github.com/DiopBabacarEdu/TaoueY-HPC/tree/main/SLURM)")
