# Accueil

## Projet intégrateur

- explication (sommaire) du projet
- sommaire du cdec et docéquipe

## Idée

- explication de l'origine et de l'utilité
- sommaire du cdec et docéquipe

## Séquence

- explication des étapes du pipeline

- noeud: RPi Pico, abbreviation
	- 1 noeud
	- 3 noeuds, plus tard
	- émetteur LoRa, plus tard
	- 3^e^ noeud FOMO, plus tard
- pont
	- passerelle WiFi via serveur MQTT[^mqtt] et service MQTT
		- serveur Mosquitto et ?
		- serveur HiveHQ et ?
	- passerelle LoRaWAN, The Things Network, serveur MQTT, plus tard
- base de données: MongoDB Atlas
- tableau de bord: Streamlit
	- importation des données de Atlas dans Streamlit
	- hébergement sur GitHub
	- contruction du site avec Streamlit Cloud 

[^mqtt]: Message Queuing Telemetry Transport.
