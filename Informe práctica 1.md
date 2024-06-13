# PRACTICA 1

### CÓDIGO 1

```cpp
#include <Arduino.h>

#define LED_FLASH 23

void setup() {
  pinMode(LED_FLASH, OUTPUT);
  Serial.begin(115200);
  delay(500);
  // Serial.println("Hola mundo!");
}

void loop() {
  // put your main code here, to run repeatedly:
  Serial.println("ledhigh");
  digitalWrite(LED_FLASH,HIGH);
  delay(500);
  Serial.println("ledlow");
  digitalWrite(LED_FLASH,LOW); 
  delay(500);
}
```

### DIAGRAMA DE FLUJO DEL CÓDIGO 1

```mermaid
graph TD
    A([Inicio]) --> B[Configurar pin LED_FLASH como SALIDA]
    B --> C[Iniciar comunicación Serial a 115200 bps]
    C --> D[Esperar 500 ms]
    D --> E[Entrar en bucle LOOP]
    
    E --> F[/Imprimir ledhigh en Serial/]
    F --> G[Establecer LED_FLASH en ALTO]
    G --> H[Esperar 500 ms]
    H --> I[/Imprimir ledlow en Serial/]
    I --> J[Establecer LED_FLASH en BAJO]
    J --> K[Esperar 500 ms]

    K --> E

```
### DIAGRAMA DE TIEMPOS DEL CÓDIGO 1

```mermaid
sequenceDiagram
    participant esp32
    participant LED_FLASH

    loop Bucle de 1 segundo
        esp32->>LED_FLASH: Set high
        note over LED_FLASH: LED encendido
        esp32->>LED_FLASH: Set low
        note over LED_FLASH: LED apagado
    end

```

### CÓDIGO 2

```cpp
#include <Arduino.h>

#define ANALOG_PIN 34 // Definir el pin analógico que se va a leer
#define LED_FLASH 23

void setup() {
  Serial.begin(115200); // Iniciar la comunicación serie a 115200 baudios
  pinMode(ANALOG_PIN, INPUT); // Configurar el pin analógico como entrada
  pinMode(LED_FLASH, OUTPUT);
}

void loop() {
  int sensorValue = analogRead(ANALOG_PIN); // Leer el valor del pin analógico
  Serial.println(sensorValue); // Enviar el valor leído al puerto serie
  digitalWrite(LED_FLASH,HIGH);
  delay(1000); // Esperar un segundo antes de la próxima lectura
  Serial.println(sensorValue); // Enviar el valor leído al puerto serie
  digitalWrite(LED_FLASH,LOW);
  delay(1000);
}
```
### DIAGRAMA DE FLUJO DEL CÓDIGO 1

```mermaid
graph TD
   A([Inicio]) --> B[Iniciar comunicación Serial a 115200 bps]
   B --> C[Configurar ANALOG_PIN como entrada]
   C --> D[Configurar LED_FLASH como salida]
   D --> E[Entrar en bucle LOOP]

   E --> F[Leer valor del pin ANALOG_PIN]
   F --> G[Enviar valor leído en Serial]
   G --> H[Establecer LED_FLASH en HIGH encendido]
   H --> I[Esperar 500ms]
   I --> J[Enviar valor leído en Serial]
   J --> K[Establecer LED_FLASH en DOWN apagado]
   K --> L[Esperar 500ms]
   L --> E
```
### DIAGRAMA DE TIEMPOS DEL CÓDIGO 2

```mermaid
sequenceDiagram
    participant esp32
    participant LED_FLASH
    participant Serial

    loop Bucle de 1 segundo
        note over esp32: Leer valor del pin analogico
        esp32 ->> Serial: enviar valor a Serial
        esp32->>LED_FLASH: Set high
        note over LED_FLASH: LED encendido
        note over esp32: Esperar 500ms
        esp32 ->> Serial: enviar valor a Serial
        esp32->>LED_FLASH: Set low
        note over LED_FLASH: LED apagado
        note over esp32: Esperar 500ms
    end 

```

En el programa que se ha realizado el tiempo libre que tiene el procesador es de Xms.