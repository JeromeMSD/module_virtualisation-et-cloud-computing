# Docker - RAPPEL

[Commandes](https://docs.docker.com/get-started/docker_cheatsheet.pdf)

## Pousser une image sur Google Artifact Registry

> [!TIP]
> Un registre, ou *registry* c'est une sorte de biblithèque qui permet de stocker différentes images dans chacune de leurs versions.

Pour pousser une image sur un dépôt, il faut lui attribuer le bon tag. Cette étiquette définira où l'image va être envoyée.
Nous allons utiliser `Artifact Registry` le registre de Google. *(pour changer)*

Pour pousser une image dans un registre **Artifact Registry**, il vous faut:

* Un registre **Artifact Registry**. *(logique)*
* Y être connecter et avoir le droit de le faire. *(je vais vous l'attribuer)*
* Les `docker` et `gcloud`.

Le registre est déjà créé. Il ne vous reste que la connexion et l'envoi de votre image.

Pour vous connecter, réaliser la procédure suivantes dans un terminal :

1. Initialiser `gcloud`

    ```shell
    gcloud init
    ```

    Aux question posées, répondez :
    * Pick a configuration to use :  **[1] Re-initialize this configuration (default) with new settings**
    * Select an account : **Skip this step**

2. S'authentifier

    ```shell
    gcloud auth login
    ```

    Connectez-vous avec l'adresse email Google que vous aviez saisie dans le Google Sheet

3. Configurer le projet `polytech-dijon`

    ```shell
    gcloud config set project polytech-dijon
    ```

    Do you want to continue (Y/n) ? **Y**

4. Configurer l'outil `docker` pour pousser dans **Artifact Registry**.

    ```shell
    gcloud auth configure-docker europe-west1-docker.pkg.dev
    ```

**Vous pouvez maintenant pousser les images.** Utiliser les tags adaptés pour faire fonctionner la commande `docker push`

###### Exemple pour le frontend

```shell
europe-west1-docker.pkg.dev/polytech-dijon/polytech-dijon/frontend-2024:nom1-nom2
```

###### Exemple pour le backend

```shell
europe-west1-docker.pkg.dev/polytech-dijon/polytech-dijon/backend-2024:nom1-nom2
```

## Et comment ça ce passe côté Kubernetes ?

Kubernetes est déjà authentifié pour recupérer des images depuis ce registre.

Il suffit de fournir dans la configuration, l'adresse de l'image à récupérer dans le registre.

###### Exemple pour le frontend

```yaml
# ...
spec:
  containers:
    - name: my-ctn
      image: europe-west1-docker.pkg.dev/polytech-dijon/polytech-dijon/frontend-2024:nom1-nom2
```
