# Twitter Secret Messages

## Infos

- Catégorie : Stéganographie
- Difficulté / Points :
- Fichiers / URL fournis : une phrase en anglais

## Énoncé

*(à compléter)*

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

La phrase contient des **homoglyphes** : des caractères qui ressemblent à des lettres normales mais qui sont en réalité d'autres caractères Unicode (cyrillique, fullwidth…). Ces caractères marquent un message caché, qu'un outil permet de décoder.

## Reconnaissance

On nous donne une phrase :

```txt
Ｃhｏose a jοｂ yоu lονｅ, and you ｗіｌl ｎeｖｅｒ have tο ｗｏrk a day in yοur lіfｅ.
```

En cherchant ce qu'est un **homoglyphe** : c'est un caractère qui ressemble à un autre à l'œil nu, mais qui est différent en interne. Par exemple le `e` en ASCII et le `е` en cyrillique sont visuellement identiques.

## Exploitation

1. J'utilise un outil en ligne (détecteur d'homoglyphes) qui repère ces caractères spéciaux : il en trouve 22 dans la phrase.
2. En ne gardant que les lettres correspondant aux homoglyphes, on obtient une suite qui ne veut rien dire telle quelle.
3. Je passe par un outil de stéganographie qui fait à la fois le repérage et le décodage du message. Il révèle la phrase cachée, qui est le flag.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Les **homoglyphes** servent à cacher de l'information dans un texte qui a l'air parfaitement normal.
- Un même symbole affiché peut correspondre à plusieurs codes Unicode : regarder les codes (U+XXXX) permet de repérer l'anomalie.
