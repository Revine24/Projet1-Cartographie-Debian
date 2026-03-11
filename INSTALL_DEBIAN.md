# Rapport Technique : Configuration de la Cible SRVLX01
## Sprint 1 : Préparation de l'environnement et déploiement de services

### 1. Environnement réseau
La machine virtuelle SRVLX01 (Debian 13) est isolée sur un réseau interne segmenté. La configuration de l'hyperviseur autorise le mode promiscuité pour les besoins de l'audit réseau.

* **Segment réseau :** intnet (Internal Network)
* **Mode Promiscuité :** Allow All

> **Figure 1 : Configuration de l'interface réseau (VirtualBox)**
> ![Configuration Réseau](./SCREENSHOTS_DEBIAN/01_config_reseau_Vbox.png)

---

### 2. Configuration IP et Identité
L'adressage IP a été fixé statiquement afin de garantir la persistance de la cible lors des phases de scan. 

* **Hostname :** SRVLX01
* **IP Address :** 172.16.10.6/24
* **Credentials :** root / Azerty1*

**Procédure :** Modification du fichier `/etc/network/interfaces`.

> **Figure 2 : Paramétrage de l'adresse statique**
> ![Config Nano](./SCREENSHOTS_DEBIAN/02_fichier_interfaces.png)

> **Figure 3 : Vérification de l'état de l'interface enp0s8**
> ![IP Active](./SCREENSHOTS_DEBIAN/03_ip_active.png)

---

### 3. Services et Vulnérabilités implémentés
Deux services ont été installés pour simuler des vecteurs d'attaque standards (flux non chiffrés).

#### 3.1. Serveur Web Apache2 (Port 80)
Installation du service HTTP pour l'exposition de bannières applicatives.
* **Commande :** `apt install apache2 -y`
> ![Installation Apache](./SCREENSHOTS_DEBIAN/04_install_apache.png)

#### 3.2. Serveur FTP vsftpd (Port 21)
Installation du service de transfert de fichiers pour simuler une faille d'authentification en clair.
* **Commande :** `apt install vsftpd -y`
> ![Installation FTP](./SCREENSHOTS_DEBIAN/05_install_ftp.png)

#### 3.3. Contrôle des ports ouverts
Le pare-feu (UFW) est désactivé pour permettre l'énumération complète. La commande `ss -tunlp` confirme l'écoute des processus sur les ports 21 et 80.
> ![Ports Locaux](./SCREENSHOTS_DEBIAN/06_ports_locaux.png)

---

### 4. Validation par audit distant (Scan Nmap)
Un scan de reconnaissance a été effectué depuis la machine d'audit (UBU01 - 172.16.10.20) pour valider la visibilité des services.

**Commande :** `nmap 172.16.10.6`

> **Figure 4 : Résultat du scan de ports distant**
> ![Scan Nmap Final](./SCREENSHOTS_DEBIAN/07_audit_nmap_final.png)

**Conclusion :** La cible SRVLX01 répond correctement aux sollicitations réseau. Les ports 21/tcp et 80/tcp sont confirmés comme étant ouverts.
