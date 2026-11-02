# Steganomobile

## Infos

- Catégorie : Stéganographie
- Difficulté / Points :
- Fichiers / URL fournis : une suite de chiffres ressemblant à un numéro de téléphone

## Énoncé

*(à compléter)*

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Les chiffres correspondent au clavier **T9** d'un vieux téléphone : chaque groupe de chiffres désigne une lettre selon le nombre d'appuis sur une touche. En les décodant, on obtient le mot caché.

## Reconnaissance

On nous donne quelque chose qui ressemble à un numéro de téléphone :

```txt
222-33-555-555-7-44-666-66-33
```

J'ai pensé qu'on pouvait transformer ce numéro en lettres. Je suis parti regarder les claviers qui associent lettres et chiffres, comme celui des anciens téléphones (Nokia). Ça ne m'a pas pris longtemps à comprendre, même si je n'avais jamais tapé sur un Nokia.

## Exploitation

Sur un clavier T9, le nombre de fois qu'on appuie sur une touche donne la lettre. Par exemple `2` une fois = `a`, deux fois = `b`, trois fois (`222`) = `c` ; `33` = `e` (deux appuis sur la touche 3).

En décodant chaque groupe de la suite, on reconstitue le mot caché, qui est le flag. *(Décodage complet volontairement non publié.)*

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Le **T9** (saisie multi-appui des anciens téléphones) est un classique de l'encodage en stéganographie.
- Un format qui ressemble à un numéro de téléphone (groupes de chiffres identiques répétés) est un bon indice pour penser au clavier T9.
