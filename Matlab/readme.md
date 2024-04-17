# HPC - Didacticiel d'utilisation de MATLAB sur Taouey

## Soumission de tâches MATLAB via SLURM

Afin de soumettre des tâches MATLAB à SLURM, nous aurons besoin de deux code sources : 
* (1) un **script MATLAB** qui décrit la solution à votre problème et,
* (2) un **script SLURM** qui spécifie les ressources nécessaires, définit l'environnement et les commandes à exécuter.

## Exécution d'une tâche séquentielle avec MATLAB
La plupart du temps, l'exécution de MATLAB en mode single-thread peut répondre à vos besoins. 
Un job MATLAB séquentiel est une tâche qui nécessite uniquement un seul cœur de CPU. 
Voici un exemple d'un script MATLAB séquentiel composé d'une ligne (hello_world.m) :

```Matlab
fprintf('Bonjour le monde.\n')
```
Le script Slurm (job.slurm) ci-dessous peut être utilisé pour des travaux série :

```Matlab
#!/bin/bash
#SBATCH --job-name=matlab        # Donner un nom à votre job
#SBATCH --nodes=1                # ID du noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=1        # Cpu-cores par tâche (supérieur à 1 si multi-threading)
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:01:00          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --mail-type=all          # Notifications en cas de démarrage, d'arrêt et d'erreur

module purge
module load matlab/R2024a

matlab -singleCompThread -nodisplay -nosplash -r hello_world
```


Afin de supprimer l'interface graphique et le multithrading, nous invoquons MATLAB avec les options suivantes :

```Matlab
-singleCompThread   
-nodisplay
-nosplash
```
Pour exécuter le script MATLAB, soumettez simplement le travail à SLURM avec la commande suivante :
```Matlab
$ sbatch job.slurm
```
## Suivi d'un job MATLAB sur SLURM
Une fois la tâche soumise, il est nécessaire de pouvoir suivre l'évolution de celle-ci au fur et à mesure qu'elle s'éxécute sur Taouey.
Voici la commande pour afficher le statut d'une tâche :
```Matlab
squeue -u $USER
```
## Gérer les versions aux travers des modules
Nous avons supposé utilisée la version 2024 de Matlab. Cependant, au cas où il serait nécessaire de se calquer sur d'autres versions plus anciennes, 
taper la commande qui permet de lister les autres versions disponibles.

```
$ module avail matlab
------------ /usr/licensed/Modules/modulefiles ------------
matlab/R2022a          matlab/R2023a          matlab/R2024a
matlab/R2022b          matlab/R2023b          
```
Ensuite, charger le module correspondant en tapant la commande suivante
```
module load matlab/R2023b.
```


## Exécuter un travail MATLAB multi-thread avec Parallel Toolbox

Afin de paralléliser un job sur Matlab, il est possible de faire appel au « Parallel Toolbox » de Matlab (avec **parfor**), ou à l’implémentation BLAS de MATLAB afin de tirer partie du multi-threading.
Si votre version Matlab installée supporte le multi-threading, il est possible d'de mobiliser l'ensembles des cœurs CPU sur un seul nœud dans ce mode. 
Voici un exemple de MathWorks utilisant plusieurs cœurs (for_loop.m) :

```
poolobj = parpool;
fprintf('Number of workers: %g\n', poolobj.NumWorkers);

tic
n = 200;
A = 500;
a = zeros(n);
parfor i = 1:n
    a(i) = max(abs(eig(rand(A))));
end
toc
```
Voici le script SLURM ci-dessous pour démarrer cette tâche :
```Matlab
#!/bin/bash
#SBATCH --job-name=parfor        # Donner un nom à votre job
#SBATCH --nodes=1                # ID du noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=4        # Cpu-cores par tâche (supérieur à 1 car multi-threading)
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:00:30          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --mail-type=all          # Notifications en cas de démarrage, d'arrêt et d'erreur

module purge
module load matlab/R2024a

matlab -nodisplay -nosplash -r for_loop

```

Une directive **parfor** est un indicateur clair d'un code MATLAB parallélisé. Dans certains cas, MATLAB utilise la bibliothèque BLAS qui propose des routines multithreadées.

Deux méthodes courantes pour déterminer si un code MATLAB peut tirer parti du parallélisme sans rien savoir sur le code. 
* Première méthode: exécuter dans un prelmier temps le code en utilisant 1 cœur CPU, ensuite effectuer une deuxième exécution en utilisant par exemple 4 cœurs CPU.
Observer la différence dans les temps d'exécution des deux codes.
* Deuxième méthode: lancer d'abord le travail en utilisant par exemple 4 cœurs CPU, puis se connecter via SSH au nœud de calcul où le travail est en cours d'exécution, ensuite utiliser ```htop -u $USER``` pour inspecter l'utilisation du CPU.


Pour obtenir le nom du nœud de calcul où s'exécute votre travail, utilisez la commande suivante :

```
$ squeue -u $USER
```
La colonne la plus à droite intitulée ```"NODELIST(REASON)"``` donne le nom du nœud où s'exécute votre travail. Connectez-vous via SSH à ce nœud, par exemple :

```
$ ssh della-r3c1n14
```
Une fois sur le nœud de calcul, exécutez ```htop -u $USER```. 
Si votre travail s'exécute en parallèle, vous devriez voir un processus utilisant bien plus de 100 % dans la colonne %CPU. 
Pour 4 cœurs CPU, ce nombre idéal serait de 400 %.
















