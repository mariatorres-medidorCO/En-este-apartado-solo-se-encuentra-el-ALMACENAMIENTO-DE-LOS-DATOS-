# En-este-apartado-solo-se-encuentra-el-ALMACENAMIENTO-DE-LOS-DATOS-
En este apartado solo esta el como almacenamos los datos, en este bosquejo falta agregar la declaracion de los pines para que lea las variables lo cual se encuentra en el enlace anterior 

#include <SD.h>
#include <SPI.h>

File myFile; 

  int segundos=0;
  int minutos=0;
  int horas=0;
  int dia=0;
void setup()
{
  Serial.begin(9600);

  Serial.print("Iniciando SD ...");
  if (!SD.begin(10)) {
    Serial.println("No se pudo inicializar");
    return;
  }
  Serial.println("inicializacion exitosa");

  if(!SD.exists("datalog.csv"))
  {
      myFile = SD.open("datalog.csv", FILE_WRITE);
      if (myFile) {
        segundos=segundos+2;

          if (segundos>=60){
              segundos=segundos-60;
              minutos=minutos+1;
              if(minutos>=60){
                    minutos=minutos-60;
                    horas=horas+1;
                    if(horas>=24){
                          dia=dia+1;
                          horas=horas-24;
                       }
                } 
          }    
          myFile.print("Tiempo = dia ");
          myFile.print(dia);
          myFile.print(", ");
          if(horas<10)
          {
            myFile.print("0");
            }
          myFile.print(horas);
          myFile.print(":");
          if(minutos<10)
          {
            myFile.print("0");
            }
          myFile.print(minutos);
          myFile.print(":");
          if(segundos<10)
          {
            myFile.print("0");
            }
          myFile.print(segundos);
          myFile.print(", CO = ");//Temperatura, primer sensor
          myFile.print(sensorMq);

          myFile.close(); //cerramos el archivo              
        Serial.println("Archivo nuevo, Escribiendo encabezado(fila 1)");
        myFile.println("Tiempo(ms),Sensor1");
        myFile.close();
      } else {

        Serial.println("Error creando el archivo datalog.csv");
      }
  }

}

void loop()
{

  myFile = SD.open("datalog.csv", FILE_WRITE);//abrimos el archivo

  if (myFile) { 
        Serial.print("Escribiendo SD: ");
        myFile.print(millis());
        myFile.print(",");
        myFile.close(); //cerramos el archivo

        Serial.print("Tiempo(ms)=");
        Serial.print(millis());


  } else {
   // if the file didn't open, print an error:
    Serial.println("Error al abrir el archivo");
  }
  delay(100);
}
