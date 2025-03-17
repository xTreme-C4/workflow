Redémarrage du Serveur Discord Bot sur Pterodactyl
Ce projet permet de redémarrer un serveur Pterodactyl à l'aide d'un workflow GitHub Actions. Le serveur sera redémarré à chaque fois qu'il y a un push sur la branche principale (main) du dépôt GitHub.

Description
Ce workflow GitHub Actions utilise un script qui envoie une requête HTTP à l'API de Pterodactyl pour redémarrer un serveur. Il est configuré pour se déclencher automatiquement dès qu'un commit est effectué dans la branche main.

Le workflow est composé d'un seul job qui utilise une machine Ubuntu pour exécuter la tâche de redémarrage.

Explication du Code
Déclencheur
yaml
Copy
Edit
on:
  push:
    branches:
      - main  # Déclenche le workflow sur un push dans la branche principale
Le workflow s'exécute lorsqu'un push est effectué sur la branche main. Cela permet de s'assurer que le serveur est redémarré chaque fois qu'une modification importante est apportée au code.

Job de Redémarrage
yaml
Copy
Edit
jobs:
  restart:
    runs-on: ubuntu-latest  # Exécute le workflow sur une machine Ubuntu
Le job restart s'exécute sur une machine virtuelle Ubuntu, qui est la machine utilisée pour exécuter le script de redémarrage.

Étape de Redémarrage
yaml
Copy
Edit
steps:
  - name: Redémarrer le serveur Pterodactyl
    run: |
      curl -X POST "https://${{ secrets.PTERO_PANEL_URL }}/api/client/servers/${{ secrets.PTERO_SERVER_ID }}/power" \
      -H "Authorization: Bearer ${{ secrets.PTERO_API_KEY }}" \
      -H "Content-Type: application/json" \
      -d '{"signal": "restart"}'
Cette étape exécute un script curl qui envoie une requête HTTP POST à l'API de Pterodactyl pour redémarrer le serveur spécifié. Voici une explication détaillée des éléments :

URL de l'API : https://${{ secrets.PTERO_PANEL_URL }}/api/client/servers/${{ secrets.PTERO_SERVER_ID }}/power

Remplacez ${{ secrets.PTERO_PANEL_URL }} et ${{ secrets.PTERO_SERVER_ID }} par les variables secrètes configurées dans GitHub Actions pour l'URL du panneau et l'ID du serveur Pterodactyl.
En-têtes :

Authorization: Bearer ${{ secrets.PTERO_API_KEY }}
L'API Key pour l'authentification.
Content-Type: application/json
Le type de contenu pour la requête.
Données :

{"signal": "restart"}
Cette donnée JSON envoie le signal pour redémarrer le serveur.
Sécurisation des Variables
Les variables secrètes utilisées dans ce workflow (PTERO_PANEL_URL, PTERO_SERVER_ID, PTERO_API_KEY) doivent être configurées dans les secrets GitHub de votre dépôt pour garantir leur confidentialité.

Prérequis
GitHub Secrets : Vous devez ajouter les secrets suivants à votre dépôt GitHub :

PTERO_PANEL_URL : L'URL de votre panneau Pterodactyl.
PTERO_SERVER_ID : L'ID du serveur que vous souhaitez redémarrer.
PTERO_API_KEY : La clé API pour l'authentification avec l'API de Pterodactyl.
Pterodactyl : Assurez-vous que l'API de Pterodactyl est activée et que vous disposez des permissions nécessaires pour envoyer des requêtes de redémarrage.

Conclusion
Ce workflow GitHub Actions simplifie le processus de redémarrage de votre serveur Pterodactyl à chaque mise à jour dans le dépôt. Il permet une gestion automatisée des redémarrages, améliorant ainsi l'efficacité et la maintenance continue de votre serveur Discord Bot.
