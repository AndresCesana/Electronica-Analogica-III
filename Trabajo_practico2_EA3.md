**Universidad Nacional de Córdoba**  
![][image1]

Facultad De Ciencias Exactas, Física y Naturales   
Escuela de Electrónica

**—————————————————————————————————————**

Cátedra de Electrónica Analogica 3

**Trabajo Practico Teorico Nº2**  
Diseño de amplificadores con parámetros S

**—————————————————————————————————————**

**Profesor Titular:** Ing. Rofrigo Gabriel Bruni.  
**Profesor Adjunto:** Ing. Federico Tomas Dadam.  
**Integrantes:**  
Trucchi, Genaro  
Pieckenstainer, Mateo  
Cesana Andrés, Agustín  
Ricci, Matías

  **2025**

**Índice**

[**Introducción	3**](#introducción)

[**Cálculo de parámetros del transistor	4**](#heading=h.pvfvm0srz70y)

[**Diseño de red de polarización	5**](#diseño-de-red-de-polarización)

[**Diseño de microtiras	10**](#diseño-de-microtiras)

[**Resultado de simulación ideal	11**](#resultado-de-simulación-ideal)

[**Resultado de simulación de implementación física	12**](#resultado-de-simulación-de-implementación-física)

[**Fotografía de implementación	12**](#fotografía-de-implementación)

[**Resultado de medición	13**](#resultado-de-medición)

[**Mejoras propuestas por el equipo	14**](#mejoras-propuestas-por-el-equipo)

[**Conclusión	15**](#conclusión)

# **Introducción**  {#introducción}

Este es un trabajo realizado en la materia Electrónica analógica III perteneciente al noveno cuatrimestre de la carrera de ingeniería electrónica.

Todo el marco práctico de la experiencia se realizó en la Facultad de Ciencias Exactas Físicas y Naturales de la Universidad Nacional de Córdoba.

El objetivo de este proyecto es diseñar y construir un amplificador clase A, de baja señal, en microondas para máxima ganancia y mínimo ruido. Este amplificador se ubica en el receptor de un Tranciever, luego de la antena y su función es maximizar la ganancia y minimizar el ruido en el receptor.

# 

# 

# 

# 

# 

---

# **Selección y justificación de polarización** 

Se realizó un código en Google Colab, el cual reflejaba los puntos de polarización totalmente estables para el amplificador. Se tuvo en cuenta, que la corriente de polarización Ic sea la menor posible para poder tener mayor ganancia y que en lo posible, no estemos tan cerca de la zona de saturación. Dicho esto, también se tuvo en cuenta los puntos con un K que sea lo mayor posible a 1, sin perder de vista las consideraciones anteriores, para así poder tener una ganancia más lineal y estable a lo largo del espectro de frecuencia.

Aunque esto es complicado, debido a las limitaciones del transistor, con que sea mayor a 1,05, aproximadamente 1,1, se puede considerar correcto y habilitado para continuar con la configuración.  
La configuración elegida, fue:

* Ic=40 mA  
* VCE=1,5 V

Para ello, se realizó una simulación en Multisim con potenciómetros que permiten variar la corriente y caída de voltaje en la base y el colector para una correcta polarización y además resistencias fijas en la base y en el colector, que cumplan la función de darle un límite máximo a la corriente que pase por el transistor para no dañarlo.  
---

# **Estabilidad mediante la carta de Smith**

S1: Input plane stability circle; stable outside; K=1.09; S11=0.47 \< 161.10°; S12=0.05 \< 55.10°; S21=7.78 \< 71.90°; S22=0.15 \< \-45.80°; 2.0GHz  
S2: Output plane stability circle; stable inside; K=1.09; S11=0.47 \< 161.10°; S12=0.05 \< 55.10°; S21=7.78 \< 71.90°; S22=0.15 \< \-45.80°; 2.0GHz

# 

---

# **Diseño de red de polarización** {#diseño-de-red-de-polarización}

Para comenzar con la red de polarización nos aseguramos de realizar un etapa previa al amplificador para poder asegurar el voltaje constante.  
Utilizando un LM7805, Regulador de voltaje a 5V, teniendo en cuenta las recomendaciones del fabricante realizamos el siguiente circuito, el nos permite asegurar la salida de voltaje para la polarización (R simulará el resto del circuito).

![][image2]	

Una de las consideraciones que tuvimos a la hora de usar un regulador es su calentamiento por lo que veremos si necesitamos un disipador.

P=(Vin-Vout) Iload= (10 V-5V )0.25A \=1.25W   
![][image3]

Suponiendo una Tamb=20°C y sabiendo que nuestro encapsulado es el “TO-220F” el cual posee una RJA \=65 °C/W

 TJ=20°C \+ (1.2565) \=101,15°C

La cual es una temperatura considerable conociendo que el límite es de 125°C por lo que se va a colocar un pequeño disipador, no se realizarán los cálculos pertinentes debido que excede a la materia.   
![][image4]  
Luego de a través de un código de python pudimos ver que nuestra temperatura teórica debería bajar si nosotros bajamos la tensión de entrada. por lo que vamos a decidirnos de alimentarlo con un valor más cercano a 5V.  
Una vez hecha esta etapa comenzamos con la polarización. Para esto tuvimos que investigar en el datasheet del BFP 640 las curvas.  
![][image5]  
![][image6]  
Realizando un análisis y viendo los datos proporcionados por el fabricante vemos que nuestro objetivos se logran de la siguiente manera:

- Mínimo Ruido: Operar a corrientes de colector bajasIC5mA  
- Máxima ganancia: Operar a corrientes de colector altas IC30mA

No quedamos conforme con esto por lo que decidimos realizar una investigación más profunda donde tomamos todas las polarizaciones posibles que poseen parámetros S en el modelo del ADS, para esto realizamos un Google Colab donde primero cargamos los valores de los parámetros S.

Una de las condiciones para polarizar nuestro transistor es que sea Incondicionalmente estable, para ello se tiene que cumplir la siguiente condición

![][image7]  
donde   
![][image8]			![][image9]

Por lo que calculamos los datos necesarios para comprobar esto.

Una vez hecho todo esto decidimos que la mejor opción de polarización iba a ser   
Vce \=1.5V

* **Vce \= 1.5 V**

* **IC \= 40 mA**

---

Luego de realizar los cálculos de polarización se obtuvieron los siguientes componentes, se realizado una simulación utilizando el modelo Spice de BFP640 para corroborar

![][image10]

El circuito completo nos quedaría algo similar a esto

![][image11]  
---

Luego de implementar este circuito en una placa multiproposito nos dimos cuenta que la simulación del BFP640 no era concluyente con la realidad debido a que no conseguimos la polarización que necesitábamos, por lo que se decidió cambiar el circuito de polarización a uno de “Inyección de Base” como el que se ve en la siguiente imagen  
 	![][image12]

Este consta de dos fuentes por separados la cual poseía un voltaje independiente y una serie de resistencias y potenciómetros los cuales nos permitieron encontrar la polarización que deseamos, posteriormente se hablará de una posible mejora del circuito de polarización.  
Con este circuito conseguimos si conseguimos la polarización necesaria.

Luego se realizaron los cálculos para las micro tiras necesarias dentro de un entorno de Python, donde en el mismo no solo conseguimos los parámetros de las microtiras sino también otras características como coeficientes de reflexión de entrada y salida, impedancias de entrada y salida. Más adelante se verán las simulaciones realizadas en el ADS(Advanced Design Simulation).  
---

# **Diseño de microtiras**  {#diseño-de-microtiras}

Para proceder al diseño de las microtiras, se debe primeramente realizar la caracterización del sustrato. Se compró una placa de FR4 de 10x10 cm, a la cual se le comió un pedazo en una esquina con ácido nítrico, para sacarle el cobre y así poder medir el ancho de la placa con y sin cobre.

Donde resultó ser:

* t=0,031 mm  
* h=1,55 mm  
* r=4,3

Y sabiendo que por diseño, la impedancia característica Z0=50, entonces se puede proceder al cálculo de las microtiras.  
Para el diseño de las microtiras, se utilizan las ecuaciones de Hammerstad y Wheeler  
Realizamos un script en python que implementa las ecuaciones Hammerstad y mediante las especificaciones necesarias calcula el largo de la microtira y si tiene tapper también calcula la longitud final.   
Esto nos dio resultados en el siguiente formato:

Capacitor de microtira de salida  
A \= 1.472115691736825  
B \= 5.921902152016759  
W \= 2.9564681074142656 mm  
We \= 3.0236527070852204 mm  
Epsilom\_eff \= 3.073337699935324  
Lambda\_eff \= 0.10695377807876696 mm  
Beta \= 58.74673545942701 rad/m  
l \= 12.703453137187193 mm  
Eeff \= 42.75906107794891 grados

Una vez obtuvimos estos valores los colocamos en la simulación en ADS y realizamos la simulación. En función a los valores realizamos correcciones mediante el uso de la herramienta de optimización en ADS. Esta herramienta permite modificar los parámetros físicos de las microtiras buscando mejorar los valores de los parámetros.  
Estos valores optimizados son los que utilizamos para realizar la placa físicamente.

---

# **Resultado de simulación ideal** {#resultado-de-simulación-ideal}

Luego de obtener los parámetros S para nuestra polarización se cargaron en el modelos BFP640 SPARAM para poder realizar las simulaciones.  
![][image13]  
Estos resultados son ideales, los valles están en la frecuencia exacta de trabajo, para ver que resultados reales podemos esperar realizaremos una simulación.

---

# **Resultado de simulación de implementación física** {#resultado-de-simulación-de-implementación-física}

Para tener una mayor precisión se realizó una Cosimulacion, la cual mediante el layout del cobre que diseñamos y aplicando las ecuaciones de Maxwell sobre el mismo, logra una simulación más cercana a lo que nos podemos esperar en la realidad.

![][image14]  
Podemos ver que a diferencia de los resultados anteriores, aquí vemos corrimientos del comportamiento en frecuencia. Los valles no solo están corridos si no que ya no tienen el valor de atenuación ideal.

---

# **Fotografía de implementación** {#fotografía-de-implementación}

![][image15]

Como se puede ver la presentación no fue lo más deseable ya que se fueron haciendo arreglos sobre la marcha, tuvimos inconvenientes a la hora del planchado y demás  cuestiones que dejaron la placa sin una presentación adecuada, sin embargo se logró el cometido de polarizar el BFP640.  
---

# 

# **Resultado de medición** {#resultado-de-medición}

Los resultados de la mediciones no fueron como los esperados, varios factores pudieron afectar a la hora de implementar el circuito como una mala masa, soldaduras ineficientes o que nuestro ƐR no sea el correcto.  
Sin embargo se consigue una amplificación en la frecuencia deseada de alrededor de 8.5 dB,  
![][image16]  
Este es el resultado del analizador de espectro, aquí el resultado fue muy diferente al esperado, esto se debe a que en el momento de realizar la medición la polarización del transistor no era estable y estaba levemente alejado del punto que nosotros buscábamos. Esto causó una caída considerable de la ganancia.  
Además se realizaron no solo medición en un Analizador de espectro sino también en un NanoVNA que nos permitió obtener los verdaderos parámetros S de nuestro circuito. Luego con este archivo de parámetros lo pusimos como bloque dentro del ADS por lo que pudimos hacer una simulación real de nuestro circuito. Por lo que con esto pudimos realizar una nueva simulación comparando los 3 casos (Ideal, Cosimulacion y Real).

![][image17]  
Se puede ver en trazo azul el resultado de la simulación ideal, en rosa la cosimulación y en roja el resultado real. Por más que las formas de las curvas no sean coincidentes, podemos observar que los valores obtenidos si coinciden. Las reflexiones tienen valores satisfactorios de atenuación, la ganancia es menor que la ideal  pero sigue siendo positiva. Por último la ganancia inversa es coincidente con las simulaciones por lo que nuestro circuito es unilateral.

---

# **Mejoras propuestas por el equipo** {#mejoras-propuestas-por-el-equipo}

**Cambio de Frecuencia**

Realizamos una búsqueda de nuevas frecuencias para nuestro BFP640, y nos dimos cuenta que para valores cercanos a 7 GHz se consiguen muchos puntos más estables, esto permitiría una mejor polarización del dispositivo, mejorando sus prestaciones y haciendo que no sea tan inestable como con la nuestra.  
Aquí se muestra un gráfico con los puntos estables.

Esta propuesta está ligada directamente a los instrumentos de medición que se dispongan, si nuestro analizador no cumple con la frecuencia a la que queremos trabajar no podremos realizar los análisis correspondientes.

**Fuentes de corriente para la polarización**

Debido a los grandes problemas que tuvimos en nuestro grupo (y otros) a la hora de conseguir una polarización, sugerimos utilizar fuentes de corriente para asegurar la corriente de Colector y el Vce. A continuación se deja un ejemplo de una fuente de polarización.

![][image18]  
En esta fuente de corriente, nosotros calculamos Rbias para el valor de corriente de polarización y donde esta Rload Se coloca el transistor de alta frecuencia junto con una resistencia de colector para medir la corriente y regular la tensión VCE.  
---

---

# **Conclusión** {#conclusión}

Este trabajo nos permitió recorrer el ciclo completo de concepción de un amplificador de bajo ruido en microondas: partimos de la teoría de parámetros S y de las condiciones de mínima figura de ruido para comprender cómo las redes de adaptación influyen simultáneamente en ganancia, ruido y estabilidad; aprendimos a trasladar ese análisis al dominio práctico, usando simuladores de microondas para iterar entre el Smith chart y el layout, y confirmamos la importancia de modelar la placa, los componentes discretos y los accesorios de medición para que simulación y laboratorio converjan. El ensayo de estabilidad nos mostró por qué la retro‑alimentación inversa, aun cuando sea pequeña, puede comprometer el diseño si no se aísla adecuadamente. En laboratorio descubrimos que la soldadura, el encapsulado y el montaje introducen sutiles variaciones que obligan a re‑sintonizar, afianzando la noción de que un diseño de RF nunca está “cerrado” hasta que se mide el prototipo real. Finalmente, el proceso reforzó la utilidad de documentar cada iteración y de contrastar la teoría con la práctica: más allá de los valores concretos obtenidos, el aprendizaje clave fue interiorizar la metodología de diseño iterativo, la validación experimental rigurosa y la lectura crítica de los resultados para seguir optimizando el circuito.

[image1]: extracted_images/image01.png

[image2]: extracted_images/image02.png

[image3]: extracted_images/image03.png

[image4]: extracted_images/image04.png

[image5]: extracted_images/image05.png

[image6]: extracted_images/image06.png

[image7]: extracted_images/image07.png

[image8]: extracted_images/image08.png

[image9]: extracted_images/image09.png

[image10]: extracted_images/image10.png

[image11]: extracted_images/image11.png

[image12]: extracted_images/image12.png

[image13]: extracted_images/image13.png

[image14]: extracted_images/image14.png

[image15]: extracted_images/image15.png

[image16]: extracted_images/image16.png

[image17]: extracted_images/image17.png

[image18]: extracted_images/image18.png