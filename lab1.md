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

## 3. Ajout au Staging Area et correction de la faute
Commandes :
```bash
git add file1.txt
git status
echo "My first Git Project" > file1.txt
git status
git diff file1.txt
```
Résultat :
```
(colle ici ce que le terminal affiche après git diff)
```
Explication : `git add` place le fichier dans la zone d'index (Staging Area). `git diff` montre les modifications faites mais pas encore indexées : ici la correction de « Prject » en « Project ».