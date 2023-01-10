# Accueil

## Projet intégrateur

explication (sommaire) du projet

## Idée

explication de l'origine et de l'utilité

## Séquence

explication des étapes du pipeline

- noeud: RPi Pico, abbreviation
	- 1 noeud
	- 3 noeuds, plus tard
	- 3^e^ noeud FOMO, plus tard
- pont: WiFi
	- IP direct
	- serveur MQTT[^mqtt] footnote
		- Mosquitto
		- HiveHQ
	- LoRa, passerelle, LoRaWAN, TNN, serveur MQTT, plus tard
- base de données: MongoDB Atlas
- tableau de bord: Streamlit
	- importation des données de Atlas dans Streamlit
		- synchrone
		- asynchrone
	- hébergement sur GitHub
	- contruction du site avec Streamlit Cloud 

*[RPi Pico]: Raspberry Pi Pico

[^mqtt]: sladfjsdalojf

admonition

!!! note "Note"

    admonitions: note, abstract, tip, success, question, warning, failure, danger, bug, example, quote
	inline, end
	
??? note

    admonitions: note, abstract, tip, success, question, warning, failure, danger, bug, example, quote
	+

special characters

a^b^c

a~b~c

~~abc~~

++ctrl++

table

|   |   |
|---|---|
| a | b |

definition

`Lorem ipsum dolor sit amet`

:   Sed sagittis eleifend rutrum. Donec vitae suscipit est. Nullam tempus
    tellus non sem sollicitudin, quis rutrum leo facilisis.

`Cras arcu libero`

:   Aliquam metus eros, pretium sed nulla venenatis, faucibus auctor ex. Proin
    ut eros sed sapien ullamcorper consequat. Nunc ligula ante.