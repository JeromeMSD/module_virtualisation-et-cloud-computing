# Exigences pour le projet

Ce projet à rendre au plus tard **2 semaines après votre dernière scéance de TP.**

> [!WARNING]
> À partir de cette date, aucune modification de votre dépôt ou de son code ne sera prise en compte.

## GitHub

Vous rendrez votre code via un dépôt GitHub, auquel vous m’aurez ajouté en tant que collaborateur.

* L’historique des changements sur le dépôt devra montrer la collaboration entre les membres du groupe ( changement de sources différentes sur les fichiers du projet ).
* Le dépôt devra être documenté de 4 READMEs :
  * un pour les fondations du projet.
  * un pour la configuration Kubernetes.
  * un pour l'application microservice.
  * un global à la racine du dépôt.
* Le README principal contiendra **au minimum** les noms des **membres du groupe**, des détails sur le **déroulé du projet** et ce qui a été implémenté ainsi que des **schémas de votre infrastructure et architecture globale**.

> [!important]
> Vos READMEs jouent le rôle de **rapport de projet**, ajoutez y toutes les informations possibles sur celui-ci.

## Fondation

* Les fichiers `.tf` permettant la déclaration des éléments de fondations doivent être valides syntaxiquement.
* Le `README.md` du dossier `foundation` doit contenir un schema descriptif des fondations avec les noms de vos ressources.
* Le `README.md` du dossier `foundation` doit contenir le résultat de la commande `terraform plan`.

## Kubernetes

* Le dossier `kubernetes` doit contenir les manifestes `.yaml` permettant la déclaration de la configuration de votre environnement.
* Vos manifestes doivent être déployés sans erreur sur le cluster de rendu.
* Un schema descriptif des configuration doit être présent dans le `README.md` du dossier `kubernetes`.

## Application

### Docker

* Avoir un Dockerfile pour construire l'API (`backend`).
* Avoir un Dockerfile pour construire le `frontend` si vous utilisez `Node`, `React` ou `VueJs`.
* Avoir un Dockerfile pour construire le conteneur du consommateur de message.

> [!TIP]
> Si vous utilisez du `HTML/CSS/JS` utilisez un Dockerfile `nginx`.

### Redis

* Décrire dans le **README de l'application** la structure de donnée utilisée pour stocker les calculs.

### Interface utilisateur

* L'interface utilisateur doit permettre de demander la réalisation d'un calcul.
* L'interface utilisateur doit permettre de récupérer le résultat d'un calcul.

### API backend

* L'API doit permettre d'effectuer les quatre opérations de base.
* L'API doit permettre d'effectuer le stockage et la récupération des calculs dans `redis`.
* L'API doit permettre d'effectuer la mise en file d'attente des demandes de calcul.
