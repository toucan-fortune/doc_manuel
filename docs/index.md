# Accueil

## Projet

L'objectif est de construire une chaine calqués sur le modèle d'AIoICanada. À partir d'un objet -- un capteur, qui mesure des données environnementales quelconque, branché sur un microcontrôleur quelconque --, des données brutes et des statistiques issues de calculs à la pointe[^pointe] parcourent divers composants pour arriver dans une interface -- comme une tableau de bord.

[^pointe]: Communément qualifié de *edge computing*.

![Diagramme d'AIoTCanada](img/aiotcanada2.jpg)

![Diagramme d'AIoTCanada](img/aiotcanada1.jpg)

PLUS: sommaire du CdeC

## Idée du projet

Pour appliquer cette, nous nous sommes inspirés d'un article du magazine Elektor (version française) de juillet/août 2021.

L'article propose un projet de labo: LoRa avec le Raspbery Pi Pico, S'amuser avec MicroPython. Le projet consiste à capter des températures, de les transmettre via un réseau LoRaWAN. Ensuite, le flux de données est récupéré en temps réel grâce à un service MQTT pour être affiché sur une interface web Node-RED fonctionnant sur un Raspberry Pi.

![Article d'Elektor](img/elektor1.jpg)

## Application du projet

Ce genre de projet est classique en IoT. La valeur provient de la gestion de données. Une fois l'infrastructure en place, les données captées et les autres données obtenues par calculs à la pointe ou dans le tableau de bord permettent de renseigner les utilisateurs à des fins de gestions des revenus et des coûts.

Les usages sont nombreux:

- mesures automatisées,
- contrôle des états, de la qualité,
- prévention de pertes, d'accidents,
- alertes,
- etc. 

PLUS: sommaire du CdeC
