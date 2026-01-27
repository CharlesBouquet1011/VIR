# TD5 - Kubernetes net

Objectif du TD :
- :dart: Comprendre la partie réseau de kube

## Partie 1 : Déploiement local et jouons avec les pods

Nous allons repartir d'un déploiement initial simple. 
Vérifiez que votre système kube n'a pas de déployement en cours d'exécution (`kubectl get deployement`, `kubectl delete deployment <name>`
), puis déployez le descripteur suivant (`kubectl apply -f <descripteur.yaml>).

```yaml
# Simple application with service
apiVersion: apps/v1
kind: Deployment
metadata:
  name: steph-1
spec:
  replicas: 2
  selector:
    matchLabels:
      app: prf
  template:
    metadata:
      labels:
        app: prf
    spec:
      containers:
      - name: web
        image: nginx:1.19
```

Définissez un second descripteur `steph-2` avec un seul réplicat de label app = `elp`. Deployez le second descripteur. 
Votre système doit contenir :    
 - 1 noeud : `kubectl get nodes`   
 - 3 pods : `kubectl get pods`    
 - 2 replicatSet : `kubectl get rs`    
 
:question: A quoi correspond un replicatset ?   

A l'aide de la commande `kubectl describe pods <podId>` identifiez les adresses ip internes du cluster qui permet de joindre les différents pods.    
A l'aide de la commande `curl`, testez ces différentes adresses et vérifiez que le serveur nginx répond bien.    
A l'aide de la commande `crictl` modifiez le serveur du service `elp` afin que sa page web réponde "Bonjour ELP".    


La notion de service permet de fournir une adresse unique sur un replicatSet. En effet, un pods n'a aucune garantie de durée de vie. Les évolutions font que les pods sont remplacés à n'importe quel moment (pour différentes raisons).

:warning: Attention, veillez à ne pas redéployer votre pod `elp`.
En partant de la définition de service de la dernière séance, définissez un service d'accès aux pods `prout`.
:question: comment savoir qu'un service ne trouve pas de pods ?

Corrigez votre service afin qu'il trouve le service `elp`.
En utilisant la commande `curl`, vérifiez que vous obtenez bien "Bonjour ELP" sur l'adresse unique.


Augmentez le nombre de replicats avec la commande suivante : 
`kubectl scale deployment steph-2 --replicas=3`   

En utilisant la commande `curl`, observez ce qu'il se passe.   
:question: Est-ce conforme à vos attentes ?    

Passez à 0 replicas, vérifiez que cela colle bien,    
puis revenez à 1 replicas. testez.  
Enfin remettez 6 replicas et vérifiez que votre description de service colle bien à votre spécification.   

Listez les endpointslices.



# Liste des commandes utiles
```
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

```
