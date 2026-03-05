# 🔬 Proxmox Security Lab

Lab réseau virtualisé sous Proxmox pour pratiquer l'administration système, le firewall et la sécurité réseau en environnement isolé.

## 🏗️ Architecture

```
[Box FAI / Internet]
        │
    vmbr0 (WAN)
        │
   [ pfSense ]  ←── Firewall central
        │
   ┌────┴────┬──────────┐
vmbr1      vmbr2      vmbr3
 (LAN)     (DMZ)    (ATTACK)
   │          │          │
 [AD/       [Web      [Kali
 Clients]   Server]   Linux]
```

## 🌐 Plan d'adressage

| Interface | Réseau | IP pfSense | Rôle |
|-----------|--------|------------|------|
| WAN | DHCP box FAI | `192.168.1.x` | Accès internet |
| LAN | `10.0.0.0/16` | `10.0.0.1` | Réseau interne |
| OPT1 | `10.1.0.0/16` | `10.1.0.1` | DMZ |
| OPT2 | `10.2.0.0/16` | `10.2.0.1` | ATTACK |
| OPT3 | `10.3.0.0/16` | `10.3.0.1` | MGMT |

| Composant | Rôle |
|-----------|------|
| Proxmox VE | Hyperviseur |
| pfSense | Firewall / Routeur central |
| Windows Server | Active Directory, DNS, DHCP |
| Debian | Serveurs DMZ |
| Kali Linux | Machine attaquante |

## 📁 Documentation

| Étape | Fichier | Description |
|-------|---------|-------------|
| 01 | [Réseau Proxmox](docs/01-proxmox-network.md) | Création des bridges `vmbr0` → `vmbr3` |
| 02 | [VM pfSense](docs/02-pfsense-vm.md) | Déploiement du firewall central |

---

> Lab réalisé dans le cadre d'une formation en administration système et sécurité réseau.
