##  1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?
Segun vi en bastantes ejemplos, lo más importante era que las particulas tenian que tener bien definidas cuando su tiempo de vida se agotaba o si había alguna funcion esperando para ser llamada y exterminar algunas particulas.
Así cuando su tiempo de vida se acaba estas desapareceran y liberara algo de memoria.

## 2. ¿Cómo se aplica el marco de movimiento motion 101 en los sistemas de partículas que analizaste en la actividad anterior?
Usamos la posición y velocidad de las partículas ya que estas se actualizan cada fotograma  y por eso utilizando el marco de movimiento conseguimos el efecto deseado.
Aceleración se calcula con fuerzas externas como la gravedad o la repulsión que fueron los más usados.
El rebote en los bordes del canvas respeta la ley de conservación de la energía (cambio de dirección con una pequeña pérdida de velocidad) y en el caso del ultimo ejercicio cambio de forma y aumento de vida.

## 3. ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?
Usualmente las fuerzas que se le aplicabana  la particula era la fuerza de gravedad ya que estas una vez generadas se desplazan hacia abajo, usualmente estas no se veian afectadas por otras condiciciones ya que no estaba 
programadas, y hablando de cosas extra programadas el repeller, que por ejemplo en el 5.2.5 se añadió el método move(x, y) en Repeller, que actualiza su position. En mouseDragged(), se llama a 
repeller.move(mouseX, mouseY). Por qué: Permite al usuario manipular la simulación en tiempo real, haciendo la interacción más dinámica.

## 4. ¿Cómo se aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?
Herencia:
Se creó una clase base Particle con propiedades y métodos comunes (posición, velocidad, aceleración, tiempo de vida).
Se definieron clases hijas como Confetti y TriangleParticle, que heredan de Particle y reutilizan su funcionalidad.

Polimorfismo:

Cada subclase sobrescribe el método show(), cambiando la forma en que se dibuja la partícula (círculo, cuadrado o triángulo) además del color que se les adhiere.
En Emitter.js, se usa un arreglo de particles, permitiendo manejar distintos tipos de partículas sin cambiar la lógica principal.
