# Parcial domiciliario PARTE 3.


## Integrante: 
- Tomás León Curto Eivers


## Proyecto: VENTILADOR CON FOTORESISTOR Y SENSOR DE TEMPERATURA.
![Tinkercad](./img/ArduinoTinkercad3.jpg)


## Descripción
contador de 0 a 99 utilizando dos displays de 7 segmentos (ánodo común) y dos botones para controlar la cuenta y un switch para mostrar solo numeros primos o todos los enteros.
Se utiliza la técnica de multiplexación para mostrar los dígitos en los displays. 
El contador debe comienza en 0 y es capaz de aumentar o disminuir su valor en una unidad o al siguiente numero primo.
Todas las resistencias son de 220 Ohm exepto la del transitor que es de 1kOhm y la del fototransitor que es de 10kOhms, los pulsadores son de tipo pullup interno y los displays son ánodo común.
Ademas se agrego un motor CC (teoricamente funcionando como ventilador) el cual es regulado por el sensor de temperatura, cuando la temperatura es mayor al número ingresado en modo normal se activa el motor, 
y a mayor temperatura mayor velocidad de giro.
Por ultimo se agrego un fotoresistor el cual esta mapeado en una escala 1-100 y si su lectura es menor a la ingresada con como botones en modo numero primo el motor se apaga, con la intencion de apagar el motor 
automaticamente de noche.


## Funcion principal
Esta función se encargan de todos los controles del circuito.

SWITCH, MOTOR, SENSOR y ILUMINACION son #define que utilizamos para conectar al switch, al motor, al sensor de temperatura y al fotoresistor respectivamente, asociandolos a pines de la placa arduino.

~~~ C++ (lenguaje en el que esta escrito)
//LOOP
void loop()
{  
  //VERIFICO SI EL MODO ES NORMAL O PRIMOS:
  estado_switch = digitalRead(SWITCH);// detecta el estado del switch.
  
  //PRESION DE PULSADOR ANTI-REBOTE PULLUP INTERNO "SUMA":
  pulsado_suma = digitalRead(SUMA);// detecta si se presiona el pulsador.
  if (pulsado_suma == LOW)// pregunta si se esta presionando el pulsador.
  {
  	estado_suma = 1;// cambia de valor la variable estado.
  }
  if (estado_suma == 1 && pulsado_suma == HIGH)// pregunta si el estado es 1 pero se solto el boton.
  {
    if (estado_switch == LOW)//Detecta si el switch esta en modo normal.
    {  
  		suma_normal();
    }
    else //Detecta si el switch esta en modo primos.
    {
    	suma_primos();
    }
    estado_suma = 0;// devulve al estado original.
  }
  
  //PRESION DE PULSADOR ANTI-REBOTE PULLUP INTERNO "RESTA":
  pulsado_resta = digitalRead(RESTA);// detecta si se presiona el pulsador.
  if (pulsado_resta == LOW)// pregunta si se esta presionando el pulsador.
  {
  	estado_resta = 1;// cambia de valor la variable estado.
  }
  if (estado_resta == 1 && pulsado_resta == HIGH)// pregunta si el estado es 1 pero se solto el boton.
  {
  	if (estado_switch == LOW)//Detecta si el switch esta en modo normal.
    {  
  		resta_normal();
    }
    else //Detecta si el switch esta en modo primos.
    {
    	resta_primos();
    }
    estado_resta = 0;// devulve al estado original.
  }
  
  if (estado_switch == LOW)//Detecta si el switch esta en modo normal.
  {
  	decena(numero); // muestra el digito de la decena.
  	unidad(numero); // muestra el digito de la unidad.
  }
  else //Detecta si el switch esta en modo primos.
  {
  	decena(numero_primo); // muestra el digito de la decena.
  	unidad(numero_primo); // muestra el digito de la unidad.
  }
    // se utiliza el metodo de Multiplexación por el cual
    // se encieden y apagan de forma alternada los displays
    // por medio de los pines A4 y A5, a una velocidad tal que
    // el ojo humano percibe los dos prendidos al mismo tiempo.
  apagar(); // apaga los displays
  delay(15);// retraso de 15ms. agiliza el cambio en los leds 
  
  // FUNCIONAMIENTO SENSOR Y MOTOR QUE AUMENTA VELOCIDAD CON 
  // TEMPERATURA:
  lectura = analogRead(SENSOR);
  temperatura = map(lectura,20,358,-40,125);
  velocidad = map(temperatura,0,125,0,255);
  
  // DEFINO INTENSIDAD DE ILUMINACION A PARTIR DE LA CUAL SE
  // APAGARA EL MOTOR PARA QUE EL VENTILADOR NO HAGA RUIDO DE 
  // NOCHE SE REGULA DEL 0 AL 100 CON LOS NUMROS PRIMOS.
  valorLDR = analogRead(ILUMINACION);
  brillo = map(valorLDR, 1023, 0, 105, -5);
  Serial.println(brillo);
  
  // EL SISTEMA SOLO SE ACTIVARA SI LA TEMPERATURA ES MAYOR AL 
  // NUMERO NORMAL (NO NECESARIAMENTE PRIMO) Y LA ILUMINACIÓN
  // MAYOR A LA DEL DISPLAY DE NUMEROS PRIMOS EN LA ESCALA QUE
  // SE DEFINIO.
  if (temperatura > numero && brillo >= numero_primo)
  {
   	analogWrite(MOTOR,velocidad); 
  }
  else
  {
  	analogWrite(MOTOR,LOW); 
  }
}  
~~~

## :robot: Link al proyecto
- [proyecto](https://www.tinkercad.com/things/i77Lrzm76wg-copy-of-parcial-domiciliario-parte-2-curto-eivers-tomas-leon/editel?sharecode=NF3FVEAS1_0ooMY2fami3nE5PWlEv1cgaRUR6gzlRxA)

---
### Fuentes
- [Botón antirrebote con Arduino](https://www.youtube.com/watch?v=FoTFJW5Hyz8).

- [MULTIPLEXACIÓN DISPLAY 7 SEGMENTOS CON ARDUINO](https://www.youtube.com/watch?v=bScD6wptNws&t=188s).

- [Slideswitch](https://www.youtube.com/watch?v=cFFwFCuSZN4).

- [Temperature-Controlled Cooling](https://www.youtube.com/watch?v=mtB97aFkdHs).

- [CONTROL DE UN MOTOR DE CORRIENTE CONTINUA CON ARDUINO EN TINKERCAD](https://www.youtube.com/watch?v=fJKPeiwi0Pc&t=843s).

- [Conexión del fotoresistor](https://programarfacil.com/blog/arduino-blog/ldr-arduino/#Que_es_un_LDR).

- [Mapeo del fotoresistor](https://www.circuitbasics.com/how-to-use-photoresistors-to-detect-light-on-an-arduino/).
---
