## Consolidación
#### ¿Qué concepto de oscilación utilizaste como base para tu obra? Describe cómo lo implementaste.
En mi obra generativa, utilicé el concepto de oscillación a través de un sistema de resortes y ondas. El movimiento de los objetos (bob1 y bob2) simula la interacción
 de dos cuerpos conectados por un resorte, donde bob2 responde de manera oscilante a la posición de bob1. Para agregar un efecto visual, utilicé la función sin() para mover
 bob2 en un patrón de onda, y a medida que se mueve, su color cambia, creando una transición de rojo a azul, representando la oscilación de una onda física.

#### ¿Cómo funciona la interacción en tu obra? Explica cómo el usuario puede modificar la obra en tiempo real.
La interacción en mi obra es dinámica: el usuario puede arrastrar bob1 con el mouse, lo que afecta directamente el movimiento de bob2. Además, al presionar la tecla E,
 el sistema se mueve a una nueva posición aleatoria en el lienzo y genera un círculo en la posición actual de bob2, lo que da una sensación de interacción continua. Esta 
interacción permite que el usuario modifique el comportamiento de los objetos en tiempo real, afectando tanto su movimiento como el color de los mismos.

#### ¿Qué desafíos encontraste durante el proceso de creación? ¿Cómo los superaste?
Uno de los desafíos que enfrenté fue lograr que bob2 se moviera de forma realista como un resorte, respondiendo al movimiento de bob1. Tuve que ajustar las fuerzas del 
resorte y la frecuencia del movimiento para que el comportamiento fuera fluido y natural. Otro desafío fue la interpolación de colores, ya que necesitaba que el cambio 
fuera suave y visualmente atractivo. Para esto, utilicé la función lerpColor() para mapear el movimiento de bob2 y crear un cambio de color gradual. Además de que al momento de implementar 
los cambios que queria hubo varios fallos con la idea y me toco replantear como funcionaban ciertas cosas

#### ¿Qué aprendiste sobre las oscilaciones y su aplicación en el arte generativo?
Aprendí que las oscilaciones no solo son útiles en simulaciones físicas, sino también como herramienta visual en el arte generativo. Usarlas para manipular tanto el 
movimiento como el color crea una experiencia inmersiva, donde los usuarios pueden interactuar con el sistema y explorar cómo las oscilaciones afectan el comportamiento 
de los objetos. Esto me permitió experimentar con la interacción entre la física y el arte, logrando una obra en la que la dinámica y la estética están intrínsecamente conectadas.
