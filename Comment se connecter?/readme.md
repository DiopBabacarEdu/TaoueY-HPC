# HPC - Connexion et transfert de fichiers
## Création de compte utilisateur
* Dans un premier temps,assurez-vous d'avoir un compte utilisateur sur le système. Sinon contactez l'administrateur du système (support@cineri.sn).
* Assurez-vous également d'avoir une connexion Internet stable.

## Se connecter
* Ouvrir un terminal sur votre ordinateur.
* Pour se connecter, on peut utiliser la clé SSH (Secure Shell), la commande ressemble à ceci :
```
ssh nomUtilisateur@taouey.cineri.sn
```
Remplacer le **nomUtilisateur** par votre nom d'utilisateur.
Entrer votre mot de passe lorsque vous y êtes invités.
Une fois les paramètres de connexion validés, vous obtenez normalement ce qui est présenté sur cette  :

<img width="887" alt="image" src="https://github.com/DiopBabacarEdu/TaoueY-HPC/assets/20286290/cde5ed07-04df-4df7-9411-e44e0c4b2515">


## Transfert de fichiers
Une fois entré le mot de passe, il peut être nécessaire d'effectuer un transfert de fichier depuis votre hote local.
On peut faire le transfert de fichiers sources depuis une machine hote vers une machine source en utlisant les commandes :
scp et sftp.

```
scp cheminlocal/mon-code /home/nomUtilisateur
```
 ou
 ```
sftp cheminlocal/mon-code /home/nomUtilisateur
```
Assurez-vous que le chemin d'accès existe sur votre ordinateur et sur le supercalculateur.

