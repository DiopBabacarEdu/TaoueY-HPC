# SLURM et commandes de base

|              |      |
| ----------- | ----------- |
| **Définition**      | SLURM (Simple Linux Utility for Resource Management) est un gestionnaire de ressources et de planification de tâches essentiel dans les environnements de calcul distribué et de haute performance (HPC).        |
| **Rôle**  | Son rôle principal consiste à allouer dynamiquement les ressources disponibles aux travaux en fonction de leurs besoins spécifiés, à planifier efficacement l'exécution des tâches en tenant compte des priorités et des contraintes, à surveiller et à gérer les travaux en cours, ainsi qu'à organiser les files d'attente et les partitions pour une utilisation optimale des ressources.        |
| **Planification**   | En fournissant des fonctionnalités avancées de gestion des ressources et de planification, SLURM permet aux utilisateurs de maximiser l'efficacité et la performance de leurs calculs tout en garantissant une utilisation équitable des ressources disponibles.        |
```
```

# Exemple de script SLUM
Ci-dessous nous présentons un exemple de script SLURM, permettant de soumettre une job MPI.
```Matlab
#!/bin/bash
#SBATCH --job-name=slurm_job     # Donner un nom à votre job
#SBATCH --nodes=1                # ID du noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=1        # Cpu-cores par tâche (supérieur à 1 si multi-threading)
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:01:00          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --mail-type=all          # Notifications en cas de démarrage, d'arrêt et d'erreur

module purge
module load mpi/openmpi/3.1.0/gcc-7.2.0 
mpicc mon_programme.c -o mon_programme
mpiexec -n 4 ./mon_programme_mpi
```
## Explications des directives SLURM utilisées dans ce script :
```
--job-name=my_mpi_job      : Nom du job
--nodes=4                  : Nombre de nœuds requis
--ntasks-per-node=1        : Nombre de tâches MPI par nœud
--cpus-per-task=4          : Nombre de cœurs CPU par tâche MPI
--time=00:10:00            : Durée maximale d'exécution du travail (10 minutes dans cet exemple)
--partition=your_partition : Partition sur laquelle exécuter le travail
--output=output_%j.txt     : Nom du fichier de sortie
--error=error_%j.txt       : Nom du fichier d'erreur
```
Assurez-vous d'ajuster les valeurs des directives en fonction de votre configuration et des besoins de votre programme MPI. 
Vous devrez également remplacer ./mon_programme_mpi par le chemin vers votre programme MPI. 
Enfin, assurez-vous d'avoir chargé le module MPI approprié si nécessaire .

# Comment soumettre votre job via SLURM ?
Pour soumettre une tâche à partir de SLURM, vous pouvez utiliser la commande ```sbatch``` suivie du nom de votre script Slurm. Assurez-vous que votre script Slurm contient toutes les directives nécessaires, telles que le nombre de cœurs CPU, la mémoire, le temps d'exécution, etc.

Voici un exemple de commande pour soumettre une tâche à SLURM :

```
sbatch job.slurm
```

Assurez-vous d'être dans le répertoire où se trouve votre script Slurm (job.slurm dans cet exemple) lorsque vous exécutez la commande sbatch. 
SLURM planifiera alors votre tâche et l'exécutera dès que les ressources demandées seront disponibles.

Vous pouvez également utiliser des options supplémentaires avec sbatch pour spécifier des paramètres supplémentaires de la tâche, par exemple :

```
sbatch -p partition --cpus-per-task=4 job.slurm
```

Cela spécifierait la partition sur laquelle exécuter la tâche (-p partition) ainsi que le nombre de cœurs CPU à utiliser (--cpus-per-task=4).

Assurez-vous de consulter la documentation de SLURM et les directives de votre système spécifique en cas de besoin de plus d'informations sur la soumission de tâches.


# Comment suivre votre tâche lancée sur SLURM ?
Une fois la tâche soumise, il est nécessaire de pouvoir suivre l'évolution de celle-ci au fur et à mesure que le job s'éxécute sur Taouey. Voici la commande pour afficher le statut d'une tâche :

```
squeue -u $USER
```

# Autres commandes SLURM usuelles

## Annulation de job
```
scancel
```  
Cette commande est utilisée pour annuler un travail en cours d'exécution. Elle prend en argument l'identifiant du travail à annuler.

## Infos détaillées de job
```
scontrol show job <job_id>
```  
Cette commande permet d'afficher des informations détaillées sur un travail spécifique, y compris les ressources allouées, les nœuds utilisés, les variables d'environnement, etc.

## Infos détaillées sur les partitions
```
sinfo
```  
Cette commande affiche des informations sur les partitions du cluster, y compris leur disponibilité, la capacité, les nœuds, etc.

## Infos sur les travaux antérieurs
```
sacct
```  
Cette commande permet d'afficher des informations sur les travaux antérieurs, y compris l'état de l'exécution, les ressources utilisées, le temps d'exécution, etc. L'option -u <utilisateur> permet de filtrer les travaux d'un utilisateur spécifique.
