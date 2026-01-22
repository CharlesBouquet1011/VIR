# TD3 - Kubernetes

Objectif du TD :
- :dart: Démarrer kubernetes / k3s dans un environnement maitrisé
- :dart: Prendre en main les premières commandes kube

## Partie 1 : Lancement
Nous avons installé sur les clés les outils permettant de démarrer une infrastructure [kubernetes](https://kubernetes.io/docs). 
Il existe de nombreuses implantations de Kubernetes et comme Kubernetes est principalement spécifié via ses API elles sont assez interchangeable. 
Nous avons fait le choix d'utiliser [k3s](https://k3s.io/). A partir de maintenant, quand nous parlerons de Kubernetes, nous sous-entendrons souvent
l'implantation spécifique utilisée (ici k3s).


### Démarrer k3s
k3s est présent sur la clé. Vous allez donc pouvoir le lancer et le manipuler. Mais avant cela il faut comprendre la notion de services. 

L'objectif de Kubernetes est de fournir une infrastructure d'exploitation (Run) large échelle offrant des garanties d'exécution 24/7. Pour cela, l'infrastructure fonctionne de manière autonome et va se placer au dessus de votre système d'exploitation comme le ferrait une machine virtuelle. On peut assimiler cela à un gestionnaire de machines virtuelles, a la différence que la granularité d'exploitation n'est pas une machine mais un conteneur au sens de docker. 

Dans ce cadre, le gestionnaire doit fonctionner comme tâche de fond du système d'exploitation. Tous les systèmes d'exploitation (Linux, Windows, Mac) offrent une (ou plusieurs) applications de gestion des tâches de fond. Il s'agit des 'services windows' dans windows https://fr.wikipedia.org/wiki/Service_(Windows), du système lauchnd sous macos. Sous Linux de nombreuses propositions ont étés faites, car il s'agit d'un enjeu majeur du bon fonctionnement des systèmes d'exploitation. Un gestionnaire de service doit pouvoir lancer et arrêter un service, le surveiller ou bien encore définir s'il doit être lancé systématiquement au démarrage ou uniquement selon des besoins spécifiques. Enfin, avant de pouvoir le lancer, il est nécessaire de vérifier que les services, les drivers ou les processus dont ils dépend sont disponibles. Lancer un 'service' n'est pas simple et c'est pourquoi les outils de gestion de services ne sont pas si simples qu'ils n'y parraissent. Debian utilise [systemd](https://medium.com/@sebastiancarlos/systemds-nuts-and-bolts-0ae7995e45d3) comme remplaçant à initd (Une excellente présentation des systèmes d'init peut se trouver [ici](https://hal.science/inria-00155663/) quoi qu'elle date un peu). Systemd repose sur les commande systemctl ou journalctl disponibles sur votre machine. Par exemple la commande `systemctl list-unit-files` liste les services et leur état actuel. La commande `journalctl -uf <nomdeservice>`permet de suivre les traces de logs d'un service. 

:question: Utilisez `journalctl` pour tracer l'activité de ssh, et montrez que vous obtenez des traces. 

L'installation que nous avons fait de k3s, est une installation sans démarrage automatique du service. La commande d'installation initiale que nous avons réalisé sur la clé est donc `INSTALL_K3S_SKIP_ENABLE=true ./install-k3.sh`. Les variables disponibles pour k3s sont [ici](https://docs.k3s.io/reference/env-variables). Le service existe, mais il n'est pas démarré comme un service. Dans le cadre du cours, nous n'allons pas l'utiliser comme un service mais directement dans une fenêtre en appelant le shell `startk3sServer.sh`.

:question: Ouvrez le shell et vérifier la commande
:question: Démarrez le shell et vérifiez que les logs fonctionnent
:question: Arrêter le processus k3s en faisant un CTRL-C dans la fenêtre

Nous ne passons pas par un service, car toutes les actions précédente sont plus complexes en passant par systemd. Mais sauriez-vous les retrouver ?





### Environnnement de travail

Vous pouvez maintenant cloner le projet git https://github.com/hreymond/VIR et vous rendre dans le repertoire TD1-Docker.

# Liste des commandes utiles
```
systemctl list-unit-files
journalctl -uf <nomdeservice>
```

# Liste des packages contenant des utilitaires utiles
```
  netstat --> net-tools
  ps --> procps
``` 

# Pour le root et ses cheat sheets
https://gist.github.com/rxaviers/7360908


