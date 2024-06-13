# Informe de la interrupcion por GPIO.

### CÓDIGO UTILIZADO

```cpp
#include <Arduino.h>

struct Button {
 const uint8_t PIN;
 uint32_t numberKeyPresses;
 bool pressed;
};
Button button1 = {18, 0, false};
void IRAM_ATTR isr() {
 button1.numberKeyPresses += 1;
 button1.pressed = true;
}
void setup() {
 Serial.begin(115200);
 pinMode(button1.PIN, INPUT_PULLUP);
 attachInterrupt(button1.PIN, isr, FALLING);
}
void loop() {
 if (button1.pressed) {
 Serial.printf("Button 1 has been pressed %u times\n",
 button1.numberKeyPresses);
 button1.pressed = false;
 }

 //Detach Interrupt after 1 Minute
 static uint32_t lastMillis = 0;
 if (millis() - lastMillis > 60000) {
 lastMillis = millis();
 detachInterrupt(button1.PIN);
 Serial.println("Interrupt Detached!");
 }
}
```
### FUNCIONAMIENTO
Esta parte de la práctica hace uso de un pulsador/botón para realizar una interrupcion. 
La estructura ***button*** define el pulsador con tres campos diferentes, estos son:
* **PIN:** El número de pin del esp32-s3 al que se conecta el pulsador.
* **numberKeyPresses:** El número de veces que se ha pulsado el botón.
* **pressed:** Indica si el botón está actualmente pulsado.

La funión ***isr()*** es la rutina de interrupción, la qual se llama cuando el botón se pulsa. Al llamar a esta funión se incrementa el campo **numberKeyPresses** de la estructura ***button*** y se establece el campo **pressed** a **true**.

En la función ***setup()*** se inicializa la comunicación serial a 115200 bps. Se establece el pin al que está conectado el botón como entrada con resistencia de pull-up utilizando ***pinMode***. Se adjunta la rutina de interrupción ***isr*** al pin utilizando ***attachInterrupt***. Se especifica que la rutina de interrupción se debe llamar cuando el nivel del pin cambia de alto a bajo (**FALLING**).

Cada vez que se presiona el botón aparece por pantalla: **"Button 1 has been pressed %u times\n"**. Dónde %u corresponde con la cantidad de veces que hayamos presionado el botón.

Si al cabo de un minuto no se ha presionado el botón se llama a la función ***detachInterrupt()*** para que el esp32-s3 deje de monitorizar un pin y así ya no se pueden realizar interrrpciones con el botón. Al llamar a la función ***detachInterrupt()*** también se muestra por pantalla el texto: **"Interrupt Detached!"**.

# Informe de la interrupción por timer

### CÓDIGO UTILIZADO

```cpp
#include <Arduino.h>

volatile int interruptCounter;
int totalInterruptCounter;
hw_timer_t * timer = NULL;
portMUX_TYPE timerMux = portMUX_INITIALIZER_UNLOCKED;
void IRAM_ATTR onTimer() {
 portENTER_CRITICAL_ISR(&timerMux);
 interruptCounter++;
 portEXIT_CRITICAL_ISR(&timerMux);
}
void setup() {
 Serial.begin(115200);
 timer = timerBegin(0, 80, true);
 timerAttachInterrupt(timer, &onTimer, true);
 timerAlarmWrite(timer, 100000, true);
 timerAlarmEnable(timer);
}
void loop() {
 if (interruptCounter > 0) {
 portENTER_CRITICAL(&timerMux);
 interruptCounter--;
 portEXIT_CRITICAL(&timerMux);
 totalInterruptCounter++;
 Serial.print("An interrupt as occurred. Total number: ");
 Serial.println(totalInterruptCounter);
 }
}
```
### FUNCIONAMIENTO
En esta segunda parte, se configura un temporizador (timer) para generar interrupciones cada cierto tiempo.
En el código nos encontramos con diferenes variables y objetos, estos són:
* ***interruptCounter:*** Variable volátil que cuenta el número de interrupciones que se producen. Es volátil porque se accede a ella desde una rutina de interrupción (ISR, Interrupt Service Routine).
* ***totalInterruptCounter:*** Variable que almacena el número total de interrupciones que se han producido.
* ***timer:*** Objeto que representa el temporizador. Se utiliza para configurar y controlar el temporizador. 
* ***timerMux*** Objeto que se utiliza para sincronizar el acceso a la variable ***interruptCounter*** des de la rutina de interrupción y el bucle principal.

La función ***onTimer()*** es la rutina de interrupción que se llama cuando el temporizador genera una interrupción. Al llamar a la función se usa ***portENTER_CRITICAL_ISR(&timerMux)*** para entrar en una sección una crítica, lo que significa que se deshabilitan las interrupciones y se garantiza que no se produzcan otras interrupciones mientras se ejecuta esta sección de código. Se incrementa la variable ***interruptCounter***. Y finalmente se usa ***portEXIT_CRITICAL_ISR(&timerMux)*** para salir de una sección crítica y habilitar nuevamente las interrupciones.

En la funicón ***setup()*** se inicializa la comunicación serial a 115200 bps. Se crea un objeto **timer** y se configura para utilizar el temporizador número 0, con una frecuencia de división de 80 y en modo de conteo ascendente.Se adjunta la rutina de interrupción ***onTimer*** al temporizador utilizando ***timerAttachInterrupt***. Se configura el temporizador para generar una alarma cada 100000 ticks (aproximadamente 1 segundo) utilizando ***timerAlarmWrite***. Se habilita la alarma del temporizador utilizando ***timerAlarmEnable***.

Así pues cada 100000 ticks de temporizador ocurre una interrupción. El temporizador, al estar configurado para trabajar a una frecuencia de 80 Mhz, tiene un periodo de tick de 12.5 ns. Por lo tanto el tiempo entre dos interrupciones consecutivas es de 100000 x 12.5 ns = 1.25 ms. 
De esta forma cada 1.25 ms aparee por pantalla el texto: **An interrupt as occurred. Total number:**.