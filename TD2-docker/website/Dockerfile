# Récupèrer une image de base
FROM python:3.14-slim

# Définir le repertoire /app comme repertoire courant
WORKDIR /app

COPY ./requirements.txt ./

RUN pip install -r requirements.txt
RUN apt update && apt install -y gcc make libc6-dev
# Copier le contenu du dossier courant (.) de l'hôte vers le dossier courant de l'image (ici /app)
COPY . /app

RUN cd /app/SuperDB && make

# Installer les dépendances python

# Définir la variable d'environnement FLASK_APP, nécessaire pour flask
ENV FLASK_APP=flask_minimal.py

# Définit la commande à lancer au lancement du conteneur
CMD ["flask", "run", "--host=0.0.0.0"]
