# Avance fabricación digital y prototipado electrónico

# Prototipo

<div align="center">
  <a href="https://cad.onshape.com/documents/66bdc8115f8216136ee39e0e/w/f0350b9cb4d82463dcd478aa/e/11e09c14942bc6f2c0ac0664">
    <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/prototipo%203D.png?raw=true" alt="Onshape">
  </a>
</div>

>link de onshape: https://cad.onshape.com/documents/66bdc8115f8216136ee39e0e/w/f0350b9cb4d82463dcd478aa/e/11e09c14942bc6f2c0ac0664

# Piezas a imprimir 3D

>Base caja

<p align="center">
  <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/Base%20caja.png" alt="Base caja" width="600">
</p>

>Tapa de la base 

<p align="center">
  <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/Tapa%20de%20la%20base%20caja.png" alt="Tapa de la base caja" width="600">
</p>

>gancho derecho e izquierdo

<p align="center">
  <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/gancho.png" alt="gancho" width="600">
</p>

# Modelado electrónico en maqueta

>Reducido
 
<p align="center">
  <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/Maqueta%20funciona_2.jpeg" alt="Maqueta funciona_2" width="600">
</p>

>Extendido

<p align="center">
  <img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/Maqueta%20funcional_1.jpeg" alt="Maqueta funcional_1" width="600">
</p>

# Código usado

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

