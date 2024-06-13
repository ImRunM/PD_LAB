# Práctica 7

## Ejercicio webradio
Para realizar el ejercicio se ha utilizado el codigo de ejemplo de la libreria:  earlephilhower/ESP8266Audio@^1.9.7. 
### Salidas puerto serie

1. **Conexión y Estado Inicial:**
   - Mensajes indicando la conexión a WiFi y la dirección IP asignada al dispositivo.
   - Notificaciones de la inicialización de variables y objetos relacionados con la reproducción de audio.

2. **Manejo de Solicitudes HTTP:**
   - Mensajes cuando se reciben solicitudes HTTP, como "Sending INDEX" para indicar que se está enviando la página principal.
   - Mensajes relacionados con el manejo de las solicitudes, por ejemplo, al cambiar la URL de la radio, ajustar el volumen, detener la reproducción, etc.

3. **Estado de Reproducción:**
   - Actualización del estado actual de la reproducción, como "Playing", "Stopped", etc.
   - Mensajes relacionados con el inicio y finalización de la reproducción de audio.
   - Mensajes de error si no se puede conectar a una URL de transmisión.

4. **Depuración General:**
   - Información periódica sobre el tiempo de ejecución total y la cantidad de memoria libre disponible en el dispositivo.
   - Mensajes para monitorear la ejecución del decodificador de audio (`PumpDecoder()`).

5. **Inicialización y Configuración:**
   - Mensajes de inicialización al inicio del programa, como la asignación de memoria para buffers y codecs de audio.
   - Mensajes al cargar configuraciones guardadas desde la EEPROM.

6. **Errores y Problemas de Conexión:**
   - Mensajes de error si no se puede conectar a una URL especificada, por ejemplo, "Unable to connect to URL".
   - Manejo de errores para solicitudes HTTP no válidas (404).

### Funcionamiento
### 1. Configuración y Variables Iniciales

El código comienza con la inclusión de las bibliotecas necesarias para el manejo de audio, WiFi y la definición de variables como el nombre y contraseña de la red WiFi (`ssid` y `password`). También se define una serie de variables para manejar la reproducción de audio, como `decoder` (decodificador de audio), `file` (fuente de archivo de audio), `buff` (buffer de audio), `out` (salida de audio), entre otros.

### 2. Setup (`setup()`)

- **Preasignación de Memoria:** Se asigna memoria estáticamente para el buffer y los codecs de audio para evitar problemas de fragmentación de memoria durante la ejecución (`preallocateBuffer` y `preallocateCodec`).

- **Inicialización Serial y WiFi:** Se inicia la comunicación serial para la depuración y se conecta el dispositivo a la red WiFi configurada (`WiFi.begin(ssid, password)`).

- **Espera de Conexión WiFi:** El programa espera hasta que se conecte exitosamente a la red WiFi (`while (WiFi.status() != WL_CONNECTED)`).

- **Inicio del Servidor Web:** Se inicia el servidor web en el puerto 80 (`server.begin()`).

- **Inicialización de Variables:** Se inicializan las variables de estado (`url`, `status`, `title`) y objetos relacionados con la reproducción de audio.

- **Carga de Configuración:** Se intenta cargar la última configuración guardada (URL de la radio, tipo de codec y volumen) desde la memoria EEPROM.

### 3. Loop (`loop()`)

- **Manejo de Solicitudes HTTP:** En cada iteración del `loop()`, el servidor web verifica si hay nuevas solicitudes de clientes. Si hay una solicitud válida, se procesa según la acción solicitada (`HandleIndex`, `HandleStop`, `HandleStatus`, `HandleTitle`, `HandleVolume`, `HandleChangeURL`).

- **Reproducción de Audio:** Se llama repetidamente a `PumpDecoder()` para manejar la reproducción de audio. Este método verifica si el decodificador de audio está funcionando correctamente y maneja su estado (reproducción, detención, etc.).

- **Actualización de Estado:** Se actualizan y envían actualizaciones periódicas del estado actual, como el título de la pista en reproducción, el estado del reproductor y el volumen, a través de solicitudes AJAX en la interfaz web.

### 4. Manejo de Funciones HTTP

- **HandleIndex:** Genera la página HTML principal que muestra el estado actual del sistema, el título de la pista en reproducción, el control de volumen y permite cambiar la URL de la radio.

- **HandleStop:** Detiene la reproducción de audio actual y redirige al cliente de vuelta a la página principal.

- **HandleStatus y HandleTitle:** Devuelven el estado actual y el título de la pista en reproducción respectivamente, utilizados para actualizaciones automáticas en la interfaz web.

- **HandleVolume:** Ajusta el volumen de reproducción según el valor enviado desde la interfaz web y actualiza la configuración.

- **HandleChangeURL:** Cambia la URL de la radio y el tipo de codec (MP3 o AAC) según los parámetros enviados desde la interfaz web.

### 5. Funciones Auxiliares

- **LoadSettings y SaveSettings:** Cargan y guardan la configuración (URL de la radio, tipo de codec y volumen) en la memoria EEPROM para que el dispositivo pueda recordar la configuración después de un reinicio.

- **StartNewURL y StopPlaying:** Inicia una nueva URL de transmisión de audio o detiene la reproducción actual.

### 6. Manejo de Errores y Depuración

- **Manejo de Errores:** Si no se puede conectar a una URL de transmisión, se muestra un mensaje de error en la interfaz web y se intenta nuevamente después de un corto intervalo.

- **Depuración Serial:** Se imprime información útil de depuración a través del puerto serial, como mensajes de estado, errores y seguimiento del uso de memoria.

## Ejercicio saampleac

Para realizar el ejercicio se ha utilizado el codigo de ejemplo de la libreria: earlephilhower/ESP8266Audio@^1.9.7

### Salidas puerto serie

- `info <info>`
- `id3data <info>`
- `eof_mp3 <info>`
- `station <info>`
- `streaminfo <info>`
- `streamtitle <info>`
- `bitrate <info>`
- `commercial <info>`
- `icyurl <info>`
- `lasthost <info>`
- `eof_speech <info>`

Cada una de estas salidas imprimirá un mensaje específico seguido de la información relevante que se pasa como argumento a la función correspondiente.

### Funcionamiento
1. **Configuración Inicial:**
   - Se inicializan los pines utilizados para la comunicación SPI y la interfaz I2S, así como el pin para el chip select de la tarjeta SD.
   - La comunicación serial se establece para la depuración y seguimiento mediante mensajes que se imprimirán en el puerto serie.

2. **Inicialización de la Tarjeta SD:**
   - Se configura y se inicializa la tarjeta SD para que el ESP32 pueda acceder y leer los archivos de audio almacenados en ella.

3. **Configuración del Audio:**
   - Se establecen los pines necesarios para la salida de audio utilizando el protocolo I2S, que es comúnmente utilizado para la reproducción de audio digital.
   - Se ajusta el volumen de reproducción según los valores permitidos por el sistema de audio.

4. **Carga y Reproducción del Archivo de Audio:**
   - Se especifica el archivo de audio que se desea reproducir desde la tarjeta SD.
   - El programa se encarga de manejar la reproducción continua del archivo de audio, asegurándose de gestionar eventos como el fin de archivo u otros eventos relacionados con la reproducción.

5. **Manejo de Eventos de Audio:**
   - Se han definido varias funciones para manejar diferentes eventos que pueden ocurrir durante la reproducción del audio.
   - Estas funciones imprimen mensajes específicos en el puerto serie para informar sobre eventos como información general del audio, metadatos ID3, estado del archivo de audio, etc.

6. **Bucle Principal:**
   - En el bucle principal (`loop()`), el programa continúa ejecutando el método que gestiona la reproducción de audio de manera continua.
   - Esto permite que el sistema funcione de manera autónoma mientras reproduce el archivo de audio especificado desde la tarjeta SD.

