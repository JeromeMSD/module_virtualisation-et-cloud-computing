# Kubernetes - RAPPEL

Kubernetes, c'est le Linux des systèmes distribués, il permet d'abstraire les machines physiques pour créer une platforme pour exécuter des applications conteneurisées.

Pour permettre de manipuler les applications, Kubernetes met à votre disposition des ressources avec différents comportements et des fonctionnalités (Pod, Deployment, Service, etc.).

## `kubectl`

`kubectl` est l'outil qui permet de manipuler les ressources dans un environnement Kubernetes. Il envoit à l'API de Kubernetes des requêtes **CRUD** (HTTP) pour demander à ce que des actions ou des changements soit réalisés au sein de l'environnement.

> [!important]
> **RAPPEL -** Kubernetes cherche à se rapprocher au maximum de l'état désiré défini par les operateurs.

Pour installer `kubectl`, suivez [les instructions d'installation](https://kubernetes.io/docs/tasks/tools/#kubectl) adaptées à votre système d'exploitation, puis [configurez la connexion au cluster.](#connexion-au-cluster)

Vous trouverez [ici](https://kubernetes.io/fr/docs/reference/kubectl/cheatsheet/), un récapitulatif des commandes et actions possible avec cet outil.

### Connexion au cluster

Un cluster Kubernetes a été mis à votre disposition pour ce module. Utiliser le fichier fourni via le channel Teams du module pour vous y connecter.

> [!caution]
> Merci de ne pas partager le fichier qui permet la connexion au cluster sur internet.

**Copiez** le contenu du fichier et ouvrez votre terminal pour configurer l'accès.

1. Rendez-vous dans votre `HOME`.

  ```shell
  cd
  ```

2. Créer un dossier `.kube` et créer dans ce dossier un fichier `config` avec votre éditeur préféré.

  ```shell
  mkdir .kube && vim .kube/config
  ```

3. **Coller** le contenu du fichier dans ce `config` et sauvegarder le fichier.

    > *`Esc` + `:wq` pour quitter `vim` en sauvegardant le fichier*

4. Vérifier le fichier et son contenu :

   ```shell
    ls -la .kube
    cat .kube/config
   ```

> [!note]
> Le fichier que vous venez de créer contient la configuration (adresse + identifiants) pour se connecter à un cluster GKE ([Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine?hl=fr)).
> Nous le mettons dans ce dossier car c'est l'emplacement par défaut où `kubectl` va aller récuperer sa configuration.
>
> *Alternative :* On peut également passer un fichier `config` à `kubectl` directement via l'option `--kubeconfig`.

### Manipuler les ressources du cluster

Vous pouvez effectuer des actions dans l'environnement Kubernetes, vous disposez de plusieurs commandes à

```shell
# Commandes de creation (C de CRUD)
kubectl create <type-resource> <nom-objet>          # Crée l'instance <nom-objet> du type de ressource <type-ressource> (seulement pour certain type de ressource)
kubectl apply -f mon-fichier.yaml                   # Crée la ou les instance(s) de ressources décrite(nt) dans le fichier mon-fichier.yaml
kubectl run <nom-objet> --image=<nom-image>         # Crée un Pod <nom-objet> qui execute l'image <nom-image>

# Commandes de lecture (R de CRUD)
kubectl get <type-ressource>                        # Liste les instances du type de ressource <type-ressource>
kubectl get <type-ressource> <nom-objet>            # Liste l'instance <nom-objet> du type de ressource <type-ressource>
kubectl describe <type-de-ressource> <nom-objet>    # Afficher le manifest décrivant une instance de ressource 

# Commandes de mide à jour (U de CRUD)
kubectl edit <type-resource> <nom-objet>            # Ouvre un editeur pour modifier en live l'instance <nom-objet> du type de ressource <type-ressource>.

# Commande de suppression (D de CRUD)
kubectl delete <type-resource> <nom-objet>          # Supprime l'instance <nom-objet> du type de ressource <type-ressource>           
kubectl delete -f mon-fichier.yaml                  # Supprime la ou les instance(s) de ressources décrite(nt) dans le fichier mon-fichier.yaml
```

> [!tip]
> Utilisez `kubectl <command> --help` pour plus d'information sur une commande.

### Debugger avec `kubectl`

Pour effectuer des actions de debug, voici quelques exemples :

```shell
# Pour n'importe quelle type de ressource
kubectl describe <type-de-ressource> <nom-objet>    # Afficher le manifest décrivant une instance de ressource 
kubectl events                                      # Lister les evenements qui se sont produit dans le namespace

# Pour les Pods
kubectl logs <nom-du-pod>                                               # Affiche les logs du Pod <nom-du-pod> (ajouter -f pour affichage en continu)
kubectl exec <nom-du-pod> -- <command>                                  # Execute la commande <command> dans le Pod <nom-du-pod>
kubectl exec <nom-du-pod> -it -- bash                                   # Ouvre un shell interactif dans le Pod <nom-du-pod> 
kubectl port-forward <nom-du-pod> <port-hote>:<port-pod>                # Créer un tunnel http entre le port <port-hote> de votre machine et le <port-pod> du Pod <nom-du-pod>
kubectl port-forward svc/<nom-du-service> <port-hote>:<port-service>    # Créer un tunnel http entre le port <port-hote> de votre machine et le <port-service> du service <nom-du-service>
kubectl cp <nom-du-pod>:<chemin/vers/fichier> <chemin/destination>      # Copie des fichiers du pod vers votre machine
```

## Hiérarchie des objects dans Kubernetes

![Resources hierarchy](https://supports.uptime-formation.fr/images/kubernetes/k8s_objects_hierarchy.png?width=600px)

> [!important]
> Toutes les questions sur Kubernetes, son fonctionnement et ses concepts trouvent une réponse dans la [documentation de Kubernetes](https://kubernetes.io/docs/home/) (utilisez la barre de recherche à gauche).
