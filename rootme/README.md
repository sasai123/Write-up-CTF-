# Root-Me

> Conformément aux règles de Root-Me, **aucun flag ni mot de passe de validation n'est publié**.
> Les writeups présentent la méthodologie, les notions apprises et la manière de corriger chaque faille.

## Web - Serveur

| Challenge | Points | Notions |
|---|---|---|
| [HTTP - Cookies](web-serveur/http-cookies.md) | 20 | Cookies, contrôle d'accès côté client |
| [JWT - Introduction](web-serveur/jwt-introduction.md) | 20 | JWT, algorithme `none` |
| [GraphQL - Introspection](web-serveur/graphql-introspection.md) | 20 | GraphQL, introspection |
| [Insecure Code Management](web-serveur/insecure-code-management.md) | 20 | `.git` exposé, git-dumper |
| [XSS - Server Side](web-serveur/xss-server-side.md) | 20 | XSS côté serveur, PDF dynamique |
| [Directory traversal](web-serveur/directory-traversal.md) | 25 | Path traversal |
| [File upload - Null byte](web-serveur/file-upload-null-byte.md) | 25 | Null byte `%00` |
| [Node - Eval](web-serveur/node-eval.md) | 30 | `eval()`, RCE Node.js |
| [File upload - Double extensions](web-serveur/file-upload-double-extensions.md) | — | Double extension, webshell PHP |
| [File upload - Type MIME](web-serveur/file-upload-type-mime.md) | — | Content-Type, Burp Suite |
| [Flask - Unsecure session](web-serveur/flask-unsecure-session.md) | — | Session Flask, flask-unsign |
| [API - Mass Assignment](web-serveur/api-mass-assignment.md) | — | API REST, escalade de privilèges |
| [CRLF](web-serveur/crlf.md) | — | Injection CRLF, log poisoning |
| [Nginx - SSRF Misconfiguration](web-serveur/nginx-ssrf-misconfiguration.md) | — | SSRF, config nginx, DNS |

## Web - Client

| Challenge | Points | Notions |
|---|---|---|
| [CSRF - 0 protection](web-client/csrf-0-protection.md) | — | CSRF, formulaire auto-soumis |
| [XSS - Stored 1](web-client/xss-stored-1.md) | — | XSS stockée, vol de session |

## Réseau

| Challenge | Points | Notions |
|---|---|---|
| [LDAP - Null Bind](reseau/ldap-null-bind.md) | — | LDAP, bind anonyme, ldapsearch |
| [Bluetooth - Fichier inconnu](reseau/bluetooth-fichier-inconnu.md) | — | Analyse de capture Bluetooth |

## Stéganographie

| Challenge | Points | Notions |
|---|---|---|
| [George et Alfred](steganographie/george-et-alfred.md) | — | Acrostiche |
| [Point à la ligne](steganographie/point-a-la-ligne.md) | — | Message caché par la ponctuation |
| [Twitter Secret Messages](steganographie/twitter-secret-messages.md) | — | Homoglyphes Unicode |
| [Steganomobile](steganographie/steganomobile.md) | — | Clavier T9 |
