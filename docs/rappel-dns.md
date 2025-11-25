# DNS - Rappel

Le **Domain Name System (DNS)** est un système distribué qui permet de faire la correspondance entre des noms de domaine (ex. `esirem.u-bourgogne.fr`) en adresses IP (ex. `193.52.234.14`). 
C'est un des composants clés du fonctionnement d'internet (souvent responsable d'incident majeur)

![It's always DNS](https://itsfoss.community/uploads/default/optimized/2X/a/ad3e17ff0a5a13b9c6f48716e7d35f965ec8137d_2_1024x1024.jpeg) | ![It was DNS](https://www.cscdbs.com/blog/wp-content/uploads/2021/05/Its-always-DNS-06-SSBroski.jpg)
|---|---|

## Résolution DNS
Voici les étapes principales lorsqu’un client souhaite accéder à un domaine :

1. Le navigateur vérifie le **cache local DNS**. (fichier `/etc/hosts` ou `C:\Windows\System32\drivers\etc\hosts`)
2. S’il ne trouve rien, il interroge le **resolver DNS** (souvent fourni par le FAI ou configuré manuellement).
3. Le resolver interroge les serveurs DNS de manière hiérarchique :
    - Serveur **racine** (`.`)
    - Serveur **TLD** (.com, .fr…)
    - Serveur **autoritatif** du domaine
4. Le serveur autoritatif renvoie l’adresse IP associée.
5. Le resolver met en cache la réponse pour une durée définie par le **TTL**.

```mermaid 

```

## Types d’enregistrements courants

| Type | Description |
|------|-------------|
| **A** | Associe un nom de domaine à une adresse IPv4 |
| **AAAA** | Associe un nom à une adresse IPv6 |
| **CNAME** | Alias vers un autre nom de domaine |
| **MX** | Serveurs de messagerie associés au domaine |
| **TXT** | Contient du texte (ex. SPF, vérifications Google, etc.) |
| **NS** | Indique les serveurs DNS autoritatifs |
| **SOA** | Informations administratives sur la zone |
| **PTR** | Résolution DNS inverse (IP → nom) |

**exemple**
```shell
❯ dig esirem.u-bourgogne.fr

; <<>> DiG 9.10.6 <<>> esirem.u-bourgogne.fr
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28118
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;esirem.u-bourgogne.fr.         IN      A

;; ANSWER SECTION:
esirem.u-bourgogne.fr.  2838    IN      CNAME   tokyo.dmz.u-bourgogne.fr.
tokyo.dmz.u-bourgogne.fr. 3079  IN      A       193.52.234.14
```

Ici, on retrouve:
- un enregistrement `CNAME` qui défini `esirem.u-bourgogne.fr.` -> `tokyo.dmz.u-bourgogne.fr.`
- un enregistrement `A` qui défini `esirem.u-bourgogne.fr.` en l'IPv4 `193.52.234.14` 

> [!tip]
> **Autres informations clés**
> Une **zone DNS** est une portion de l’espace de nommage gérée par un ensemble de serveurs DNS autoritatifs.
> 
> Dans `esirem.u-bourgogne.fr.` : `*.u-bourgogne.fr.` est une sous-zone de `*.fr.`, elle même une sous-zone de `*.`.
>
> Le **TTL (Time To Live)**  est une durée (en secondes) indiquant combien de temps un enregistrement peut rester en cache.
> 
> Les DNS peuvent être public ou privé
> - **DNS public** : accessible sur Internet (Google DNS 8.8.8.8, Cloudflare 1.1.1.1…)
> - **DNS privé** : sont accessible dans les entreprises ou les clusters via VPN et permettent de choisir son propre découpage IP.
