# ldapsearch — Cheatsheet

## Options de base
| Option | Rôle |
|--------|------|
| `-x` | Bind simple (auth basique) |
| `-H ldap://host:389` | URL du serveur (389 = LDAP, 636 = LDAPS) |
| `-b "dc=..."` | Base DN (point de départ dans l'arbre) |
| `-D "cn=user,dc=..."` | DN de bind (identité de connexion) |
| `-w pass` / `-W` | Mot de passe (en clair / interactif) |
| `-s base\|one\|sub` | Portée : l'entrée / enfants directs / tout le sous-arbre |
| `-LLL` | Sortie propre (sans commentaires ni version) |

## Découverte (anonyme)
```bash
# RootDSE : infos serveur, base DN, SASL supportés
ldapsearch -x -LLL -H ldap://cible -s base -b "" "*" "+"

# Juste la/les base(s) DN
ldapsearch -x -LLL -H ldap://cible -s base -b "" namingContexts

# Mécanismes SASL disponibles
ldapsearch -x -LLL -H ldap://cible -s base -b "" supportedSASLMechanisms
```

## Null bind (anonyme)
```bash
# Bind anonyme = pas de -D ni -w
ldapsearch -x -H ldap://cible -b "dc=..."

# Bind non authentifié (DN + mot de passe vide)
ldapsearch -x -H ldap://cible -D "cn=admin,dc=..." -w "" -b "dc=..."
```

## Énumération
```bash
# Dump complet (selon droits)
ldapsearch -x -LLL -H ldap://cible -b "dc=..."

# Lister les personnes, attributs choisis
ldapsearch -x -LLL -H ldap://cible -b "dc=..." "(objectClass=person)" cn uid mail

# Bind authentifié
ldapsearch -x -LLL -H ldap://cible -D "cn=user,dc=..." -w pass -b "dc=..."
```

## Schéma (souvent lisible en anonyme)
```bash
# Contenu du Subschema (attributs opérationnels avec "+")
ldapsearch -x -LLL -H ldap://cible -s base -b "cn=Subschema" "+"

# Liste propre des noms d'attributs
ldapsearch -x -LLL -H ldap://cible -s base -b "cn=Subschema" attributeTypes \
  | grep -oiE "NAME '[^']+'" | sort -u
```

## Filtres utiles
```
(uid=jean)              égalité
(uid=*)                 joker (existence)
(&(a=x)(b=y))           ET
(|(a=x)(b=y))           OU
(objectClass=person)    tous les objets de ce type
```

## Chercher des secrets
```bash
ldapsearch -x -LLL -H ldap://cible -b "dc=..." | grep -iE "pass|secret|flag|pwd|description|info"
```

## Codes de retour fréquents
| Code | Sens |
|------|------|
| `0`  | Succès |
| `48` | Inappropriate auth (anonyme refusé) |
| `49` | Invalid credentials (mauvais identifiants) |
| `50` | Insufficient access (bind OK mais pas les droits de lecture) |
| `32` | No such object (base DN inexistante) |
