# Práctica 3

### Código
```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>
// SSID & Password
const char* ssid = "xxxx"; // Enter your SSID here
const char* password = "xxxx"; //Enter your Password here
WebServer server(80); // Object of WebServer(HTTP port, 80 is defult)
void handle_root(void);
void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);
// Connect to your wi-fi modem
WiFi.begin(ssid, password);
// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
server.handleClient();
}
// HTML & CSS contents which display on web server
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\
<p> Hola Mundo!</p>\
<img src='https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Grosser_Panda.JPG/640px-Grosser_Panda.JPG'/>\
</body>\
</html>";
// Handle root url (/)
void handle_root() {
server.send(200, "text/html", HTML);
}
String HTML ="<!DOCTYPE html>\
<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera Pagina con ESP32 - Station Mode &#128522;</h1>\
<p> Hola Mundo!</p>\
<img src='https://i.ytimg.com/vi/evGXSPukd_Y/sddefault.jpg'/>\
</body>\
</html>";
```
### Salida serial 

Try Connecting to 
<tu SSID>
.........
WiFi connected successfully
Got IP: <tu IP asignada>
HTTP server started

### Visualización de la conexión a la página web
<img src="web.png" alt="web." width="300"/>


### Funcionamiento
### Conexión a la Red Wi-Fi

1. **Inicialización Serial**: El código comienza inicializando la comunicación serial para poder enviar mensajes al monitor serie.

2. **Configuración de la Red Wi-Fi**: Se define el nombre de la red Wi-Fi (SSID) y la contraseña a la que se va a conectar el ESP32.

3. **Conexión a Wi-Fi**: Se inicia la conexión del ESP32 a la red Wi-Fi utilizando el SSID y la contraseña proporcionados. El código entra en un bucle donde verifica constantemente el estado de la conexión. Durante este tiempo, imprime puntos en el monitor serie para indicar que está intentando conectarse.

4. **Confirmación de Conexión**: Una vez que el ESP32 se conecta a la red Wi-Fi, se muestra un mensaje en el monitor serie indicando que la conexión fue exitosa y se imprime la dirección IP asignada al ESP32 en la red.

### Configuración del Servidor Web

1. **Inicialización del Servidor Web**: Se crea un objeto de servidor web en el puerto HTTP por defecto (puerto 80).

2. **Manejador de Raíz**: Se configura el servidor para manejar las solicitudes a la URL raíz ("/"). Se define una función que se ejecutará cada vez que el servidor reciba una solicitud a esta URL.

3. **Inicio del Servidor**: El servidor web se inicia y se confirma con un mensaje en el monitor serie.

### Manejo de Solicitudes del Cliente

1. **Bucle Principal**: En el bucle principal del programa, el servidor web se mantiene ejecutando continuamente, manejando las solicitudes de los clientes que se conectan.

2. **Generación de Respuesta HTML**: Cuando un cliente accede a la URL raíz, el servidor responde enviando una página HTML. Esta página contiene un título, un párrafo con un mensaje y una imagen.

.


