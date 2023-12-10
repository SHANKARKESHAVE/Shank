# Shank
Interface the LM35 Temp Sensor
const int sensorPin = A0;  // LM35 sensor connected to A0 pin
const int ledPin = 13;     // Arduino onboard LED pin

unsigned long previousMillis = 0;   // Stores the last time LED was updated
const long tempertureLow = 250;      // Interval for temperature below 30°C
const long tempertureHigh = 500;     // Interval for temperature above 30°C

void setup() 
{
  pinMode(ledPin, OUTPUT);  // Initialize the LED pin as an output
  Serial.begin(9600);       // Initialize serial communication for debugging
}

void loop() 
{
  int temperature = readTemperature();  // Read temperature from LM35 sensor
  if (temperature < 30) 
  {
    blinkLED(tempertureLow);  // Blink LED with 250ms interval
  } 
  else 
  {
    blinkLED(tempertureHigh); // Blink LED with 500ms interval
  }
}

int readTemperature() 
{
  int sensorValue = analogRead(sensorPin);  // Read analog value from sensor
  float voltage = sensorValue * (5.0 / 1023.0);  // Convert analog value to voltage
  float temperatureC = (voltage - 0.5) * 100.0; // Convert voltage to temperature in Celsius
  Serial.print("Temperature: ");
  Serial.print(temperatureC);
  Serial.println(" °C");
  return temperatureC;
}

void blinkLED(long workled) 
{
  unsigned long currentMillis = millis();  // Get the current time
  if (currentMillis - previousMillis >= workled) 
     {
       previousMillis = currentMillis;  // Save the last time LED blinked
       digitalWrite(ledPin, !digitalRead(ledPin)); // Toggle LED state
     }
}
