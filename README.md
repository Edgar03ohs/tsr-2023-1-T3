# tsr-2023-1-T1
# Investigación: Algoritmo "Go to Point" Simple

Entrega de la tarea No. 3 de la asignatura


| Código | Description |
| ------:| ----------- |
| ***Asignatura*** | Código del Trabajo o Número de Tarea | 
| **TSR-2023-I** | Tarea 03 |

## Contenido


- [tsr-2023-1-T1](#tsr-2023-1-t1)
- [Investigación: Algoritmo "Go to Point" Simple](#investigación-algoritmo-go-to-point-simple)
  - [Contenido](#contenido)
  - [Introducción](#introducción)
  - [Desarrollo](#desarrollo)
  - [Conclusiones](#conclusiones)
  - [Autor](#autor)
  - [Referencias](#referencias)

## Introducción

De manera general, podemos decir que un algoritmo es un conjunto ordenado de operaciones sistemáticas con un fin determinado. Dentro de la robótica móvil, algo primordial es conseguir que el robot se desplace de un punto inicial a otro distinto. Cuando se comienza a indagar al respecto, es posible encontrarse con varios tipos de algoritmos que hacen posible esta tarea pero de distintas formas, debido a que las necesidades específicas de desplazamiento son diversas y dependen del caso.

Esta investigación se centra en el algoritmo "Go to Point" simple, que se centra en mover un robot móvil del tipo (2,0) o diferencial, que se mueve y cambia de dirección gracias a la regulación de las velocidades de cada una de sus dos llantas motorizadas.


## Desarrollo

El algoritmo Go to Point simple logra llevar al robot de un punto P(x0,y0) con una rotación de θ radianes en su eje Z hasta el punto P(x1, y1) sin importar su orientación (θ radianes de giro sobre su eje Z). Esto es que basta con tomar en cuenta la orientación que se tiene en un inicio, para poder re dirigirla y que apunte a su destino, considerandose cumplido el objetivo cuando la nueva ubicación del robot coincida con la de destino sin tomar en cuenta la orientación en el nuevo punto o la que pueda tomar luego de su llegada.

Para empezar con el movimiento del robot, deben conocerse las coordenadas de la posición desde la que se iniciará el movimiento, por lo que la primera parte del código que se desarrolle para implementar el algoritmo deberá incluir la inicialización de variables correspondientes a la posición del robot, en ambos ejes del plano en el que se mueve, y a la orientación del mismo. Luego se debe recurrir a la odometría del robot y a recibir mensajes que contengan la información ya mencionada para asignarla a las variables declaradas usando "Pose".

Ya que se cuenta con el punto de partida y previo a dar las indicaciones o comandos, se  calcula la distancia total y unidireccional existente de P0 a P1, a través de las distancias en ambos ejes, "x" y "y", de los puntos para hacer uso del teorema de pitágoras con la raiz cuadrada de la suma de los cuadrados de las distancias de coordenadas.

Después, ya conocida la trayectoria que debe tomar el robot, se debe hacer que coincida su destino con el lugar al que apunta. Conseguimos dicha variación en el ángulo en que apunta calculando la diferencia que hay entre el ángulo obtenido del mensaje del publicador y el que forma la recta que se dibuja por la dirección entre origen y destino. Para el ángulo formado por la linea de dirección que se debe tomar, usamos la razón trigonométrica que involucra a ambos catetos de un triángulo (la distancia en ambas coordenadas) y, recordando que en el círculo unitario dividido en cuadrantes las secciones en contra esquina ofrecen los mismos resultados, es necesario distinguir de qué sección exacta se trata con la herramienta que si contempla esto "atan2()" que, antes de hacer la división entre catetos, considera su signo para distinguir cuadrante y al final, de ser necesario agrega pi al ángulo.

Así, entonces, se indica al robot que gire no necesario para que coincidan el angulo que forman la horizontal y su linea de avance con la dirección establecida previamente. Ya con esto se procede a avanzar la distancia total entre los puntos, que se calculó al principio y, al momento de llegar al punto P1 o P(x1,y1) se puede considerar que la tarea ya fue concluida exitosamente.

Sin embargo no todo termina en este momento para todos los casos, pues si bien en una simulación no tan detallada se puede conseguir esto, cuando se tiene la consideración de motores apegada a la realidad es necesario tomar en cuenta que los motores tienen un tiempo de arranque y uno de frenado, puesto que tienen que vencer la inercia de, primero, un estado original de reposo y, segundo, del giro que ya llevan las llantas y la cantidad de movimiento del robot per se. Lo anterior se ve reflejado en la necesidad de calcular variaciones en las velocidades y aceleraciones de las llantas según la posición.

Por otro lado, si lo que se requiere es que el robot termine viendo en una dirección específica que no sea necesariamente la que llevaba durante su desplazamiento, entonces es necesario agregar pasos al algoritmo.

Para ello se debe calcular también el ángulo respecto de la horizontal que forma la línea de la dirección en la que se quiere que esté apuntando el robot y restarlo del que ya se tenía de la recta de la dirección de desplazamiento también respecto de la horizontal y, así, realizar el ajuste correspondiente como en los primeros pasos.

De una manera general e idealizada eso sería todo, pero la realidad es que, ya en los escenarios reales los terrenos o superficies no son lisos o regulares del todo y lo mismo sucede con las llantas, no son perfectas ni exactamente iguales una de la otra y de la misma manera pueden haber variaciones en el funcionamiento de los motores, entre otros pequeños detalles internos o externos que provoquen que, a lo largo del trayecto del robot, este sufra variaciones en su orientación o velocidad, ya sea lineal o angular. Para solucionar este problema o evitar a tiempo estas situaciones, es conveniente un monitoreo constante de la velocidad de cada llanta y con la odometría del robot conocer su pose a través de los mensajes de reporte.

Con conocimiento de la pose del robot en los distintos puntos del trayecto y una comparación del estado real en esos puntos, con el que debería ser su estado según el planteamiento inicial, por medio de restas de ángulos como las explicadas anteriormente, se puede asegurar un desplazamiento con la menor cantidad de variaciones posibles, pues, en cuanto se detectara una incongruencia se realizaría una acción de ajuste y compensación.


## Conclusiones

La diferencia que me imagino existe entre este algoritmo "simple" es en variaciones en los métodos de obtencion de las condiciones de Pose en los distintos puntos de la trayectoria o métodos diferentes para el cálculo de las distancias según los criterios, como considerando desviaciones que tenga que tomar el robot en su camino.

Este fue un recordatorio de que muchas veces una tarea, puede parecernos cosa simple en un pensamiento inicial. Sin embargo, cuando comenzamos a plantear el problema, desmenuzarlo, y expresar su solución con un orden específico y conseguir que se entienda, se vuelve una tarea compleja y más larga de lo que parecía en un inicio.

## Autor

**Autor** Hernández Sosa Edgar Omar [GitHub profile](https://github.com/Edgar03ohs)

## Referencias

<a id="1">[1]</a> Del canal "The Constructor": [ROS Q&A] 053 - How to Move a Robot to a Certain Point Using Twist, disponible en: https://www.youtube.com/watch?v=eJ4QPrYqMlw
<a id="2">[2]</a> Funciones Acos, Acot, Asin, Atan, Atan2, Cos, Cot, Degrees, Pi, Radians, Sin y Tan en Power Apps, disponible en: https://learn.microsoft.com/es-es/power-platform/power-fx/reference/function-trig
<a id="3">[3]</a> Notas y videos de apoyo de las sesiones de clase grabadas.