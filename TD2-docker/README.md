# Docker TD-2 : Création d'images

Objectifs du TD : 
- :dart: Tester une application dans un environnement de développement
- :dart: Porter une application comme une Image docker

L'équipe de développement s'est rendu compte des limitations d'utiliser un fichier json comme base de donnée. 
Ils ont donc sorti une nouvelle version du site web, qui utilise une base de donnée maison : "SuperDB".
Cette fois ci, c'est à vous de créer l'image Docker.

## Anatomie d'une Image Docker

### Choix d'une image de base

Pour commencer, déchiffrons le `Dockerfile` actuel, étape par étape :

```dockerfile
# Start with a base image
FROM python:3.14-slim
```
Un Dockerfile commence toujours par la même instruction `FROM <image>`. 
Cette ligne permet de spécifier une image de base, qui servira de fondation pour notre nouvelle image. Cette image de base contient un système d'exploitation et des outils essentiels. Ici, nous utiliserons l'image `python:3-14-slim`, une distribution linux très légère qui inclut Python.

### Installation des dépendances 

La suite du `Dockerfile` s'intéresse à l'installation des dépendances :

```dockerfile
# Copy the application content COPY <src> <dst>
COPY . /app

# Set up our working directory
WORKDIR /app

# Install the Python packages we need
RUN pip install -r requirements.txt
```

Tout d'abord, on copie le contenu du dossier et des sous-dossiers  (requirements.txt, flask_minimal.py, ...) dans le dossier `app/` de notre image. 
Ensuite, on définit `/app` comme dossier courant.
Enfin, on installe les dépendances de notre application python, listées dans le fichier `requirements.txt`.

### Configuration 

```dockerfile
# Tell Flask which application to run
ENV FLASK_APP=flask_minimal.py

# Specify the command to start our application
CMD ["flask", "run", "--host=0.0.0.0"]
```

Pour définir des variable d'environnement nécessaires à l'application, on utilise l'instruction `ENV`. Ici, on définit la variable `FLASK_APP` utilisée par le framework web Flask pour savoir quelle application lancer.

Enfin, on définit la commande qui sera utilisée pour lancer notre application, ici `flask run --host=0.0.0.0`.

:question: Peut-on se passer de l'instruction WORKDIR ?   
:question: Peut-on se passer du repertoire /app ?

## Comprendre les couches (layers)

La construction d'une image docker s'effectue par l'ajout de couches ou _layers_, à l'image de base.
Chaque _layer_ représente un ensemble de modifications (ajout, suppression, modification) apportées au _layer_ précédent.

Par exemple, pour notre image
- la couche correspondant à l'instruction `COPY` ajoute 4 fichiers à l'image de base.
- la couche `RUN pip install ...` ajoute les modules python nécessaires. Pour cela, elle dépend du fichier `requirements.txt` apportés par la couche `COPY`

![Couches de notre image](../figures/layers.png)

À noter que l'image `python` que l'on utilise est elle-même dérivée d'une image de base `debian`, à laquelle on a ajouté plusieurs couches correspondant à l'installation de Python.
 
Pour éviter d'avoir à reconstruire l'entièreté de l'image à chaque  `podman build .`, Podman garde en cache les différentes couches.

À votre tour :
- Relancer la construction de l'image : docker vous informe qu'il utilise les couches déjà construites avec `Using cache ...` 
- Modifier le fichier "Readme.md" dans `website`, et relancer la création d'une image. 

La construction de la couche `COPY . .` est relancée car un des fichiers copiés est modifié. Dans ce cas, les couchent suivantes, qui dépendent de `COPY`, sont relancées.  Est-ce pertinent de relancer l'installation des paquets python pour une modification du Readme ?

- Modifier le Dockerfile de manière à ce que l'installation des paquets python (couche `RUN pip install ...`) ne soit relancée que si le fichier `requirements.txt` est mis à jour.


# À vous de jouer !
## Validation du développement initial
Les sources du logiciel "SuperDB" sont dans le répertoire SuperDB.    
Sans utiliser podman, faire tourner l'application utilisant superDB. 

Pour cela, il faut 
- Compiler le logiciel SuperDB (lire le Readme.md du dossier SuperDB)
- Mettre en place un environnements de virtualisation python (venv) : 
  - `python3 -m venv venv`
  - `source venv/bin/activate`
- Installer les dépendances du site, et lancer le site. Pour ce faire, on pourra s'inspirer du Dockerfile analysé plus haut dans le TD.
- Testez l'application 

L'important est de comprendre les commandes à passer pour faire fonctionner votre application.

## Déploiement podman
Modifier le Dockerfile pour :
- Installer les dépendances permettant de compiler SuperDB
- Compiler le superDb.c pour produire l'exécutable superDBExe
- Construire la nouvelle image, et la tagger `v2` : `podman build . -t website:v2`
- Une fois l'image conçue, vérifier que tout fonctionne : le site démarre, fonctionne, et le fichier `queried_names.txt` est bien créé.

:white_check_mark: La version 2 du site web est déployée !

# BONUS
Si vous souhaitez améliorer l'image actuelle, vous pouvez accomplir ces tâches secondaires : 

- Add an HealthCheck to your website : https://docs.docker.com/reference/dockerfile/#healthcheck
- Do not expose port manually, instead use the EXPOSE command : https://docs.docker.com/reference/dockerfile/#expose et https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/
- Mettre en place de l'intégration continue : faire un fork de ce dépôt sur github ou gitlab, et faire en sorte que chaque nouveau commit sur `main` déclenche la création de l'image docker de website. 
  - Sur github, en utilisant les github actions : [https://docs.github.com/en/actions/get-started/understand-github-actions](https://docs.github.com/en/actions/get-started/understand-github-actions)
  - Sur gitlab, en utilisant une pipeline de CI/CD : [https://docs.gitlab.com/ci/](https://docs.gitlab.com/ci/)

# Liste des commandes utilisées
```
 apt update
 apt install <package>

 python3 -m venv toto
 . ./toto/bin/activate
 deactivate

 make
 mv
 flask --app ./flask_minimal.py run --host=0.0.0.0 
```
