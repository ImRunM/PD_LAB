# Ejercicio Practico 1 LECTURA/ESCRITURA DE MEMORIA SD

### Salida puerto serie 

1. **Intento de inicialización de la tarjeta SD**:
   - El primer mensaje será:
     ```
     Iniciando SD ...
     ```

2. **Resultado de la inicialización de la tarjeta SD**:
   - Si la inicialización falla, se imprimirá:
     ```
     No se pudo inicializar
     ```
     y el programa terminará en `setup()`.
   - Si la inicialización es exitosa, se imprimirá:
     ```
     inicializacion exitosa
     ```

3. **Apertura y lectura del archivo**:
   - Si el archivo "archivo.txt" se abre correctamente, se imprimirá:
     ```
     archivo.txt:
     ```
     seguido del contenido del archivo, carácter por carácter.
   - Si hay un problema al abrir el archivo, se imprimirá:
     ```
     Error al abrir el archivo
     ```

### Funcionamiento
