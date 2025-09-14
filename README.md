# 🔍 Guide d’Introduction à Nmap

## 📌 Version
![Version](https://img.shields.io/badge/version-1.0-blue?style=flat&logo=git)
![Mise à jour](https://img.shields.io/badge/last_update-14/09/2025-green?style=flat&logo=github)
![Nmap](https://img.shields.io/badge/tool-Nmap-yellow?style=flat&logo=linux)

---
## 📂 Table des matières
1. [Introduction à Nmap](#-introduction)  
2. [Installation de Nmap](#-installation-de-nmap)  
3. [Premiers pas avec Nmap](#-premiers-pas-avec-nmap)  
   - 3.1 Scan basique d’une IP  
   - 3.2 Scan d’un domaine  
   - 3.3 Scan d’une plage d’adresses  
4. [Options importantes de Nmap](#-options-importantes-de-nmap)  
   - 4.1 Scan de ports spécifiques (-p)  
   - 4.2 Détection de services et versions (-sV)  
   - 4.3 Détection d’OS (-O)  
   - 4.4 Scan furtif (SYN Scan, -sS) 
   - 4.5 Scan UDP (-sU)  
5. [Exportation et reporting](#-exportation-et-reporting)  
6. [Scripts NSE (Nmap Scripting Engine)](#-scripts-nse-nmap-scripting-engine)  
   - 6.1 Utilisation de scripts prédéfinis  
   - 6.2 Détection de vulnérabilités  
7. [Cas pratiques](#-cas-pratiques)  
   - 7.1 Analyse d’un réseau local  
   - 7.2 Scan d’un site web  
   - 7.3 Recherche de failles connues  
8. [Bonnes pratiques & limites](#-bonnes-pratiques--limites)  
9. [Ressources utiles](#-ressources-utiles)  
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
