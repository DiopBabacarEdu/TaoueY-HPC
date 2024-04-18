# HPC - Didacticiel d'utilisation de Python sur Taouey

## Soumission de tâches Python via SLURM

Afin de soumettre des tâches Python à SLURM, nous aurons besoin de deux code sources : 
* (1) un **script Python** écrit qui décrit la solution à votre problème et,
* (2) un **script SLURM** qui spécifie les ressources nécessaires, définit l'environnement et les commandes à exécuter.
Dans cet exemple, nous utilisons la version **Python-3.9.7**.

## Hello World sur Python
Un job Python est une tâche qui peut nécessiter un ou plusieurs cœurs de CPU. Voici un exemple de script Python (hello_world.c) :

```Python
def hello_world():
    print("Hello, world!")

# Appel de la fonction
hello_world()
```

Le script Slurm ci-dessous peut être utilisé pour soumettre un job Python :

```Python
#!/bin/bash
#SBATCH --job-name=hello_world_  # Donner un nom à votre job
#SBATCH --nodes=1                # ID du noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=1        # Cpu-cores par tâche 
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:01:00          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --mail-type=all          # Notifications en cas de démarrage, d'arrêt et d'erreur

# Chargement du module Python
module purge
module load Python/3.9.7/Python-3.9.7

# Exécution du programme
python hello.py
```

Pour exécuter ce script, soumettez simplement le travail à SLURM avec la commande suivante :
```Python
$ sbatch ./hello_world.sh
```
## Suivi d'un job Python 
Une fois la tâche soumise, il est nécessaire de pouvoir suivre l'évolution de celle-ci au fur et à mesure qu'elle s'éxécute sur Taouey.
Voici la commande pour afficher le statut d'une tâche :
```Python
squeue -u $USER
```
## Gérer les versions de Python aux travers des modules
Nous avons utilisé dans notre exemple la version 3.9.7 de Python. Cependant, au cas où il serait nécessaire de se calquer sur d'autres versions, 
taper la commande qui permet de lister les autres versions disponibles.

```
$ module avail python
------------ /software/modulefiles ----------------------------
Python/3.9.7/Python-3.9.7
------------ /etc/modulefiles ---------------------------------

```
Ensuite, charger le module correspondant en tapant la commande suivante
```
module load Python/3.9.7/Python-3.9.7
```


## Un second exemple

Voici un exemple de code Python pour calculer la somme des éléments d'un tableau :

```python
# -*- coding: utf-8 -*-
import random
def generer_tableau(n):
    """
    Génère un tableau de taille n avec des valeurs aléatoires.
    """
    tableau = [random.randint(1, 100) for _ in range(n)]
    return tableau

def afficher_tableau(tableau):
    """
    Affiche le tableau.
    """
    print("Tableau:")
    for element in tableau:
        print(element)

if __name__ == "__main__":
    # Spécifiez la taille du tableau
    taille_tableau = 100

    # Générer le tableau
    tableau = generer_tableau(taille_tableau)

  # Afficher le tableau
    afficher_tableau(tableau)
```
Voici le script SLURM ci-dessous pour soumettre cette tâche :

```python
#!/bin/bash
#SBATCH --job-name=python_job    # Donner un nom à votre job
#SBATCH --nodes=1                # ID du noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=1        # Cpu-cores par tâche 
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:01:00          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --mail-type=all          # Notifications en cas de démarrage, d'arrêt et d'erreur

# Chargement du module Python
module purge
module load Python/3.9.7/Python-3.9.7

# Exécution du programme
python python_job.py
```

Pour obtenir le nom du nœud de calcul où s'exécute votre travail, utilisez la commande suivante :

```
$ squeue -u $USER
```
La colonne la plus à droite intitulée ```"NODELIST(REASON)"``` donne le nom du nœud où s'exécute votre travail. Connectez-vous via SSH à ce nœud, par exemple :

```
$ ssh pm1-nod010
```
Une fois sur le nœud de calcul, exécutez ```htop -u $USER```. 
Si votre travail s'exécute en parallèle, vous devriez voir un processus utilisant bien plus de 100 % dans la colonne %CPU. 




