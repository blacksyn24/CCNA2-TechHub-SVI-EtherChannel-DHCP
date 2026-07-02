# 💻 CCNA2 — VLANs + SVI L3 + EtherChannel + DHCPv4 | TechHub Bénin

![Cisco](https://img.shields.io/badge/Cisco-CCNA2-blue?style=for-the-badge&logo=cisco&logoColor=white)
![PacketTracer](https://img.shields.io/badge/Packet%20Tracer-8.x-orange?style=for-the-badge&logo=cisco)
![Status](https://img.shields.io/badge/Status-✅%20Completed-brightgreen?style=for-the-badge)
![VLANs](https://img.shields.io/badge/VLANs-4-purple?style=for-the-badge)
![Switches](https://img.shields.io/badge/Switches-3-red?style=for-the-badge)
![PCs](https://img.shields.io/badge/PCs-12-yellow?style=for-the-badge)
![DHCP](https://img.shields.io/badge/DHCP-Automatique-green?style=for-the-badge)
![EtherChannel](https://img.shields.io/badge/EtherChannel-LACP-blue?style=for-the-badge)
![SVI](https://img.shields.io/badge/Routing-SVI%20Switch%20L3-lightblue?style=for-the-badge)

---

## 📋 Description

Ce TP simule le réseau de l'entreprise **TechHub Bénin** 💻 basée
à Cotonou. Le réseau est segmenté en **4 VLANs** représentant
les départements de l'entreprise. Le routage inter-VLAN est assuré
par des **SVI sur un switch L3**, les liens sont agrégés via
**EtherChannel LACP** et les IPs distribuées automatiquement
via **DHCPv4**.

### Objectifs
- ✅ Créer et configurer **4 VLANs**
- ✅ Configurer le routage inter-VLAN via **SVI Switch L3**
- ✅ Configurer **EtherChannel LACP** (Po1 et Po2)
- ✅ Configurer **DHCPv4** sur le switch L3
- ✅ Tester la **redondance EtherChannel**
- ✅ Tester la **connectivité inter-VLAN**

---

## 🖥️ Équipements

| Équipement | Modèle | Nom | Rôle |
|-----------|--------|-----|------|
| 🔀 Switch L3 | Cisco 3560-24PS | SW-CORE | Routage SVI + DHCP |
| 🔌 Switch L2 | Cisco 2960-24TT | SW-A | Accès étage 1 |
| 🔌 Switch L2 | Cisco 2960-24TT | SW-B | Accès étage 2 |
| 💻 PC | PC-PT | PC1 à PC12 | Employés (3 par VLAN) |

**Total : 1 switch L3 + 2 switches L2 + 12 PC**

---

## 🗺️ Topologie

```
PC1──Fa0/3──┐                         ┌──Fa0/3──PC7
PC2──Fa0/4──┤                         ├──Fa0/4──PC8
PC3──Fa0/5──┤        [SW-A]           ├──Fa0/5──PC9
PC4──Fa0/6──┤       //                ├──Fa0/6──PC10
PC5──Fa0/7──┤  Po1 (Fa0/1+Fa0/2)     ├──Fa0/7──PC11
PC6──Fa0/8──┘  //                     └──Fa0/8──PC12
              //                            \\
         [SW-CORE L3]──Po2 (Fa0/3+Fa0/4)──[SW-B]
```
<img width="1920" height="1080" alt="Capture d’écran du 2026-07-02 21-08-31" src="https://github.com/user-attachments/assets/c2579d4d-4df8-482e-b5a7-57710eda8a19" />
---

## 🔌 Câblage complet

| De | Port | Vers | Port | Rôle |
|----|------|------|------|------|
| SW-CORE | Fa0/1 | SW-A | Fa0/1 | EtherChannel Po1 lien 1 |
| SW-CORE | Fa0/2 | SW-A | Fa0/2 | EtherChannel Po1 lien 2 |
| SW-CORE | Fa0/3 | SW-B | Fa0/1 | EtherChannel Po2 lien 1 |
| SW-CORE | Fa0/4 | SW-B | Fa0/2 | EtherChannel Po2 lien 2 |
| SW-A | Fa0/3 | PC1 | Fa0 | VLAN 10 DEV |
| SW-A | Fa0/4 | PC2 | Fa0 | VLAN 10 DEV |
| SW-A | Fa0/5 | PC3 | Fa0 | VLAN 10 DEV |
| SW-A | Fa0/6 | PC4 | Fa0 | VLAN 20 DESIGN |
| SW-A | Fa0/7 | PC5 | Fa0 | VLAN 20 DESIGN |
| SW-A | Fa0/8 | PC6 | Fa0 | VLAN 20 DESIGN |
| SW-B | Fa0/3 | PC7 | Fa0 | VLAN 30 MARKETING |
| SW-B | Fa0/4 | PC8 | Fa0 | VLAN 30 MARKETING |
| SW-B | Fa0/5 | PC9 | Fa0 | VLAN 30 MARKETING |
| SW-B | Fa0/6 | PC10 | Fa0 | VLAN 40 ADMIN |
| SW-B | Fa0/7 | PC11 | Fa0 | VLAN 40 ADMIN |
| SW-B | Fa0/8 | PC12 | Fa0 | VLAN 40 ADMIN |

---

## 📊 Plan d'adressage

| VLAN | 🏷️ Nom | Réseau | SVI (Passerelle) | Pool DHCP | PCs | Switch |
|------|--------|--------|-----------------|-----------|-----|--------|
| 10 | 💻 DEV | 10.10.10.0/24 | 10.10.10.1 | .11 → .50 | PC1,PC2,PC3 | SW-A |
| 20 | 🎨 DESIGN | 10.10.20.0/24 | 10.10.20.1 | .11 → .50 | PC4,PC5,PC6 | SW-A |
| 30 | 📣 MARKETING | 10.10.30.0/24 | 10.10.30.1 | .11 → .50 | PC7,PC8,PC9 | SW-B |
| 40 | 👔 ADMIN | 10.10.40.0/24 | 10.10.40.1 | .11 → .50 | PC10,PC11,PC12 | SW-B |

---

## 🖥️ Assignation des ports PC

| PC | IP attendue (DHCP) | Masque | Passerelle | Switch | Port | VLAN |
|----|-------------------|--------|------------|--------|------|------|
| PC1 | 10.10.10.11 | 255.255.255.0 | 10.10.10.1 | SW-A | Fa0/3 | 10 |
| PC2 | 10.10.10.12 | 255.255.255.0 | 10.10.10.1 | SW-A | Fa0/4 | 10 |
| PC3 | 10.10.10.13 | 255.255.255.0 | 10.10.10.1 | SW-A | Fa0/5 | 10 |
| PC4 | 10.10.20.11 | 255.255.255.0 | 10.10.20.1 | SW-A | Fa0/6 | 20 |
| PC5 | 10.10.20.12 | 255.255.255.0 | 10.10.20.1 | SW-A | Fa0/7 | 20 |
| PC6 | 10.10.20.13 | 255.255.255.0 | 10.10.20.1 | SW-A | Fa0/8 | 20 |
| PC7 | 10.10.30.11 | 255.255.255.0 | 10.10.30.1 | SW-B | Fa0/3 | 30 |
| PC8 | 10.10.30.12 | 255.255.255.0 | 10.10.30.1 | SW-B | Fa0/4 | 30 |
| PC9 | 10.10.30.13 | 255.255.255.0 | 10.10.30.1 | SW-B | Fa0/5 | 30 |
| PC10 | 10.10.40.11 | 255.255.255.0 | 10.10.40.1 | SW-B | Fa0/6 | 40 |
| PC11 | 10.10.40.12 | 255.255.255.0 | 10.10.40.1 | SW-B | Fa0/7 | 40 |
| PC12 | 10.10.40.13 | 255.255.255.0 | 10.10.40.1 | SW-B | Fa0/8 | 40 |

---

## ⚙️ Configuration complète

### 🔧 SW-CORE — Switch L3 (SVI + EtherChannel + DHCP)

```cisco
enable
configure terminal
hostname SW-CORE

vlan 10
name DEV
exit
vlan 20
name DESIGN
exit
vlan 30
name MARKETING
exit
vlan 40
name ADMIN
exit

ip routing

interface vlan 10
ip address 10.10.10.1 255.255.255.0
no shutdown
exit

interface vlan 20
ip address 10.10.20.1 255.255.255.0
no shutdown
exit

interface vlan 30
ip address 10.10.30.1 255.255.255.0
no shutdown
exit

interface vlan 40
ip address 10.10.40.1 255.255.255.0
no shutdown
exit

interface range fa0/1-2
switchport trunk encapsulation dot1q
channel-group 1 mode active
exit

interface port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20
exit

interface range fa0/3-4
switchport trunk encapsulation dot1q
channel-group 2 mode active
exit

interface port-channel 2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 30,40
exit

ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp pool VLAN10-DEV
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 10.10.20.1 10.10.20.10
ip dhcp pool VLAN20-DESIGN
network 10.10.20.0 255.255.255.0
default-router 10.10.20.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 10.10.30.1 10.10.30.10
ip dhcp pool VLAN30-MARKETING
network 10.10.30.0 255.255.255.0
default-router 10.10.30.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 10.10.40.1 10.10.40.10
ip dhcp pool VLAN40-ADMIN
network 10.10.40.0 255.255.255.0
default-router 10.10.40.1
dns-server 8.8.8.8
exit

end
write
```

---

### 🔧 SW-A — Switch L2 (Accès étage 1)

```cisco
enable
configure terminal
hostname SW-A

vlan 10
name DEV
exit
vlan 20
name DESIGN
exit

interface range fa0/1-2
channel-group 1 mode active
exit

interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20
exit

interface range fa0/3-5
switchport mode access
switchport access vlan 10
spanning-tree portfast
exit

interface range fa0/6-8
switchport mode access
switchport access vlan 20
spanning-tree portfast
exit

end
write
```

---

### 🔧 SW-B — Switch L2 (Accès étage 2)

```cisco
enable
configure terminal
hostname SW-B

vlan 30
name MARKETING
exit
vlan 40
name ADMIN
exit

interface range fa0/1-2
channel-group 2 mode active
exit

interface port-channel 2
switchport mode trunk
switchport trunk allowed vlan 30,40
exit

interface range fa0/3-5
switchport mode access
switchport access vlan 30
spanning-tree portfast
exit

interface range fa0/6-8
switchport mode access
switchport access vlan 40
spanning-tree portfast
exit

end
write
```

---

### 🖥️ Configuration PC en DHCP

```
Sur chaque PC :
1. Cliquez sur le PC
2. Desktop
3. IP Configuration
4. Cochez DHCP ← automatique !
```

---

## 🔍 Commandes de vérification

```cisco
! Sur SW-CORE
SW-CORE# show vlan brief
SW-CORE# show interfaces trunk
SW-CORE# show ip interface brief
SW-CORE# show ip route
SW-CORE# show etherchannel summary
SW-CORE# show ip dhcp binding
SW-CORE# show ip dhcp pool

! Sur SW-A
SW-A# show vlan brief
SW-A# show interfaces trunk
SW-A# show etherchannel summary

! Sur SW-B
SW-B# show vlan brief
SW-B# show interfaces trunk
SW-B# show etherchannel summary
```

---

### 📊 Résultat attendu — show etherchannel summary SW-CORE

```
Group  Port-channel  Protocol  Ports
------+-------------+---------+------------------
1      Po1(SU)       LACP      Fa0/1(P) Fa0/2(P)
2      Po2(SU)       LACP      Fa0/3(P) Fa0/4(P)

Po1 = lien vers SW-A ✅
Po2 = lien vers SW-B ✅
```

### 📊 Résultat attendu — show ip interface brief SW-CORE

```
Vlan10   10.10.10.1   up   up  ✅
Vlan20   10.10.20.1   up   up  ✅
Vlan30   10.10.30.1   up   up  ✅
Vlan40   10.10.40.1   up   up  ✅
```

### 📊 Résultat attendu — show ip dhcp binding

```
IP address    Client-ID           Type
10.10.10.11   0100.xxxx.xxxx.xx   Automatic ✅
10.10.20.11   0100.xxxx.xxxx.xx   Automatic ✅
10.10.30.11   0100.xxxx.xxxx.xx   Automatic ✅
10.10.40.11   0100.xxxx.xxxx.xx   Automatic ✅
```

---

## 🧪 Tests de connectivité

```
✅ PC1  → ping 10.10.10.1   (passerelle VLAN10)
✅ PC1  → ping 10.10.20.11  (VLAN10 → VLAN20)
✅ PC1  → ping 10.10.30.11  (VLAN10 → VLAN30 via Po2)
✅ PC1  → ping 10.10.40.11  (VLAN10 → VLAN40 via Po2)
✅ PC7  → ping 10.10.10.11  (VLAN30 → VLAN10 via Po1)
✅ PC10 → ping 10.10.20.11  (VLAN40 → VLAN20 via Po1)
```

---

## 🎭 Scénario — Test de redondance EtherChannel

### Simuler panne Fa0/1 (SW-CORE → SW-A)

```cisco
SW-CORE(config)# interface fa0/1
SW-CORE(config-if)# shutdown
```

### Vérifier

```cisco
SW-CORE# show etherchannel summary
→ Po1(SU) Fa0/1(D) Fa0/2(P)
→ Canal toujours actif ✅
```

### Retester

```
PC1 → ping 10.10.20.11 ✅
→ Fonctionne encore via Fa0/2 !
```

### Réactiver

```cisco
SW-CORE(config)# interface fa0/1
SW-CORE(config-if)# no shutdown
```

---

## 💡 Points clés à retenir

| 🔑 Commande | 📖 Rôle |
|-------------|---------|
| `ip routing` | Active le routage sur switch L3 |
| `interface vlan X` | Crée une SVI (passerelle du VLAN) |
| `channel-group X mode active` | Crée EtherChannel LACP |
| `interface port-channel X` | Configure le canal logique |
| `switchport trunk encapsulation dot1q` | Obligatoire sur switch L3 |
| `ip dhcp excluded-address` | Réserve IPs pour la passerelle |
| `ip dhcp pool` | Crée le pool DHCP par VLAN |
| `show etherchannel summary` | Vérifie EtherChannel |
| `show ip dhcp binding` | Vérifie les IPs distribuées |

---

## 📊 Comparatif SVI vs Router-on-a-Stick

| | 🏆 SVI (ce TP) | Router-on-a-Stick |
|---|---|---|
| Routeur externe | ❌ Non | ✅ Obligatoire |
| Commande clé | `ip routing` | `encapsulation dot1Q` |
| Performance | 🚀 Rapide (ASIC) | 🐢 Plus lent |
| DHCP | Sur switch L3 | Sur routeur |
| EtherChannel | ✅ Compatible | ✅ Compatible |

---

## 🛠️ Outils

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-8.x-orange?style=flat-square&logo=cisco)
![Cisco IOS](https://img.shields.io/badge/Cisco%20IOS-15.x-blue?style=flat-square)
![GitHub](https://img.shields.io/badge/GitHub-black?style=flat-square&logo=github)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420?style=flat-square&logo=ubuntu)

---

## 👨‍💻 Auteur

**Urbain Sedami Landjidé**
🎓 Étudiant en 2ème année — Licence Professionnelle
📡 Réseaux Informatique Mobilité Sécurité (RMS)
🏫 Cisco Networking Academy
📍 Cotonou, Bénin 🇧🇯

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connecter-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/urbain-sedami-landjide-9b49043a8/)

---

## 📄 Licence

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Libre d'utilisation pour l'apprentissage et la formation réseau.
