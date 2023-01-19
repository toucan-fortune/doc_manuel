# Sources et codes

## Sources

- Elektor, sch
	- https://github.com/ElektorLabs/210047-LoRa-with-the-Raspberry-Pi-Pico
- Mongo University
- sites et documentation des modules Python, MicroPython
- sites et documentation des services web (Mosquitto, HiveHQ,)
- sites et documentation des composants physiques (RPi Pico, capteurs),
- sites et documentation des logiciels (Thonny, PlantUML, ), etc.
- GitHub pour héberger
	- codes Pico
	- code Streamlit
	- MkDocs

## Codes

Les diagrammes ont été réalisés avec PlantUML avec leur [interface en ligne](https://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000) avec leur [document PDF](https://plantuml.com/fr/download).

## Diagrammes de déploiement

*deployment diagrams*

### WiFi

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

### LoRa

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

### Plus

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

## Diagrammes d'activité

*activity diagrams*

### RPi Pico

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

### Streamlit

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

## Diagramme de classe

*class diagrams*

### RPi Pico

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

## Modèle de document MongoDB

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

## Diagramme d'état

*state diagrams*

### RPi Pico

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

### Streamlit

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

## Autres diagrammes

D'autres diagrammes n'ont pas été utilisé, car inutiles:

- aucun diagrammme d'utilisation (*use case*); pas d'usager de logiciel, produit;
- aucun diagramme de séquence (*use case*); idem;
- aucun diagramme de paquetage; aucun logiciel complexe.
