# SuperDB

Un outil qui stocke et met à jour le nombre de fois qu'une clé donnée a été recherchée dans un fichier `queried_names.txt`.

## Exemple d'utilisation

Voila un exemple d'exécution du programme
-  Premier appel `./SuperDBExe Aypierre` : Crée le fichier `queried_names.txt`, et stocke le nombre  `Aypierre:1` et s'arrête avec le code de sortie 1
-  Deuxième appel `./SuperDBExe Aypierre` : Met à jour le nombre de fois que la clé `Aypierre` a été recherché  (stocke `Aypierre:2`) et renvoie 2

## Dépendances

Pour compiler l'application, il est nécessaire d'installer `gcc` et `make`.

- `apt update`
- `apt install -y gcc make libc6-dev`

## Compilation

Pour compiler le binaire, exécutez `make` dans le répertoire `SuperDB` pour produire le binaire `SuperDBExe`

- `cd SuperDB`
- `make`
