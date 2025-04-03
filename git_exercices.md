# Exercices sur git : revenir à un état antérieur

## git checkout

1. **Créer un nouveau répertoire et initialiser un dépôt Git :**
   ```bash
   mkdir exercice1_git_checkout
   cd exercice1_git_checkout
   git init
   ```

2. **Créer un premier fichier et effectuer le premier commit :**
   ```bash
   echo "Première version du fichier" > fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la première version du fichier"
   ```

3. **Modifier le fichier et effectuer un deuxième commit :**
   ```bash
   echo "Deuxième version du fichier" >> fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la deuxième version du fichier"
   ```

4. **Modifier à nouveau le fichier et effectuer un troisième commit :**
   ```bash
   echo "Troisième version du fichier" >> fichier.txt
   git add fichier.txt
   git commit -m "Ajout de la troisième version du fichier"
   ```

5. **Vérifier l’historique des commits :**
   ```bash
   git log --oneline
   ```
   Vous devriez voir quelque chose comme :
   ```
   abc1234 (HEAD -> main) Ajout de la troisième version du fichier
   def5678 Ajout de la deuxième version du fichier
   ghi9012 Ajout de la première version du fichier
   ```

6. Utilisez `git checkout` pour revenir temporairement à un commit précédent (par exemple, le deuxième commit) :
   ```bash
   git checkout <SHA-du-commit>
   ```

7. Vérifiez l’état du répertoire de travail pour observer les modifications :
   ```bash
   cat fichier.txt
   ```

8. Revenez à la branche principale (`main` ou `master`) :
   ```bash
   git checkout main
   # ou
   git checkout master
   ```

## git reset

**Reset** : Déplacement du HEAD et modification potentielle de l'index/répertoire de travail :
  - `--soft` : Déplace HEAD mais préserve l'index et le répertoire de travail
  - `--mixed` (ou sans option, par défaut) : Déplace HEAD et réinitialise l'index mais préserve le répertoire de travail
  - `--hard` : Déplace HEAD et réinitialise l'index et le répertoire de travail

**Consignes:**
1. Créez plusieurs commits sur une branche
2. Expérimentez avec les différentes options de reset
3. Annulez un commit publié avec revert

```bash
# Création d'une branche experimentations et de plusieurs commits sur cette branche
git checkout -b experimentations
echo "Commit 1" > exp1.txt
git add exp1.txt
git commit -m "Expérimentation 1"

echo "Commit 2" > exp2.txt
git add exp2.txt
git commit -m "Expérimentation 2"

echo "Commit 3" > exp3.txt
git add exp3.txt
git commit -m "Expérimentation 3"

# Affichage de l'historique
git log --oneline

# Expérimentation avec reset
# 1. Soft reset: déplace HEAD mais garde les modifications dans l'index
git reset --soft HEAD~1

# Vérification: modifications toujours dans l'index
git status

# Commit à nouveau avec un message différent
git commit -m "Message reformulé pour le même changement"

# Vérification: affichage de l'historique
git log --oneline

# Ajout d'un autre commit pour tester --mixed et --hard
echo "Pour tester reset" > test-reset.txt
git add test-reset.txt
git commit -m "Ajout fichier test-reset"

# 2. Mixed reset (par défaut): déplace HEAD et réinitialise l'index
git reset HEAD~1

# Vérification: modification dans le répertoire de travail mais pas dans l'index
git status

# Commit de test-reset.txt
git add test-reset.txt
git commit -m "Ajout fichier test-reset (à nouveau)"

# Vérification: affichage de l'historique
git log --oneline

# 3. Hard reset: tout annuler
git reset --hard HEAD~1

# Vérification: affichage de l'historique
git log --oneline
