```cpp

const int pinPalancaIzq = 2;  // Pin izquierdo palanca
const int pinPalancaDer = 3;  // Pin derecho palanca
const int botonEmergencia = 7; // Pin para el botón de parada de emergencia
const int IN1 = 4;            // Control sentido L298N
const int IN2 = 5;            // Control sentido L298N
const int ENA = 6;            // PWM para velocidad (D6)

bool sistemaBloqueado = false; // Estado del sistema (normal/bloqueado)

void setup() {
  pinMode(pinPalancaIzq, INPUT_PULLUP); // Resistencia pull-up interna
  pinMode(pinPalancaDer, INPUT_PULLUP);
  pinMode(botonEmergencia, INPUT_PULLUP); // Botón con resistencia pull-up
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(ENA, OUTPUT);
  analogWrite(ENA, 255); // Velocidad máxima inicial
}

void loop() {
  // Leer estado del botón de emergencia (LOW cuando se presiona)
  bool emergencia = !digitalRead(botonEmergencia);
  
  // Si se presiona el botón de emergencia, bloquear el sistema
  if (emergencia) {
    sistemaBloqueado = true;
  }
  
  // Si el sistema está bloqueado
  if (sistemaBloqueado) {
    // Detener el motor completamente
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    analogWrite(ENA, 0);
    
    // Leer posición de la palanca
    bool izquierda = !digitalRead(pinPalancaIzq);
    bool derecha = !digitalRead(pinPalancaDer);
    
    // Solo reactivar el sistema si la palanca está en centro (ningún lado activado)
    if (!izquierda && !derecha) {
      sistemaBloqueado = false;
    }
  } 
  else {
    // Sistema operando normalmente
    bool izquierda = !digitalRead(pinPalancaIzq); // LOW = activado
    bool derecha = !digitalRead(pinPalancaDer);    // LOW = activado

    if (izquierda) { 
      // Motor gira en sentido A (ej. adelante)
      digitalWrite(IN1, HIGH);
      digitalWrite(IN2, LOW);
      analogWrite(ENA, 255);
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
      analogWrite(ENA, 0);
    }
  }
}

```
