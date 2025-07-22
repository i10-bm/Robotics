// === Motor control pins ===
#define ENA 3
#define IN1 4
#define IN2 5
#define IN3 6
#define IN4 7
#define ENB 9

// === Ultrasonic sensor pins ===
#define TRIG_FRONT 10
#define ECHO_FRONT 11
#define TRIG_LEFT 8
#define ECHO_LEFT 2
#define TRIG_RIGHT 12
#define ECHO_RIGHT 13

long frontDist, leftDist, rightDist;

void setup() {
  Serial.begin(9600);

  // Motor pins
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);

  // Sonar pins
  pinMode(TRIG_FRONT, OUTPUT);
  pinMode(ECHO_FRONT, INPUT);
  pinMode(TRIG_LEFT, OUTPUT);
  pinMode(ECHO_LEFT, INPUT);
  pinMode(TRIG_RIGHT, OUTPUT);
  pinMode(ECHO_RIGHT, INPUT);
}

void loop() {
  // Read distances
  frontDist = readSonar(TRIG_FRONT, ECHO_FRONT);
  leftDist = readSonar(TRIG_LEFT, ECHO_LEFT);
  rightDist = readSonar(TRIG_RIGHT, ECHO_RIGHT);

  // Print distances
  Serial.print("Front: ");
  Serial.print(frontDist);
  Serial.print(" cm\t");
  Serial.print("Left: ");
  Serial.print(leftDist);
  Serial.print(" cm\t");
  Serial.print("Right: ");
  Serial.println(rightDist);

  // Decision logic
  if (frontDist > 35) {
    forward();
  } else if (rightDist > 35) {
    rotateRight();
    delay(250);
  } else if (leftDist > 35) {
    rotateLeft();
    delay(250);
  } else {
    // Dead-end: turn around
    rotateRight();
    delay(750);
  }

  delay(200);
}

// === Movement functions ===

void forward() {
  setMotors(HIGH, LOW, HIGH, LOW, 200, 200);
}

void backward() {
  setMotors(LOW, HIGH, LOW, HIGH, 200, 200);
}

void rotateLeft() {
  // Left wheel backward, right wheel forward
  setMotors(LOW, HIGH, HIGH, LOW, 200, 200);
}

void rotateRight() {
  // Left wheel forward, right wheel backward
  setMotors(HIGH, LOW, LOW, HIGH, 200, 200);
}

void stopMotors() {
  setMotors(LOW, LOW, LOW, LOW, 0, 0);
}

// === Motor control helper ===
void setMotors(bool in1, bool in2, bool in3, bool in4, int speedA, int speedB) {
  digitalWrite(IN1, in1);
  digitalWrite(IN2, in2);
  digitalWrite(IN3, in3);
  digitalWrite(IN4, in4);
  analogWrite(ENA, speedA);
  analogWrite(ENB, speedB);
}

// === Ultrasonic sensor reading ===
long readSonar(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duration = pulseIn(echoPin, HIGH, 35000);  // 35 ms timeout
  long distance = duration * 0.034 / 2;
  return distance;
}
