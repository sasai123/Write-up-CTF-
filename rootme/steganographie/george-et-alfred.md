# George et Alfred

## Infos

- Catégorie : Stéganographie
- Difficulté / Points :
- Fichiers / URL fournis : un texte (correspondance entre George Sand et Alfred de Musset)

## Énoncé

Un échange de lettres. Utilisez la dernière « phrase cachée » pour valider l'épreuve.

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Le texte cache un message par **acrostiche** : il faut lire seulement le premier mot de chaque ligne. La dernière phrase cachée donne le flag.

## Reconnaissance

Ce challenge m'a bien fait rire. En lisant le texte, j'ai tout de suite vu les sous-entendus du premier paragraphe, mais je savais qu'il était impossible que ce soit ça, la phrase cachée.

En lisant le deuxième paragraphe, une phrase m'a mis la puce à l'oreille : « lisez le premier mot ». J'ai donc relu tous les premiers mots du premier paragraphe : rien. Et là, en faisant pareil sur le texte suivant, le message caché apparaît.

## Exploitation

En lisant uniquement le **premier mot de chaque ligne**, la réponse de Musset forme une phrase, et celle de George Sand en forme une autre. C'est la dernière, demandée par l'énoncé, qui sert de flag.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Un **acrostiche** est un message caché formé par les premières lettres (ou premiers mots) de chaque ligne.
- L'indice était directement dans le texte (« lisez le premier mot ») : il faut toujours bien lire l'énoncé et le contenu avant de chercher compliqué.
