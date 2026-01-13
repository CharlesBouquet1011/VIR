# A minimal minecraft head display server

Un serveur pour afficher le skin d'un joueur minecraft : entrer le nom d'utilisateur du joueur, et vous aurez sa tête.

# Installation du site

## Installation

- Installer les dépendances : `pip install -r requirements.txt`
- Définir la variable d'environnement FLASK_APP : `export FLASK_APP=flask_minimal.py` 

# Installation de SuperDB

## Dépendances

Pour compiler l'application, il est nécessaire d'installer `gcc` et `make`.

- `apt update`
- `apt install -y gcc make`

## Compilation

Pour compiler le binaire, exécutez `make` dans le répertoire `SuperDB` pour produire le binaire `SuperDBExe`

- `cd SuperDB`
- `make`
