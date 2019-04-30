[click here](https://particle.hackster.io/gusgonnet/temperature-humidity-monitor-with-blynk-7faa51)

## RESET PHOTONS
[reset photon](https://docs.particle.io/tutorials/device-os/led/core/#safe-mode)

# Project2

[Click Here](https://particle.hackster.io/yatinagarwal/light-detecting-email-sender-db18f4)

## Monitor Temp & Humidity

Required Parts:
<br>- Particle Photon
<br>- DHT11 Sensorf (link)[https://www.amazon.com/gp/product/B01H3J3H82/ref=ppx_yo_dt_b_asin_title_o06_s01?ie=UTF8&psc=1]
<br>- OPTIONAL: PowerShield (Battery) [link](https://www.amazon.com/gp/product/B06XJ64G8G/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
<!---
[link](https://docs.particle.io/tutorials/hardware-projects/maker-kit/#tutorial-3-conference-room-monitor)
--->

### Step 1: Set up your Photon
- Using Your Computer: Go to particle.io/setup and follow the instructions to create an account and set up your Photon.
- Using Your Phone & Download the Particle Mobile App [iPhone](https://itunes.apple.com/us/app/particle-build-photon-electron/id991459054?ls=1&mt=8) & [Android](https://play.google.com/store/apps/details?id=io.particle.android.app) to create an account and set up your Photon.

<img src="1.jpg" width="500">

<hr>

### Step 2: Connect Photo Sensor & Resistor to Photon
- Connect your sensor onto the Photon. Follow these examples for hooking up common sensors.
```
Sensor Pin  | Photon Pin
       pin1 | VIN (on the left) of the sensor to +5V
       pin2 | D2 middle pin
       pin3 | D0 (on the right) of the sensor to GROUND
       Connect a 10K resistor from pin 2 (data) to pin 1 (power) of the sensor
       
```
#### Set up the hardware

Should look like this

<img src="1E4AAE01-EDB9-4FE4-AF50-1A2EAB607F43.PNG" width="500">
<br>
<hr>

### Step 3: Setup BLYNK APP

- Download iOS or Android App and setup account on BLYNK (link)[https://blynk.io/en/getting-started]
- Log In to Blynk app and press QR button in Projects gallery & SCAN BELOW QR CDOE TO GET PROJECT
<img src="IMG_1317.jpg" width="500">
- Connect Blynk App to Photon: Tap on Octagon Icon and add your photon under devices
<img src="IMG_1316.jpg" width="500">
<br>

### Step 4: Create Particle App

- Go to https://build.particle.io/build/new 
- Title: LightDetect
- Paste Below Code
```

// This #include statement was automatically added by the Particle IDE.
#include "Adafruit_DHT_Particle.h"
#include "blynk.h"
// DHT humidity/temperature sensors
// Written by Chuck Konkol

#define DHTPIN D2     // what pin we're connected to

// Uncomment whatever type you're using!
//#define DHTTYPE DHT11		// DHT 11 
#define DHTTYPE DHT11		// DHT 22 (AM2302)
//#define DHTTYPE DHT21		// DHT 21 (AM2301)

// Connect pin 1 (on the left) of the sensor to +5V
// Connect pin 2 of the sensor to whatever your DHTPIN is
// Connect pin 4 (on the right) of the sensor to GROUND
// Connect a 10K resistor from pin 2 (data) to pin 1 (power) of the sensor

DHT dht(DHTPIN, DHTTYPE);
int loopCount;

//DANGER - DO NOT SHARE!!!!
char auth[] = "blynktoken"; // Put your blynk token here


void setup() {
     Blynk.begin(auth);
	Particle.publish("state", "DHTxx test start");

	dht.begin();
	loopCount = 0;
	delay(2000);
}

void loop() {
    Blynk.run();
// Wait a few seconds between measurements.
//	delay(2000);

// Reading temperature or humidity takes about 250 milliseconds!
// Sensor readings may also be up to 2 seconds 'old' (its a 
// very slow sensor)
	float h = dht.getHumidity();
// Read temperature as Celsius
	float t = dht.getTempCelcius();
// Read temperature as Farenheit
	float f = dht.getTempFarenheit();
	
	int t1 = dht.getTempFarenheit();
	
	int h1 =  dht.getHumidity();
	
	char *fh;
	
	char *hd;
	 Particle.publish("temps",  String(t1));
  
// Check if any reads failed and exit early (to try again).
	if (isnan(h) || isnan(t) || isnan(f)) {
		Serial.println("Failed to read from DHT sensor!");
		return;
	}
	
	//Incorrect reading due to noice spikes false reading
   if (t1 > 100 || t1 < 30){
       Particle.publish("error",  String(t1));
       	return;
   }

// Compute heat index
// Must send in temp in Fahrenheit!
	float hi = dht.getHeatIndex();
	float dp = dht.getDewPoint();
	float k = dht.getTempKelvin();
	
	//String timeStamp = Time.timeStr();
	Particle.publish("readings", String::format("{\"Hum(\%)\": %4.2f, \"Temp(°C)\": %4.2f, \"DP(°C)\": %4.2f, \"HI(°C)\": %4.2f}", h, f, dp, hi));
	//virtual pin 1 will be the temperature
   String sf(f, 0);
   String sh(h, 0);
   
    Blynk.virtualWrite(V1, sf);
     //virtual pin 2 will be the humidity
    Blynk.virtualWrite(V2, sh);

	delay(10000);
}

```
- Click Save
- Click Flash

That’s It! You should now see updates in Blynk App





