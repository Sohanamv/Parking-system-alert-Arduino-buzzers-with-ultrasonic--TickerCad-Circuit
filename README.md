# Parking-system-alert-Arduino-buzzers-with-ultrasonic--TickerCad-Circuit
// Pin definitions
const int trigPin = 9;      // Pin connected to Trig of HC-SR04
const int echoPin = 10;     // Pin connected to Echo of HC-SR04
const int buzzerPin = 8;    // Pin connected to the buzzer
const int ledPin = 7;       // Pin connected to the LED

// Variables for calculating distance
long duration;
int distance;

void setup() {
  // Initialize serial communication at 9600 bits per second
  Serial.begin(9600);

  // Set pin modes
  pinMode(trigPin, OUTPUT);  // Trig is an output pin
  pinMode(echoPin, INPUT);   // Echo is an input pin
  pinMode(buzzerPin, OUTPUT);  // Buzzer is an output pin
  pinMode(ledPin, OUTPUT);   // LED is an output pin

  // Initial states
  digitalWrite(ledPin, LOW);     // LED off initially
}

void loop() {
  // Clear the trigPin by setting it LOW
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // Trigger the sensor by sending a 10us HIGH pulse
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Read the echoPin, and measure the time it takes for the pulse to return (in microseconds)
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance in cm
  distance = duration * 0.034 / 2;
  
  // Print the distance to the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // Check if the object is within 10 cm
  if (distance < 10 && distance > 0) {
    // Object is close, activate buzzer and LED
    tone(buzzerPin, 1000);  // Generate a 1kHz sound (adjust frequency as needed)
    digitalWrite(ledPin, HIGH);  // Turn on the LED
  } else {
    // Object is far, deactivate buzzer and LED
    noTone(buzzerPin);      // Stop the tone
    digitalWrite(ledPin, LOW);  // Turn off the LED
  }

  // Wait a little before taking the next reading
  delay(200); // 200ms delay
}
