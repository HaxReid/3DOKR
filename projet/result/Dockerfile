# Utilise une image de base Node.js
FROM node:latest

# Définit le répertoire de travail dans le conteneur comme "/app"
WORKDIR /app

# Copie les fichiers "package.json" et "package-lock.json" depuis le répertoire source dans le répertoire de travail du conteneur
COPY ./package*.json ./

# Exécute la commande "npm install" pour installer les dépendances définies dans le fichier "package.json"
RUN npm install

# Copie tous les fichiers depuis le répertoire source dans le répertoire de travail du conteneur
COPY . .

# Expose le port 8888 pour les connexions entrantes 
EXPOSE 8888

# Définit la commande par défaut à exécuter lorsque le conteneur est lancé, ici "npm start"
CMD ["npm", "start"]
