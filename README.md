# DAT – Architecture Réseau Sécurisée (PME multi-sites) – ISR-SRE4

Ce dépôt contient le **Document d’Architecture Technique (DAT)** du projet ISR-SRE4 : conception d’une **architecture réseau sécurisée** pour une PME aéronautique répartie sur **2 sites** (≈100 utilisateurs) avec interconnexion, accès nomade, Wi‑Fi séparés et exposition d’un serveur Web en HTTPS.

---

## 🎯 Objectifs du projet

- Concevoir une architecture **multi-sites** sécurisée et exploitable  
- Mettre en place :
  - **Segmentation VLAN** (métiers, serveurs, DMZ, Wi‑Fi, admin…)
  - **VPN site-à-site IPsec** entre les deux sites
  - **VPN nomade** (SSL VPN ou IKEv2) avec **MFA**
  - **Wi‑Fi collaborateurs** + **Wi‑Fi invités** isolé (Internet only)
  - **DMZ** pour le serveur Web accessible en **HTTPS**
- Fournir une **étude de risques** + des mesures de réduction associées

---

## 🧩 Périmètre & hypothèses (résumé)

- Les serveurs internes existent déjà (fichiers, mail, BDD) et sont au **Site 1**
- Le Site 2 n’héberge pas de serveurs métiers, uniquement un serveur “infra” (RADIUS/DNS cache/logs/supervision)
- Solutions **agnostiques constructeur** (pas dépendantes d’une marque)
- Pas de chiffrage financier ni PCA/PRA avancé dans le scope

---

## 🏗️ Architecture (vue d’ensemble)

- **Zones logiques** : Utilisateurs, Données sensibles (RH/Finance/Direction), Serveurs internes, DMZ, Wi‑Fi collab, Wi‑Fi invités, Infra/management, VPN nomades, VPN intersites
- **Filtrage centralisé via pare-feu** (inter‑VLAN + WAN)
- **DMZ** : exposition uniquement en **443**, flux DMZ→BDD strictement limités
- **Journalisation / supervision** : Syslog + SNMP sur VLAN Infra

---

## 🧠 Segmentation VLAN (exemples)

- VLAN 10 : Technique / Production  
- VLAN 20 : Commercial  
- VLAN 30 : Finance  
- VLAN 40 : RH  
- VLAN 50 : Direction  
- VLAN 60 : Serveurs internes  
- VLAN 70 : DMZ (serveur Web)  
- VLAN 80 : Wi‑Fi collaborateurs  
- VLAN 90 : Wi‑Fi invités (isolé)  
- VLAN 100 : Infrastructure/management  
- VLAN 110 : VPN nomades  
- VLAN 120 : VPN site‑à‑site  

---

## 🌐 Plan d’adressage (principe)

- RFC1918
- Site 1 : `10.10.<VLAN>.0/24`
- Site 2 : `10.20.<VLAN>.0/24`
- Passerelle : `.1`
- Statique : `.10 – .49`
- DHCP : `.50 – .199`
- Réserves : `.200 – .254`
- Pools VPN : `10.200.110.0/24` (nomades) et `10.200.120.0/30` (S2S si besoin)

---

## 🔐 Sécurité (mesures clés)

- **Pare-feu NGFW** : inspection stateful, règles WAN/DMZ/VPN, journalisation
- **Wi‑Fi**
  - Collab : WPA2/WPA3‑Enterprise + RADIUS (802.1X), isolation client‑to‑client
  - Invités : VLAN dédié, **Internet only via NAT**, aucun routage interne
- **VPN**
  - Site‑à‑site : IPsec/IKEv2, AES‑256/SHA‑256, UDP 500/4500, ACL restrictives
  - Nomades : SSL VPN ou IKEv2 + **MFA**, filtrage par rôle (RBAC)
- **LAN**
  - Ports inutilisés désactivés, anti‑spoofing (DHCP snooping, DAI…), admin via VLAN infra

---

## 📄 Livrable

- `dossier/DAT.pdf` : Document complet (risques, architecture logique, VLAN, adressage, VPN, sécurité LAN/WAN, schémas, etc.)

---

## 📁 Structure conseillée du dépôt

```text
.
├── dossier/
│   └── DAT.pdf
└── README.md
```

---

## 👤 Auteur

- Thomas Teixeira

---

## 📚 Référentiels

- RGPD
- ISO/IEC 27001 & 27002
- Guides ANSSI
