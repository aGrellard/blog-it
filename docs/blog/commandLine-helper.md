# De l'aide pour votre ligne de commande

## open-interpreter

De l'IA dans votre terminal, et ouioui.

1. requierment
	- python
	- pip
2. création d'un environnment virtuel python
3. installation d'open-interpreter

## tldr

Une alternative au man de vos fonctions.

1. requierment
	- docker
2. Dockerfile

```yaml
# Utilise une image de base officielle de Debian
FROM debian:latest

# Installe les dépendances nécessaires
RUN apt-get update && \
    apt-get install -y nodejs npm && \
    npm install -g tldr

# Définit le point d'entrée pour le conteneur
ENTRYPOINT ["sleep", "infinity"]
```

2. Build

`docker build -t tldr-docker .`

3. Demarer le container

`docker run -d --name tldr-container tldr-docker`

## pandoc et lynx

Pour visualiser des fichiers markdown dans votre terminal.
