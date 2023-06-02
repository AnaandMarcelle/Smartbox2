# Smartbox
# Description du projet:

Le projet Smart Box vise à résoudre deux problèmes clés dans notre appartement : les coûts énergétiques élevés dus à l'utilisation continue du ventilateur de la salle de bain  et le risque de dommages causés par les fuites d'eau. Pour cela, nous avons conçu une boîte intelligente qui automatise le fonctionnement du ventilateur et intègre des capteurs de détection des fuites d'eau.

# Fonctionnement:

Le fonctionnement de la Smart Box repose sur l'automatisation du ventilateur de la salle de bain et la détection des fuites d'eau. Voici une description détaillée de son       fonctionnement :

*Automatisation du ventilateur :

La Smart Box est équipée de capteurs d'humidité qui détectent le niveau d’humidité de la salle de bain.Lorsque le capteur détecte un seuil supérieur a (x valeur) de la salle de bain, il envoie un signal a la carte UCA de la Smart Box.Le microcontrôleur active alors le relais correspondant au ventilateur, permettant ainsi son fonctionnement. Une fois que le niveau d’humidité est inférieur au valeur X, le microcontrôleur désactive le relais du ventilateur, éteignant ainsi le ventilateur automatiquement.

*Détection des fuites d'eau :

La Smart Box est équipée de capteurs d'eau, ces capteurs surveillent en permanence la présence d'eau anormale. Si une fuite est détectée, le capteur envoie un signal au       microcontrôleur.Le microcontrôleur reçoit le signal de détection de fuite et déclenche une alerte en temps réel.
# Matériels utilisés
o	Une carte UCA,
o	Trois relais Arduino 
o	Des capteurs de pluie FC-37
o	Des câbles femelles et male
o	Une électrovanne 
o	Une mini pompe.  
o	Un plateau pour assembler les éléments. 
o	Une boite en plastique pour simuler le passage d’eau. 
# Logiciels et Applications utilisés
o	Arduino
o	The Things Networks
o	LoRa
o	LoRaWan
o	TagoIO

