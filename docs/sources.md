# Codes et sources

## Codes

Les diagrammes ont été réalisés avec PlantUML avec leur [interface en ligne](https://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000) avec leur [document PDF](https://plantuml.com/fr/download). Voici les codes utilisés.

### Diagrammes de déploiement

*deployment diagrams*

#### WiFi

```
@startuml
Title "Diagramme de déploiement (WiFi)"
'----------
'NOEUDS
'----------
node "Noeud" {
  ["Capteur \n(température, humidité; caméra)"] #Darkorange/white 
  ["RPi Pico \n(avec WiFi et BLE)"] #Indianred/white
  note right : Analyses \n\n(ou Teensy et \nTinyML potentiel)
  ("de l'ordinateur") #DarkGray
}

node "PC ou autre \nservant de passerelle" {
  ["serveur \nMQTT"] #Khaki/white
}

cloud Internet/Infonuagique #LightCyan {
  ["service \nMQTT"] #DodgerBlue/white
  database "MongoDB Atlas" #Yellowgreen/white {
    collections "Documents" #Greenyellow/white
  }
  database GitHub #Green/white {
    collections "repo Tableau" #Seagreen/white
  }
  cloud "Streamlit Cloud" #Cyan {
    ["Tableau \nde bord"] #Seagreen/white
  }
}

node "Poste de développement" {
  package "Streamlit" #Magenta/white {
    ["projet Tableau"] #Violet/white
  }
  package "Thonny" #Magenta/white {
    ["Programmes \nMicroPython"] #Violet/white
  }
  ("vers le MCU") #DarkGray
  note right: aussi \nEDI et programme C/C++
}
'----------
'LIENS
'----------
["Capteur \n(température, humidité; caméra)"] <--> ["RPi Pico \n(avec WiFi et BLE)"] : GPIO: numérique ou analoqique
("de l'ordinateur") --> ["RPi Pico \n(avec WiFi et BLE)"] : USB
["RPi Pico \n(avec WiFi et BLE)"] <.-> ["serveur \nMQTT"]: WiFi

["serveur \nMQTT"] .-> ["service \nMQTT"] : IP

["service \nMQTT"] .-> "Documents" : IP
"Documents" .-> ["projet Tableau"] : IP
["projet Tableau"] <.-> "repo Tableau" : IP/git
"repo Tableau" .--> ["Tableau \nde bord"]: IP
"Documents" .-> ["Tableau \nde bord"] : API

["Programmes \nMicroPython"] --> ("vers le MCU") : USB

@enduml
```

#### LoRa

```
@startuml
Title "Diagramme de déploiement (LoRa)"
'----------
'NOEUDS
'----------
node "Noeud" {
  ["Capteur \n(température, humidité; caméra)"] #Darkorange/white 
  ["RPi Pico \n(avec WiFi et BLE)"] #Indianred/white
  note right : Analyses \n\n(ou Teensy et \nTinyML potentiel)
  ("de l'ordinateur") #DarkGray
  ["émetteur-transmetteur \nLoRa"] #Darkorange/white
}

node "Passerelle" {
  [LoRaWAN] #Khaki/white
}

cloud Internet/Infonuagique #LightCyan {
  ["The \nThings \nNetwork"] #DodgerBlue/white
  database "MongoDB Atlas" #Yellowgreen/white {
    collections "Documents" #Greenyellow/white
  }
  database GitHub #Green/white {
    collections "repo Tableau" #Seagreen/white
  }
  cloud "Streamlit Cloud" #Cyan {
    ["Tableau \nde bord"] #Seagreen/white
  }
}

node "Poste de développement" {
  package "Streamlit" #Magenta/white {
    ["projet Tableau"] #Violet/white
  }
  package "Thonny" #Magenta/white {
    ["Programmes \nMicroPython"] #Violet/white
  }
  ("vers le MCU") #DarkGray
  note right: aussi \nEDI et programme C/C++
}
'----------
'LIENS
'----------
["Capteur \n(température, humidité; caméra)"] <--> ["RPi Pico \n(avec WiFi et BLE)"] : GPIO: numérique ou analoqique
("de l'ordinateur") --> ["RPi Pico \n(avec WiFi et BLE)"] : USB
["RPi Pico \n(avec WiFi et BLE)"] <--> ["émetteur-transmetteur \nLoRa"] : GPIO: UART ou SPI?

["émetteur-transmetteur \nLoRa"] <.-> [LoRaWAN]: RF
[LoRaWAN] <.-> ["The \nThings \nNetwork"] : IP

["The \nThings \nNetwork"] .-> "Documents" : MQTT?
"Documents" .-> ["projet Tableau"] : IP
["projet Tableau"] <.-> "repo Tableau" : IP/git
"repo Tableau" .--> ["Tableau \nde bord"]: IP
"Documents" .-> ["Tableau \nde bord"] : IP

["Programmes \nMicroPython"] --> ("vers le MCU") : USB

@enduml
```

#### Plus

```
@startuml
Title "Diagramme de déploiement"
'----------
'NOEUDS
'----------
cloud Internet/infonuagique #LightCyan {
  database "MongoDB Atlas" #Yellowgreen/white {
    collections "Documents" #Greenyellow/white
  }
}

node "Poste de développement" {
  package "pymongo \nNumpy-Pandas \nvisualisation... \nScikit-Learn, statsmodels..." #Magenta/white {
    ["Analyses et \nML potentiel"] #Violet/white
  }
  package "Streamlit" #Magenta/white {
    ["projets"] #Violet/white
  }
}
'----------
'LIENS
'----------
"Documents" <.-> ["Analyses et \nML potentiel"] : IP
["Analyses et \nML potentiel"] -> ["projets"]

@enduml
```

### Diagrammes d'activité

*activity diagrams*

#### RPi Pico

Diagramme réel

```
@startuml
repeat
  partition Capteur {
    :Mesurer;
  }
  partition RPi Pico {
    :Recevoir;
    :Envoyer données \nà passerelle WiFi;
  }
repeat while (Intervalle de temps \nparamétré) is (nouveau \ncycle)
stop
@enduml
```

Diagramme planifié

```
repeat
  partition Capteur {
    :Mesurer;
  }
  partition RPi Pico {
    :Recevoir et \nenregistrer mesures \nen mémoire flash;
    :Analyser mesures et \ncalculer statistiques;
    :Envoyer données \npar passerelle, serveur;
  }
repeat while (Intervalle de temps \nparamétré) is (nouveau \ncycle)
stop
```

#### Streamlit

```
@startuml
partition Site Streamlit {
start
:Ouvrir page web;
:Connecter à \nMongoDB Atlas;
if (S'authentifier?) is (réussi) then
  :Utiliser paramètres \npar défaut;
  (A)
  kill
else
  :Message erreur;
kill

(A)
if (Changer paramètres?) is (fait) then
  :Mettre à jour \net continuer;
  (B)
  kill
else
  :Continuer;
  (B)
  kill

(B)
:Lancer l'extraction;
repeat
  :Extraire données de \nMongoDB Atlas;
  (C)
'backward:Vérifier;
repeat while (Intervalle de temps \nparamétré) is (nouveau \ncycle)
:Arrêter l'extraction;
kill

(C)
:Traiter et présenter \ndonnées;
:Fermeture page web;
stop
@enduml
```

### Diagramme de classe

*class diagrams*

#### RPi Pico

```
@startuml
class User {
  username
  password
  +sign_in()
}

class Group {
  name
}

class Member {
  roles
}

User .. Member
Group .. Member
@enduml
```

### Modèle de document MongoDB

*display JSON*

```
@startjson
{
"datetime": "2023-01-13 16:15:39.401550",
"noeud": "pico01",  
"capteur": "temperature",
"metrique": "brute",
"valeur": 23.4519
}
@endjson
```

### Diagramme d'état

*state diagrams*

#### RPi Pico

```
@startuml
[*] --> Branché

Branché: RPi Pico branché \npour code et \nalimenté (On)
Branché-> Débranché
Débranché: RPi Pico débranché et \nnon alimenté (On)
Branché<- Débranché

Branché--> [*]
Débranché--> [*]
@enduml
```

#### Streamlit

```
@startuml
[*] --> Actif

Actif: Session du site \nStreamlit active
Actif-> Inactif
Inactif: Aucun session active
Actif<- Inactif

Actif--> [*]
Inactif--> [*]
@enduml
```

### Autres diagrammes

D'autres diagrammes n'ont pas été utilisé, car inutiles:

- aucun diagrammme d'utilisation (*use case*); pas d'usager de logiciel, produit;
- aucun diagramme de séquence (*use case*); idem;
- aucun diagramme de paquetage; aucun logiciel complexe.

## Outils

### Services

- Documentation
	  - PlantUML (diagrammes)
	  - Markdown (MkDocs, GitHub, etc.)
	  - MkDocs
	  - tableur pour la comptabilité, le budget (pièces et autres)
- Git
	  - Git
	  - GitHub ou autre service
- Gestion du projet et d'équipe
	  - Trello
	  - Teams
- Streamlit Cloud

### Base de données
	
- MongoDB, MongoDB Atlas
- Compass

### Logiciels

- JupyterLab pour Python (pour préparer des parties de Streamlit)
- Spyder pour Streamlit, langages web et Python
- VS pour Markdown et Python
- Thonny pour MicroPython

## Composants

- microcontrôleur Raspberry Pi Pico W (sans entêtes)
- entêtes 2x20 à souder
- platine de développement
- alimentation 3.3V (câble USB ou piles)
- capteur de température-humidité numérique I2C DS 18B20

## Sources

### Fiches Techniques

- "RP002-1.0.3 LoRaWAN® Regional Parameters" (5 mai, 2021)
"( CHT8305 ) Digital Humidity & Temperature Sensor" (octobre 2017, Rev. 1.1) SENSYLINK Microelectronics Co., LTD

### Livres et Documents PDF

- "Connecting to the Internet with Raspberry Pi Pico W" (novembre, 2022)
- "Getting started with Raspberry Pi Pico" (novembre, 2022)
- "MicroPython Documentation" (13 janvier, 2023)
- "Raspberry Pi Pico Python SDK" (novembre, 2022)
- "Get started with MicroPython on Raspberry Pi Pico", par Gareth Halfacree and Ben Everard, Raspberry Pi PRESS (THE OFFICIAL RASPBERRY PI PICO GUIDE)
- Building with Patterns: A Summary, de MongoDB

### Articles de magazines

- Make: "GO LONG! Need to monitor remote sensors on your next project. LoRa might be your best wireless bet." par Andreas Spiess (makeprojects.com)
- Elektor: "LoRa avec le Raspberry Pi Pico - S'amuser avec MicroPython" (mars/avril 2020) (www.elektormagazine.fr)
    - https://github.com/ElektorLabs/210047-LoRa-with-the-Raspberry-Pi-Pico
- Elektor: "LoRaWan: décollage facile - Avec Blue Pil, passerelle LoRa et The Things Network" (juillet/août 2021) (www.elektormagazine.fr)

### Electronique

- https://onlinedocs.microchip.com/pr/GUID-61202E6F-BC03-4D81-AAB9-269E87F9B49C-en-US-15/index.html?GUID-45D0726D-4F75-4875-96B2-876AF5F2EDB2
- https://www.electricaltechnology.org/2020/04/electrical-electronic-abbreviations-full-forms.html
- https://docs.micropython.org/en/latest/esp8266/tutorial/onewire.html
- https://learn.adafruit.com/working-with-i2c-devices/pull-up-resistors
- https://www.electronics-tutorials.ws/logic/pull-up-resistor.html
- https://en.wikipedia.org/wiki/1-Wire
- https://micropython-lego-ri5.readthedocs.io/en/latest/library/_onewire.html
- https://www.build-electronic-circuits.com/pull-up-resistor/
- https://woopcb.com/blog/what-is-vcc-vss-vdd-vee-in-electronics
- https://control.com/technical-articles/what-is-baud-rate-and-how-does-it-work/
- https://electronics.stackexchange.com/questions/44670/what-is-bit-banging
- https://circuitdigest.com/article/introduction-to-bit-banging-spi-communication-in-arduino-via-bit-banging
- https://mpython.readthedocs.io/en/master/library/micropython/onewire.html

### Helium

- https://www.okdo.com/getting-started/get-started-with-helium-network/#h-0-step-1-helium-toc
- https://www.helium.com/
- https://www.helium-iot.eu/
- https://www.reddit.com/r/HeliumNetwork/comments/uyvmis/hip51_and_why_its_important_for_hnt/

### HiveMQ

- https://www.hivemq.com/downloads/hivemq/
- https://www.hivemq.com/blog/mqtt-toolbox-mqtt-spy/
- https://www.hivemq.com/mqtt-toolbox/
- https://www.hivemq.com/public-mqtt-broker/
- https://www.hivemq.com/docs/hivemq/4.10/user-guide/system-requirements.html#os

### I2C

- https://stackoverflow.com/questions/45008131/how-can-i-read-unsigned-shorts-using-python
https://realpython.com/python-bitwise-operators/#overview-of-pythons-bitwise-operators
- https://docs.micropython.org/en/latest/library/machine.I2C.html
- https://en.wikipedia.org/wiki/Endianness
- https://stackoverflow.com/questions/5030919/how-to-read-write-binary-16-bit-data-in-python-2-x
- https://mipea.readthedocs.io/en/latest/i2c.html
- https://i2c.info/
- https://learn.adafruit.com/micropython-hardware-i2c-devices/i2c-main
- https://core-electronics.com.au/i2c-temperature-humidity-sensor-stainless-steel-shell.html
- https://wiki.dfrobot.com/SKU_SEN0546_I2C_Temperature_and_Humidity_Sensor_Stainless_Steel_Shell
- https://www.digikey.com/en/maker/projects/raspberry-pi-pico-rp2040-i2c-example-with-micropython-and-cc/47d0c922b79342779cdbd4b37b7eb7e2
- https://www.i2c-bus.org/i2c-Interface/
- https://www.waveshare.com/wiki/%E3%80%90MicroPython%E3%80%91machine.I2C_Function
- https://learn.adafruit.com/working-with-i2c-devices/terminology

### LoRa

- https://www.loraantenna.com/lorawan-frequency-plans-by-country-region/
- https://www.nexpcb.com/lorawan-selection-guide
- https://www.nexpcb.com/blog/lorawan-country-coverage
- https://github.com/ElektorLabs/210047-LoRa-with-the-Raspberry-Pi-Pico/blob/main/Software/lora.py

### MicroPython

- https://github.com/micropython/micropython
- https://stackoverflow.com/questions/75089980/convert-a-bytes-string-to-dictionary-in-python
- https://docs.micropython.org/en/latest/genrst/core_language.html
- https://docs.micropython.org/en/latest/library/index.- html#python-standard-libraries-and-micro-libraries
- https://how2electronics.com/micropython-ds18b20-temperature-sensor-esp32/
- https://docs.micropython.org/en/latest/
- https://stackoverflow.com/questions/57982782/type-codes-in-python-for-array
- https://docs.micropython.org/en/latest/esp8266/tutorial/dht.html
- https://docs.micropython.org/en/latest/esp8266/tutorial/repl.html
- https://docs.micropython.org/en/latest/rp2/quickref.html#delay-and-timing
- https://zetcode.com/python/bytes-type/
- https://micropython-usermod.readthedocs.io/en/latest/usermods_09.html
- https://www.educative.io/answers/what-are-type-codes-in-python
- https://stackoverflow.com/questions/52808481/unpack-unsigned-short-in-python
- https://wiki.micropython.org/gollum/Importing-Modules
- https://docs.python.org/3/library/ctypes.html
- http://python-notes.curiousefficiency.org/en/latest/python_concepts/import_traps.html

### MongoDB

- https://pymongo.readthedocs.io/en/stable/api/pymongo/mongo_client.html#pymongo.mongo_client.MongoClient
- https://www.mongodb.com/docs/drivers/pymongo/
- https://pypi.org/project/pymongo/
- Mongo University, M100, MongoDB for SQL Pros

### MQTT

- http://www.steves-internet-guide.com/mqtt/
- https://www.mosquitto.org/download/
- https://www.eclipse.org/paho/index.php?page=clients/c/index.php
- https://mqtt.org/getting-started/
- https://github.com/kamilfb/mqtt-spy/wiki/GettingStarted
- https://stackoverflow.com/questions/56455358/how-to-build-paho-mqtt-c-on-windows
- http://www.steves-internet-guide.com/into-mqtt-python-client/
- https://www.mosquitto.org/man/mosquitto-8.html
- https://stackoverflow.com/questions/26716279/how-to-test-the-mosquitto-server
- https://medium.com/python-point/mqtt-basics-with-python-examples-7c758e605d4
- https://mqtt.org/mqtt-specification/
- https://myqtthub.com/en
- https://github.com/kamilfb/mqtt-spy/wiki/Overview
- https://pypi.org/project/paho-mqtt/
- https://mqtt.org/software/

### Raspberry Pi Pico

- https://www.raspberrypi.com/tutorials/
- https://www.raspberrypi.com/documentation/microcontrollers/
- https://datasheets.raspberrypi.com/rp2040/rp2040-product-brief.pdf
- https://github.com/micropython/micropython/wiki/Differences
- https://github.com/dhylands/rshell
- https://learn.adafruit.com/micropython-basics-how-to-load-micropython-on-a-board/serial-terminal
- https://www.chiark.greenend.org.uk/~sgtatham/putty/
- https://realpython.com/lessons/repl-workflow/
- https://codewith.mu/en/tutorials/1.0/repl
- https://picockpit.com/raspberry-pi/raspberry-pi-pico-and-micropython-on-windows/
- https://zadig.akeo.ie/downloads/
- https://zadig.akeo.ie/
- https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-driver-installation-based-on-compatible-ids
- https://electrocredible.com/raspberry-pi-pico-tutorial-for-beginners/
- https://electrocredible.com/raspberry-pi-pico-i2c-tutorial-examples/
- https://electrocredible.com/raspberry-pi-pico-temperature-sensor-tutorial/
- https://raspberrypi.github.io/pico-sdk-doxygen/group__hardware__i2c.html
- https://www.youtube.com/watch?v=__v2P_RmXSY
- https://www.etechnophiles.com/raspberry-pi-pico-pinout-specifications-datasheet-in-detail/
- https://raspberrytips.com/raspberry-pi-pico-pinout/

### PubNub

- https://www.pubnub.com/docs
- https://www.pubnub.com/docs/sdks/javascript/
- https://www.pubnub.com/docs/sdks/python
- https://www.pubnub.com/docs/sdks/python/api-reference/mobile-push
- https://www.pubnub.com/blog/realtime-mongodb-to-fetch-and-stream-report-data/
- https://www.pubnub.com/docs/sdks/javascript/api-reference/publish-and-subscribe
- https://www.pubnub.com/docs/sdks/python/api-reference/publish-and-subscribe
- https://www.pubnub.com/docs/sdks/python/api-reference/configuration

### Python, Streamlit, Plotly

- https://numpy.org/doc/stable/
- https://pandas.pydata.org/docs/index.html
- https://docs.streamlit.io/
- https://daniellewisdl-streamlit-cheat-sheet-app-ytm9sg.streamlit.app/
- https://www.youtube.com/playlist?list=PLM8lYG2MzHmRpyrk9_j9FW0HiMwD9jSl5
- https://plotly.com/python/