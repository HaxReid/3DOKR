# 3DOKR Benjamin MAURY / Paul-Etienne Guérin

Voici un README pour la création d'un cluster Docker Swarm et des raisons qui nous ont poussé à mettre les images sur Docker Hub plutôt que d'appeler les Dockerfiles dans le Docker Compose.

---

# Guide pour la Création d'un Cluster Docker Swarm

## Introduction

Ce guide explique comment créer un cluster Docker Swarm pour orchestrer nos conteneurs Docker sur un ensemble de machines. Un cluster Swarm permet de gérer et de distribuer des conteneurs sur plusieurs nœuds, y compris des Managers et des Workers. Il fournit une haute disponibilité et une scalabilité pour nos applications conteneurisées.

## Prérequis

- Plusieurs machines virtuelles ou serveurs accessibles via SSH, par exemple, Manager et Workers.
- Docker installé sur chaque machine virtuelle.

## Étapes

### Étape 1 : Création du Cluster Docker Swarm

1. **Sélectionner un nœud comme Manager** : Nous choisissons une machine virtuelle pour être le Manager du cluster. Nous pouvons lancer les commandes de gestion du cluster à partir de ce nœud.

2. **Initialiser le Swarm** : Sur le Manager, nous exécutons la commande suivante pour initialiser le cluster Swarm :

   ```bash
   docker swarm init --advertise-addr VOTRE_ADRESSE_IP
   ```

   En remplaçant `VOTRE_ADRESSE_IP` par l'adresse IP du Manager, qui, pour nous, était "192.168.99.100".

3. **Faire rejoindre les Workers au Swarm** : Après l'initialisation, le Manager génère une commande `docker swarm join` que nous exécutons sur les Workers pour les rejoindre au cluster. Nous avons exécuté cette commande sur chaque Worker :

   ```bash
   docker swarm join --token VOTRE_TOKEN ADRESSE_IP_DU_MANAGER:PORT
   ```

   En remplaçant `VOTRE_TOKEN` par le token généré par le Manager et `ADRESSE_IP_DU_MANAGER:PORT` par l'adresse IP et le port du Manager.

4. **Vérifier l'état du cluster** : Sur le Manager, nous exécutons la commande suivante pour vérifier l'état du cluster et afficher la liste des nœuds :

   ```bash
   docker node ls
   ```

   En nous assurant que les Workers sont listés et ont l'état "Ready".

### Étape 2 : Déploiement de Services avec Docker Compose

1. **Créer un fichier Docker Compose** : Définir les services à déployer sur le cluster Swarm dans un fichier Docker Compose. S'assurer d'inclure les réseaux, les volumes, et d'autres configurations nécessaires.

2. **Construire les images et lancement des services** : Sur le Manager, nous utilisons Docker Compose pour construire les images des services (si nécessaire) et les déployer sur le cluster Swarm. Avec les commandes suivantes :

   ```bash
   docker-compose build
   docker-compose up -d
   ```

   Ces commandes construiront les images des services définis dans le fichier Docker Compose (si elles ne sont pas déjà construites) et les déployeront sur le cluster Swarm.

3. **Vérifier les services** : Sur le Manager, nous exécutons la commande suivante pour vérifier l'état de nos services déployés :

   ```bash
   docker service ls
   ```

   Pour nous assurer que les services sont en cours d'exécution sur les Workers.

## Pourquoi Pousser les Images sur Docker Hub ?

**Justification** : Plutôt que d'appeler les Dockerfiles dans le Docker Compose, il est souvent préférable de pousser les images sur Docker Hub pour plusieurs raisons :

1. **Réutilisation facile** : Les images stockées sur Docker Hub sont faciles à réutiliser dans divers projets et sur différentes machines sans avoir à reconstruire les images à chaque fois.

2. **Distribution rapide** : Les images sur Docker Hub sont accessibles à partir de n'importe où dans le monde, ce qui permet une distribution rapide des images à d'autres utilisateurs ou serveurs.

3. **Séparation des préoccupations** : La création et la gestion d'images Docker sont séparées de la configuration de déploiement. Cela permet une gestion plus claire et une plus grande flexibilité.

4. **Versionnement des images** : Docker Hub prend en charge le versionnement des images, ce qui facilite la gestion des versions des conteneurs.

5. **Sécurité et gestion des accès** : Nous pouvons gérer l'accès aux images stockées sur Docker Hub en fonction de nos besoins, assurant ainsi une meilleure sécurité.

Nous gardons à l'esprit que cette approche suppose que nous maintenons nos images à jour sur Docker Hub lorsque nous apportons des modifications au code source, ce qui assure une gestion centralisée des images Docker pour nos applications.

---