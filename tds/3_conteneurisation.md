# Conteneurisation

La conteneurisation permet d'encapsuler une application et ses dépendances dans un conteneur isolé, offrant ainsi une solution portable et légère pour le déploiement des applications.

Parmis les outils qui nous permettent de créer des conteneurs, le plus populaire est `docker`.

## Une impression de déjà vu

Conteneuriser un serveur web simple. Dans un dossier `frontend`, exécuter les actions suivantes :

1. Dans un premier temps, créer une page web simple (HTML/CSS) dans un fichier `index.html`.

> [!tip]
> Les fichiers `.html` peuvent être ouvert avec un navigateur web pour en voir le contenu.
> 
> Vous pouvez prendre un peu de temps pour faire de la mise en forme via [CSS](https://developer.mozilla.org/fr/docs/Learn/CSS/First_steps/Getting_started). (Pas toute la scéance non plus, on a beaucoup de choses à tester 😉)

2. Nous allons utiliser `nginx` pour servir cette page. ( cf. [Liens utiles](#liens-utiles) ). <br> Créer un `Dockerfile` qui se base sur la dernière version de l'image `nginx`.
3. Ajouter les fichiers au conteneur via le paramêtre `COPY`.

   ```dockerfile
    COPY index.html /usr/share/nginx/html/

    # Si vous avez d'autres fichiers ou dossier, copier les également
    COPY ./img /usr/share/nginx/html
   ```

4. Préciser le port d'ouverture du conteneur via le paramètre `EXPOSE`, il s'agit du port servi par `nginx` ( `80` par défaut ).
5. Sauvegarder le fichier `Dockerfile` parmi vos fichiers.

**Quel est le point d'entrée de l'application ? Quelle commande va s'executer au démarrage du conteneur ?**

6. Nous avons décrit notre conteneur, construiser maintenant l'image de ce conteneur via la commande `docker build`.

    ```shell
    docker build . -t mon-frontend
    ```

7. Executer votre conteneur via `docker run`.

> [!tip]
> Pour celle-ci, je vous laisse y réfléchir :)
>
> **Informations clés :** On cherche à déployer un conteneur qui execute notre image en redirigeant le port `8080`
> de notre machine sur le port d'exposition du conteneur `80`. (le nom de l'image doit être le dernier argument de la commande)

8. Dans votre navigateur préféré, rendez-vous sur [http://localhost:8080](http://localhost:8080).

> [!important]
> **Félicitation !** Vous venez (peut être) de déployé votre premier site web conteneurisé ! 🚀

9.  Modifier le contenu de votre de fichier `index.html` et redéployer à nouveau le conteneur.

**Que pouvez-vous observer ?**

## Explorer ce micro-environnement

Dans un nouveau dossier nommé `backend`, créer un nouveau `Dockerfile`.

### Construire un conteneur de debug

1. Prendre pour base une image `Debian` ou `Ubuntu`.

```dockerfile
FROM debian:latest
```

2. Ajouter les paquets `bash`, `htop`, `vim`, `netstat` (via l'argument `RUN`).
3. Ajouter une variable d'environnemnet `OWNER` contenant votre prénom.

Les images `Debian` et `Ubuntu` n'ont pas de commande par défaut, nous utiliseront `sleep 3600` pour que le conteneur reste actif.

**Pourquoi n'avons-nous pas utilisé `EXPOSE` ?**

Construire l'image avec le tag `toolbox` et le nom `toolbox-ctn`.

### Utiliser ce conteneur

1. Première étape, démarrer le conteneur.
2. Dans un second terminal, connectez-vous au conteneur via `docker exec` :

   ```shell
   docker exec -it <ctn-id ou ctn-name> /bin/bash
   ```

3. Tester les différentes outils installés. Ensuite, installer `curl`.
4. Dans un troisième terminal, démarrer le conteneur [frontend](#une-impression-de-déjà-vu).
5. Revener dans le second terminal et exécuter la commande :

    ```shell
    curl http://<IP-host>:8080
    ```

> [!important]
> Utiliser l'IP de l'interface réseau dédiée à `docker` (à récuéperer avec `ip -4 addr show` ou `ipconfig` suivant votre OS)

**Que constatez-vous ?**

> [!tip]
> Besoin d'arrêter votre conteneur pour modifier son Dockerfile ?
> 
> ```shell
> docker ps                     # Lister des conteneurs en cours d'exécution
> docker stop 1705f0d2a0c5      # Stop le conteneur avec l'id 1705f0d2a0c5
> docker rm 1705f0d2a0c5        # Supprime le conteneur avec l'id 1705f0d2a0c5
> ```

### Gestion des droits utilisateur

1. Une fois connectez dans le conteneur en execution, utilisez la commande `whoami`
2. Installer `curl`.
3. Quitter l'exécution, dans le `Dockerfile` ajouter un nouvelle utilisateur pour l'execution.

    ```dockerfile
    RUN useradd nonroot
    USER nonroot
    ```

4. Construire et deployer le conteneur, et tester à nouveau `apt install curl` et `whoami`.

**Qu'en déduisez-vous ?**

## Sauvegardons ce joli frontend

Dans vos projets Cloud, vous serez heureux de ne pas reconstruire la même image à chaque fois, ou tout simplement d'éviter de les perdre dans un malheureux crash de votre ordinateur
Pour stocker les images, les organisations ont recours à des **registres**.

> [!TIP]
> Un registre, ou *registry* c'est une sorte de biblithèque qui permet de stocker différentes images dans chacune de leurs versions.

Pour pousser une image sur un dépôt, il faut lui attribuer le bon tag. Cette étiquette définira où l'image va être envoyée.
Nous allons utiliser `Artifact Registry` le registre de Google. *(pour changer)*

Pour pousser une image dans un registre **Artifact Registry**, il vous faut:

* Un registre **Artifact Registry**. *(logique)*
* Le droit de le faire. *(je vais vous l'attribuer)*
* Les `docker` et `gcloud`.

Le registre est déjà créé. Il ne vous reste que la connexion et l'envoi de votre image.

0. Commencer, si ce n'est pas déjà fait, par installer [gcloud](https://docs.cloud.google.com/sdk/docs/install-sdk?hl=fr#latest-version).
1. Nous allons utiliser [le même identifiant que pour le TD sur Terraform](../docs/rappel-terraform.md#google), pour l'utiliser dans ce mode nous devons d'abord l'ajouter à gcloud via la commande suivante : 
    ```shell
    gcloud auth activate-service-account student@polytech-dijon.iam.gserviceaccount.com --key-file="./student.json"
    ```

2. Configurer le projet `polytech-dijon`

    ```shell
    gcloud config set project polytech-dijon
    ```

    Do you want to continue (Y/n) ? **Y**

3. Configurer l'outil `docker` à partir de la configuration `gcloud` pour pousser dans **Artifact Registry**.

    ```shell
    gcloud auth configure-docker europe-west1-docker.pkg.dev
    ```

**Vous pouvez maintenant pousser les images.** Utiliser les tags adaptés pour faire fonctionner la commande `docker push`

> [!important]
> Quant vous utiliser des registres SaaS, le tag de l'image doit contenir: 
> * Les informations liés au founisseur, dans notre cas `<region>-<type-de-registre>.pkg.dev/<projet-gcp>`
> * Le nom du registre, ici `polytech-dijon`
> * Et enfin le nom de l'image suivi de son tag: `<image>:<tag>`
> Ce qui pourrait donner, une fois concaténé: `europe-west1-docker.pkg.dev/polytech-dijon/polytech-dijon/mon-app:1.0.0`

###### Exemple pour le frontend

```shell
europe-west1-docker.pkg.dev/polytech-dijon/polytech-dijon/frontend-2026:nom1-nom2
```

> [!note]
> Les commandes doivent être executé dans le même terminal.

> [!tip]
> **C'est terminé !** 🚀
>
> Faites moi signe pour que l'on vérifie ensemble que l'image a bien été poussé sur le registre. 👋

---

## Bonus - Utilisez les conteneurs pour tester des outils

[![](https://img.shields.io/badge/rabbitmq-%23FF6600.svg?&style=for-the-badge&logo=rabbitmq&logoColor=white)](https://rabbitmq.com/)

Dans un terminal, démarrez un conteneur [**RabbitMQ**](https://rabbitmq.com/) qui servira de serveur de file d'attente.

```bash
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management
```

Le serveur est installé, il ne reste qu'à tester et pour les tests c'est ici ! 👉 [ RabbitMQ Hello World ](https://rabbitmq.com/tutorials/tutorial-one-python.html)

Réalisez le **TP Hello World de RabbitMQ !**, vous y créerez un créateur et un consomateur de message ( Producer / Consumer ).

---

##### Liens utiles

* [Docker](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
* [Dockerfile](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)
* [NGINX](https://nginx.org/en/docs/)
* [RabbitMQ](https://www.rabbitmq.com/docs)
