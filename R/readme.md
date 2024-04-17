# HPC - Comment soumettre un job R avec SLURM

## Didacticiel d'utilisation de R sur Taouey 
Afin de soumettre des tâches R à SLURM, nous aurons besoin principalement de deux code sources :

* (1) un script R qui décrit la solution à votre problème et,
* (2) un script SLURM qui spécifie les ressources nécessaires, définit l'environnement et les commandes à exécuter.

# "Hello world" 
Supposons qu’on ait un exemple hello.R simple pour commencer :

``` R
# Afficher "Hello, world!" dans la console
print("Hello, world!")
```
Sauvegardez ce code dans un fichier nommé **hello.R**.

## Écriture du script SLURM 
Maintenant, vous devez créer un script SLURM **hello.sh** pour soumettre votre travail. Voici un exemple de script SLURM :

``` R
#!/bin/bash
#SBATCH --job-name=hello_world
#SBATCH --output=hello_world.out
#SBATCH --error=hello_world.err
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=1:00:00

# Chargement du module R
module purge
module load R/4.3.3/R-4.3.3

# Exécution du script R
Rscript hello.R
```

Pour exécuter le code R, soumettez simplement le travail à SLURM avec la commande suivante :
```
$ sbatch ./hello.sh

```

# Un second exemple 
## Basiques sur la manipulation de données sur R
``` R
# Création d'un tableau de données
data <- data.frame(
  Name = c("Cheikh", "Babacar", "Maodo", "Adama"),
  Age = c(25, 30, 35, 40),
  Score = c(85, 90, 95, 100)
)

# Affichage du tableau
print(data)

# Calcul de la moyenne des scores
mean_score <- mean(data$Score)
cat("Moyenne des scores :", mean_score, "\n")

# Filtrer les lignes avec l'âge supérieur à 30
donnees_filtrees <- subset(data, Age  > 30)

# Afficher les lignes filtrées
print(donnees_filtrees)
mean_score1 <- mean(donnees_filtrees$Score)

cat("Moyenne des scores1 :", mean_score1, "\n")
```
## Script SLURM
Maintenant, vous devez créer un script SLURM **table_creation.sh** pour soumettre votre travail. Voici un exemple de script SLURM :

``` R
#!/bin/bash
#SBATCH --job-name=table_creation
#SBATCH --output=table_creation.out
#SBATCH --error=table_creation.err
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=1:00:00

# Chargement du module R
module purge
module load R/4.3.3/R-4.3.3

# Exécution du script R
Rscript table_creation.R
```

## Exécution 
```
$ sbatch ./hello.sh

```
# Suivi des job R sur SLURM
Afin de suivre l'état d'avancement de votre job une fois soumis, se reférer au didacticiel sur SLURM.  
cat("[Didacticiel sur SLURM](https://github.com/DiopBabacarEdu/TaoueY-HPC/tree/main/SLURM)")
