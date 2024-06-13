# Práctica 8

### Código
```cpp
#include <Arduino.h>

void setup() {
  Serial.begin(9600); // Inicializa el puerto Serial con una velocidad de 9600 baudios
  Serial1.begin(9600); // Inicializa el puerto Serial1 con una velocidad de 9600 baudios
}

void loop() {
  if (Serial.available() > 0) { // Verifica si hay datos disponibles en el puerto Serial 
    char test1;
    test1 = Serial.read();
  //  Serial.print("Dato test 1 = ");
    // Serial.println(test1);
      Serial1.write(test1); // Lee un byte del puerto Serial y lo envía al puerto Serial1
  }
  if (Serial1.available() > 0) { // Verifica si hay datos disponibles en el puerto Serial1
  char test2;
    test2 = Serial1.read();
    // Serial.print("Dato test 2 = ");
    // Serial.println(test2);
      Serial.write(test2); // Lee un byte del puerto Serial1 y lo envía al puerto Serial
    }
  delay(10);
  }
  ```

  ### Funcionamiento

- Esl código actúa como un puente de comunicación bidireccional a través de los puertos seriales `Serial` y `Serial1`.
  
- Cada vez que un byte de datos llega a `Serial`, se envía inmediatamente a `Serial1`. Del mismo modo, cualquier dato recibido en `Serial1` se transmite de vuelta a `Serial`.
  
- La función `delay(10)` al final del bucle `loop()` introduce un pequeño retraso para controlar la velocidad de ejecución del bucle y evitar un consumo excesivo de recursos de procesamiento.

En resumen, después de compilar y cargar el código en el microcontrolador, el programa establece una pasarela de comunicación serial bidireccional entre dos puertos seriales. Esto permite la transferencia continua de datos en ambas direcciones entre el microcontrolador y otros dispositivos conectados a través de los puertos seriales `Serial` y `Serial1`.