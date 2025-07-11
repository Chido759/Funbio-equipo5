```cpp

const int pinPalancaIzq = 2;  // Pin izquierdo palanca
const int pinPalancaDer = 3;  // Pin derecho palanca
const int IN1 = 4;            // Control sentido L298N
const int IN2 = 5;            // Control sentido L298N
const int ENA = 6;            // PWM para velocidad (D6)

void setup() {
  pinMode(pinPalancaIzq, INPUT_PULLUP); // Resistencia pull-up interna
  pinMode(pinPalancaDer, INPUT_PULLUP);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(ENA, OUTPUT);
  analogWrite(ENA, 255); // Velocidad máxima inicial (ajusta según necesites)
}

void loop() {
  bool izquierda = !digitalRead(pinPalancaIzq); // LOW = activado
  bool derecha = !digitalRead(pinPalancaDer);    // LOW = activado

  if (izquierda) { 
    // Motor gira en sentido A (ej. adelante)
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);
    analogWrite(ENA, 255); // Velocidad máxima (cambia a 128 para el 50%, etc.)
  } 
  else if (derecha) { 
    // Motor gira en sentido B (ej. atrás)
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, HIGH);
    analogWrite(ENA, 255); 
  } 
  else { 
    // Motor detenido (palanca en centro)
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    analogWrite(ENA, 0); // Velocidad = 0 (opcional, IN1/IN2 LOW ya lo detienen)
  }
}

```