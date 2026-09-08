#define trigPin 9

#define echoPin 10

#define buzzer 6

#define vibration 5

#define soilSensor A0


long duration;

int distance;

int soilValue;


void setup()

{

pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
pinMode(buzzer, OUTPUT);
pinMode(vibration, OUTPUT);
Serial.begin(9600);
}
void loop()
{
// ===== Ultrasonic =====
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
duration = pulseIn(echoPin, HIGH);
distance = duration * 0.034 / 2;
// ===== Soil Moisture =====
soilValue = analogRead(soilSensor);
bool obstacle = (distance > 0 && distance <= 100);
bool water = (soilValue < 400);
// Adjust if needed
Serial.print("Distance: ");
Serial.println(distance);
Serial.print("Soil: ");
Serial.println(soilValue);
// ===== BOTH DETECTED =====
if(obstacle && water)
{
for(int i=0;i<6;i++)
{
digitalWrite(buzzer, HIGH);
digitalWrite(vibration, HIGH);
delay(80);
digitalWrite(buzzer, LOW);
digitalWrite(vibration, LOW);
delay(80);
}
}
// ===== ONLY OBSTACLE =====
else if(obstacle)
 {
digitalWrite(buzzer, HIGH);
digitalWrite(vibration, HIGH);
}
// ===== ONLY SOIL MOISTURE =====
else if(water)
{
// ERM type pulsing alert
digitalWrite(vibration, HIGH);hhhy
digitalWrite(buzzer, HIGH);
delay(150);
digitalWrite(vibration, LOW);
digitalWrite(buzzer, LOW);
delay(150);
}
// ===== NOTHING DETECTED =====
else
{
digitalWrite(buzzer, LOW);
digitalWrite(vibration, LOW);
}
delay(40);
}
