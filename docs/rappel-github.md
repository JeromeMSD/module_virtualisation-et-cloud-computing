# Github & Readme - RAPPEL

Vos fichiers `README.md` peuvent être améliorés et plus lisible en utilisant les fonctionnalités suivantes.

## Titres & sections

```text
# Titre principale
## Titre secondaire
### Titre tertiaire 
#### etc..

---

***
```

## Insertions & code

### Mise en forme de texte

```text
Ce _projet_ a été réalisé via **Kubernetes** et l'outil `kubectl`.
```

Ce _projet_ a été réalisé via **Kubernetes** et l'outil `kubectl`.

### Images & badges

###### code markdown

```text
![image](./chemin-image-du-depot/image.png)

![ESIREM](https://upload.wikimedia.org/wikipedia/commons/1/1f/Polytech_DIJON.png)

![badge](https://img.shields.io/badge/terraform-8957E5?style=for-the-badge&logo=terraform&logoColor=white)
```

###### résultats

![image](./chemin-image-du-depot/image.png)

![ESIREM](https://upload.wikimedia.org/wikipedia/commons/1/1f/Polytech_DIJON.png)

![badge](https://img.shields.io/badge/terraform-8957E5?style=for-the-badge&logo=terraform&logoColor=white)

### Bloc de code

###### code markdown

```text

```<langage>
<code>
```

```python
def hello():
    println("Hello world !")
```

```dockerfile
FROM nginx:latest
COPY . /usr/share/nginx/html/
```
```

###### résultats

```python
def hello():
    println("Hello world !")
```

```dockerfile
FROM nginx:latest
COPY . /usr/share/nginx/html/
```
