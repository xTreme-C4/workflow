
# 🕒 Mise à jour automatique de `date.php` avec GitHub Actions

Ce dépôt utilise un **workflow GitHub Actions** qui génère automatiquement un fichier `date.php` à chaque `push` sur la branche `main`.  
Le fichier contient la **date et l'heure de la dernière mise à jour**, ajustée à un **décalage horaire de -4 heures** (par exemple, pour le fuseau horaire de Montréal).

---

## 🔧 Fonctionnement du workflow

Le fichier `.github/workflows/update-date.yml` contient un job appelé `update-date` qui s'exécute automatiquement à chaque `push` sur la branche `main`.

### Étapes du workflow :

1. 📦 **Clone du dépôt**
   - Utilise `actions/checkout` pour récupérer le code source.

2. 🛠️ **Création du fichier `date.php`**
   - Utilise la commande `date` avec un décalage de `-4 heures` :
     ```bash
     date -u -d '-4 hours' '+%Y-%m-%d %H:%M:%S'
     ```
   - Écrit ce contenu dans le fichier `date.php` :
     ```php
     <?php
     // Dernière mise à jour automatique
     $last_update = 'AAAA-MM-JJ HH:MM:SS' ;
     ?>
     ```

3. ✅ **Commit et push**
   - Configure Git avec l'identité `github-actions[bot]`
   - Commit le fichier `date.php`
   - Push vers la branche `main`

---

## 📂 Exemple de contenu du fichier `date.php`

```php
<?php
// Dernière mise à jour automatique
$last_update = '2025-04-07 02:41:22' ;
?>
```

---

## 🛡️ Configuration requise

### 🟢 Autoriser GitHub Actions à pousser dans le dépôt

1. Aller dans l'onglet **Settings** du dépôt
2. Cliquer sur **Actions** → **General**
3. Descendre à **Workflow permissions**
4. Sélectionner :
   > 🔘 **Read and write permissions**
5. Enregistrer avec **Save**

### 🟡 Si la branche `main` est protégée

1. Aller dans **Settings** → **Branches**
2. Cliquer sur la règle qui protège `main`
3. Cocher :
   > ✅ **Allow GitHub Actions to push to matching branches**

---

## ✏️ Personnalisation

- Modifier le décalage horaire :  
  Par défaut, le workflow utilise UTC - 4h.  
  Tu peux ajuster cette ligne dans le workflow :
  ```bash
  date -u -d '-4 hours' '+%Y-%m-%d %H:%M:%S'
  ```

- Modifier le format de la date :  
  Utilise les options de `date` (voir `man date` sur Linux) pour afficher la date comme tu le veux.

---

## 📁 Fichiers importants

- `.github/workflows/update-date.yml`  
  Contient le script GitHub Actions.

- `date.php`  
  Fichier généré automatiquement à chaque push.

---

## 📌 Résultat attendu

À chaque `git push` sur la branche `main`, le fichier `date.php` est mis à jour avec la nouvelle date/heure, puis automatiquement commité et repoussé dans le dépôt.

---

## ❓ Pourquoi ce système ?

Cela permet par exemple :
- D’afficher la dernière mise à jour d’un site
- De garder une trace automatisée des changements
- D’utiliser la date dans un tableau de bord, une API, etc.

---

🎉 **Plus besoin de mettre à jour manuellement la date dans le code !**
