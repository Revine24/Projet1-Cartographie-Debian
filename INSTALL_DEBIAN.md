# Rapport Technique : Analyse et Cartographie des Ports Réseau
**Projet :** TSSR - Sprint 1 (Infrastructure et Vulnérabilisation)
**Technicien :** Revine
**Cible :** SRVLX01 (Debian 13)

---

## I. Introduction et Contexte
Dans le cadre du projet d'analyse réseau de l'Équipe 3, ma mission consiste à préparer une cible Linux (Debian 13) isolée. L'objectif est d'implémenter des services spécifiques afin de simuler des vulnérabilités exploitables lors des phases d'audit. Ce document détaille la configuration système et l'ouverture des vecteurs d'attaque.

## II. Architecture Réseau et Isolation
La machine virtuelle est déployée sous VirtualBox. Pour garantir l'étanchéité de l'environnement de test et permettre une analyse de trafic non filtrée par l'hyperviseur, la configuration suivante a été appliquée :

* **Mode d'accès :** Réseau interne (`intnet`)
* **Mode Promiscuité :** "Autoriser tout"

> ![Configuration Réseau VirtualBox](./SCREENSHOTS_DEBIAN/01_config_reseau_Vbox.png)

## III. Configuration de la Couche Réseau (OS)
### 3.1 Adressage IP Statique
Afin d'assurer la persistance de l'hôte lors des scans de découverte, l'interface `enp0s8` a été configurée en adressage statique via l'édition du fichier `/etc/network/interfaces`.

**Commande :** `sudo nano /etc/network/interfaces`

* **IPv4 :** 172.16.10.6
* **Masque :** 255.255.255.0

> ![Édition du fichier interfaces](./SCREENSHOTS_DEBIAN/02_fichier_interfaces.png)

### 3.2 Validation de l'état de l'interface
Après application des paramètres et redémarrage du service networking, la commande `ip a` confirme que l'interface est opérationnelle ("UP") et possède l'adressage IP cible.

**Commande :** `ip a`

> ![Vérification IP active](./SCREENSHOTS_DEBIAN/03_ip_active.png)

---

## IV. Implémentation des Services (Vulnérabilisation)
Conformément au planning du Sprint 1, j'ai procédé à l'installation de deux daemons exposant des ports TCP critiques pour simuler une surface d'attaque.

### 4.1 Serveur HTTP (Apache2) - Port 80
Installation du serveur Web Apache pour simuler une interface de gestion non sécurisée.
```bash
apt update && apt install apache2 -y
