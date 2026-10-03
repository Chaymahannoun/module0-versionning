# Lab 1 : Git intro

## 1. Installation et configuration

```bash
git --version
git config --global user.name "chayma"
git config --global user.email "hannounchayma@gmail.com"
```

Git est installé (version 2.55.0). Le nom et l'e-mail sont associés à chaque commit.

## 2. Création du dépôt

```bash
mkdir -p pratique/FirstProject
cd pratique/FirstProject
git init
```

`git init` crée un dépôt vide avec un dossier caché `.git`.

## 3. Création des fichiers

```bash
touch file1.txt file2.txt
echo "My first Git Prject" > file1.txt
echo "Hello World" > file2.txt
git status
```

`git status` affiche `file1.txt` et `file2.txt` dans « Untracked files » : Git ne les suit pas encore.

## 4. Staging Area et correction de la faute

```bash
git add file1.txt
echo "My first Git Project" > file1.txt
git diff file1.txt
```

Résultat de `git diff` :
```diff
-My first Git Prject
+My first Git Project
```

`git add` place le fichier dans la zone d'index. `git diff` montre la correction faite après l'ajout.

## 5. Fichier privé et .gitignore

```bash
touch private.txt
git add -A
git rm --cached private.txt
echo private.txt >> .gitignore
git add -A
```

`git add -A` a ajouté `private.txt` par erreur. `git rm --cached` le retire de l'index sans le supprimer, et `.gitignore` l'ignore pour la suite.

## 6. Annuler une modification

```bash
echo '' > file1.txt
git checkout file1.txt
cat file1.txt
```

Résultat :
```
My first Git Project
```

`git checkout` restaure la version qui était dans la zone d'index.

## 7. Premier commit

```bash
git commit -m "First commit"
git status
```

Résultat :
```
nothing to commit, working tree clean
```

Le commit valide `.gitignore`, `file1.txt` et `file2.txt`.

## 8. Modifier le message du dernier commit

```bash
echo 'Ligne erreur' >> file2.txt
git add file2.txt
git commit -m "Ajout ligne erreur"
git commit --amend -m "j'ai ajouté une ligne erreur"
```

`--amend` remplace le dernier commit par un nouveau avec un message modifié.

## 9. Annuler un commit

```bash
sed -i 's/Ligne erreur/Ligne corrigée/' file2.txt
git reset --soft HEAD~1
git add file2.txt
git commit -m "Ajout ligne corrigée"
git log --oneline
```

`git reset --soft HEAD~1` annule le dernier commit mais garde les modifications. Je corrige puis je refais un commit propre.

## 10. Cloner un dépôt

```bash
cd ..
git clone https://git.savannah.gnu.org/git/hello.git Gnu-Hello
```

`git clone` récupère tout l'historique et le code d'un projet existant.

