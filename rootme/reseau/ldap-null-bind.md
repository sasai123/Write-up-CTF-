# LDAP - Null Bind

## Infos

- Catégorie : Réseau
- Difficulté / Points :
- Fichiers / URL fournis : serveur LDAP sur `challenge01.root-me.org:54013`, base `dc=challenge01,dc=root-me,dc=org`

## Énoncé

Une nouvelle branche où un utilisateur anonyme se cache a été ajoutée à l'annuaire.

> Challenge réalisé au terminal (Linux) : les étapes sont retranscrites en commandes, il n'y a donc pas de captures d'écran.

## TL;DR

Le serveur LDAP accepte un **null bind** : on se connecte sans identifiants. En interrogeant la bonne branche de l'annuaire (`ou=anonymous`), on peut lister ses entrées et récupérer l'information cherchée.

## Reconnaissance

**LDAP** est un protocole qui permet de communiquer avec des annuaires. Un annuaire stocke des informations (nom, prénom, mail, mot de passe…) utilisées par exemple par Active Directory. LDAP sert aussi principalement à l'authentification à ces annuaires.

Infos fournies pour se connecter :
- Hôte : `challenge01.root-me.org`, port `54013`
- Base : `dc=challenge01,dc=root-me,dc=org`

J'utilise l'outil **ldapsearch**, qui permet de se connecter à un annuaire, d'extraire des données et d'interagir avec. La vulnérabilité du challenge est le **null bind** : se connecter sans fournir d'identifiant et énumérer la base.

## Exploitation

1. J'interroge d'abord la branche principale :
   ```bash
   ldapsearch -x -H ldap://challenge01.root-me.org:54013 -b "dc=challenge01,dc=root-me,dc=org" "(objectClass=*)"
   ```
   ```txt
   result: 50 Insufficient access
   ```
   Erreur `50` : le bind anonyme fonctionne, mais je n'ai pas les droits de lire cette branche.
2. L'énoncé parle d'une branche où se cache un anonyme. J'ajoute donc une unité d'organisation `ou=anonymous` devant la base (`ou` = branche secondaire) :
   ```bash
   ldapsearch -x -H ldap://challenge01.root-me.org:54013 -b "ou=anonymous,dc=challenge01,dc=root-me,dc=org"
   ```
3. Cette fois la requête réussit (`result: 0 Success`) et renvoie une entrée utilisateur (`uid=sabu`), avec notamment son champ `mail`, qui est le flag.

## Flag

`masqué` *(règles Root-Me)*

## Ce que j'ai appris

- Un **null bind** permet de s'authentifier anonymement sur un serveur LDAP mal configuré.
- Le code de retour `50 Insufficient access` veut dire que le bind est accepté mais qu'on n'a pas les droits de lecture : il faut viser la bonne branche.
- La structure d'un annuaire se lit de droite à gauche : `dc` (domaine) → `ou` (branche) → `uid` (entrée).
- Pour corriger : désactiver le bind anonyme et restreindre les droits de lecture sur l'annuaire.

## Ressources

- Voir aussi mon [cheatsheet ldapsearch](ldapsearch-cheatsheet.md).
