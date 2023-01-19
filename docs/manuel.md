# Manuel de l'utilisateur

## Apprentissage

### Services

- Documentation
	- Diagrammes PlantUML
	- https://plantuml.com/fr/starting
	-  https://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000
	- Markdown
	- MkDocs
	- Fritzing
	- tableur pour la comptabilité, le budget (pièces et autres)
- Git
	- Git
	- GitHub ou autre service
	- Markdown
- Gestion du projet et d'équipe
	- Trello
	- Teams

### Base de données
	
- MongoDB, MongoDB Atlas
- architectures, schéma, patrons de collections et documents
- Compass
- CLI, JS

### Protocols

- WiFi et sa programmation
- LoRa, LoRaWAN, TTN ou TPN et sa programmation

### Logiciels

- Streamlit, langages web, Python
- VS pour Markdown et Python
- Spyder et JupyterLab pour Python (pour préparer des parties de Streamlit)
- Thonny pour MicroPython
- programmer en MicroPython, en C/C++

### Composants

- microcontrôleur Raspberry Pi Pico W (sans entêtes)
	- https://hackspace.raspberrypi.com/books/micropython-pico
- entêtes 2x20 à souder
- platine de développement
- alimentation 3.3V (câble USB ou piles)
- capteur de température-humidité numérique I2C DS 18B20

---

- carte LoRa RFM95-915
- carte BoB pour monter et alimenter la carte RFM95
- antenne 915MHz
- fil
- passerelle LoRaWAN
- plus de microcontrôleurs pour construire plus de noeuds WiFi, puis plus de composants LoRa pour les convertir en neouds LoRa

---

- Autre MCU: ESP32, Teensy
- Manuel et sites

## Séquence

explication des étapes du pipeline

état de la situation à ce point du projet poursuite du projet

![](img/diagramme_deploiement_wifi.png)

![](img/diagramme_deploiement_lora.png)


### Protocols -- LoRa

- autre LPWAN, et protocols non
- scale de 1 à 3 noeud et plus
- Avec LoRa, une fois les 1 noeud en place et une passerelle, on pourrait installer 500 noeuds sur la campus du collège, avec une passerelle extérieure en hauteur pour gérer le volume en jouant sur les fréquences des envois.
- envoi à tous les 10s, 30s, 10min, 1h, 1j, 1sem, selon le capteur
- transite dans le pipeline en 1s, le goulot d'étranglement est le site, techno comme Grafana, PubNub, Node-Red plus en temps réel, avec Streamlit, intervalles et asynchrone comme le UART, l'instantané n'est pas critique
- équipement LoRa de différentes gammes, gamme industrielle, passerelles
- fiable, sécurisé, pas de perte

- choix du LoRa, sch reseaux
- grand potentiel
- LoRa plus compliqué au démarrage, achats pour la passerelle, TTN, bref, investissement de départ en temps et argent
	- sch noeud et passerelle
	- notes passerelles
	- LoRa et autres LPWAN, sch protocols
	- sch avantages LoRa, sch canaux LoRa
	- pdf de la passerelle
	- sites, vidéos, livres, Udemy
- Passerelles abordables, d'intérieur (si on exclut les passerelles d'extérieur ou les produit de gamme industrielle, plus chers); sans passerelle, une unité (un MCU ou ordinateur avec module pour émission-transmission) ne peut accéder au réseaux LoRaWAN (The Things Network, The Things Industries, The People's Network par Helium) et seul les échanges p-to-p entre 2 ou plusieurs unités sont possibles (un LAN)
- Passerelles prête
	- LPS8N Indoor LoRaWAN Gateway 915 MHz (North America), 263$
	- Seeedstudio SenseCAP M2 Data-Only LoRaWAN Indoor Gateway (SX1302) - EU868, 292$
	- Seeedstudio SenseCAP M2 Data-Only LoRaWAN Indoor Gateway (SX1303) - EU868 , 292$
	- SeeedStudio LoRa/LoRaWAN Gateway - 915MHz for Raspberry Pi 3, 395$
	- STM32L0 Discovery kit LoRa, Sigfox, low-power wireless, The B-L072Z-LRWAN1 LoRa®/Sigfox™ Discovery kit is a development tool to learn and develop solutions based on LoRa®, Sigfox™, and FSK/OOK technologies. This Discovery kit features the all-in-one CMWX1ZZABZ-091 open module by Murata. The module is powered by an STM32L072CZ microcontroller and SX1276 transceiver. The transceiver features the LoRa® long-range modem, providing ultra-long-range spread-spectrum communication and high interference immunity, minimizing current consumption. Since CMWX1ZZABZ-091 is an open module, the user has access to all STM32L072CZ peripherals such as ADC, 16-bit timer, LP-UART, I2C, SPI, and USB 2.0 FS (supporting BCD and LPM).
- Pour fabriquer une passerelle
	- Abra, WAVE-22613, SX1303 915M Module Passerelle LoRaWAN Avec HAT Pour Raspberry Pi, Prise Mini-PCIe Standard, Transmission Longue Portée, 173$
	- RobotShop, SX1302 LoRaWAN Gateway HAT for Raspberry Pi, SX1302 868M EU868, GNSS Module, 151$

### Protocols -- WiFi

- remise en question, remiser LoRa
- WiFi pour débuter, facile, simple, LoRa futur
- gère moins de noeuds, plus lours, plus énergivore, moins pratique pour la portée et les zone non courverts par les bornes WiFi
- la fréquence des envois, la robustesse de l'équipement change pas
- Websocket envisagé

### Protocols

- WiFi et LoRa envoient à une passerelle, Web ou LoRaWAN, va sur le web
- WiFi, via un serveur MQTT, payload, service, puis BD
- LoRa, TTN ou TPN, MQTT, puis BD
- MQTT à un endroit ou un autre
- Mosquitto et HiveHQ

### Base de données

- BD, MongoDB Atlas, bon choix persistence
- Atlas est capable de récupérer d'immenses volumes
- sch types, sch pattern
- microcontrôleurs petite mémoire flash, persistence limitée
- passerelles, réseaux et MQTT pour la transition
- BD pour la persistence
- SQL aurait été une bonne option avec MariaDB ou PostgreSQL car le modèle est stable, à essayer

### Tableau de bord

- récupérer pour présenter, analyser, valeur ajoutée, dashboard
- Streamlit, bon choix, facile
- Ensuite on fetch sans problème, on transforme en pd.DataFrame, on fait les calculs et on affiche (métriques en boite, deltas), graphiques en tout genre, cartes, etc.
- GitHub, Streamlit Cloud
- ou Heroku, à essayer
- Grafana, à essayer
- sch Node-Red, à essayer
