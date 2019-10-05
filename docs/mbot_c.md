# Instructies mBot

## Inhoud
- [Instructies mBot](#instructies-mbot)
	- [Inhoud](#inhoud)
- [Introductie](#introductie)
	- [mBot](#mbot)
	- [Arduino](#arduino)
- [Verbinden](#verbinden)
	- [De mBot code toevoegen aan de Arduino IDE.](#de-mbot-code-toevoegen-aan-de-arduino-ide)
	- [De mBot met de computer verbinden](#de-mbot-met-de-computer-verbinden)
- [De mBot](#de-mbot)
	- [Inputs](#inputs)
		- [Ultrasoon](#ultrasoon)
		- [Lijnvolger](#lijnvolger)
		- [Lichsterkte](#lichsterkte)
	- [Outputs](#outputs)
		- [Motoren](#motoren)
		- [LEDs](#leds)
		- [Buzzer](#buzzer)
	- [Opdrachten](#opdrachten)
		- [1. Een vierkant rijden](#1-een-vierkant-rijden)
		- [2. Sensorwaarden laten zien](#2-sensorwaarden-laten-zien)
		- [3. Help, een muur!](#3-help-een-muur)
	- [Uitdagingen](#uitdagingen)
  
# Introductie
## mBot

- Voor je de mBot aanzet: <b>de mBot eerst op de grond zetten of op de kop leggen!</b>
- Hou rekening met de beperkte ruimte: laat het vermogen van de wielen eerst gewoon op 50% staan.
s
## Arduino

Als we met Arduino een programma schrijven dan hebben we altijd twee 'functies' die we altijd gebruiken, Arduino roept deze voor ons aan.

De functies zijn als volgt:

```Arduino
//Deze functie wordt aangeroepen zodra de arduino opstart,
//dit gebeurt dan ook maar 1 keer.
void setup() {

}

//Zodra de setup() functie is afgerond,
//wordt de loop() functie constant aangeroepen.
void loop() {

}
```

Het is dan ook verstandig om in de `void setup()` functie alle instellingen voor het programma uit te voeren.

Vervolgens kunnen we logica, zoals vooruit rijden, in de `void loop()` functie zetten.

<details>
    <summary>Spiekbriefje mBot arduino functies</summary>

```Arduino

Zorg ervoor dat je altijd het volgende bovenaan je schets hebt staan:

#include "MeMCore.h"

------------------------------------------------------------------------------------------

//Ultrasone sensor
MeUltrasonicSensor ultraSensor(PORT_7); // De ultrasone module kan alleen verbonden zijn met poort 3, 4, 5, 6, 7 of 8.

double MeUltrasonicSensor::distanceCm() //Dit geeft een double waarde voor de afstand in centimeters, iedere 100 milliseconden wordt dit getal geupdate.

Serial.print("Afstand : ");
Serial.print(ultraSensor.distanceCm() );
Serial.println(" cm");
delay(100); 

------------------------------------------------------------------------------------------

//Lichtsterkte sensor
MeLightSensor lightSensor(PORT_6);

int16_t MeLightSensor::read() //Dit geeft een integer waarde terug met de sterkte van de lichtsensor.

int16_t waarde = lightSensor.read();
Serial.print("waarde = ");
Serial.println(waarde);
delay(100);

------------------------------------------------------------------------------------------

//Lijnvolger sensor
MeLineFollower lineFinder(PORT_3); // De lijnvolger module kan alleen verbonden zijn met poort 3, 4, 5, of 6. 

uint8_t MeLineFollower::readSensors() // Dit geeft een getal terug, waarbij de waardes voor verschillende sensorwaarden staan:

switch(sensorState)
{
	case S1_IN_S2_IN: Serial.println("Sensor 1 en 2 zijn binnen een zwarte lijn"); break;
	case S1_IN_S2_OUT: Serial.println("Sensor 1 is binnen, en sensor 2 is buiten een zwarte lijn"); break;
	case S1_OUT_S2_IN: Serial.println("Sensor 2 is binnen, en sensor 1 is buiten een zwarte lijn"); break;
	case S1_OUT_S2_OUT: Serial.println("Sensor 1 en 2 zijn buiten een zwarte lijns"); break;
	default: break;
}

------------------------------------------------------------------------------------------

//Motoren

MeDCMotor motor1(PORT_1);

void MeDCMotor::run(int16_t speed) //Draaien met een gegeven snelheid
void MeDCMotor::stop(void) //Stoppen.	

uint8_t motorSpeed = 100;

motor1.run(motorSpeed); //Waarde tussen -255 en 255
delay(2000);
motor1.stop();
delay(100);
motor1.run(-motorSpeed);
delay(2000);
motor1.stop();
delay(2000);

------------------------------------------------------------------------------------------

//Led

MeRGBLed led(PORT_1);

// Ledjes worden in het RGB spectrum uitgedrukt. 
// De combinatie van rood, groen en blauw dus. 
// Is dit nieuw voor je? Kijk eens hier : https://www.colorspire.com/rgb-color-wheel/

// lednummer - rood - groen - blauw
bool MeRGBLed::setColorAt(uint8_t index, uint8_t red, uint8_t green, uint8_t blue)

led.setColor(0, 255, 255, 255); //De LED's wit maken
led.show(); //Om de kleur te activeren, moet je de .show() functie aanroepen.

------------------------------------------------------------------------------------------

//Buzzer

void buzzerOn()
void buzzerOff()

buzzerOn();
delay(1000);
buzzerOff();
delay(1000);



```

  </details>

# Verbinden

We gaan voor het progorammeren van de mBot de Arduino IDE gebruiken. 
Deze ontwikkelomgeving is te downloaden via de officiele arduino site: [arduino.org](https://arduino.com)

Ook hebben we de code nodig van de mBot voor de Arduino IDE. Deze code is hier te downloaden: [mblock code](https://example.com)

## De mBot code toevoegen aan de Arduino IDE.

- Start Arduino IDE
- Navigeer naar het Schets menu
  - Bibliotheek gebruiken	
  - .ZIP bibliotheek toevoegen
- Navigeer nu naar het .ZIP bestand dat je reeds hebt gedownload

## De mBot met de computer verbinden

- Start Arduino IDE
- Sluit de mBot aan met de USB-kabel en zet de robot aan
  - Zorg dat je bij de instellingen van de arduino IDE het volgende hebt gechecked:
    - Hulpmiddelen:
      - Board: Arduino Uno (/ Genuino Uno)
      - Poort: De correcte poort die je net hebt aangesloten
- Test of de verbinding gelukt is door de volgende schets in te laden: 
	```Arduino

		#include <MeMCore.h>

		MeRGBLed led(0, 30);

		void setup()
		{
			led.setpin(13);
		}
  
		void loop()
		{
			led.setColor(255, 255, 255); //Beide LED's wit maken
			led.show();                  //Om de kleur te activeren, moet je de .show() functie aanroepen.
			delay(500); //500 milliseconden wachten

			led.setColorAt(0, 255, 0, 0); //LED0 rood maken  (RGBLED1) (Rechterkant)
			led.setColorAt(1, 0, 0, 255); //LED1 blauw maken (RGBLED2) (Linkerkant)
			led.show();
			delay(500); //500 milliseconden wachten

			led.setColorAt(0, 0, 0, 255); //LED0 blauw maken  (RGBLED1) (Rechterkant)
			led.setColorAt(1, 255, 0, 0); //LED1 rood maken  (RGBLED2) (Rechterkant)
			led.show();
			delay(500); //500 milliseconden wachten	
		}

	```
  - Nu kun je op upload klikken. (De knop linksboven, met het pijltje naar rechts)
- Je bent nu klaar om een programma te gaan schrijven!

# De mBot

Nu we alles hebben ingesteld kunnen we aan de slag! Eerst gaan we door de zintuigen van de mBot, we noemen dit ook wel de inputs en outputs van de robot.

## Inputs

Met de inputs kan de mBot zijn omgeving waarnemen. Zo kan de mBot er bijvoorbeeld achter komen of hij niet tegen een muur aan gaat rijden, wel zo handig natuurlijk!

### Ultrasoon
dit zijn de twee 'ogen' voorop de mBot. De mBot gebruikt net als een vleermuis echo's om voorwerpen te 'zien'. Het ene oog stuurt een geluidje en het andere oog vangt de echo op.
<details>
<summary>Functies voor de ultrasoon</summary>

```Arduino

//Ultrasone sensor
MeUltrasonicSensor ultraSensor(PORT_7); // De ultrasone module kan alleen verbonden zijn met poort 3, 4, 5, 6, 7 of 8.

double MeUltrasonicSensor::distanceCm() //Dit geeft een double waarde voor de afstand in centimeters, iedere 100 milliseconden wordt dit getal geupdate.

Serial.print("Afstand : ");
Serial.print(ultraSensor.distanceCm() );
Serial.println(" cm");
delay(100); 

```

</details>

### Lijnvolger
voor het voorwiel zitten twee sensoren die het verschil tussen licht en donker kunnen meten. Als de mBot over een lijn rijdt kan hij op deze manier zien of er een bocht aankomt.

<details>
<summary>Functies voor de lijnvolger</summary>

```Arduino

//Lijnvolger sensor
MeLineFollower lineFinder(PORT_3); // De lijnvolger module kan alleen verbonden zijn met poort 3, 4, 5, of 6. 

uint8_t MeLineFollower::readSensors() // Dit geeft een getal terug, waarbij de waardes voor verschillende sensorwaarden staan:

switch(sensorState)
{
	case S1_IN_S2_IN: Serial.println("Sensor 1 en 2 zijn binnen een zwarte lijn"); break;
	case S1_IN_S2_OUT: Serial.println("Sensor 1 is binnen, en sensor 2 is buiten een zwarte lijn"); break;
	case S1_OUT_S2_IN: Serial.println("Sensor 2 is binnen, en sensor 1 is buiten een zwarte lijn"); break;
	case S1_OUT_S2_OUT: Serial.println("Sensor 1 en 2 zijn buiten een zwarte lijns"); break;
	default: break;
}

```

</details>

### Lichsterkte
bovenop (onder het plastic kapje) zit een sensor die meet hoe licht het in de ruimte is.

<details>
<summary>Functies voor de lichtsterkte</summary>

```Arduino

//Lichtsterkte sensor
MeLightSensor lightSensor(PORT_6);

int16_t MeLightSensor::read() //Dit geeft een integer waarde terug met de sterkte van de lichtsensor.

int16_t waarde = lightSensor.read();
Serial.print("waarde = ");
Serial.println(waarde);
delay(100);

```

</details>


## Outputs

Met de outputs kan de mBot acties in zijn omgeving uitvoeren. De mBot kan hier bijvoorbeeld snel mee vooruit rijden. Een ander voorbeeld is dat de mBot ook geluid kan maken, om te laten weten dat hij in de buurt is.

### Motoren
Ieder wiel wordt met een aaprte motor bestuurd.

<details>
<summary>Functies voor de motoren</summary>

```Arduino

//Motoren

MeDCMotor motor1(PORT_1);

void MeDCMotor::run(int16_t speed) //Draaien met een gegeven snelheid
void MeDCMotor::stop(void) //Stoppen.	

motor1.run(motorSpeed); /* Waarde: tussen -255 en 255. */
delay(2000);
motor1.stop();
delay(100);
motor1.run(-motorSpeed);
delay(2000);
motor1.stop();
delay(2000);

```

</details>

### LEDs
Bovenop (Onder het platstic kapje) zitten twee LEDs die je elke kleur kunt maken die je maar wilt.

<details>
<summary>Functies voor de LEDs</summary>

```Arduino

MeRGBLed led(PORT_1);

// Ledjes worden in het RGB spectrum uitgedrukt. 
// De combinatie van rood, groen en blauw dus. 
// Is dit nieuw voor je? Kijk eens hier : https://www.colorspire.com/rgb-color-wheel/

// lednummer - rood - groen - blauw
bool MeRGBLed::setColorAt(uint8_t index, uint8_t red, uint8_t green, uint8_t blue)

led.setColor(0, 255, 255, 255); //De LED's wit maken
led.show(); //Om de kleur te activeren, moet je de .show() functie aanroepen.

```

</details>

### Buzzer
Ook onder het kapje zit een buzzer waarmee je de mBot verschillende toonhoogtes kunt laten maken.

<details>
<summary>Functies voor de buzzer</summary>

```Arduino

void buzzerOn()
void buzzerOff()

buzzerOn();
delay(1000);
buzzerOff();
delay(1000);

```

</details>

## Opdrachten

### 1. Een vierkant rijden

Aangezien de mBot bij dit programma moet rijden is het verstandig je programma naar de mBot te <i>uploaden</i> en het programma te starten als deze op de grond staat. Je kunt het programma laten starten bij het aanzetten van de mBot; in dit voorbeeld start het programma als de knop bovenop wordt ingedrukt.

- De eerste stap is om de mBot een stuk vooruit te laten rijden. 50% van het vermogen is prima om mee te beginnen. Start het programma door op het zwarte knopje bovenop de mBot te drukken.
  <details>
	<summary>mBot arduino code</summary>

	```Arduino

	#include "MeMCore.h"

	MeDCMotor motor1(PORT_1);

	MeDCMotor motor2(PORT_2);

	uint8_t motorSpeed = 100;

	void setup()
	{
		pinMode(7, INPUT); //Define button pin as input
		while (analogRead(7) > 100) { // While (Button is not pressed)
			delay(50);
		}
	}

	void loop()
	{
		motor1.run(motorSpeed);
		motor2.run(motorSpeed);
	}

	```

	</details>
- Nu rijdt de mBot eindeloos door! Zorg dus dat deze na een paar seconden weer stopt met rijden.
	<details>
	<summary>mBot arduino code</summary>

	```Arduino

	#include "MeMCore.h"

	MeDCMotor motor1(PORT_1);

	MeDCMotor motor2(PORT_2);

	uint8_t motorSpeed = 100;

	void setup()
	{
		pinMode(7, INPUT); //Define button pin as input
		while (analogRead(7) > 100) { // While (Button is not pressed)
			delay(50);
		}

	}

	void loop()
	{
		motor1.run(motorSpeed);
		motor2.run(motorSpeed);
		delay(2000);
		motor1.stop();
		motor2.stop();
	}

	```

	</details>

- Laat nu de mBot een bocht maken. Probeer de tijd zo in te stellen dat ie rechtsaf (of linksaf) slaat.					<details>
	<summary>mBot arduino code</summary>

	```Arduino

	#include "MeMCore.h"

	MeDCMotor motor1(PORT_1);

	MeDCMotor motor2(PORT_2);

	uint8_t motorSpeed = 100;

	void setup()
	{
		pinMode(7, INPUT); //Define button pin as input
		while (analogRead(7) > 100) { // While (Button is not pressed)
			delay(50);
		}
	}

	void loop()
	{
		motor1.run(motorSpeed);
		motor2.run(motorSpeed);
		delay(2000);
		motor1.run(0);
		motor2.run(motorSpeed);
		delay(2000);
		motor1.run(motorSpeed);
		motor2.run(motorSpeed);
		delay(2000);
		motor1.stop();
		motor2.stop();

		while(true){
			delay(1000);
		}
	}

	```

	</details>

- Kan de robot nu een vierkantje rijden?


### 2. Sensorwaarden laten zien

Om opdrachten te kunnen programmeren is het vaak handig om te weten wat de sensoren van de mBot meten. Om dit te kunnen zien moet je de gemeten waarde bewaren in een <i>variabele</i>. Kijk bijvoorbeeld wat de lichtsensor meet als je je hand bovenop de mBot houdt, of de ultrasoonsensor als je je hand heen en weer beweegt voor de mBot.


- Wat is de functie ook al weer om de lichtsterkte uit te meten? Weet jij het nog?
   <details>
	<summary>mBot arduino code</summary>

	```Arduino

	#include "MeMCore.h"
	MeLightSensor lightSensor(PORT_6);

	void setup()
	{
		Serial.begin(9600);
	}

	void loop()
	{
		int16_t waarde = lightSensor.read();
		Serial.print("waarde = ");
		Serial.println(waarde);
		delay(100);
	}

	```

	</details>
- Deze waarde wordt nu door de mBot naar de computer gestuurt. Dit kunnen wij lezen in de seriele monitor. Wat zie jij nu in de seriele monitor?
- De Arduino IDE heeft ook een seriele plotter. Wat is hier het verschil?
- Zullen we een geluidje maken als de lichtsterkte te hoog (Of laag is?)? (Hint, buzzer!)

### 3. Help, een muur!
In deze opdracht is het de bedoeling om te voorkomen dat de mBot tegen de muur botst (nadat je 'm er wel naar toe laat rijden natuurlijk).

- Laat de mBot rijden met een druk op de knop
- Zorg ervoor dat de mBot stopt met rijden als hij minder dan 20 centimeter van een muur is.
  <details>
	<summary>mBot arduino code</summary>

	```Arduino

	#include "MeMCore.h"

	MeDCMotor motor1(PORT_1);

	MeUltrasonicSensor ultraSensor(PORT_7);

	MeDCMotor motor2(PORT_2);

	uint8_t motorSpeed = 100;

	void setup()
	{
		pinMode(7, INPUT); //Define button pin as input
		while (analogRead(7) > 100) { // While (Button is not pressed)
			delay(50);
		}

	}

	void loop()
	{
		double afstandTotObject = ultraSensor.distanceCm();
		
		if(afstandTotObject < 20) {
			motor1.stop();
			motor2.stop();
		} else {
			motor1.run(motorSpeed);
			motor2.run(motorSpeed);
		}
	}

	```

	</details>
- Gebeurt er nu wat je wil? Zo niet, denk dan eens na waarom niet? Heb je een stukje code vergeten?
- Sla maar eens flink alarm met licht en geluid om duidelijk te maken dat de mBot bijna gebotst was! Natuurlijk kan dit op veel manieren, doe iets geks!


## Uitdagingen
 
Natuurlijk kunnen we heel veel doen met de mBot. Daarom hebben we ook verschillende uitdagingen voor je. Kun jij er misschien zelf een bedenken? Zelf hebben wij het volgende lijstje bedacht. Ze beginnen makkelijk en worden
steeds moeilijker.

- Rijd een achtje (Of extra moeilijk: een spipraal)
- Lichten aan in de tunnel
- Aan de slag als politieauto of ambulance
- Volg de lijn
- Ontwijk de voorwerpen (Rijd er omheen?)
- Volg een voorwerp

