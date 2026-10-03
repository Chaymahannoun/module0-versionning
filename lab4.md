# Lab 4 : Installation et exploitation de Git et Forgejo

## 1. Installation et configuration de Git (VM Ubuntu)

```bash
sudo apt update
sudo apt install git -y
git --version
git config --global user.name "chayma"
git config --global user.email "hannounchayma@gmail.com"
git config --global init.defaultBranch main
```

Git est installé sur la VM. Le nom et l'e-mail sont associés à chaque commit.

## 2. Dépôt local et trois commits

```bash
mkdir ProjectForgejo
cd ProjectForgejo
git init
touch file1.txt file2.txt
git add .
git commit -m "Initialisation du projet"
echo "ligne 1" >> file1.txt
git commit -am "Commit 2"
echo "ligne 2" >> file2.txt
git commit -am "Commit 3"
git log --oneline
```

Résultat :
```
4a40d1e (HEAD -> main) Commit 3
fee4ae0 Commit 2
9c46c9c Initialisation du projet
```

Le projet contient trois commits, comme demandé.

## 3. Préparation de la VM

```bash
hostname -I
nproc
free -h
ping -c 2 google.com
```

La VM Ubuntu a 2 processeurs, environ 3 Go de mémoire et l'adresse IP `10.0.2.15`. Le `ping` vers `google.com` réussit sans perte : la VM a accès à Internet.

## 4. Installation de Docker

```bash
sudo apt install docker.io -y
sudo systemctl enable --now docker
```

Docker est installé et démarre automatiquement avec la VM.

## 5. Déploiement de Forgejo dans un conteneur

```bash
sudo docker volume create forgejo_data
sudo docker run -d --name forgejo -p 3000:3000 -p 2222:22 -v forgejo_data:/data codeberg.org/forgejo/forgejo:11
sudo docker ps
```

Résultat :
```
CONTAINER ID   IMAGE                             STATUS         PORTS                                          NAMES
54b785b680b7   codeberg.org/forgejo/forgejo:11   Up 4 minutes   0.0.0.0:3000->3000/tcp, 0.0.0.0:2222->22/tcp   forgejo
```

Le volume `forgejo_data` conserve les données de Forgejo. Le port 3000 sert à l'interface web et le port 2222 à Git par SSH.

Remarque : le sujet utilise l'image `forgejo:latest`, mais ce tag n'existe pas car Forgejo publie ses images par numéro de version. J'ai donc utilisé la version `11`.

## 6. Accès à Forgejo

J'ouvre `http://localhost:3000` dans le navigateur de la VM (la VM est en réseau NAT, son adresse `10.0.2.15` n'est pas accessible depuis Windows). Dans l'assistant d'installation, je garde les valeurs par défaut (base SQLite3) et je crée le compte administrateur `chayma`.

## 7. Création du dépôt Forgejo

Depuis l'interface web, je crée le dépôt vide `tp-forgejo` (menu « New Repository »), sans initialiser de README.

## 8. Liaison et publication

```bash
cd ~/ProjectForgejo
git remote add origin http://localhost:3000/chayma/tp-forgejo.git
git remote -v
git push -u origin main
```

`git remote add` relie le projet local au dépôt Forgejo. `git push -u` envoie la branche `main` et la mémorise comme destination par défaut. La page du dépôt affiche ensuite 3 commits et les fichiers `file1.txt` et `file2.txt`.

## 9. Clonage et synchronisation

```bash
cd ~
git clone http://localhost:3000/chayma/tp-forgejo.git clone-test
cd clone-test
echo "ligne 3" >> file1.txt
git commit -am "Commit 4 depuis le clone"
git push
cd ~/ProjectForgejo
git pull
git log --oneline
```

`git clone` crée une copie complète du dépôt dans `clone-test`. J'y ajoute un commit et je l'envoie avec `git push`. De retour dans le projet d'origine, `git pull` récupère ce commit. Le dépôt Forgejo affiche alors 4 commits, le dernier étant « Commit 4 depuis le clone ».

## Conclusion

J'ai installé Git sur une VM Ubuntu, déployé Forgejo dans un conteneur Docker, créé un dépôt, réalisé plusieurs commits, puis publié, cloné et synchronisé le projet.