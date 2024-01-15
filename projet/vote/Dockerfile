# Utilise une image de base Python version 3.11-slim
FROM python:3.11-slim

# Définit le répertoire de travail dans le conteneur comme "/app"
WORKDIR /app

# Copie le fichier "requirements.txt" depuis le répertoire source dans le répertoire de travail du conteneur
COPY requirements.txt .

# Exécute la commande "pip install" pour installer les dépendances listées dans "requirements.txt"
# L'option "--no-cache-dir" évite de mettre en cache les fichiers téléchargés
RUN pip install --no-cache-dir -r requirements.txt

# Copie tous les fichiers depuis le répertoire source dans le répertoire de travail du conteneur
COPY . .

# Expose le port 8080 pour les connexions entrantes (doit être pris en charge par l'application Python)
EXPOSE 8080

# Crée un utilisateur "user1" sans mot de passe pour des raisons de sécurité
RUN adduser --disabled-password --gecos '' user1

# Définit l'utilisateur par défaut à "user1" pour l'exécution de l'application (meilleure sécurité)
USER user1

# Définit la commande par défaut à exécuter lorsque le conteneur est lancé, ici "python app.py"
CMD ["python", "app.py"]
