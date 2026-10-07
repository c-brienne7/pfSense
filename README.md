# TP PfSense – DMZ & Addon  
**Auteur : Charles BRIENNE**  
**BTS SIO – SISR**  
**Date: 11/10/2026**

---

# 🧪 Mise en situation 1 — Monitoring du LAN

## 🎯 Objectif
Le DSI souhaite monitorer le trafic du réseau LAN via une interface web accessible depuis le navigateur.  
La solution doit être légère, simple à installer et compatible avec pfSense.

## 🟦 Choix de la solution : Darkstat
J’ai choisi **Darkstat**, un package pfSense permettant :

- la capture du trafic réseau en temps réel,
- l’analyse des flux par IP,
- l’affichage via une interface web intégrée,
- une installation simple et rapide.

Darkstat est **nativement supporté par pfSense**, ce qui garantit une intégration propre et fiable.

## 🟩 Installation
1. pfSense → **System → Package Manager → Available Packages**  
2. Recherche : `darkstat`  
3. Installation  
4. Activation via : **Services → Darkstat**

## 🟧 Validation
Accès à l’interface web :

http://192.168.20.1:666 (192.168.20.1 in Bing)

Darkstat affiche correctement :
- les IP du LAN,
- les volumes entrants/sortants,
- les connexions actives.

---

# 🧪 Mise en situation 2 — Serveur Windows dans la DMZ accessible en RDP

## 🎯 Objectif
Le DSI souhaite un serveur Windows dans la DMZ, accessible en RDP depuis Internet via un port WAN personnalisé.

Schéma demandé :

@WAN_PFSENSE:PORT_CHOISI → @WS_DMZ:3389

---

# 🟦 Mise en place de la DMZ
- Réseau DMZ : `192.168.6.0/24`  
- IP pfSense DMZ : `192.168.6.1`  
- Serveur Windows DMZ : `192.168.6.100`  
- RDP activé sur le serveur

---

# 🟩 NAT WAN → DMZ (RDP)

## 🔧 Configuration NAT
pfSense → **Firewall → NAT → Port Forward**

| Paramètre | Valeur |
|----------|--------|
| Interface | WAN |
| Protocole | TCP |
| Destination | WAN address |
| Port WAN | `50000` |
| Redirect target IP | `192.168.6.100` |
| Redirect target port | `3389` |
| Description | NAT RDP DMZ |
| Add associated firewall rule | ✔️ |

## 🟧 Validation
Test depuis un réseau externe (4G) :

mstsc → 192.168.20.183:50000

Connexion réussie → accès au serveur Windows dans la DMZ.

---

# 🌐 Mise en situation 3 — Accès Apache2 dans la DMZ depuis Internet

## 🎯 Objectif
Accéder à une VM Apache2 située dans la DMZ depuis Internet via un port WAN dédié.

## 🟦 Informations de la VM Apache
- IP Apache DMZ : `192.168.6.6`  
- Service : Apache2 (port 80)

---

# 🟩 NAT WAN → DMZ (Apache)

pfSense → **Firewall → NAT → Port Forward**

| Paramètre | Valeur |
|----------|--------|
| Interface | WAN |
| Protocole | TCP |
| Port WAN | `8080` |
| Redirect target IP | `192.168.6.6` |
| Redirect target port | `80` |
| Description | NAT Apache DMZ |

## 🟧 Validation
Test depuis un réseau externe :

http://192.168.20.183:8080 (192.168.20.183 in Bing)

Affichage de la page Apache2 → ✔️

---

# 📸 Captures à fournir
- Interface Darkstat  
- NAT RDP DMZ  
- NAT Apache DMZ  
- Règles WAN associées  
- Test RDP externe  
- Test Apache externe  
- Topologie DMZ / LAN / WAN  
- ipconfig du serveur Windows DMZ  

---
