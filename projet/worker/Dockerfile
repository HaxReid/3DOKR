# Utilise une image de base du SDK .NET version 7.0 pour créer l'environnement de construction
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env

# Définit le répertoire de travail dans le conteneur comme "/app"
WORKDIR /app

# Copie le fichier "Worker.csproj" dans le répertoire de travail du conteneur
COPY Worker.csproj ./

# Exécute la commande "dotnet restore" pour restaurer les dépendances de l'application
RUN dotnet restore

# Copie tous les fichiers du répertoire source dans le répertoire de travail du conteneur
COPY . ./

# Exécute la commande "dotnet publish" pour générer une version publiée de l'application en mode Release
# Les fichiers publiés seront placés dans le répertoire "/app/out"
RUN dotnet publish -c Release -o /app/out

# Utilise une image de base du Runtime .NET version 7.0 pour créer l'environnement d'exécution
FROM mcr.microsoft.com/dotnet/runtime:7.0 AS runtime

# Définit le répertoire de travail dans le conteneur comme "/app"
WORKDIR /app

# Copie les fichiers publiés depuis l'environnement de construction dans le répertoire d'exécution
COPY --from=build-env /app/out ./

# Définit la commande par défaut à exécuter lorsque le conteneur est lancé
CMD ["dotnet", "Worker.dll"]
