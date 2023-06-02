# Smartbox
# Description du projet:

Le projet Smart Box vise à résoudre deux problèmes clés dans notre appartement : les coûts énergétiques élevés dus à l'utilisation continue du ventilateur de la salle de bain  et le risque de dommages causés par les fuites d'eau. Pour cela, nous avons conçu une boîte intelligente qui automatise le fonctionnement du ventilateur et intègre des capteurs de détection des fuites d'eau.

# Fonctionnement:

Le fonctionnement de la Smart Box repose sur l'automatisation du ventilateur de la salle de bain et la détection des fuites d'eau. Voici une description détaillée de son       fonctionnement :

*Automatisation du ventilateur :

La Smart Box est équipée de capteurs d'humidité qui détectent le niveau d’humidité de la salle de bain.Lorsque le capteur détecte un seuil supérieur a (x valeur) de la salle de bain, il envoie un signal a la carte UCA de la Smart Box.Le microcontrôleur active alors le relais correspondant au ventilateur, permettant ainsi son fonctionnement. Une fois que le niveau d’humidité est inférieur au valeur X, le microcontrôleur désactive le relais du ventilateur, éteignant ainsi le ventilateur automatiquement.

*Détection des fuites d'eau :

La Smart Box est équipée de capteurs d'eau, ces capteurs surveillent en permanence la présence d'eau anormale. Si une fuite est détectée, le capteur envoie un signal au      microcontrôleur.Le microcontrôleur reçoit le signal de détection de fuite et déclenche une alerte en temps réel.
# Matériels utilisés
+ Une carte UCA,
+ Trois relais Arduino 
+ Des capteurs de pluie FC-37
+ Des câbles femelles et male
+ Une électrovanne 
+ Une mini pompe.  
+ Un plateau pour assembler les éléments. 
+	Une boite en plastique pour simuler le passage d’eau. 
# Logiciels et Applications utilisés
+	Arduino
+	The Things Networks
+	LoRa
+	LoRaWan
+	TagoIO

# Montage
Connecter un relais à une carte Arduino.
+ ![image](https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/1cad0f34-3359-40df-9bf8-d0cb48a98736)
+ Le relais et la carte UCA seront connectés comme suit : 
+ !<img width="156" alt="conexion de un relais" src="https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/ad3d1537-beae-4134-ab98-0fbf3261e7ba">
+ Une fois le câblage effectué, nous utiliserons le code suivant sur ARDUINO.
``` 
int Relay =8;
void setup()
{
  pinMode(Relay, OUTPUT);   //set pin as output
}
void loop()
{
  digitalWrite(Relay, HIGH);   //turn off relay
  delay(5000);
  digitalWrite(Relay, LOW);   //turn on relay
  delay(5000);
}
```
Pour que le ventilateur de la salle de bain s'éteigne ou s'allume en fonction de l'humidité détectée, nous utiliserons le code suivant.

```
#include <Wire.h>
#include "SHTSensor.h" // http://librarymanager/All#arduino_shtc3

#define RELAYPIN 8 // Définit la broche de commande du relais
#define HUMIDITY_THRESHOLD 60 // Définit le seuil d'humidité pour déclencher le relais

SHTSensor sht;

void setup() {
  pinMode(RELAYPIN, OUTPUT); // Configure la broche de commande du relais en sortie

  Serial.begin(115200);
  Wire.begin();
  sht.init();
  sht.setAccuracy(SHTSensor::SHT_ACCURACY_MEDIUM); // only supported by SHT3x

  Serial.println("Temperature:,Humidity:");   // Plot labels

}
void loop() {
    sht.readSample();
    Serial.print(sht.getTemperature());
    Serial.print(F(" "));
    Serial.println(sht.getHumidity());
    

uint8_t temp = map(sht.getTemperature(), 20,35,170,0); // Map value from luminosity sensor to LED

  int humidity =sht.getHumidity(); // Lit la valeur d'humidité du capteur

  if (humidity > HUMIDITY_THRESHOLD) {
    digitalWrite(RELAYPIN, HIGH); // Active le relais
  } else {
    digitalWrite(RELAYPIN, LOW); // Désactive le relais
  }

```
# Schéma pour la connexion de l’électrovanne.
![image](https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/594d9ec1-7e29-4fda-8396-bf64e3be5b12)

# Assemblage d'électrovanne.

![image](https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/944f8ba3-bf55-44ac-9339-3a37d1048615)

# Détection des fuites d'eau.
Code de détection des fuites d'eau:
```
int sensorPin = A0; // Pin du capteur
int valvulaPin = 8; // Pin de l'électrovanne

int seuil = 600; // Valeur de référence du capteur
int fuiteEau = 0; // Indicateur de fuite d'eau

void setup() {
pinMode(sensorPin, INPUT); // Configure le pin du capteur comme une entrée
pinMode(valvulaPin, OUTPUT); // Configure le pin de l'électrovanne comme une sortie
digitalWrite(valvulaPin, LOW); // Initialement, l'électrovanne est ouvert
Serial.begin(9600); // Configuration de la communication série
Serial.println("BEGIN");
}

void loop() {
int valeurCapteur = analogRead(sensorPin); // Lecture de la valeur du capteur
if (valeurCapteur < seuil) { // Si la valeur du capteur est supérieure au seuil 
  fuiteEau = 1; // Indiquer qu'il y a une fuite d'eau
  digitalWrite(valvulaPin, HIGH); // fermer l'électrovanne pour couper le flux d'eau
  Serial.println("désactivation de la vanne");
  Serial.println("Une fuite d'eau a été détectée !"); // Notifier la détection de la fuite
}
else if (fuiteEau == 1) { // Si une fuite a été détectée précédemment et maintenant le niveau d'eau a baissé
  fuiteEau = 0; // Réinitialiser l'indicateur de fuite d'eau
  digitalWrite(valvulaPin, LOW); // Ouvrir l'électrovanne pour permettre à l'eau de s'écouler à nouveau
  Serial.println("La fuite d'eau a été réparée !"); // Notifier la réparation de la fuite
}
delay(1000); // Délai d'une seconde avant de réaliser la prochaine lecture

}

```

# Montage du raccord de la mini pompe à eau.
!![cenexion minipompe](https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/34b809ad-ae8f-4223-b178-0d21a952521d)

Code de la mini pompe à eau.
```
int sensorPin = A0; // Pin du capteur
int valvulaPin = 8; // Pin de l'électrovanne

int seuil = 600; // Valeur de référence du capteur
int fuiteEau = 0; // Indicateur de fuite d'eau

void setup() {
pinMode(sensorPin, INPUT); // Configure le pin du capteur comme une entrée
pinMode(valvulaPin, OUTPUT); // Configure le pin de l'électrovanne comme une sortie
digitalWrite(valvulaPin, HIGH); // Initialement, l'électrovanne est ouvert
Serial.begin(9600); // Configuration de la communication série
Serial.println("BEGIN");
}

void loop() {
int valeurCapteur = analogRead(sensorPin); // Lecture de la valeur du capteur
if (valeurCapteur < seuil) { // Si la valeur du capteur est supérieure au seuil 
  fuiteEau = 1; // Indiquer qu'il y a une fuite d'eau
  digitalWrite(valvulaPin, LOW); // fermer l'électrovanne pour couper le flux d'eau
  Serial.println("désactivation de la vanne");
  Serial.println("Une fuite d'eau a été détectée !"); // Notifier la détection de la fuite
}
else if (fuiteEau == 1) { // Si une fuite a été détectée précédemment et maintenant le niveau d'eau a baissé
  fuiteEau = 0; // Réinitialiser l'indicateur de fuite d'eau
  digitalWrite(valvulaPin, HIGH); // Ouvrir l'électrovanne pour permettre à l'eau de s'écouler à nouveau
  Serial.println("La fuite d'eau a été réparée !"); // Notifier la réparation de la fuite
}
delay(1000); // Délai d'une seconde avant de réaliser la prochaine lecture

```
# Assemblage final de la smart box.
!![Montagefinal](https://github.com/AnaandMarcelle/Smartbox2/assets/128797197/97c555a9-e187-4bb0-8db3-831fee9b11cd)



