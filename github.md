Résumé des commandes : http://johnelder.org/code

### Initier un nouveau répertoire
1. ```git init``` initialise un nouveau répertoire

### Cloner dans GIT
1. ```mkdir -p ~/dossier``` créer un dossier si nécessaire
2. ```cd /dossier``` rentrer dans le dossier
3. fork sur github
4. ```git clone URL``` (URL que l'on trouve sur la fiche projet Github)
5. ```cd /dossierDuProjetClone```
6. ```clone .``` pour ouvrir dans VSC

### Sauvegarder dans GIT
1. ```git add .``` ajoute tous les fichiers 
2. ```git commit -am "Description du commit"``` regroupe les éléments
3. ```git status``` pour vérifier le dernier status
4. ```git push -u origin master```

### Action nécessaire avant
1. aller sur github / settings / SSH and GPG keys / **New SSH keys**
2. taper dans console mac : **cat ~/.ssh/id_rsa.pub**
3. copier l’ensemble du code généré et le coller dans New SSH keys
4. créer un Répertoire manuellement sur Github
5. suivre les instructions du bloc « …or push an existing repository from the command line » (2 lignes à taper)

### Blacklister un fichier ou un dossier 
```$ touch gitignore```

### Relier un repo à github
```
$ git remote add origin https://github.com/user/repo.git
# Set a new remote

$ git remote -v
# Verify new remote
> origin  https://github.com/user/repo.git (fetch)
> origin  https://github.com/user/repo.git (push)
```

### Ne pas synchroniser sur git (mais garder en local)
```$ git rm -r --cached directory/```