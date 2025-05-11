# Système de traitement automatisé d'images sur AWS

## Description du projet
Ce projet implémente une architecture serverless pour le traitement automatique d'images. Lorsqu'un fichier est déposé dans un bucket S3, le système enregistre ses métadonnées dans une base DynamoDB sans intervention manuelle.

## Architecture technique
L'infrastructure repose sur les services AWS suivants :
- Amazon S3 : Stockage des fichiers images
- AWS Lambda : Traitement des événements S3
- DynamoDB : Journalisation des métadonnées
  *EC2: Instance de test préconfigurée

Le déploiement s'effectue via un template CloudFormation unique garantissant une configuration reproductible.

## Prérequis
- Compte AWS avec permissions suffisantes
- AWS CLI configuré (pour le déploiement en local)
- Client SSH (pour accéder à l'instance de test)






## Documentation du projet :

J'ai créé un système automatisé sur AWS qui permet de gérer des fichiers images de manière intelligente. Lorsqu'un utilisateur dépose une image dans un espace de stockage (bucket S3), le système enregistre automatiquement des informations sur ce fichier (comme son nom et son emplacement) dans une base de données. Pour tester le tout, j'ai aussi configuré un petit serveur virtuel (instance EC2) qui peut consulter ces informations.

Comment ça marche ?
1. Stockage des images
Toutes les images uploadées par les utilisateurs sont stockées dans un bucket S3. Ce bucket porte un nom personnalisé en fonction de l'environnement (par exemple, dev-file-metadata-bucket pour l'environnement de développement).

2. Traitement automatique
Dès qu'une image est déposée dans le bucket, une fonction Lambda se déclenche automatiquement. Cette fonction, écrite en Python, récupère le nom du fichier et le nom du bucket, puis enregistre ces informations dans une table DynamoDB. La table s'appelle par exemple FileMetadata-dev et utilise le nom du fichier comme identifiant unique.

3. Consultation des données
Pour vérifier que tout fonctionne, j'ai déployé une instance EC2 (un serveur virtuel) avec une configuration minimale (t2.micro). Ce serveur peut se connecter à la base DynamoDB pour lire les métadonnées des fichiers. Il est protégé par un groupe de sécurité qui autorise uniquement les connexions SSH depuis une adresse IP spécifique.

Déploiement simplifié avec CloudFormation :
Toute l'infrastructure est définie dans un fichier template.yaml, qui utilise AWS CloudFormation pour tout déployer en une seule fois. Ce template permet de :

1.Créer le bucket S3 avec les bonnes permissions.

2.Configurer la Lambda pour qu'elle réagisse aux nouveaux fichiers.

3.Déployer la table DynamoDB avec la bonne structure.

4.Lancer l'instance EC2 avec les paramètres réseau nécessaires.

Comment tester le système ?
-Uploader une image dans le bucket S3 (via l'interface AWS ou un script).

-Vérifier dans DynamoDB que les métadonnées ont bien été enregistrées.

-Se connecter en SSH à l'instance EC2 pour interroger la base de données manuellement.


