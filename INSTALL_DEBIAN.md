# Rapport Technique : Analyse et Cartographie des Ports Réseau
**Projet :** TSSR - Sprint 1 (Infrastructure et Paramétrage)
**Technicien :** Revine
**Cible :** SRVLX01 (Debian 13)

---

## I. Introduction et Contexte
L'objectif de ce premier sprint est la mise en place d'une infrastructure virtualisée pour tester la communication réseau entre différents hôtes. Ma mission s'est concentrée sur la préparation de la machine cible Debian, son adressage IP statique et l'activation de services réseaux spécifiques pour vérifier la visibilité des ports.

## II. Architecture Réseau et Isolation
La machine virtuelle est déployée sous VirtualBox. Afin de placer la machine dans le réseau de l'équipe (L'équipe 3), la configuration suivante a été appliquée :

* **Adaptateur 2 :** Réseau interne (`intnet`).
* **Mode Promiscuité :** "Autoriser tout".

> ![Configuration Réseau VirtualBox](./SCREENSHOTS_DEBIAN/01_config_reseau_Vbox.png)

## III. Configuration de la Couche Réseau (OS)
### 3.1 Adressage IP Statique
Pour garantir que la machine conserve la même identité sur le réseau, j'ai configuré l'interface `enp0s8` manuellement via le fichier `/etc/network/interfaces`.

* **IPv4 :** 172.16.10.6
* **Masque :** 255.255.255.0

> ![Édition du fichier interfaces](./SCREENSHOTS_DEBIAN/02_fichier_interfaces.png)

### 3.2 Validation de l'interface
Après le redémarrage du service, la commande `ip a` confirme que l'interface est active et correctement adressée.

> ![Vérification de l'IP active](./SCREENSHOTS_DEBIAN/03_ip_active.png)

## IV. Installation et Paramétrage des Services
J'ai procédé à l'installation de deux services pour ouvrir les ports correspondants sur le réseau interne.

### 4.1 Service Web (Apache2) - Port 80
Installation du serveur Apache pour ouvrir le port HTTP.
```bash
apt update && apt install apache2 -y
