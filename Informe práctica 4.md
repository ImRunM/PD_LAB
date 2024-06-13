# Ejercicio práctico 1

### Código 1

```cpp
# include <Arduino.h>

void anotherTask (void * parameter);

void setup()
{
Serial.begin(115200);
/* we create a new task here */
xTaskCreate(
anotherTask, /* Task function. */
"another Task", /* name of task. */
10000, /* Stack size of task */
NULL, /* parameter of the task */
1, /* priority of the task */
NULL); /* Task handle to keep track of created task */
}
/* the forever loop() function is invoked by Arduino ESP32 loopTask */
void loop()
{
Serial.println("this is ESP32 Task");
delay(1000);
}
/* this function will be invoked when additionalTask was created */
void anotherTask( void * parameter )
{
/* loop forever */
for(;;)
{
Serial.println("this is another Task");
delay(1000);
}
/* delete a task when finish,
this will never happen because this is infinity loop */
vTaskDelete( NULL );
}

```
### Salida del puerto seria 
Este código crea dos tareas que se ejecutan concurrentemente en el ESP32:

1. **Tarea principal (`loop`)**:
   - Imprime "this is ESP32 Task" cada segundo.

2. **Tarea adicional (`anotherTask`)**:
   - Imprime "this is another Task" cada segundo.

Ambas tareas utilizan `delay(1000);` para esperar un segundo entre impresiones. La función `xTaskCreate` se utiliza para crear la tarea adicional, y la función `anotherTask` contiene un bucle infinito para mantener la tarea en ejecución indefinidamente. La función `vTaskDelete(NULL);` se incluye para eliminar la tarea si alguna vez sale del bucle infinito, lo cual en este caso no ocurrirá.
### Funcionamiento

#### Declaración de Funciones

```cpp
void anotherTask(void * parameter);
```

- `void anotherTask(void * parameter);`: Declara una función que define la tarea adicional que se ejecutará en paralelo con la tarea principal de Arduino.

#### Configuración Inicial (`setup`)

```cpp
void setup() {
  Serial.begin(115200);
  /* we create a new task here */
  xTaskCreate(
    anotherTask,    /* Task function. */
    "another Task", /* name of task. */
    10000,          /* Stack size of task */
    NULL,           /* parameter of the task */
    1,              /* priority of the task */
    NULL);          /* Task handle to keep track of created task */
}
```

- `Serial.begin(115200);`: Inicia la comunicación serie a 115200 baudios para la depuración.
- `xTaskCreate(...)`: Esta función crea una nueva tarea que se ejecutará concurrentemente con el `loop` de Arduino.
  - `anotherTask`: La función que define el código de la tarea.
  - `"another Task"`: El nombre de la tarea.
  - `10000`: El tamaño de la pila de la tarea.
  - `NULL`: No se pasa ningún parámetro a la tarea.
  - `1`: La prioridad de la tarea. Las prioridades más altas se ejecutan antes.
  - `NULL`: No se necesita un identificador de tarea aquí, por lo que se pasa `NULL`.

#### Bucle Principal (`loop`)

```cpp
void loop() {
  Serial.println("this is ESP32 Task");
  delay(1000);
}
```

- `Serial.println("this is ESP32 Task");`: Imprime un mensaje en el monitor serie indicando que esta es la tarea principal.
- `delay(1000);`: Espera 1 segundo antes de repetir el bucle.

#### Función de la Tarea Adicional (`anotherTask`)

```cpp
void anotherTask(void * parameter) {
  /* loop forever */
  for(;;) {
    Serial.println("this is another Task");
    delay(1000);
  }
  /* delete a task when finish, this will never happen because this is infinity loop */
  vTaskDelete(NULL);
}
```

- Esta función define el código que se ejecuta en la tarea adicional.
- `for(;;) { ... }`: Un bucle infinito que ejecuta el código dentro repetidamente.
  - `Serial.println("this is another Task");`: Imprime un mensaje en el monitor serie indicando que esta es la tarea adicional.
  - `delay(1000);`: Espera 1 segundo antes de repetir el bucle.
- `vTaskDelete(NULL);`: Esta línea está presente para completar la función, pero nunca se ejecutará debido al bucle infinito. Si se ejecutara, eliminaría la tarea actual.


# Ejercicio práctico 1

### Código 2
```cpp
#include <Arduino.h>
//#define LED_FLASH 23

void turnOnLedTask(void *parameter){
    for(;;){
    Serial.println("ledhigh");
    //digitalWrite(LED_FLASH,HIGH);
    vTaskDelay(2000 / portTICK_PERIOD_MS);
    }
}

void turnOffLedTask(void * parameter){
    for(;;){
    Serial.println("ledlow");
    //digitalWrite(LED_FLASH,LOW);
    vTaskDelay(2000 / portTICK_PERIOD_MS);
    }
}


void setup() {
    Serial.begin(112500);
    //pinMode(LED_FLASH, OUTPUT);
    xTaskCreate(turnOnLedTask, "turnOnLedTask", 1024, NULL, 1, NULL);
    delay(1000);
    xTaskCreate(turnOffLedTask, "turnOffLedTask", 1024, NULL, 1, NULL);
}

void loop() {
  
}
```
Por la falta de un led, se ha comentado el codigo referente al control del led y se ha sustituido por un `serial.println();` indicando en que estado ha de estar el led en cada momento.