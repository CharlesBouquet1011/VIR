# TD6 - Kubernetes registry

Objectif du TD :
- :dart: Comprendre les dépôts locaux

## Partie 1 : Création d'une registry locale
Une image docker, podman, kubernetes (containerd) est stockée dans un repository. 
Par exemple lorsque vous récupérez l'image `traefik/whoami`, de la session précédente, celle-ci est puisée sur dans un dépot de référence comme docker.io
`https://hub.docker.com/r/traefik/whoami`.

Si vous souhaitez télécharger cette image vous pouvez passer la commande `podman pull <image>` comme indiqué sur le site.

Vous allez fabriquer vos propres images, il va être nécessaire d'exécuter votre propre registry. Voici les étapes

### Lancer et tester une registry locale
La registry est une application contenerisée. 
Vous pouvez lancer la version 2 de registry dans une fenêtre avec la commande `podman run`.
La registry tourne sur le port conteneur `5000`. Vous pouvez décider de la lancer sur un autre port. 

Vous pouvez vérifier qu'elle s'est bien lancé avec la commandes  
`curl http://localhost:<portHost>/v2/_catalog`

Vous pouvez maintenant déposer une image dessus. 

Une solution simple est de marquer (`tag`) une image existante à votre nom et la déposer dans la registry.   
`podman tag traefik/whoami <url>/whoami:sfr`

Lorsque vous allez pousser l'image dans la registry vous allez avoir une erreur https vs http. Il faut modifier votre configuration podman afin de pouvoir déposer des images sans utiliser de protocole sécurisé. Pour cela vous devez modifier votre fichier `/etc/containers/registries.conf` pour y déclarer l'accès non sécurisé à votre registry.

```
#/etc/contaniers/registries.conf
[[registry]]
location = "localhost:5010"
insecure = true
```

Vous pouvez pousser votre image
`podman push <image>`

Et vérifier que l'image est dans votre registry

`curl <url>/v2/_catalog`
`curl <url>/v2/traefik/whoami/tags/list`

## Utiliser cette registry dans Kubernetes




# Liste des commandes utiles
```
podman pull <image:tag>
podman run -p <portHote:portGuest> <nomImage>




kubectl get deployement
kubectl delete deployement <deploymentname>

kubectl apply -f <descripteur.yaml>
kubectl describe pod <podId>
kubectl get pods

kubectl get endpoints
kubectl get endpointslices

kubectl scale deployment <deploymentId> --replicas=<#>`

crictl ps
crictl exec -it <contenerId> /bin/bash

echo "xxx" > /usr/share/nginx/html/index.html

helm repo update
helm uninstall <xxx>
helm install <xxx>
curl -H "Host: whoami-gatewayapi2.toto" 10.56.171.180 
```
