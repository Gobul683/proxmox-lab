# 02 — Déploiement de la VM pfSense

## Objectif

Déployer pfSense comme firewall central du lab. Il sera le seul point d'entrée/sortie entre le réseau domestique (WAN) et les réseaux virtuels internes (LAN, DMZ, ATTACK).

## Résultat attendu

- VM pfSense opérationnelle avec 4 interfaces réseau
- pfSense accessible depuis le LAN sur `192.168.10.1`
- Routage inter-réseaux fonctionnel

---

## Procédure

### Téléchargement de l'ISO

Dans Proxmox : **Datacenter > [nœud] > local > ISO Images > Download from URL**

```
https://nyifiles.netgate.com/mirror/downloads/pfSense-CE-2.7.2-RELEASE-amd64.iso.gz
```

> 📸 `assets/02-iso-download.png`

---

### Création de la VM

**Datacenter > Créer VM**

#### Général
| Paramètre | Valeur |
|-----------|--------|
| VM ID | `100` |
| Nom | `pfSense` |

#### OS
| Paramètre | Valeur |
|-----------|--------|
| ISO | `pfSense-CE-2.7.2-RELEASE-amd64.iso` |
| Type | Other |

#### Système
| Paramètre | Valeur |
|-----------|--------|
| BIOS | Default (SeaBIOS) |
| Machine | Default (i440fx) |
| SCSI Controller | VirtIO SCSI single |

#### Disque
| Paramètre | Valeur |
|-----------|--------|
| Bus | SCSI |
| Taille | `20 GB` |
| Cache | Default |

#### CPU & RAM
| Paramètre | Valeur |
|-----------|--------|
| Cores | `2` |
| RAM | `2048 MB` |

> 📸 `assets/02-vm-hardware.png`

---

### Ajout des interfaces réseau

Par défaut la VM est créée avec une seule interface. Il faut en ajouter 3 autres.

**Hardware > Add > Network Device**

| Interface | Bridge | Rôle |
|-----------|--------|------|
| `net0` | `vmbr0` | WAN |
| `net1` | `vmbr1` | LAN |
| `net2` | `vmbr2` | DMZ |
| `net3` | `vmbr3` | ATTACK |

> 📸 `assets/02-vm-network-interfaces.png`

---

### Installation de pfSense

Démarrer la VM, ouvrir la console, suivre l'installeur :

1. Accept → Install pfSense
2. Keymap : French (ou laisser default)
3. Partitioning : Auto (ZFS) → `da0`
4. Reboot — retirer l'ISO avant le redémarrage

**Proxmox > VM > Hardware > CD/DVD Drive > Do not use any media** avant le reboot.

> 📸 `assets/02-pfsense-install-complete.png`

---

### Assignation des interfaces (console pfSense)

Au premier démarrage, pfSense demande d'assigner les interfaces.

```
Should VLANs be set up now? → n
Enter WAN interface: vtnet0
Enter LAN interface: vtnet1
Enter Optional 1: vtnet2
Enter Optional 2: vtnet3
```

> 📸 `assets/02-pfsense-interface-assign.png`

---

## Validation

Le menu principal de pfSense affiche les 4 interfaces avec leurs IPs :

```
WAN  → vtnet0 → DHCP (IP box FAI)
LAN  → vtnet1 → 192.168.10.1/24
OPT1 → vtnet2 → (à configurer)
OPT2 → vtnet3 → (à configurer)
```

> 📸 `assets/02-pfsense-menu.png`

L'interface web pfSense est accessible depuis une VM sur le LAN : `https://192.168.10.1`

---

⬅️ Étape précédente : [01 — Réseau Proxmox](01-proxmox-network.md)
