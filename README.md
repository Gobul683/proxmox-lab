# 🔬 Proxmox Security Lab

Lab réseau virtualisé sous Proxmox pour pratiquer l'administration système, le firewall, l'Active Directory et la sécurité réseau en environnement isolé.

## 🏗️ Architecture

```
[Box FAI / Internet]
        │
    vmbr0 (WAN)
        │
   [ pfSense ]  ←── Firewall central + IDS Suricata + VPN OpenVPN
        │
   ┌────┼────┬──────────┬──────────┐
vmbr1  vmbr2  vmbr3    vmbr4
 (LAN)  (DMZ) (ATTACK)  (MGMT)
   │      │       │
 [DC01  [...]  [Kali
 CLIENT01]     Linux]
```

## 🌐 Plan d'adressage

| Interface | Réseau | IP pfSense | Rôle |
|-----------|--------|------------|------|
| WAN | DHCP box FAI | `192.168.1.192` | Accès internet |
| LAN | `10.0.0.0/16` | `10.0.0.1` | Réseau interne |
| DMZ | `10.1.0.0/16` | `10.1.0.1` | Zone démilitarisée |
| ATTACK | `10.2.0.0/16` | `10.2.0.1` | Réseau attaque |
| MGMT | `10.3.0.0/16` | `10.3.0.1` | Supervision |

## 🖥️ Machines virtuelles

| VM | ID | IP | OS | Rôle |
|----|----|----|-----|------|
| pfSense | 105 | `10.0.0.1` | pfSense CE 2.7.2 | Firewall / Routeur / VPN / IDS |
| WS2025-DC01 | 102 | `10.0.0.2` | Windows Server 2025 | Contrôleur de domaine AD |
| WIN10-CLIENT01 | 103 | `10.0.0.50` | Windows 10 Pro | Poste client joint au domaine |
| debian-admin | 101 | `10.0.128.x` | Debian 12 | Administration / Tests |
| Kali-Attack | 104 | `10.2.7.12` | Kali Linux | Machine attaquante |

## 🔒 Politique de sécurité réseau

| Source | Destination | Règle |
|--------|-------------|-------|
| LAN | Any | ✅ Autorisé |
| DMZ | LAN | ❌ Bloqué |
| DMZ | Internet | ✅ Autorisé |
| ATTACK | LAN | ❌ Bloqué |
| ATTACK | Internet | ✅ Autorisé |
| MGMT | Any | ✅ Autorisé |
| VPN | Any | ✅ Autorisé |

## 🏢 Active Directory — lab.local

| Élément | Détail |
|---------|--------|
| Domaine | `lab.local` |
| NetBIOS | `LAB` |
| DC | `DC01.lab.local` (`10.0.0.2`) |
| OUs | `Utilisateurs_Lab`, `Ordinateurs_Lab`, `Groupes_Lab`, `Serveurs_Lab` |
| Groupes | `GRP_Admin`, `GRP_Users`, `GRP_Serveurs` |
| GPO | Mot de passe, Verrouillage, Audit, USB bloqué, Gestionnaire tâches |

## 📁 Documentation

| Étape | Fichier | Description |
|-------|---------|-------------|
| 01 | [Réseau Proxmox](docs/01-proxmox-network.md) | Création des bridges `vmbr0` → `vmbr4` |
| 02 | [VM pfSense](docs/02-pfsense-vm.md) | Déploiement du firewall central |
| 03 | [Configuration pfSense](docs/03-pfsense-config.md) | Interfaces, firewall, DHCP, NAT |
| 04 | [Debian Admin](docs/04-debian-admin.md) | VM d'administration Linux |
| 05 | [OpenVPN](docs/05-openvpn.md) | Accès VPN distant sécurisé |
| 06 | [Windows Server AD](docs/06-windows-server-ad.md) | Active Directory, GPO, utilisateurs |
| 07 | [Windows 10 Client](docs/07-win10-client.md) | Poste client joint au domaine |
| 08 | [Kali Linux](docs/08-kali-linux.md) | VM d'attaque et tests de pénétration |
| 09 | [Suricata IDS/IPS](docs/09-suricata.md) | Détection d'intrusion sur pfSense |

## 🛠️ Stack technique

| Couche | Technologie |
|--------|-------------|
| Hyperviseur | Proxmox VE |
| Firewall | pfSense CE 2.7.2 |
| IDS/IPS | Suricata + Emerging Threats rules |
| VPN | OpenVPN (SSL/TLS + User Auth) |
| Annuaire | Active Directory (Windows Server 2025) |
| Attaque | Kali Linux |
| Admin | Debian 12 |

## 🚧 En cours / À venir

- [ ] Wazuh (SIEM)
- [ ] Fail2ban (hardening Debian)
- [ ] GLPI (ticketing / inventaire)
- [ ] Zabbix (supervision)
- [ ] Samba + partages réseau
- [ ] HTTPS / certificats TLS internes
- [ ] Proxmox Backup Server
- [ ] WDS (déploiement OS)
- [ ] RADIUS / 802.1X

---

> Lab réalisé dans le cadre d'une formation **Administrateur d'Infrastructures Sécurisées (AIS)** — TSSR.
