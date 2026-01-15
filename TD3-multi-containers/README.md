# TP3 Multi-conteneurs et docker compose

Jusqu'à présent notre superbe site se reposait sur un fichier JSON ou l'outil SuperDB pour stocker ses données.

Seulement, ces deux méthodes ne permettent pas d'accèder en parallèle à la base de donnée de manière sûre. Seulement, la popularité de notre site monte en flèche, et il sera bientôt nécessaire d'avoir plusieurs instances du site en parallèle pour supporter la demande.

Pour cela, nous décidons d'utiliser une base de donnée qui supporte les accès parallèles, et bien plus : Postgres. Cette base de donnée s'exécutera dans son propre conteneur. Les conteneurs du site et de la base de données devront communiquer entre eux.

L'architecture finale sera la suivante :

![Architecture finale](../figures/multi-container.png)

Avant de vous lancer, nous allons voir comment faire en sorte que deux conteneurs puissent communiquer ensemble.

- Cloner ce dépôt : `git clone https://github.com/hreymond/VIR/`
- Entrer dans le répertoire du TD3 : `cd TD3-multi-containers`

## Communication entre conteneurs

Pour comprendre comment fonctionne le réseau, nous utiliserons la troisième version du site, dont l'image Docker contient des utilitaires réseau (ping, ip, ifconfig)

- Changer de dossier `cd website` 
- Construire l'image `website:v3` : `podman build . -t website:v3`

Ensuite, nous allons tenter de ping un conteneur depuis un autre : 

- Créer un conteneur de `website:v3`, en mode interactif nommé A : `podman run -it --rm --name A website:v3`
- Dans un autre terminal, créer un autre conteneur nommé B : `podman run -it --rm --name B website:v3`

Vous obtiendrez alors un terminal à l'intérieur du conteneur A et du conteneur B.
À partir de là, récuperer les addresses IP des deux conteneurs. Quelles sont vos conclusions...

## Mise en réseau de conteneurs

> [!NOTE] 
>
> **Mise en réseau de conteneurs**
>
> Les conteneurs ont besoin d'accéder à internet, ils possèdent donc une adresse réseau qui est routée vers l'extérieur. Mais pour des raisons d'isolation le routage vers un conteneur n'existe pas.
>
> Pour cela vous devez inscrire votre conteneur dans un réseau spécifique géré par podman. Les commandes `podman network ls` `podman network inspect <network_name>` vous permet de voir leur description. 
> 
> 
:question: Que pouvez-vous constater sur la configuration actuelle ?

- Arrêter les conteneurs A et B (Utiliser Ctrl-D ou taper la commande `exit`) 
- Dans deux terminaux différents, créer deux conteneurs A et B, qui se connectent au réseau disponible  (paramètre `--network`) :
    - `docker run -it --network <nom> --rm --name A website:v3`.
    - `docker run -it --network <nom> --rm --name B website:v3`
- Dans un autre terminal, créer un conteneur C, qui n'est pas connecté à `website-net` :  `docker run -it --rm --name C website:v3`
- Afficher les ips de chacun des conteneurs. On observe que A et B sont sur les mêmes sous-réseaux, et C sur un autre.

Vérifier que :
- Le conteneur A peut pinguer le conteneur B via son ip
- Le conteneur C ne peut accéder ni à A, ni à B

> En effet, si tous les conteneurs partagent le même réseau, cela présente une surface d'attaque de sécurité.
>
> ![Isolation réseau](../figures/isolation_reseau.png) 

:question: Vous avez nommé vos deux conteneurs A et B, sauriez-vous retrouver ces noms dans le conteneur ? Histoire de savoir comment je m'appelle ?

A ce stade, vous pouvez donc pinguer le conteneur A et le conteneur B par leur addresse IP. Mais pas leur nom.

> Le réseau par défaut de podman n'offre pas un support DNS (`dns_enabled: false`), et ce dernier nous serait utile pour accéder aux conteneurs par le nom, plutôt que leurs IPs.

Si vous créez un nouveau réseau `website-net`, vous pourrez constater que sa valeur dns_enabled est true. 

Notre sous réseau `website-net` fourni un DNS par défaut. Verifier qu'il est désormais possible :
- Que le conteneur A accède au conteneur B via son ID (accessible via `podman ps`)
- Que le conteneur A accède au conteneur B via le nom du conteneur : `ping B`

# TODO 

- [ ] Ajouter liaison Postgresql
- [ ] Ajouter docker-compose
- [ ] Demander extensions à 4 -> Connecter le frontend au voisins


# Liste des commandes utilisées
```
podman run -it --rm --name <nom> <tag> --> A quoi correspondent les options utilisées
podman network
ping
```