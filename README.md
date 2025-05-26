# Practica5: Buses de Comunicación I (Introducción y I2C)
## Objetivo: 
El objetivo de esta práctica es comprender el funcionamiento de los buses de comunicación entre periféricos, en concreto el protocolo I2C. Es un protocolo que
permite la conexión de múltiples dispositivos solo con dos líneas: SDA, transportador de datos y SCL, proporciona la señal de sincronización. Para ello haremos dos ejercicios introductorios.
## Apartado 1:
En este apartado implementamos un escáner I2C y asi detectar que dispositivos están conectados a este y las direcciones de ellos en el monitor série. 
Los materiales usados han sido:
1. ESP32-S3
2. Escáner I2C
3. Cables

### Código
```
#include <Arduino.h>
#include <Wire.h>
void setup()
{
  Wire.begin(10,9);
  Serial.begin(115200);
  while (!Serial);             // Leonardo: wait for serial monitor
  Serial.println("\nI2C Scanner");
}
void loop()
{
  byte error, address;
  int nDevices;
  Serial.println("Scanning...");
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  {
    // The i2c_scanner uses the return value of
    // the Write.endTransmisstion to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
  delay(5000);           // wait 5 seconds for next scan
}
```

### Funcionamiento del código:
Este código recorre todas las direcciones, del 1 al 126 intentando comunicarse con cada una de ella. Si se realiza correctamente la comunicación, este dispositivo está en la dirección correcta así 
que se imprime esta en formato hexadecimal. Sino, se comunica que no encontró ningun dispositivo. 

###Salidas:
```
Scanning...
I2C device found at address 0x34 !
```
