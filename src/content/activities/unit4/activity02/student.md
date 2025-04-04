### Conceptos fundamentales 
#### ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?
Se generan 2 circulos con una linea entre ellos, cada vez que la persona presiona una tecla el angulo va aumentando constantemente en sentido horario siendo posible dar varias vueltas si se presionan las teclas lo suficiente.
#### Trasladando el origen del sistema de coordenadas al centro 
1-Al centrar hacemos de manera más visible el sistema y nos aseguramos que el ususario sea capaz de girarlo indefinidamente y verlo siempre en el centro de la pantallá.
2-Al mover constantemente el sistema al centro támbien nos aseguramos que al aumentar constantemente el angúlo el sistema siempre sea consistente con su posición y movmiento cada que se le da a una tecla.
#### función rotate()
La función rotate se relacióna con sistema de coordenadas cuando se llamá a la función keyPressed para que este valor aumente y el sistema siga aumentando el angulo en sentido positivo u horario.
``` js
function keyPressed(){
  angle += 0.1;
}
``` 
#### ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?
Los elementos parecen dibujarse en (0, 0) porque se usa translate(width / 2, height / 2), que mueve el origen al centro del lienzo. Los objetos rotan porque se aplica el rotate(angle), lo que rota todo el sistema de coordenadas y afecta a todo lo que se dibuja después de esa transformación.

#### Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?
En este caso, el marco de Motion 101 implica la simulación básica de un objeto en movimiento bajo la influencia de aceleración, donde:

La aceleración es calculada en función de la dirección hacia un objetivo (el ratón).
La velocidad se ajusta con la aceleración y se limita a un valor máximo.
La posición del objeto se actualiza con la velocidad.
Es un enfoque simple para simular el movimiento de un objeto en un espacio 2D, usando principios básicos de física (aceleración, velocidad, y posición).

#### 1.¿Qué hace la función heading()?
La función heading() devuelve el ángulo (en radianes) de la dirección del vector de velocidad. Es decir, indica la orientación del vector de velocidad en el plano 2D.

#### 2. ¿Qué hace las funciones push() y pop()?
push() guarda el estado actual de las transformaciones (como translate(), rotate(), etc.).
pop() restaura el estado guardado, deshaciendo las transformaciones realizadas después de push().
Experimento: si eliminas push() y pop(), las transformaciones (como la rotación) afectan a todos los elementos dibujados, pero con push() y pop(), solo afectan al objeto que se encuentra dentro de ese bloque.

#### 3. ¿Qué hace rectMode(CENTER)?
rectMode(CENTER) cambia el modo de dibujo de los rectángulos, de modo que su posición se define desde el centro del rectángulo, en lugar de desde la esquina superior izquierda (que es el comportamiento predeterminado).

Experimento: si cambio a rectMode(CORNER) en lugar de CENTER, el rectángulo se dibuja desde la esquina superior izquierda en lugar de desde el centro.

#### 4. Relación entre el ángulo de rotación y el vector de velocidad:
El ángulo de rotación se calcula a partir de la dirección del vector de velocidad. Si se dibujo el vector de velocidad en un papel (como una flecha desde el centro), el ángulo de rotación es el ángulo que esa flecha forma con el eje horizontal. Este ángulo se utiliza para rotar el rectángulo en la simulación y que siempre apunte en la dirección del movimiento del objeto Mover.
