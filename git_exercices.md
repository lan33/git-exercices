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

8. Revenez à la branche principale (`main`) :
   ```bash
   git checkout main
   ```
