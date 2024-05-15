
# PRACTICA 3: WIFI y BLUETOOTH 

## Practica A: Generación de una pagina web

CODIGO:
```CPP
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "Iphone de Pablo"; 
const char* password = "pabloperez"; 

WebServer server(80); 

void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);

WiFi.begin(ssid, password);


while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi Connected");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); 
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
server.handleClient();
}

String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\
</body>\
</html>";

// Handle root url (/)
void handle_root() {
server.send(200, "text/html", HTML);
}
```

FUNCIONAMIENTO

El objetivo de este código es establecer un servidor web en un ESP32, que se conectará a través de mi móvil. Luego, mostrará una página HTML cuando se acceda a ella mediante un navegador web.

Primero, la ESP32 se conecta a la red WiFi proporcionada en el código:

```cpp

const char* ssid = "Iphone de Pablo"; 
const char* password = "pabloperez";
```
Creamos un objeto WebServer y definimos una ruta. Esta será manejada con la función handle_root().

La función handle_root() se llama cuando se accede al servidor web. Esta envía una respuesta, que en este caso será la página web.

SALIDA POR EL TERMINAL

En el terminal podremos ver información sobre el WiFi y la dirección IP asignada al ESP32. Si se conecta correctamente a la red WiFi, mostrará un mensaje indicando la conexión exitosa y la dirección IP del ESP32.

  ```cpp
    while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
   }
    Serial.println("");
    Serial.println("WiFi Connected");
    Serial.print("Got IP: ");
    Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
    server.on("/", handle_root);
      server.begin();
    Serial.println("HTTP server started");
    delay(100);

  ```
  ## Practica B Comunicación bluetooth con el movil

  CODIGO:
```cpp
#include "BluetoothSerial.h"
BluetoothSerial SerialBT;


void setup() {
 Serial.begin(115200);
 SerialBT.begin("ESP32 Pablo"); 
 Serial.println("The device started, now you can pair it with bluetooth!");
}


void loop() {
 if (Serial.available()) {
   SerialBT.write(Serial.read());
 }
 if (SerialBT.available()) {
   Serial.write(SerialBT.read());
 }
 delay(20);
}
```
**FUNCIONAMIENTO**


En esta segunda parte de la práctica, vamos a utilizar la librería BluetoothSerial.h, que nos permitirá crear variables de tipo BluetoothSerial. Estas variables incluyen funciones que facilitan la conexión Bluetooth y permiten enviar y recibir mensajes entre el microcontrolador (MP) y un dispositivo conectado, creando un chat básico.

Primero, incluimos la librería y declaramos la variable SerialBT de tipo BluetoothSerial. Luego, en el void setup(), inicializamos el puerto serie. Utilizando la función .begin de la librería, nuestro MP creará una red Bluetooth que llamaremos "ESP32 Pablo".

En el void loop, tenemos dos if que se ejecutan independientemente. Ambos cumplen la misma función, pero uno se activa si el puerto serie del MP está operativo, y el otro, si lo está el puerto serie del dispositivo conectado. Cuando estas condiciones se cumplen, el puerto serie del MP escribe lo que lee del puerto serie del dispositivo conectado y viceversa. De este modo, si envío un mensaje desde mi dispositivo, se mostrará en el puerto serie del MP, y si envío un mensaje desde el MP, se mostrará en el puerto serie del dispositivo, creando un chat rudimentario similar a una versión inicial de WhatsApp.

En la práctica, logramos establecer una conexión Bluetooth entre el MP y nuestros móviles. Usando la aplicación Serial Bluetooth Terminal, pudimos enviar mensajes tanto desde nuestros móviles al MP como del MP a nuestros móviles.
