# HPC - MATLAB sur GPU
## Didacticiel d'utilisation de Taouey sur MATLAB

Afin de soumettre des tâches MATLAB à SLURM, nous aurons besoin de deux code sources : 
* (1) un **script MATLAB** qui décrit la solution à votre problème et,
* (2) un **script SLURM** qui spécifie les ressources nécessaires, définit l'environnement et les commandes à exécuter.


De nombreuses routines MATLAB ont été écrites pour s'exécuter sur un GPU. 
Ci-dessous se trouve un script MATLAB (svd_matlab.m) qui effectue une décomposition de matrice en utilisant un GPU :

```
gpu = gpuDevice();
fprintf('Using a %s GPU.\n', gpu.Name);
disp(gpuDevice);

X = gpuArray([1 0 2; -1 5 0; 0 3 -9]);
whos X;
[U,S,V] = svd(X)
fprintf('trace(S): %f\n', trace(S))
quit;
```
Le script SLURM suivant peut être utlisé pour déployer ce job :

```
#!/bin/bash
#SBATCH --job-name=matlab-svd    # Donner un nom à votre job
#SBATCH --nodes=1                # ID Noeud
#SBATCH --ntasks=1               # Allouer le nombre total de tâches
#SBATCH --cpus-per-task=1        # Cpu-cores par tâche 
#SBATCH --mem-per-cpu=4G         # Mémoire allouée par cpu-core 
#SBATCH --time=00:01:00          # Limite de temps d'exéctution (HH:MM:SS)
#SBATCH --gres=gpu:1             ######################### Nombre de GPUs par noeud 
#SBATCH --mail-type=begin        # Envoi de notification au démarrage
#SBATCH --mail-type=end          # Envoi de notification à l'arrêt
#SBATCH --mail-user=your-email@entreprise.com

module purge
module load matlab/R2024a

matlab -singleCompThread -nodisplay -nosplash -r svd_matlab
```
