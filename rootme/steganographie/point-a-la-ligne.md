# Point à la ligne

## Infos

- Catégorie : Stéganographie
- Difficulté / Points :
- Fichiers / URL fournis : un texte

## Énoncé

*(à compléter)*

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Le message est caché autour des **points** du texte. En lisant les lettres situées au-dessus/autour de chaque point, dans le bon sens, on reconstitue le flag.

## Reconnaissance

J'ai d'abord pensé à un message caché qu'on ne voyait pas, dans les métadonnées ou les données exif. Mais le nom du challenge n'oriente pas vers ça.

Après avoir lu le texte, j'ai trouvé une façon de rédiger assez bizarre : il doit y avoir un lien avec les **points**.

## Exploitation

J'ai regardé les lettres placées juste avant et après chaque point, et j'ai testé plusieurs combinaisons (première lettre, dernière lettre, lecture depuis la fin, code César…). La plupart ne donnaient rien de lisible.

La bonne piste : en prenant les lettres situées **au-dessus du point**, on obtient une station de métro parisienne suivie d'une heure. C'est la combinaison la plus cohérente.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- En stéganographie sur du texte, la mise en forme (ponctuation, espaces, retours à la ligne) est souvent le support du message.
- Il faut tester méthodiquement plusieurs sens de lecture avant de trouver le bon, et garder celui qui donne un résultat cohérent.
