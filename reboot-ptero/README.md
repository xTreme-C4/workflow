# Redémarrage du Serveur Discord Bot sur Pterodactyl

Ce fichier YAML est un workflow GitHub Actions qui redémarre un serveur Pterodactyl chaque fois qu'il y a un push dans la branche `main`.

## Fonctionnement du Workflow

Le workflow est déclenché par un événement de type `push` sur la branche `main`. Cela garantit que le serveur sera redémarré chaque fois qu'une modification importante est apportée au code.

## Description des Étapes du Workflow

1. **Déclencheur :**
   Le workflow est déclenché lorsqu'un push est effectué sur la branche `main`. Cela permet de s'assurer que le redémarrage du serveur ne se produit que lorsque des changements sont effectués dans la branche principale.

2. **Job de redémarrage :**
   Le job `restart` s'exécute sur une machine virtuelle Ubuntu, qui est la machine utilisée pour exécuter les tâches du workflow.

3. **Étape de redémarrage du serveur :**
   Cette étape utilise la commande `curl` pour envoyer une requête HTTP POST à l'API de Pterodactyl. La requête contient les informations nécessaires pour redémarrer le serveur spécifié dans le panneau Pterodactyl.
   
   Les informations suivantes sont envoyées dans la requête :
   - **URL de l'API :** L'URL de l'API de Pterodactyl est construite à partir de l'URL du panneau Pterodactyl et de l'ID du serveur (tous deux stockés en tant que secrets dans GitHub).
   - **Clé API :** L'authentification est réalisée à l'aide de la clé API stockée dans les secrets de GitHub.
   - **Signal de redémarrage :** Le signal `"restart"` dans le corps de la requête indique à l'API de redémarrer le serveur spécifié.

## Prérequis

1. **GitHub Secrets :**
   Vous devez ajouter les secrets suivants à votre dépôt GitHub :
   - `PTERO_PANEL_URL` : L'URL de votre panneau Pterodactyl.
   - `PTERO_SERVER_ID` : L'ID du serveur que vous souhaitez redémarrer.
   - `PTERO_API_KEY` : La clé API pour l'authentification avec l'API de Pterodactyl.

2. **Pterodactyl :**
   Assurez-vous que l'API de Pterodactyl est activée et que vous disposez des permissions nécessaires pour envoyer des requêtes de redémarrage.

## Conclusion

Ce workflow GitHub Actions simplifie le processus de redémarrage de votre serveur Pterodactyl à chaque mise à jour dans la branche `main`. Il permet une gestion automatisée du redémarrage du serveur, garantissant que votre environnement de bot Discord reste toujours à jour.
