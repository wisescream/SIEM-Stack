# Avant toute configuration ou utilisation commencez par cloner ce dépôt sur votre machine
![Screenshot 2025-05-29 071919](https://github.com/user-attachments/assets/951f7681-2722-4de0-945d-6752f0886780)
![Screenshot 2025-05-29 064325](https://github.com/user-attachments/assets/2fc4acab-9ccc-4112-a550-74f6cda04b33)
![Screenshot 2025-05-29 063645](https://github.com/user-attachments/assets/7b289721-ab74-4d19-8e68-b152f64bb8cf)
![Screenshot 2025-05-29 063443](https://github.com/user-attachments/assets/252b9149-11d3-421e-a829-4905c4f11743)
![Screenshot 2025-05-29 062716](https://github.com/user-attachments/assets/807a1b85-35da-470a-a277-7d4ec4611026)
![Screenshot 2025-05-29 062649](https://github.com/user-attachments/assets/3abff032-8e24-4505-989e-0566338ea2bf)
![Screenshot 2025-05-29 055635](https://github.com/user-attachments/assets/4a6ca70f-865c-4c23-80fe-be7307dd2c71)
![Screenshot 2025-05-29 055340](https://github.com/user-attachments/assets/f857d923-e421-4747-b9bf-625d09d3f8b0)
![Screenshot 2025-05-29 054730](https://github.com/user-attachments/assets/0b5542d9-df90-4e9e-8a77-2067e9a33a2d)
![Screenshot 2025-05-29 055623](https://github.com/user-attachments/assets/b0f2eb01-28a1-4557-b6b4-26a89f54beae)

''
https://github.com/socfortress/OSSIEM.git
https://github.com/wisescream/SIEM-Stack.git
''
## OSSIEM (Open Source SIEM)

 (Open Source Security Information and Event Management) est une solution SIEM open source développée à l'origine par AlienVault. Elle permet de collecter, corréler et analyser les événements de sécurité provenant de différentes sources (logs, IDS, honeypots, etc.) pour détecter les menaces et incidents de sécurité.

### Fonctionnalités principales :
- Collecte centralisée des logs et événements de sécurité
- Corrélation d'événements pour la détection d'incidents
- Tableaux de bord et rapports de sécurité
- Intégration avec de nombreux outils open source (Snort, Suricata, Nagios, etc.)

### Différences avec la stack Elastic/T-Pot :
- **OSSIEM** est une solution SIEM tout-en-un, avec son propre moteur de corrélation et ses outils intégrés.
- **T-Pot/SIEM Stack** s'appuie principalement sur Elastic Stack (Elasticsearch, Logstash, Kibana) pour la collecte, le stockage et la visualisation des événements, et intègre de nombreux honeypots.
- OSSIM peut être utilisé en complément ou en alternative à Elastic Stack selon les besoins de l'organisation.




SIEM Stack 

![SIEM Stack]
SIEM Stack est la plateforme honeypot tout-en-un, optionnellement distribuée, multi-architecture (amd64, arm64), prenant en charge plus de 20 honeypots et d'innombrables options de visualisation grâce à Elastic Stack, des cartes d'attaques animées en temps réel et de nombreux outils de sécurité pour améliorer l'expérience de déception.
<br><br>

# TL;DR
1. Respectez les [prérequis système](#system-requirements). L'installation de SIEM Stack nécessite au moins 8-16 Go de RAM, 128 Go d'espace disque libre ainsi qu'une connexion Internet fonctionnelle (sortante et non filtrée).
2. [Téléchargez](#choose-your-distro) ou utilisez une distribution supportée déjà en cours d'exécution.
3. Installez l'ISO avec le moins de paquets/services possible (`ssh` requis)
4. Installez `curl` : `$ sudo [apt, dnf, zypper] install curl` si ce n'est pas déjà fait
5. Lancez l'installateur en tant qu'utilisateur non-root depuis `$HOME` :
```
env bash -c "$(curl -sL https://github.com/telekom-security/tpotce/raw/master/install.sh)"
```
   * Suivez les instructions, lisez les messages, vérifiez les éventuels conflits de ports et redémarrez

<!-- TOC -->
- [SIEM Stack - La plateforme multi-honeypot tout-en-un](#SIEM Stack---la-plateforme-multi-honeypot-tout-en-un)
- [TL;DR](#tldr)
- [Concept technique](#concept-technique)
  - [Honeypots et outils](#honeypots-et-outils)
  - [Architecture technique](#architecture-technique)
  - [Services](#services)
  - [Types d'utilisateurs](#types-dutilisateurs)
- [Prérequis système](#prerequis-systeme)
  - [Exécution en VM](#execution-en-vm)
  - [Exécution sur matériel physique](#execution-sur-materiel-physique)
  - [Exécution dans le cloud](#execution-dans-le-cloud)
  - [Ports requis](#ports-requis)
- [Placement du système](#placement-du-systeme)
- [Installation](#installation)
  - [Choisir votre distribution](#choisir-votre-distribution)
  - [Obtenir et installer SIEM Stack](#obtenir-et-installer-siem-stack)
  - [macOS & Windows](#macos--windows)
  - [Types d'installation](#types-dinstallation)
    - [Standard / Hive](#standard--hive)
    - [Distribué](#distribue)
- [Premier démarrage](#premier-demarrage)
  - [Premier démarrage autonome](#premier-demarrage-autonome)
  - [Déploiement distribué](#deploiement-distribue)
    - [Planification et certificats](#planification-et-certificats)
    - [Déploiement des capteurs](#deploiement-des-capteurs)
    - [Suppression des capteurs](#suppression-des-capteurs)
  - [Soumission de données communautaires](#soumission-de-donnees-communautaires)
  - [Soumission de données HPFEEDS en opt-in](#soumission-de-donnees-hpfeeds-en-opt-in)
- [Accès à distance et outils](#acces-a-distance-et-outils)
  - [SSH](#ssh)
  - [Page d'accueil SIEM Stack](#page-daccueil-siem-stack)
  - [Tableau de bord Kibana](#tableau-de-bord-Kibana)
  - [Carte d'attaque](#carte-dattaque)
  - [Cyberchef](#cyberchef)
  - [Elasticvue](#elasticvue)
  - [Spiderfoot](#spiderfoot)
- [Configuration](#configuration)
  - [Fichier de configuration SIEM Stack](#fichier-de-configuration-siem-stack)
  - [Personnaliser les honeypots et services SIEM Stack](#personnaliser-les-honeypots-et-services-siem-stack)
  - [Redémarrage quotidien](#redemarrage-quotidien)
  - [Démarrer SIEM Stack](#demarrer-siem-stack)
  - [Arrêter SIEM Stack](#arreter-siem-stack)
  - [Dossier de données SIEM Stack](#dossier-de-donnees-siem-stack)
  - [Persistance des logs](#persistance-des-logs)
  - [Réinitialisation d'usine](#reinitialisation-dusine)
  - [Afficher les conteneurs](#afficher-les-conteneurs)
  - [Blackhole](#blackhole)
  - [Ajouter des utilisateurs à Nginx (WebUI SIEM Stack)](#ajouter-des-utilisateurs-a-nginx-webui-siem-stack)
    - [Exporter](#exporter)
    - [Importer](#importer)

<br><br>


# Concept technique
Les principaux composants de SIEM Stack ont été déplacés dans l'image Docker `tpotinit`, permettant désormais à SIEM Stack de supporter plusieurs distributions Linux, voire macOS et Windows (bien que limités aux fonctionnalités de Docker Desktop). SIEM Stack utilise [docker](https://www.docker.com/) et [docker compose](https://docs.docker.com/compose/) pour atteindre son objectif de faire fonctionner simultanément autant de honeypots et d'outils que possible, utilisant ainsi au maximum le matériel de l'hôte.
<br><br>

=======
## Outils
- SIEM Stack propose des images Docker pour les honeypots suivants :<br>
[adbhoney](https://github.com/huuck/ADBHoney),
[beelzebub](https://github.com/mariocandela/beelzebub),
[ciscoasa](https://github.com/Cymmetria/ciscoasa_honeypot),
[citrixhoneypot](https://github.com/MalwareTech/CitrixHoneypot),
[conpot](http://conpot.org/),
[cowrie](https://github.com/cowrie/cowrie),
[ddospot](https://github.com/aelth/ddospot),
[dicompot](https://github.com/nsmfoo/dicompot),
[dionaea](https://github.com/DinoTools/dionaea),
[elasticpot](https://gitlab.com/bontchev/elasticpot),
[endlessh](https://github.com/skeeto/endlessh),
[galah](https://github.com/0x4D31/galah),
[go-pot](https://github.com/ryanolee/go-pot),
[glutton](https://github.com/mushorg/glutton),
[h0neytr4p](https://github.com/pbssubhash/h0neytr4p),
[hellpot](https://github.com/yunginnanet/HellPot),
[heralding](https://github.com/johnnykv/heralding),
[honeyaml](https://github.com/mmta/honeyaml),
[honeypots](https://github.com/qeeqbox/honeypots),
[honeytrap](https://github.com/armedpot/honeytrap/),
[ipphoney](https://gitlab.com/bontchev/ipphoney),
[log4pot](https://github.com/thomaspatzke/Log4Pot),
[mailoney](https://github.com/awhitehatter/mailoney),
[medpot](https://github.com/schmalle/medpot),
[miniprint](https://github.com/sa7mon/miniprint),
[redishoneypot](https://github.com/cypwnpwnsocute/RedisHoneyPot),
[sentrypeer](https://github.com/SentryPeer/SentryPeer),
[snare](http://mushmush.org/),
[tanner](http://mushmush.org/),
[wordpot](https://github.com/gbrindisi/wordpot)

Ainsi que les outils suivants :
* [Autoheal](https://github.com/willfarrell/docker-autoheal) : redémarre automatiquement les conteneurs dont les vérifications de santé échouent.
* [Cyberchef](https://gchq.github.io/CyberChef/) : application web pour le chiffrement, l'encodage, la compression et l'analyse de données.
* [Elastic Stack](https://www.elastic.co/videos) : visualise magnifiquement tous les événements capturés par SIEM Stack.
* [Elasticvue](https://github.com/cars10/elasticvue/) : interface web pour naviguer et interagir avec un cluster Elasticsearch.
* [Fatt](https://github.com/0x4D31/fatt) : script basé sur pyshark pour extraire des métadonnées réseau et des empreintes à partir de fichiers pcap et du trafic réseau en direct.
* [SIEM Stack-Attack-Map](https://github.com/t3chn0m4g3/SIEM Stack-attack-map) : carte d'attaque animée pour SIEM Stack.
* [P0f](https://lcamtuf.coredump.cx/p0f3/) : outil de fingerprinting passif du trafic réseau.
* [Spiderfoot](https://github.com/smicallef/spiderfoot) : outil d'automatisation de renseignement open source.
* [Suricata](https://suricata.io/) : moteur de surveillance de la sécurité réseau.

... pour vous offrir la meilleure expérience prête à l'emploi possible et un système multi-honeypot facile à utiliser.
<br><br>


## Architecture technique
![Architecture](doc/Architecture.png)

Le code source et les fichiers de configuration sont entièrement stockés dans le dépôt GitHub de SIEM Stack. Les images Docker sont construites et préconfigurées pour l'environnement SIEM Stack.

Les Dockerfiles individuels et les configurations se trouvent dans le [dossier docker](https://github.com/telekom-security/tpotce/tree/master/docker).
<br><br>

## Services
SIEM Stack propose un certain nombre de services, répartis en cinq groupes principaux :
1. Services système fournis par l'OS
    * SSH pour un accès distant sécurisé.
2. Elastic Stack
    * Elasticsearch pour stocker les événements.
    * Logstash pour ingérer, recevoir et envoyer des événements à Elasticsearch.
    * K
    ibana pour afficher les événements sur de magnifiques tableaux de bord.
3. Outils
    * NGINX fournit un accès distant sécurisé (reverse proxy) à K
    ibana, CyberChef, Elasticvue, GeoIP AttackMap, Spiderfoot et permet aux capteurs SIEM Stack de transmettre en toute sécurité les données d'événements à la ruche SIEM Stack.
    * CyberChef : application web pour le chiffrement, l'encodage, la compression et l'analyse de données.
    * Elasticvue : interface web pour naviguer et interagir avec un cluster Elasticsearch.
    * SIEM Stack Attack Map : carte d'attaque animée pour SIEM Stack.
    * Spiderfoot : outil d'automatisation de renseignement open source.
4. Honeypots
    * Une sélection des 23 honeypots disponibles selon le `docker-compose.yml` sélectionné.
5. Network Security Monitoring (NSM)
    * Fatt : script basé sur pyshark pour extraire des métadonnées réseau et des empreintes à partir de fichiers pcap et du trafic réseau en direct.
    * P0f : outil de fingerprinting passif du trafic réseau.
    * Suricata : moteur de surveillance de la sécurité réseau.
<br><br>

## Types d'utilisateurs
Lors de l'installation et de l'utilisation de SIEM Stack, il existe deux types de comptes avec lesquels vous travaillerez. Assurez-vous de bien connaître les différences, car c'est **de loin** la cause la plus fréquente d'erreurs d'authentification.

| Service          | Type de compte | Nom d'utilisateur / Groupe | Description                                                        |
| :--------------- | :----------- | :--------------- | :----------------------------------------------------------------- |
| SSH              | OS           | `<OS_USERNAME>`  | L'utilisateur choisi lors de l'installation de l'OS.              |
| Nginx            | BasicAuth    | `<WEB_USER>`     | `<web_user>` choisi lors de l'installation de SIEM Stack.           |
| CyberChef        | BasicAuth    | `<WEB_USER>`     | `<web_user>` choisi lors de l'installation de SIEM Stack.           |
| Elasticvue       | BasicAuth    | `<WEB_USER>`     | `<web_user>` choisi lors de l'installation de SIEM Stack.           |
| Geoip Attack Map | BasicAuth    | `<WEB_USER>`     | `<web_user>` choisi lors de l'installation de SIEM Stack.           |
| Spiderfoot       | BasicAuth    | `<WEB_USER>`     | `<web_user>` choisi lors de l'installation de SIEM Stack.           |
| SIEM Stack            | OS           | `tpot`           | `tpot` cet utilisateur/groupe est toujours réservé aux services SIEM Stack. |
| SIEM Stack Logs       | BasicAuth    | `<LS_WEB_USER>`  | `LS_WEB_USER` sont gérés automatiquement.                           |


<br><br>

# Prérequis système

Selon les [images de distributions Linux supportées](#choose-your-distro), ruche/capteur, installation sur [matériel réel](#running-on-hardware), en [machine virtuelle](#running-in-a-vm) ou autres environnements, différents prérequis doivent être respectés concernant l'OS, la RAM, le stockage et le réseau pour une installation réussie de SIEM Stack (vous pouvez toujours ajuster `~/tpotce/docker-compose.yml` et `~/tpotce/.env` selon vos besoins pour dépasser ces exigences).
<br><br>

| Type SIEM Stack | RAM  | Stockage   | Description                                                                                      |
| :--------- | :--- | :-------- | :----------------------------------------------------------------------------------------------- |
| Hive       | 16GB | 256GB SSD | En règle générale, plus il y a de honeypots, capteurs & données, plus il faut de RAM et de stockage.      |
| Sensor     | 8GB  | 128GB SSD | Les logs des honeypots sont conservés (~/tpotce/data) pendant 30 jours, le stockage dépend donc du volume d'attaques. |

SIEM Stack nécessite ...
- une adresse IPv4 via DHCP ou assignée statiquement
- une connexion Internet fonctionnelle, non-proxyfiée
... pour une installation et un fonctionnement réussis.
<br><br>
*Si vous avez besoin du support proxy ou d'autres fonctionnalités non standards, consultez la documentation des [images de distributions Linux supportées](#choose-your-distro) et/ou la [documentation Docker](https://docs.docker.com/).*
<br><br>

## Exécution en VM
Toutes les [images de distributions Linux supportées](#choose-your-distro) fonctionneront en VM, ce qui signifie que SIEM Stack fonctionnera parfaitement. Les solutions suivantes ont été testées / signalées comme fonctionnelles :
* [UTM (Intel & Apple Silicon)](https://mac.getutm.app/)
* [VirtualBox](https://www.virtualbox.org/)
* [VMWare Fusion](https://www.vmware.com/products/fusion/fusion-evaluation.html) et [VMWare Workstation](https://www.vmware.com/products/workstation-pro.html)
* KVM est également signalé comme fonctionnel.

***Quelques conseils de configuration / installation :***
- Les versions Intel sont stables, mais le support Apple Silicon (arm64) présente des problèmes connus qui, sous UTM, peuvent nécessiter de passer l'affichage sur "Console Only" lors de l'installation initiale de l'OS puis de revenir à "Full Graphics".
- Lors de la configuration, vous devrez peut-être activer le mode promiscuité pour l'interface réseau afin que fatt, suricata et p0f fonctionnent correctement.
- Si vous souhaitez utiliser une carte wifi comme NIC principale pour SIEM Stack, sachez que tous les pilotes d'interface réseau ne prennent pas en charge toutes les cartes sans fil. Dans VirtualBox, par exemple, il faut choisir le modèle *"MT SERVER"* pour la NIC.
<br><br>

## Exécution sur matériel physique
SIEM Stack n'est limité que par le support matériel des [images de distributions Linux supportées](#choose-your-distro). Il est recommandé de vérifier la HCL (liste de compatibilité matérielle) et de tester les distributions supportées avec SIEM Stack avant d'investir dans du matériel dédié.
<br><br>

## Exécution dans le cloud
SIEM Stack a été testé et fonctionne sur ...
* Telekom OTC en utilisant la méthode post-install
... d'autres peuvent fonctionner, mais n'ont pas été testés.


## Ports requis
En plus des ports généralement nécessaires à l'OS (DHCP, DNS, etc.), SIEM Stack nécessitera les ports suivants pour les connexions entrantes/sortantes. Consultez l'[architecture SIEM Stack](#technical-architecture) pour une représentation visuelle. Certains ports apparaîtront en double, ce qui est normal car utilisés dans différentes éditions.

=======
## Ports requis
En plus des ports généralement nécessaires à l'OS (DHCP, DNS, etc.), SIEM Stack nécessitera les ports suivants pour les connexions entrantes/sortantes. Consultez l'[architecture SIEM Stack](#technical-architecture) pour une représentation visuelle. Certains ports apparaîtront en double, ce qui est normal car utilisés dans différentes éditions.

| Port                                                                                                                                  | Protocole | Direction | Description                                                                                         |
| :------------------------------------------------------------------------------------------------------------------------------------ | :------- | :-------- | :-------------------------------------------------------------------------------------------------- |
| 80, 443                                                                                                                               | tcp      | sortant  | Gestion SIEM Stack : installation, mises à jour, logs (OS, GitHub, DockerHub, Sicherheitstacho, etc.)        |
| 11434                                                                                                                                 | tcp      | sortant  | Honeypots basés LLM : accès à votre installation Ollama                                                |
| 64294                                                                                                                                 | tcp      | entrant  | Gestion SIEM Stack : transmission des données capteur vers la ruche (via NGINX reverse proxy) vers 127.0.0.1:64305 |
| 64295                                                                                                                                 | tcp      | entrant  | Gestion SIEM Stack : accès SSH                                                                     |
| 64297                                                                                                                                 | tcp      | entrant  | Gestion SIEM Stack : accès au reverse proxy NGINX                                                      |
| 5555                                                                                                                                  | tcp      | entrant  | Honeypot : ADBHoney                                                                                  |
| 22                                                                                                                                    | tcp      | entrant  | Honeypot : Beelzebub  (LLM requis)                                                                 |
| 5000                                                                                                                                  | udp      | entrant  | Honeypot : CiscoASA                                                                                  |
| 8443                                                                                                                                  | tcp      | entrant  | Honeypot : CiscoASA                                                                                  |
| 443                                                                                                                                   | tcp      | entrant  | Honeypot : CitrixHoneypot                                                                            |
| 80, 102, 502, 1025, 2404, 10001, 44818, 47808, 50100                                                                                  | tcp      | entrant  | Honeypot : Conpot                                                                                    |
| 161, 623                                                                                                                              | udp      | entrant  | Honeypot : Conpot                                                                                    |
<<<<<<< HEAD

## Tableau de bord K
ibana
Sur la page d'accueil SIEM Stack, cliquez sur « K
ibana » pour accéder aux tableaux de bord. Vous pouvez sélectionner parmi une grande variété de dashboards et de visualisations adaptés aux honeypots supportés par SIEM Stack.

![Kibana Dashboard 1](doc/Kibana_a.png)
![Kibana Dashboard 2](doc/Kibana_b.png)
![Kibana Dashboard 3](doc/Kibana_c.png)
<br><br>

## Carte d'attaque
Sur la page d'accueil SIEM Stack, cliquez sur « Carte d'attaque » pour y accéder. La carte utilise les websockets, il peut être nécessaire de ressaisir les identifiants `<WEB_USER>`.

![Attack Map](doc/Attackmap.png)
<br><br>

## Cyberchef
Sur la page d'accueil SIEM Stack, cliquez sur « Cyberchef » pour y accéder.

![Cyberchef](doc/Cyberchef.png)
<br><br>

## Elasticvue
Sur la page d'accueil SIEM Stack, cliquez sur « Elasticvue » pour y accéder.

![Elasticvue](doc/Elasticvue.png)
<br><br>

## Spiderfoot
Sur la page d'accueil SIEM Stack, cliquez s²ur « Spiderfoot » pour y accéder.

![Spiderfoot](doc/Spiderfoot.png)
<br><br>
