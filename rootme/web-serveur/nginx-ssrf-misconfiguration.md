# Nginx - SSRF Misconfiguration

## Infos

- Catégorie : Web - Serveur
- Difficulté / Points :
- Fichiers / URL fournis : l'application + sa configuration nginx

## Énoncé

*(à compléter)*

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Le bloc `/dir_enum` de la config nginx recolle ce qu'on écrit après `dir_enum` derrière le nom du serveur backend, sans `/`. On contrôle donc la fin du nom d'hôte appelé. En le faisant pointer vers `127.0.0.1`, nginx se fait une requête à lui-même et donne accès au dossier `/uploads/`, normalement réservé au local : c'est une **SSRF** (*Server-Side Request Forgery*) due à une mauvaise configuration.

## Reconnaissance

J'ai d'abord lu toute la config nginx pour comprendre ce que fait chaque bloc :

```nginx
location /uploads/ {
    allow 127.0.0.1;   # autorise seulement les requêtes venant du serveur lui-même
    deny all;          # rejette les autres
    autoindex on;      # affiche la liste des fichiers du dossier
    alias /var/www/app/uploads/;
}

location ~ /dir_enum(.*) {
    # tout ce qui suit /dir_enum est enregistré dans $1
    proxy_pass http://web-serveur-ch94-apache$1;  # requête vers ce nom + $1, collés
    proxy_redirect off;
}
```

Deux blocs ressortent : `/uploads/`, qui liste les fichiers mais seulement pour `127.0.0.1`, et `/dir_enum`, dont je contrôle la fin de l'URL appelée. Comme `$1` est collé **directement** après le nom du backend (sans `/`), je ne contrôle pas que le chemin, mais aussi la fin du **nom d'hôte**.

Mon idée : me servir de `/dir_enum` pour que nginx fasse lui-même la requête vers `/uploads/`, depuis `127.0.0.1`.

## Exploitation

1. J'ai d'abord tenté une confusion du parser d'URL avec un `@`, pour que l'hôte devienne `127.0.0.1`. J'obtiens une erreur `502 Bad Gateway`, donc ça ne passe pas.
2. L'objectif restait de faire résoudre le nom final vers `127.0.0.1`. J'utilise un service DNS « wildcard » (`nip.io`) : un nom de la forme `quelquechose.127.0.0.1.nip.io` se résout toujours vers `127.0.0.1`. Le préfixe que nginx colle devant est alors ignoré, seule la fin compte.
3. Avec cette astuce (trouvée dans une ressource de pentest sur la SSRF), la requête reconstruite part bien en loopback. La condition `allow 127.0.0.1;` du bloc `/uploads/` est satisfaite, l'`autoindex` devient accessible, et je peux lister le dossier puis trouver le fichier qui contient le flag.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Une **SSRF** permet de faire émettre au serveur une requête vers une destination qu'on choisit, ici son propre réseau interne.
- Quand un serveur se connecte à un **nom** plutôt qu'à une **IP**, c'est le DNS qui décide de la destination réelle. Contrôler la réponse DNS (via `nip.io`, `sslip.io`…) revient à rediriger le serveur où on veut.
- Coller `$1` directement derrière un nom d'hôte dans `proxy_pass` est l'erreur de config qui rend tout ça possible.
- Pour corriger : ajouter un `/` de séparation dans `proxy_pass`, valider strictement l'entrée, et ne pas se reposer uniquement sur `allow 127.0.0.1;` pour protéger une ressource.
