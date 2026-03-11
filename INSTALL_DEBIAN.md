# Rapport Technique : Analyse et Cartographie des Ports Réseau
**Projet :** TSSR - Sprint 1 (Infrastructure et Vulnérabilisation)
**Auteur :** Revine
**Cible :** SRVLX01 (Debian 13)

---

## I. Introduction et Contexte
Dans le cadre de ce projet de cartographie réseau, ma mission consiste à préparer une cible Linux (Debian 13) au sein d'une infrastructure virtualisée. L'objectif est d'implémenter des services spécifiques afin de simuler des vulnérabilités exploitables lors des phases d'audit et de reconnaissance réseau du Sprint 2.

## II. Architecture Réseau et Isolation
La machine virtuelle est déployée sous l'hyperviseur VirtualBox. Pour garantir l'étanchéité de l'environnement de test et permettre une analyse de trafic non filtrée (indispensable pour Nmap), la configuration suivante a été appliquée sur l'interface secondaire :

* **Adaptateur 2 :** Réseau interne (`intnet`)
* **Mode Promiscuité :** "Autoriser tout"

> ![Configuration Réseau VirtualBox](./01_config_reseau_Vbox.png)

## III. Configuration de la Couche Réseau (OS)
### 3.1 Adressage IP Statique
Pour assurer la persistance de l'hôte lors des scans de découverte, j'ai configuré l'interface `enp0s8` en adressage statique via l'édition du fichier `/etc/network/interfaces`.

* **IPv4 :** 172.16.10.6
* **Masque :** 255.255.255.0

> ![Édition du fichier interfaces](./02_fichier_interfaces.png)

### 3.2 Validation de l'état de l'interface
Après application de la configuration et redémarrage du service `networking`, la commande `ip a` confirme que l'interface est opérationnelle ("UP") et l'adressage correct.

> ![Vérification IP active](./03_ip_active.png)

## IV. Implémentation des Services et Vulnérabilisation
Conformément au Product Backlog, j'ai procédé à l'installation de deux daemons exposant des ports TCP critiques pour simuler une surface d'exposition vulnérable.

### 4.1 Service HTTP (Apache2) - Port 80
Installation du serveur Web Apache pour simuler une interface de gestion accessible sans chiffrement.
```bash
apt update && apt install apache2 -y
