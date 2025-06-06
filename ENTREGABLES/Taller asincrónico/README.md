# Taller asincrónico
![portada](https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/portada.jpg?raw=true)
## Miembros del equipo:
### Alvaro Sebastian Segura Huanatico
### Micaela Sumaq Yupanqui Muñoz
### Andrea Celeste Tapia Luque
### Edwing Amir Josemaria Saavedra Pairazaman
### Diego Alessandro Benavides Rodriguez

# Objetivos

Poder usar ESP32 en conjunto MIT app Inventor para enviar los datos medidos a un app en tiempo real por medio de bluetooth o wifi.

# Imágenes de taller
<img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/ejercicio1.png?raw=true" alt="ejercicio1" width="600">
<img src="https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/ejercicio2.png?raw=true" alt="ejercicio2" width="600">

# Codigo comentado

```cpp
#include <WiFiManager.h>  //conexion al ESP32 por wifi
#include <WebServer.h>    //creacion del servidor web para el ESP32

const int FLEX_PIN = A0;             // definir analógico conectado
WebServer server(80);                //se procede a crear el servidor a usar

void handleADC(){
  int raw = analogRead(FLEX_PIN);          // leer el valor analógico
  int mv  = map(raw, 0, 4095, 0, 3300);    // conversiones
  server.send(200,"text/plain",String(mv)); 
}

void setup(){
  Serial.begin(115200);        //inicio de monitor serial
  //Serial.print("IP Address: "); se usó para encontrar/comprobar el IP
  //Serial.println(WiFi.localIP());
  WiFiManager().autoConnect("FlexAP","12345678"); //intento de conectar a wifi, si procede o no se puede mostrar con función print
  server.on("/readADC", handleADC);      //si se conecta, prosigue
  server.begin(); //inicio de servidor
}

void loop(){ server.handleClient();  //responder ante conexión
} 

```

# Video
[![Video](https://github.com/Chido759/Funbio-equipo5/blob/master/IM%C3%81GENES%20Y%20VIDEOS/WhatsApp%20Image%202025-05-25%20at%203.15.14%20PM.jpeg?raw=true)](https://github.com/Chido759/Funbio-equipo5/raw/refs/heads/master/IM%C3%81GENES%20Y%20VIDEOS/WhatsApp%20Video%202025-05-25%20at%203.15.15%20PM.mp4)