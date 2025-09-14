# 🔍 Guide d’Introduction à Nmap

## 📌 Version
![Version](https://img.shields.io/badge/version-1.0-blue?style=flat&logo=git)
![Mise à jour](https://img.shields.io/badge/last_update-14/09/2025-green?style=flat&logo=github)
![Nmap](https://img.shields.io/badge/tool-Nmap-yellow?style=flat&logo=linux)

---
## 📖 Introduction
**Nmap** (Network Mapper) est un outil très connu en cybersécurité et en administration réseau.
Il sert principalement à analyser un réseau et à découvrir les machines et services qui s’y trouvent.

**On l’utilise pour :**
- Savoir quels ordinateurs sont connectés sur un réseau.
- Découvrir quels ports (portes de communication) sont ouverts ou fermés.
- Identifier les services en cours d’exécution (exemple : serveur web, serveur FTP, SSH…).
- Avoir parfois une idée du système d’exploitation utilisé (Windows, Linux, etc.).

En résumé, **Nmap est comme une lampe torche pour voir ce qui se cache sur un réseau.
C’est pour cela qu’il est indispensable pour les :

- **🔐 Pentesters** (testeurs de sécurité)
- **👨‍💻 Administrateurs** systèmes/réseaux
- **🧑‍🎓 Étudiants en cybersécurité.**

**⚠️ Attention**: Nmap doit être utilisé uniquement sur des réseaux que tu as l’autorisation de scanner.
Sinon, cela peut être considéré comme une tentative d’attaque.

---

## 🎯 Objectifs du guide
- Comprendre les bases de Nmap.  
- Savoir réaliser un scan simple et interpréter les résultats.  
- Explorer les options principales de Nmap.  
- Apprendre à automatiser des analyses.  
- Approfondir avec des techniques avancées de scan utiles en pentesting.  

---
## 🛠️ Installation de Nmap
### 📌 Sur Kali Linux / Debian / Ubuntu
```bash
sudo apt update
sudo apt install nmap -y
```
➡️ Vérifier l’installation :
```bash
nmap --version
```
### 📌 Sur Fedora / CentOS / Red Hat
```bash
sudo dnf install nmap -y
```
ou
```bash
sudo yum install nmap -y
```
### 📌 Sur Windows
- Télécharger depuis le site officiel : https://nmap.org/download.html
- Lancer l’installateur (inclut Zenmap, interface graphique).
- Ouvrir cmd ou PowerShell et taper:
```bash
nmap --version
```

### 📌 Sur macOS
- Avec Homebrew :
```bash
brew install nmap
```
- Sinon, télécharger depuis : https://nmap.org/download.html

---
## Spécification des cibles
- Tout ce que tu mets après nmap (sans tiret -) est considéré comme une cible.
- Ça peut être : une IP, un nom de domaine, une plage d’IP, ou un fichier de liste d’IP.
  
A. **scan simple**:
<img width="951" height="510" alt="image" src="https://github.com/user-attachments/assets/b3c16ec4-778f-4876-913a-2e7f2d5dde00" />

**👉 Discrétion**: très basique, rien de furtif, on annonce clairement qu’on scanne.

B. **CIDR (notation réseau**
<img width="948" height="680" alt="image" src="https://github.com/user-attachments/assets/60ae3dc6-78eb-4b81-8301-7e7010d26201" />

---
## Découverte des hôtes (Host Discovery)
**👉 Objectif**: trouver quelles IP sont actives sur un réseau.
C’est l’équivalent d’un “ping sweep” : balayer une plage d’adresses et repérer qui répond.

- **-sL** : Liste simple
   - Ce que ça fait : affiche seulement la liste des IP (et noms DNS si dispo).
   - Discrétion : ⭐⭐⭐⭐⭐ (totalement discret, aucun paquet envoyé).
   - Utilité : vérifier ta syntaxe avant un vrai scan.

- **-sn** : Ping scan
   - Envoie un ping ICMP sur le port par defaut TCP/80 pour voir qui est en ligne.
   - Il est peu visible dans les logs

- **-Pn** : Pas de ping
     - suppose que toutes les machines sont actives et lance directement un scan complet.
     - très bruyant, car tu touches tout
     - utile si le réseau bloque les pings ICMP.

- **-PS** : Ping TCP SYN
     - envoie des paquets SYN (comme pour initier une connexion). Si la cible répond (SYN/ACK ou RST), elle est active.
     - contourner les pare-feux qui bloquent ICMP mais pas TCP.

- **-PA** : Ping TCP ACK
   - envoie un paquet ACK (comme si une connexion existait déjà). La cible répond souvent par un RST.
   - plus discret que SYN, utile contre certains pare-feux
   - contourner les firewalls qui bloquent les SYN mais laissent passer les ACK.

- **-PU** : Ping UDP
   - envoie un paquet UDP vide.
        - Si le port est fermé → la cible renvoie un ICMP port unreachable.
        - Si le port est ouvert → souvent aucune réponse.
   - détecter des hôtes derrière un firewall qui filtre TCP mais pas UDP.

- **-PE, -PP, -PM** : Ping ICMP
   - Envoie différents types de pings ICMP.
   - facilement bloqué par pare-feux
   - tester si certains types d’ICMP passent alors que d’autres sont bloqués.

- **-PR** : Ping ARP (Lan local)
   - Envoie des requêtes ARP → ultra efficace sur un réseau local.
   - méthode la plus fiable pour un réseau LAN.

- **-PO** : Ping par protocole IP
   - envoie des paquets IP avec différents protocoles (ICMP, IGMP, etc.).
   - peu commun, peut passer inaperçu sur certains IDS
   - détecter des hôtes même quand TCP et ICMP sont bloqués.

- **reason** : Pourquoi la cible est considérée active
   - indique pourquoi une machine est marquée active (ex : réponse ICMP, RST, SYN/ACK).
   - comprendre comment Nmap a détecté un hôte.

---
 ## Les 6 états de ports dans Nmap
 
