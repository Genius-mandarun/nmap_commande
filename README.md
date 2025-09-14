# 🔍 Guide d’Introduction à Nmap

## 📌 Version
- **Version du document** : 1.0  
- **Dernière mise à jour** : 14/09/2025  
- **Outil étudié** : [Nmap](https://nmap.org/) – Network Mapper

---

## 📖 Introduction
Nmap (**Network Mapper**) est un outil open-source puissant utilisé pour :  
- La **découverte d’hôtes** sur un réseau.  
- L’**analyse des ports** ouverts.  
- La **détection des services** et versions en cours d’exécution.  
- L’**identification du système d’exploitation** et des vulnérabilités potentielles.  

Il est largement utilisé en **administration système**, **cybersécurité** et surtout en **pentesting**, car il permet de collecter des informations précieuses sur les machines cibles.  

L’objectif de ce guide est d’apprendre progressivement les commandes et options les plus importantes de Nmap, de la base vers l’avancé.

---

## 🎯 Objectifs du guide
- Comprendre les bases de Nmap.  
- Savoir réaliser un scan simple et interpréter les résultats.  
- Explorer les options principales de Nmap.  
- Apprendre à automatiser des analyses.  
- Approfondir avec des techniques avancées de scan utiles en pentesting.  

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

## 🛠️ Installation de Nmap
Sur **Kali Linux**, Nmap est généralement préinstallé.  
Sinon, installez-le avec :  

```bash
sudo apt update && sudo apt install nmap -y
