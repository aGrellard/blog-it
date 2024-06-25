---
title: Docker powershell
description: Container docker pour pogrammer en powershell sur un serveur vscode en ssh gavec l'extension Remote - SSH
tags:
  - docker
  - powershell
  - vscode
---

# Docker powershell

## Présentation

Bonjour à toutes et à tous,

Récemment, j'ai eu besoin de programmer en PowerShell sous Linux. Pour cela, j'ai décidé de créer un conteneur Docker. J'écris cet article pour partager cette expérience avec vous.

Ce conteneur Docker permettera d'offrir un environnement de développement PowerShell sur Debian, accessible via Visual Studio Code en utilisant l'extension Remote - SSH.

## Voici les principaux objectifs et utilités de ce conteneur

1. **Environnement de Développement Isolé** : Fournit un environnement isolé pour développer et tester des scripts PowerShell, sans affecter l'installation de l'hôte.
2. **Accessibilité et Gestion à Distance** : Permet l'accès et la gestion du conteneur à distance via SSH, intégré dans VS Code pour une expérience de développement fluide et sans interruption.
3. **Utilisation de VS Code Server** : Intègre VS Code Server dans le conteneur pour permettre l'utilisation de VS Code comme interface de développement directement connectée au conteneur.
4. **Permissions et Gestion des Fichiers** : Configure les permissions nécessaires pour l'utilisateur `vscode`, permettant de créer et modifier des fichiers et des dossiers dans le répertoire personnel de l'utilisateur dans le conteneur.
5. **Automatisation et Répétabilité** : Utilise Docker et Docker Compose pour automatiser la création et la configuration de l'environnement de développement, garantissant une réplication facile et une configuration cohérente à chaque déploiement.

En somme, ce conteneur facilite le développement et le test de scripts PowerShell dans un environnement contrôlé, tout en offrant une intégration transparente avec les outils de développement modernes comme VS Code.

## Créer les Fichiers Nécessaires

`docker-compose.yml`

```yml
version: '3.8'

services:
  powershell-vscode:
    build: .
    ports:
      - "8080:8080"
      - "2222:22"  # Expose le port SSH
    volumes:
      - .:/workspace
    container_name: powershell_vscode
    tty: true
```

`Dockerfile`
```yml
FROM mcr.microsoft.com/powershell:latest

# Installer des dépendances nécessaires pour VS Code Server et SSH
RUN apt-get update && \
    apt-get install -y curl wget git openssh-server sudo

# Créer le répertoire manquant pour le serveur SSH
RUN mkdir -p /run/sshd

# Installer VS Code Server
RUN mkdir -p /home/vscode && \
    cd /home/vscode && \
    wget https://update.code.visualstudio.com/latest/server-linux-x64/stable -O vscode-server.tar.gz && \
    tar -xzf vscode-server.tar.gz && \
    rm vscode-server.tar.gz && \
    mv vscode-server-* vscode-server

# Définir le répertoire de travail
WORKDIR /workspace

# Définir l'utilisateur non-root
RUN useradd -ms /bin/bash vscode && \
    echo 'vscode:vscode' | chpasswd

# Ajouter l'utilisateur vscode au groupe sudoers pour utiliser sudo sans mot de passe
RUN echo 'vscode ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Configurer les permissions pour le répertoire /home/vscode
RUN chown -R vscode:vscode /home/vscode

# Configurer SSH
RUN mkdir /home/vscode/.ssh && \
    chown vscode:vscode /home/vscode/.ssh && \
    chmod 700 /home/vscode/.ssh

# Configurer les clés pour VS Code Server
RUN mkdir -p /home/vscode/.vscode-server/extensions && \
    mkdir -p /home/vscode/.vscode-server-insiders/extensions && \
    chown -R vscode:vscode /home/vscode/.vscode-server* && \
    chmod -R 755 /home/vscode/.vscode-server*

# Exposer le port pour SSH
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```


