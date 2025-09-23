<img width="2500" height="1307" alt="image" src="https://github.com/user-attachments/assets/7c71f1cd-27c6-4a50-9ad8-070282be4702" />
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
- **Ouvert (open)** : une application est à l’écoute et accepte les connexions (TCP ou UDP).
- **Fermé (closed)** : Le port répond, mais aucun service n'écoute
- **Filtré (filtered)** : Nmap ne reçoit aucune réponse, ou bien un message ICMP disant que la communication est bloquée.
- **Non-filtré (unfiltered)** : le port est accessible, mais Nmap ne peut pas dire s’il est ouvert ou fermé.
- **Ouvert | Filtré (open|filtered)** : Nmap n’arrive pas à décider si le port est ouvert ou filtré, car les deux cas ne répondent pas.

---
## Techniques de scan de ports dans Nmap
- **-sS** : Scan SYN (demi-ouvert)
    - Principe : envoie un SYN, attend la réponse :
      - SYN/ACK → port ouvert
      - RST → port fermé
      - Pas de réponse → filtré

- **-sT** : Scan TCP connect()
    - Principe : demande directement au système (API sockets) d’ouvrir une connexion → donc connexion complète (SYN → SYN/ACK → ACK).
    - plus lent, plus bruyant (chaque tentative est loggée par la cible).

- **sU** : Scan UDP
  - Principe : envoie un paquet UDP vide
      - ICMP “port unreachable” → fermé
      - Réponse UDP → ouvert
      - Pas de réponse → open|filtered

- **-sN, -sF, -sX** : Scans furtifs RFC (Null, FIN, Xmas)
    - Principe : envoie des paquets bizarres (sans SYN/ACK, ou avec FIN/URG/PSH).
    - inefficace sur Windows et Cisco (qui répondent toujours RST).

- **-sA** : Scan ACK
  - cartographie les règles de firewall (stateful/stateless).
  - ne donne pas “open/closed”.
  - Sert pour comprendre la topologie firewall.

- **--scanflags** : Scan personnalisé
  - tu choisis toi-même les drapeaux TCP.
  - permet de tromper IDS/firewalls mal configurés.
  - demande de l’expérience.
  - variable (peut être très furtif ou très bruyant).

- **-sI** : Idle scan (scan zombie)
  - utilise un “zombie” (machine tierce) → ta cible croit être scannée par lui.
  - ultra furtif 🕵️ (ton IP n’apparaît jamais).
  - trouver un zombie valide est difficile.

- **-sO** : Scan du protocole IP
  - teste quels protocoles IP (TCP, UDP, ICMP, IGMP…) sont supportés.
  - Utile pour cartographie bas niveau.

**NB** : 
- Pour 80% des cas → utilise -sS (rapide + discret).
- Pour UDP → combine -sS -sU.
- Pour furtivité → pense au -sI (idle scan).

---
## Détection de services et de versions

- **-sV** :  Permet de detecter la version
  - **But** : interroger les ports découverts pour connaître le service et la version.
  - **Pour qui** : admins, pentesters (avec autorisation), inventaires.
  - Envoie plusieurs requêtes, donc visible dans les logs.

- **--version-intensity <0–9>** :
  - sert à régler la quantité d’efforts que l’outil va mettre pour identifier la version exacte d’un service qui tourne sur un port.
  - choisir combien de tests/probes Nmap va essayer (0 = très peu, 9 = tout).
  - Taux de détection :
      - 0–2 : faible → moyen
      - 3–6 : moyen
      - 7–9 : élevé
  - plus l’intensité monte, plus on est bruyant.

- **--version-light** :
  - Raccourci pour --version-intensity 2.
  -  il peut rater des versions que --version-all ou une intensité élevée auraient trouvées.


- **--version-all** :
  - raccourci pour --version-intensity 9.

- **--version-trace** :
  - affiche tout ce que Nmap envoie/reçoit lors de la détection (debugging).

---
## Détection OS 

- **-O** : demander à Nmap d’essayer d’identifier le système d’exploitation distant.
- **--osscan-limit** :  n’essaye l’OS detection que si Nmap a trouvé au moins un port ouvert ET un port fermé (ou conditions favorables).
  - Permet de gagner du temps et éviter le faux positif
  - améliore la discrétion globale parce qu’on évite de lancer des tests OS inutiles.

- **--max-os-tries <n>** : nombre max de tentatives OS
  - contrôler combien de fois Nmap retentera la détection d’OS sur une cible.
  - 
---
## Nmap Scripting Engine (NSE)

- **--script <catégories|répertoire|nom|all>** :  contrôle précisément quels scripts exécuter : noms, catégories (ex: vuln), répertoire local, ou all

  - **Pour qui :** admins / pentesters qui veulent cibler un type précis de test.
  
  - **Taux d’utilité :** très élevé si tu choisis des scripts pertinents.
  
  - **Discrétion :** dépend des scripts choisis — vuln et intrusive sont bruyants.

- **--script-args=<name1=value1,name2={...}>** : Passe des paramètres aux scripts (login, mot de passe, options spécifiques).
  - nécessaire pour scripts qui requièrent des credentials ou configuration (bruteforce, auth tests).


---
## Timing et Performances

---
## Évitement de pare-feux/IDS et mystification
Les options d’« évasion » ne rendent pas un scan invisible : elles modifient le signal pour contourner des règles mal conçues, tester une défense, ou ralentir la détection. Elles peuvent toutefois produire des effets indésirables (crashs, imprécisions) et déclencher des mesures légales si utilisées sans droit.

- **-f (fragmentation) et --mtu <taille>** :  découpe les paquets IP en petits fragments (par défaut 8 bytes après l’en-tête, -f deux fois = 16, ou --mtu pour valeur précise).

- **-D <decoy1,decoy2,...,ME>** :  ajoute des adresses IP leurres dans le scan pour masquer l’origine réelle.

- **-S <IP>** :  falsifie l’adresse IP source utilisée pour envoyer les paquets.


---
## 
