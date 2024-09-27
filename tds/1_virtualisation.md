# Virtualisation

Dans ce TD, nous allons utilisez [VirtualBox](https://www.virtualbox.org/) pour transformer une partie de notre système machine virtuelle (VM).

## VirtualBox

### Installation

Installer [VirtualBox](https://www.virtualbox.org/wiki/Downloads) en suivant les instructions adaptées à votre système d'exploitation.

### Créer votre première VM Linux

Nous allons utilisez une image de système d'exploitation, aussi appelé **ISO**.
L'objectif sera de créer sur votre machine une VM [Ubuntu](https://ubuntu.com/) à partir de l'[ISO Ubuntu](https://ubuntu.com/download).

1. Télécharger l'[ISO Ubuntu](https://ubuntu.com/download) sur votre machine.

> [!tip]
> **Déjà une machine Ubuntu ?** Utilisez un ISO de windows ou celui d'une autre distribution Linux :
> 
> [ISO Windows](https://www.microsoft.com/FR-FR/software-download/windows10ISO)
> ou
> [ISO Elementary](https://elementary.io/docs/installation#installation)

1. Dans VirtualBox, créer une nouvelle machine virtuelle.
    Ubuntu nécessite [les caracteristiques suivantes](https://ubuntu.com/download/desktop#system-requirements) pour fonctionner correctement:
    * 2 GHz de puissance de calcul.
    * 4 GB de RAM.
    * 25 GB de stockage.
    * Une connexion à internet (facultatif mais c'est mieux)

> [!tip]
> Dans notre cas, VirtualBox virtualise une carte réseau à partir des interfaces réseau de l'hôte, pratique !

1. Configurer votre machine virtuelle en allouant au minimum les besoins si dessus **si votre machine vous le permet**, si non diviser le CPU et la RAM par 2 (évidemment la vm sera plus lente).
2. Pour la partie compte utilisateur, vous êtes libre de choisir vos `login` & `password`.

> [!tip]
> La VM est uniquement accessible si vous êtes connectés à votre machine hôte, donc pas besoin d'être excessif au niveau du mot de passe.
>
> 💬 - *vous me remercierez plus tard*

1. Valider la configuration ! Vous n'avez plus qu'à suivre l'installation de votre nouveau système d'exploitation.

### Créer votre première VM Windows

1. Dans VirtualBox, créer une nouvelle machine virtuelle.
    Windows 11 nécessite [les caracteristiques suivantes](https://www.microsoft.com/en-us/windows/windows-11-specifications) pour fonctionner correctement:
    * 1 GHz de puissance de calcul.
    * 4 GB de RAM.
    * 64 GB de stockage.

2. Configurer votre machine virtuelle en allouant au minimum les besoins si dessus **si votre machine vous le permet**, si non diviser le CPU et la RAM par 2 (évidemment la vm sera plus lente).
3. Pour la partie compte utilisateur, vous êtes libre de choisir vos `login` & `password`.
4. Valider la configuration ! Vous n'avez plus qu'à suivre l'installation de votre nouveau système d'exploitation.

## Découvrir votre nouvel environnement

### VM Linux

Bienvenue sur votre machine virtuel ! 🚀

Connectez-vous avec les identifiants définis précédemment et essayez les commandes suivantes

> [!important]
> Par défault le clavier est en [QWERTY](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSJ1JrzRDzXo_nKM5gJI78-K5Es5MQL5GkbCw&s), ce qui est modifiable **après** la première connexion.
>
> 💬 - *je vous avais dis que vous me remercieriez :)*
> 
> Un petit coup de pouce :
>
> ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSJ1JrzRDzXo_nKM5gJI78-K5Es5MQL5GkbCw&s)

#### Configurer le clavier AZERTY - *facultatif*

[Tutoriel Ubuntu](https://doc.ubuntu-fr.org/tutoriel/configurer_le_clavier)

#### Mettre à jour le système

```bash
sudo apt update
sudo apt upgrade
```

#### Configurer la résolution écran

[Résolution écran Ubuntu x VirtualBox](https://www.tecmint.com/install-virtualbox-guest-additions-in-ubuntu/)

#### Ressources

Les resources disponibles dans l'environnement sont celles que vous avez définis lors de l'installation.

```bash
sudo apt-get install htop
```

```bash
htop
```

#### Reseaux

Tester la configuration reseau.

```shell
ifconfig
```

```shell
ping esirem.u-bourgogne.fr
```

```shell
curl wttr.in
```

**Que pouvez-vous observer sur cet environnement ?**

### VM Windows

Ouvrez une invite de commande (`Command prompt`) pour tester l'environnement via ces commandes:

* Accéder aux disques de stockage.

  ```shell
  diskmgmt
  ```

* Observer les resources disponibles.

  ```shell
  tskmgr
  ```

* Tester l'accès réseau.

  ```shell
  ipconfig
  ```

  ```shell
  ping esirem.u-bourgogne.fr
  ```

  ```shell
  curl wttr.in
  ```

**Que pouvez-vous observer sur cet environnement ?**

---

## Allez plus loin ? - *facultatif*

### WSL - Le linux embarquer pour windows

Le Sous-système Windows pour Linux (WSL) permet aux développeurs d’installer une distribution Linux (comme Ubuntu, OpenSUSE, Kali, Debian, Arch Linux, etc.) et d’utiliser des applications, des utilitaires et des outils en ligne de commande Bash Linux directement sur Windows, sans modification, sans devoir passer par une machine virtuelle traditionnelle ou une configuration à double démarrage (Dual Boot).

#### Comment ça marche ?

[Plongez dans le fonctionement de WSL 🧐](https://devblogs.microsoft.com/commandline/a-deep-dive-into-how-wsl-allows-windows-to-access-linux-files/)

#### Activer WSL sur votre machine Windows

Si il n'est pas déjà activé, vous pouvez le mettre en place en suivant un des tutoriaux suivants:

* [Tutoriel Windows](https://learn.microsoft.com/fr-fr/windows/wsl/install)
* [Tutoriel tiers](https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10)

---

### The hard way

> [!caution]
> With great power comes great responsibility 🕷️

Il est possible de partitionner complètement votre machine en deux OS isolés en réalisant un **Dual Boot**.

> [!tip]
> Très pratique pour optimimser l'utilisation des resources de votre machine au sein de chaque OS !
>
> Vous ne virtualiserez pas un Linux dans Windows mais les deux systèmes d'exploitation seront côte à côte.

Pour tenter l'expérience, voici quelques liens utiles:

* https://lecrabeinfo.net/installer-ubuntu-20-04-lts-dual-boot-windows-10.html
* https://www.youtube.com/watch?v=B97KkFDv86s
* https://opensource.com/article/18/5/dual-boot-linux

> [!caution]
> [@JeromeMSD](https://github.com/JeromeMSD) ne pourra être tenu responsable de l'état de vos machines suite à la réalisation d'un **Dual Boot**.
> Vos données et/ou des ressources sont sous votre entière responsabilité
