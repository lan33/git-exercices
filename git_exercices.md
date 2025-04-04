# Exercices sur git

## Quelques défnitions et concepts
- **Contrôle de version** : Système qui enregistre les modifications d'un fichier ou ensemble de fichiers au fil du temps
- **Git** : Système de contrôle de version distribué créé par Linus Torvalds en 2005
- **Repository (dépôt)** : Collection complète des fichiers et de leur historique de versions
- **Commit** : Instantané des fichiers à un moment donné, identifié par un SHA-1
- **SHA-1** : Identifiant unique de 40 caractères pour chaque commit
- **HEAD** : Pointeur sur le dernier commit de la branche courante
- **Branche** : Ligne de développement indépendante, simple pointeur vers un commit
- **Répertoire .git** : Stocke les métadonnées et la base de données des objets du projet
- **Répertoire de travail** : Extraction d'une version du projet sur le disque
- **L'index (staging area)** : Zone de préparation pour le prochain commit
- **Dépôt distant (remote)** : Version du projet hébergée ailleurs (GitHub, GitLab, etc.)
- **Origin** : Nom conventionnel du dépôt distant principal
- **Clone** : Copie complète d'un dépôt distant
- **Fetch** : Récupération des modifications sans fusion
- **Pull** : Récupération et fusion des modifications (fetch + merge)
- **Push** : Envoi des modifications locales vers le dépôt distant

## Revenir à un état antérieur : avec `git checkout`

1. **Créer un nouveau répertoire et initialiser un dépôt Git :**
   ```bash
   mkdir git_exercices
   cd git_exercices
   git init
   ```

2. **Créer une nouvelle branche :**
   ```bash
   git switch -c exo-checkout
   ```

3. **Créer un premier fichier et effectuer le premier commit :**
   ```bash
   echo "Première version du fichier" > fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la première version du fichier"
   ```

4. **Modifier le fichier et effectuer un deuxième commit :**
   ```bash
   echo "Deuxième version du fichier" >> fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la deuxième version du fichier"
   ```

5. **Modifier à nouveau le fichier et effectuer un troisième commit :**
   ```bash
   echo "Troisième version du fichier" >> fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la troisième version du fichier"
   ```

6. **Vérifier l’historique des commits :**
   ```bash
   git log --oneline
   ```
   
   Vous devriez voir quelque chose comme :
   ```
   abc1234 (HEAD -> exo-checkout) Ajout de la troisième version du fichier
   def5678 Ajout de la deuxième version du fichier
   ghi9012 Ajout de la première version du fichier
   ```

7. **Utiliser `git checkout` pour revenir temporairement à un commit précédent (par exemple, le deuxième commit):**
   ```bash
   git checkout <SHA-du-commit>

   # Revenir au deuxième commit
   git checkout def5
   ```

8. **Vérifier l’état du répertoire de travail pour observer les modifications, vérifier l’historique des commits :**
   ```bash
   # La commande cat dans Bash permet d'afficher le contenu du fichier
   cat fichier.txt

   git log --oneline
   ```

9. **Utiliser `git checkout` pour revenir au premier commit et vérifier :**
   ```bash
   # Revenir au premier commit
   git checkout ghi9

   cat fichier.txt

   git log --oneline
   ```

10. **Revenir à l'état actuel de la branche :**
    ```bash
    git checkout exo-checkout
    ```

11. **Pour aller plus loin : revenir au premier commit, créer une branche pour continuer le travail à partir du premier commit, vérifier l’historique des commits de toutes les branches :**
    ```bash
    # Revenir au premier commit
    git checkout ghi9

    git switch -c depuis-premier-commit

    git log --oneline --all
    ```

12. **Pour aller plus loin : revenir au premier commit, retrouver le deuxième commit et y retourner :**
    ```bash
    # Revenir sur la branche exo-checkout
    git checkout exo-checkout

    # Revenir au premier commit
    git checkout ghi9

    # Afficher l'historique des modifications sur les références (comme HEAD ou les branches) :
    git reflog

    # Trouver l'identifiant du deuxième commit dans les reflogs et revenir au deuxième commit :
    git checkout def5

    # Vérifier
    git log --oneline

    # Revenir à l'état actuel de la branche
    git checkout exo-checkout
    ```

## Revenir à un état antérieur : avec `git reset`

**Reset** : Déplacement du HEAD et modification potentielle de l'index/répertoire de travail :
  - `--soft` : Déplace HEAD mais préserve l'index et le répertoire de travail
  - `--mixed` (par défaut) : Déplace HEAD et réinitialise l'index mais préserve le répertoire de travail
  - `--hard` : Déplace HEAD et réinitialise l'index et le répertoire de travail

```bash
# Création d'une branche exo-reset et de plusieurs commits sur cette branche
git checkout -b exo-reset
echo "Commit 1" > test1.txt
git add exp1.txt
git commit -m "Test 1"

echo "Commit 2" > test2.txt
git add exp2.txt
git commit -m "Test 2"

echo "Commit 3" > test3.txt
git add exp3.txt
git commit -m "Test 3"

# Affichage de l'historique
git log --oneline
```

```bash
# 1. Soft reset: Déplace HEAD mais préserve l'index et le répertoire de travail

# Déplace HEAD un pas en arrière
git reset --soft HEAD~1
# ou
git reset --soft <SHA-du-dernier-commit>

# Déplace HEAD deux pas en arrière
git reset --soft HEAD~2
# ou
git reset --soft <SHA-de-l'avant-dernier-commit>

# Vérification
git status

# Commit à nouveau avec un message différent
git commit -m "Message reformulé pour le même changement"

# Vérification
git log --oneline
```

```bash
# Ajout d'un autre commit pour tester --mixed et --hard
echo "Pour tester reset mixed et hard" > test-reset.txt
git add test-reset.txt
git commit -m "Ajout fichier test-reset"

# 2. Mixed reset (par défaut): Déplace HEAD et réinitialise l'index mais préserve le répertoire de travail
git reset HEAD~1
# ou
git reset <SHA-du-dernier-commit>

# Vérification
git status

# Commit de test-reset.txt
git add test-reset.txt
git commit -m "Ajout fichier test-reset (à nouveau)"

# Vérification
git log --oneline
```

```bash
# 3. Hard reset: Déplace HEAD et réinitialise l'index et le répertoire de travail
git reset --hard HEAD~1
# ou
git reset --hard <SHA-du-dernier-commit>

# Vérification
git log --oneline
```

## `git checkout` vs `git reset`

| **Commande**            | **Effet sur HEAD**                                | **Effet sur l'index (staging)**                         | **Effet sur le répertoire de travail**                        | **Usage recommandé**                                                                 |
|--------------------------|--------------------------------------------------|--------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------|
| **`git checkout <commit>`** | Déplace HEAD vers le commit spécifié ou une branche         | Aucun effet                                             | Met le répertoire de travail à l’état du commit                                                    | Pour naviguer temporairement ou pour créer une nouvelle branche à partir d'un commit |
| **`git reset --soft <commit>`** | Déplace HEAD vers le commit spécifié              | Les changements prêts à être commitées sont gardés dans l'index | Aucun effet                                                    | Pour annuler un commit tout en gardant les modifications prêtes à être re-commitées |
| **`git reset --mixed <commit>` (par défaut)** | Déplace HEAD vers le commit spécifié              | Réinitialise l'index (les changements ne sont plus dans l'index) | Aucun effet                                                    | Pour annuler un commit tout en repositionnant les changements dans le répertoire de travail |
| **`git reset --hard <commit>`** | Déplace HEAD vers le commit spécifié              | Réinitialise l'index et supprime les modifications      | Réinitialise le répertoire de travail pour qu'il corresponde au commit | Pour revenir à un état propre, en supprimant toutes les modifications locales depuis cet état      |


## Annuler un commit : avec `git revert`

**Revert** : Création d'un nouveau commit qui annule les effets d'un commit précédent

```bash
# Récupération du SHA du commit que vous souhaitez annuler
git log --oneline

git revert <SHA-du-commit-à-annuler>

# Vérification: affichage de l'historique
git log --oneline
```

## Travailler avec les Branches

1. **Création et suppression d'une branche**

```bash
# Création d'une nouvelle branche depuis main ou master
git branch to-delete

# Vérification
git branch
```

```bash
# Suppression de la branche
git branch -D to-delete

# Vérification
git branch
```

2. **Fusion de branches**

```bash
# Création d'une nouvelle branche depuis main ou master
git branch fonctionnalite
# Basculement sur la nouvelle branche
git checkout fonctionnalite

# OU avec Git 2.23+, création d'une nouvelle branche et basculement
git switch -c fonctionnalite
```

```bash
# Modification d'un fichier
echo "Nouvelle fonctionnalité" > index.html

# Commit des modifications
git add index.html
git commit -m "Ajout de la nouvelle fonctionnalité"
```

```bash
# Retour sur la branche principale
git checkout main  # ou master
# Ou
git switch main    # ou master
```

```bash
# Fusion de la branche fonctionnalite
git merge fonctionnalite
```

```bash
# Visualisation du graphe des commits
git log --graph --oneline --all
```

3. **Résolution de conflits**

```bash
# Création d'une nouvelle branche
git switch -c conflit

# Modification sur la branche conflit
echo "Cette ligne va créer un conflit" > conflit.txt
git add conflit.txt
git commit -m "Ajout d'une ligne dans conflit.txt (branche conflit)"
```

```bash
# Retour sur la branche principale
git switch main  # ou master

# Création du même fichier avec contenu différent
echo "Contenu de la ligne différent qui va créer un conflit" > conflit.txt
git add conflit.txt
git commit -m "Ajout d'une ligne dans conflit.txt (branche main)"
```

```bash
# Tentative de fusion
git merge conflit
```

Ouvrez conflit.txt dans un éditeur, vous verrez :
```
<<<<<<< HEAD
Contenu de la ligne différent qui va créer un conflit
=======
Cette ligne va créer un conflit
>>>>>>> conflit
```

```bash
# Résolution manuelle du conflit : modifier le fichier pour garder le contenu souhaité
# Par exemple :
echo "Contenu résolu après le conflit" > conflit.txt
```

```bash
# Marquer le conflit comme résolu avec un commit
git add conflit.txt
git commit -m "Résolution du conflit dans conflit.txt"
```

```bash
# Visualisation du graphe des commits
git log --graph --oneline --all
```

## Bonne pratiques

- Protéger la branche `main` de l'écriture directe
- Créer une branche pour chaque fonctionnalité / user story
- Effectuer des commits fréquents et incrémentaux
- Rédiger des messages de commit clairs
- Créer un pull request pour revoir le code et merger sur `main`
