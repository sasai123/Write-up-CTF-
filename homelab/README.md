# 🖥️ Homelab

## 🎯 Objectif

Ce que tu veux apprendre avec ce lab (ex. : Active Directory, SOC/détection, segmentation réseau, pentest interne).

## 🧱 Matériel

| Élément | Détail |
|---|---|
| Hyperviseur | ex. Proxmox VE sur mini-PC (CPU, RAM, stockage) |
| Réseau | ex. pfSense/OPNsense, switch manageable, VLANs |

## 🗺️ Architecture

<!-- Ajoute un schéma : docs/architecture.png (draw.io, Excalidraw) -->
![Architecture](docs/architecture.png)

| VLAN | Rôle | Machines |
|---|---|---|
| 10 | Admin | Proxmox, pfSense |
| 20 | Lab AD | DC01 (Windows Server), WS01, WS02 |
| 30 | Attaque | Kali |
| 40 | SOC | Wazuh / ELK, Security Onion |

## 🚦 Roadmap

| Statut | Étape | Notes |
|---|---|---|
| ✅ | Installation de l'hyperviseur | |
| ✅ | Firewall + VLANs | |
| 🟡 | Domaine Active Directory | en cours |
| ⬜ | SIEM (Wazuh) + remontée de logs | |
| ⬜ | Scénarios d'attaque AD + détection | Kerberoasting, AS-REP roasting… |

## 📓 Journal

- AAAA-MM-JJ — Ce que j'ai fait, problème rencontré, solution, ce que j'ai appris.

> 🔒 Ne publie jamais d'IP publique, de mot de passe, de clé ou de config contenant des secrets.
