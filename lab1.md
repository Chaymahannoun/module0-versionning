# Lab 1 : Git init

## 1. Création du dépôt
Commandes :
```bash
mkdir -p pratique/FirstProject
cd pratique/FirstProject
git init
```
Explication : `git init` crée un dépôt Git et un dossier caché `.git`.

## 2. Création des fichiers
Commandes :
```bash
touch file1.txt file2.txt
echo "My first Git Prject" > file1.txt
echo "Hello World" > file2.txt
git status
```
Explication : les fichiers sont « untracked », Git ne les suit pas encore.