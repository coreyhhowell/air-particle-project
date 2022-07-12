//PM25AQ Sensor Library
#include <Adafruit_PM25AQI.h>

// M2.5 is UART only (UNO and others without hardware serial) so
// we must use software serial.
#include <SoftwareSerial.h>

//I2C Communication
#include <Adafruit_I2CDevice.h>
#include <Adafruit_I2CRegister.h>
#include <Adafruit_SPIDevice.h>
#include <Adafruit_BusIO_Register.h>

const int LED_highLevel = 4;
const int LED_midLevel = 5;
const int LED_lowLevel = 6;

// PIN 2 is IN from sensor (TX pin on sensor), leave pin #3 disconnected
// comment lines 18-20 if using hardware serial
SoftwareSerial pmSerial(2, 3);
Adafruit_PM25AQI aqi = Adafruit_PM25AQI();

//For Particulate Matter knowledge, three standard concentrations: PM2.5,10,100um
//Sensor can read lower and higher sizes, but using these three standards
//to keep it simple

void setup() {
  // Open Serial Monitor, and wait
  Serial.begin(115200);
  while (!Serial) delay(10);

  Serial.println("Adafruit PMSA003I Air Quality Sensor");

  // Wait one second to allow time for sensor to come online
  delay(1000);

  // If using serial, initialize it and set baudrate before starting!
  // Uncomment one of the following (hard or soft serial)
  //Serial1.begin(9600);
  pmSerial.begin(9600);

  // There are three ways to connect sensor to Arduino

  // 1. Connect to the sensor over I2C
  // if (! aqi.begin_I2C()) {

  // 2. Connect to the sensor over hardware serial
  // if (! aqi.begin_UART(&Serial1)) {

  // 3. Connect to the sensor over software serial
  if (! aqi.begin_UART(&pmSerial)) {
      Serial.println("Could not find PM 2.5 sensor!");
      while (1) delay(10);
  }

  Serial.println("PM25 found!");

  pinMode(LED_highLevel, OUTPUT);
  pinMode(LED_midLevel, OUTPUT);
  pinMode(LED_lowLevel, OUTPUT);
}

void loop() {
  PM25_AQI_Data data;

  if (! aqi.read(&data)) {
    Serial.println("Could not read from AQI");
    //delay half a second and try to read from the sensor again
    delay(500);
    return;
  }
  Serial.println("AQI reading success");

  Serial.println();
  Serial.println(F("---------------------------------------"));
  Serial.println(F("Concentration Units (standard)"));
  Serial.println(F("---------------------------------------"));
  Serial.print(F("PM 1.0: ")); Serial.print(data.pm10_standard);
  Serial.print(F("\t\tPM 2.5: ")); Serial.print(data.pm25_standard);
  Serial.print(F("\t\tPM 10: ")); Serial.println(data.pm100_standard);
  Serial.println(F("Concentration Units (environmental)"));
  Serial.println(F("---------------------------------------"));
  Serial.print(F("PM 1.0: ")); Serial.print(data.pm10_env);
  Serial.print(F("\t\tPM 2.5: ")); Serial.print(data.pm25_env);
  Serial.print(F("\t\tPM 10: ")); Serial.println(data.pm100_env);
  Serial.println(F("---------------------------------------"));
  Serial.print(F("Particles > 0.3um / 0.1L air:")); Serial.println(data.particles_03um);
  Serial.print(F("Particles > 0.5um / 0.1L air:")); Serial.println(data.particles_05um);
  Serial.print(F("Particles > 1.0um / 0.1L air:")); Serial.println(data.particles_10um);
  Serial.print(F("Particles > 2.5um / 0.1L air:")); Serial.println(data.particles_25um);
  Serial.print(F("Particles > 5.0um / 0.1L air:")); Serial.println(data.particles_50um);
  Serial.print(F("Particles > 10 um / 0.1L air:")); Serial.println(data.particles_100um);
  Serial.println(F("---------------------------------------"));


if (data.pm10_standard > 450) {
  // Blink HIGH if 10 micron particulate matter above 450 um/0.1L concentration
  digitalWrite(LED_lowLevel, HIGH);
  delay(250);
  digitalWrite(LED_lowLevel, LOW);
  delay(250);

  } else {
  // Below 450 concentration, do nothing
    digitalWrite(LED_lowLevel, LOW);
  }

if (data.pm25_standard > 700) {
  // Blink HIGH if 2.5 micron particulate matter above 700 um/0.1L concentration
  digitalWrite(LED_midLevel, HIGH);
  delay(250);
  digitalWrite(LED_midLevel, LOW);
  delay(250);

  } else {
  // Below 700 concentration, do nothing
  digitalWrite(LED_midLevel, LOW);
  }


if (data.pm100_standard > 1500) {
    // Blink HIGH if 100 micron particulate matter above 1500 um/0.1L concentration
  digitalWrite(LED_highLevel, HIGH);
  delay(250);
  digitalWrite(LED_highLevel, LOW);
  delay(250);

  } else {
  // Below 1500 concentration, do nothing
  digitalWrite(LED_highLevel, LOW);
  }
}
