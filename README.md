#include <Servo.h>

// Define servo motors
Servo foldServo1; // Servo for the first folding arm
Servo foldServo2; // Servo for the second folding arm
Servo releaseServo; // Servo for releasing folded clothes

// Pin assignments
const int sensorPin = 7; // Infrared or ultrasonic sensor pin to detect clothes
const int startButtonPin = 8; // Pin for the start button

// Servo positions
const int foldOpenPosition = 0; // Servo open position
const int foldClosedPosition = 90; // Servo closed position
const int releaseOpenPosition = 0; // Release servo open position
const int releaseClosedPosition = 90; // Release servo closed position

void setup() {
  // Attach the servo motors to their respective pins
  foldServo1.attach(9);
  foldServo2.attach(10);
  releaseServo.attach(11);

  // Initialize sensor and button pins
  pinMode(sensorPin, INPUT);
  pinMode(startButtonPin, INPUT_PULLUP);

  // Set initial servo positions
  foldServo1.write(foldOpenPosition);
  foldServo2.write(foldOpenPosition);
  releaseServo.write(releaseOpenPosition);

  // Start Serial Monitor
  Serial.begin(9600);
  Serial.println("Automated Clothes Folding Machine Initialized.");
}

void loop() {
  // Detect clothes and check if the button is pressed
  if (digitalRead(sensorPin) == HIGH && digitalRead(startButtonPin) == LOW) {
    Serial.println("Clothes detected. Starting the folding process...");
    foldClothes();
    delay(2000); // Wait before next operation
  }
}

void foldClothes() {
  // Step 1: Fold the first side
  Serial.println("Folding the first side...");
  foldServo1.write(foldClosedPosition);
  delay(2000); // Wait for the fold to complete
  foldServo1.write(foldOpenPosition);

  // Step 2: Fold the second side
  Serial.println("Folding the second side...");
  foldServo2.write(foldClosedPosition);
  delay(2000); // Wait for the fold to complete
  foldServo2.write(foldOpenPosition);

  // Step 3: Release the folded clothes
  Serial.println("Releasing folded clothes...");
  releaseServo.write(releaseClosedPosition);
  delay(2000); // Wait for the release to complete
  releaseServo.write(releaseOpenPosition);

  Serial.println("Folding process complete!");
}
