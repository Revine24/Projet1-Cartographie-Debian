# Guide d'Utilisation de l'outil de Scan (USER GUIDE)

Bienvenue dans le guide d'utilisation de notre projet de cartographie réseau. Ce document vous explique comment utiliser notre script automatisé et comment vérifier les résultats.

### 1. Le but de l'outil
Afin de simplifier l'utilisation de Nmap (qui possède beaucoup d'options complexes), nous avons créé un script interactif personnalisé. Son but est de vous guider pas à pas pour scanner une cible, découvrir les ports ouverts et identifier les services vulnérables sur notre réseau isolé.

### 2. Comment le lancer
Ouvrez un terminal sur votre machine d'attaque (par exemple notre machine Ubuntu UBU01 - 172.16.10.20), allez dans le dossier du projet, puis exécutez simplement le script avec les droits administrateur :

```bash nmap.sh

### 3. Ce qu'il faut saisir

Une fois lancé, le script fonctionne comme un menu interactif. Il va vous demander de taper un numéro pour choisir vos options :

- **Le type de scan :** Choisissez la méthode (ex: `1` pour un scan rapide, `2` pour un scan complet).
- **La vitesse du scan :** Choisissez de `T1` (très lent) à `T5` (très rapide). ⚠️ Attention : plus le scan est rapide, plus il risque de se faire repérer et bloquer par un pare-feu.
- **Le niveau d'information :** Choisissez la quantité de détails affichés (Normal, Verbeux, ou Très verbeux).
- **Les options supplémentaires :** Choisissez si vous voulez forcer la détection de la version des logiciels ou du système d'exploitation (OS).
- **L'adresse IP :** Enfin, tapez l'adresse de la cible (par exemple `172.16.10.6` pour notre serveur Debian) et appuyez sur Entrée.

### 4. Comment lire les résultats

À la fin du processus, le script affiche le résultat du scan à l'écran. Regardez la colonne **STATE** (état). Si la ligne indique **`open`** (ouvert), c'est que vous avez trouvé une cible :

- **Cible Windows :** Vous verrez notamment les ports **139** et **445** (partage de fichiers) ouverts.
- **Cible Linux :** Vous verrez notamment les ports **21** (FTP) et **80** (HTTP) ouverts.

### 5. Exemple simple : Vérification manuelle

Une fois que le script vous a montré que les ports sont ouverts, vous pouvez vérifier manuellement que les services répondent bien :

**Vérification du Port 80 (Web) :**
Ouvrez le navigateur internet de la machine d'attaque et tapez :
```http://172.16.10.6
La connectivité est confirmée si vous arrivez sur la page par défaut d'Apache.

##Vérification du Port 21 (FTP) :**
Dans votre terminal, tapez la commande ```nc 172.16.10.6 21. La réponse du service est confirmée si le serveur vous renvoie la "bannière" (le message d'accueil) de vsFTPd, prouvant qu'il est prêt à l'emploi.
