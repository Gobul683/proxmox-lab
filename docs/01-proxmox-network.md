# 01 — Configuration réseau Proxmox

## Objectif

Créer les bridges virtuels qui serviront de switches internes au lab. Proxmox n'aura d'IP que sur `vmbr0` — tous les autres bridges sont isolés, sans IP, gérés exclusivement par pfSense.

## Résultat attendu

| Bridge | Rôle | IP Proxmox |
|--------|------|------------|
| `vmbr0` | WAN — connecté au réseau physique (box FAI) | `192.168.1.100/24` |
| `vmbr1` | LAN — réseau interne du lab | aucune |
| `vmbr2` | DMZ — zone serveurs exposés | aucune |
| `vmbr3` | ATTACK — réseau isolé Kali | aucune |

---

## Procédure

### État initial

Avant toute modification, la page **Système > Réseau** ne contient que `vmbr0` (bridge existant lié à la carte physique `nic0`) et les interfaces physiques.

> 📸 `assets/01-network-initial.png`

---

### Création des bridges

Pour chaque bridge (`vmbr1`, `vmbr2`, `vmbr3`) :

**Système > Réseau > Créer > Linux Bridge**

Paramètres communs :
- IPv4/CIDR : vide
- Gateway : vide
- Bridge ports : vide
- VLAN aware : non coché

| Bridge | Commentaire |
|--------|-------------|
| `vmbr1` | `LAN` |
| `vmbr2` | `DMZ` |
| `vmbr3` | `ATTACK` |

> 📸 `assets/01-vmbr1-creation.png` — formulaire de création de `vmbr1`

---

### Application de la configuration

Cliquer sur **Appliquer la configuration** en haut de la page réseau et confirmer.

---

## Validation

La page réseau affiche les 4 bridges actifs :

| Name | Type | Active | Comment |
|------|------|--------|---------|
| `vmbr0` | Linux Bridge | Yes | WAN |
| `vmbr1` | Linux Bridge | Yes | LAN |
| `vmbr2` | Linux Bridge | Yes | DMZ |
| `vmbr3` | Linux Bridge | Yes | ATTACK |

> 📸 `assets/01-network-final.png`

---

➡️ Étape suivante : [02 — VM pfSense](02-pfsense-vm.md)
