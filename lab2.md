# Lab 2 : Travailler avec des branches

## 1. Création du dépôt du site web

```bash
mkdir -p website/site-prod
cd website
git init
echo '<h1>Hello World!</h1>' > site-prod/index.html
git add -A
git commit -m "The beginnings of my web site."
git config --global alias.lgone "log --oneline --decorate"
```

`site-prod` sert de site actif (branche `main`) et `site-dev` aux nouveautés en développement. L'alias `lgone` affiche les logs sur une seule ligne.

## 2. Branche new-features

```bash
git branch new-features
git checkout new-features
mkdir -p site-dev
echo 'Contact : chayma@gmail.com' > site-dev/contact.html
git add -A
git commit -m "Added contact.html file"
echo 'Help : God bless you' > site-dev/help.html
git add -A
git commit -m "Added help.html file"
git lgone
git checkout main
git lgone
```

Résultat sur `new-features` :
```
a81a215 (HEAD -> new-features) Added help.html file
82c5403 Added contact.html file
9fdc501 (main) The beginnings of my web site.
```

Résultat après le retour sur `main` :
```
9fdc501 (HEAD -> main) The beginnings of my web site.
```

`git branch` crée une branche et `git checkout` bascule dessus. Au retour sur `main`, les deux commits de `new-features` n'y sont pas : chaque branche garde son propre travail.

## 3. Branche php-features

```bash
git checkout -b php-features
mkdir -p site-dev
```

Je crée `site-dev/index.php` avec une page PHP « Hello World », puis :

```bash
git add -A
git commit -m "Added index.php"
git checkout main
```

`git checkout -b` crée la branche et bascule dessus en une seule commande.

## 4. Modifications sur main

Je crée `README.md` (« My First HTML Site ») et je remplace `site-prod/index.html` par un squelette HTML complet :

```bash
git add -A
git commit -m "Added html skeleton to index.html and README.md"
```

## 5. Fusion de new-features

```bash
git merge new-features -m "Merge branch new-features Adding Contact and Help files"
git lgone --graph
git branch -d new-features
```

Résultat :
```
*   f7c886f (HEAD -> main) Merge branch new-features Adding Contact and Help files
|\
| * a81a215 (new-features) Added help.html file
| * 82c5403 Added contact.html file
* | c2cee20 Added html skeleton to index.html and README.md
|/
* 9fdc501 The beginnings of my web site.
```

`git merge` intègre les commits de `new-features` dans `main`. La branche ne sert plus, donc je la supprime avec `git branch -d`.

## 6. Tag d'une version

```bash
git tag v0.1
```

Un tag donne un nom fixe à un commit précis : ici `v0.1` désigne la fusion de `new-features`.

## 7. Conflit de fusion

Sur `php-features`, je crée aussi un `README.md`, alors qu'il existe déjà sur `main` avec un autre contenu :

```bash
git checkout php-features
echo "My First PHP Site" > README.md
git add -A
git commit -m "Added README.md file"
git checkout main
git merge php-features
```

Résultat :
```
Auto-merging README.md
CONFLICT (add/add): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Git ne sait pas quel contenu garder. Il écrit des marqueurs dans le fichier :
```
<<<<<<< HEAD
My First HTML Site
=======
My First PHP Site
>>>>>>> php-features
```

Je résous le conflit à la main en gardant un seul texte :

```bash
echo "My First HTML-PHP Site" > README.md
git add README.md
git commit -m "Merge branch php-features"
git lgone --graph --all
```

Résultat :
```
*   c929827 (HEAD -> main) Merge branch php-features
|\
| * a3a5e3a (php-features) Added README.md file
| * fda1009 Added index.php
* |   f7c886f (tag: v0.1) Merge branch new-features Adding Contact and Help files
|\ \
| * | a81a215 Added help.html file
| * | 82c5403 Added contact.html file
| |/
* / c2cee20 Added html skeleton to index.html and README.md
|/
* 9fdc501 The beginnings of my web site.
```

`git add` marque le conflit comme résolu et `git commit` termine la fusion.
