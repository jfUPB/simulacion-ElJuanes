## Flujo de trabajo
setup(): Crear motor, mundo y cuerpos iniciales.

draw(): Actualizar física (Engine.update) + dibujar cuerpos (manual o automático).

Eventos: Modificar propiedades de cuerpos (ej. hacerlos dinámicos) o manejar lógica personalizada.

Diferencia clave
Matter.js se encarga de la física (posición, velocidad, colisiones).

p5.js se usa para el renderizado visual y la interacción, aunque pueden combinarse con el renderizador de Matter.js para simplificar.

## Representación visual vs. simulación física:
Posición y rotación:

Los cuerpos físicos en Matter.js tienen propiedades como position.x, position.y y angle.

En p5.js, estas propiedades se usan para dibujar las formas en el lugar correcto.

Dibujado manual:

Para formas básicas (rectángulos, círculos), se usan rect() o ellipse() en p5.js.

Para rotar, se aplica translate() y rotate() antes de dibujar.

El texto (como números o letras) se dibuja con text(), alineado al centro según la posición del cuerpo.

Desafíos:

Rotación: Matter.js rota desde el centro, p5.js desde el origen (0,0). Se corrige con push()/pop() y transformaciones.

Formas complejas: Si el cuerpo tiene una forma irregular, se dibuja con beginShape() usando sus vértices (body.vertices).

Rendimiento: Muchos cuerpos complejos pueden ralentizar el sketch. Se optimiza simplificando gráficos.

Ejemplo en el código:

El número que cae usa text() en p5.js, ubicado con currentNumber.position.x/y.


## Creación de formas complejas: 
El mayor problema en micódigo fue sincronizar la caída del número con los límites del canvas y la transición al siguiente valor.

Desafío específico:

Asegurar que el número desaparezca al tocar el suelo (fadeAlpha) y que el nuevo aparezca suavemente (nextY).

Controlar los estados (animationPhase) para que la secuencia no se rompa.

Solución:

Usar banderas (como animationPhase) y ajustar manualmente las posiciones (currentNumber.position.y, nextY).

##  Física para la semántica
Significados que funcionan bien:

Verbos/acciones: "Caer", "rebotar", "romper".

Conceptos físicos: "Peso", "gravedad", "equilibrio".

Significados difíciles:

Abstractos: "Amor", "tiempo", "libertad".

Complejos: "Democracia", "algoritmo" 

Conclusión:
Ideal para palabras con manifestación física observable, poco útil para las abstractas o simbólicas.

## Potencial exploratorio:
Juegos interactivos

Plataformas con gravedad ajustable o puzzles con piezas físicas que el usuario pueda arrastrar.

Simulaciones didácticas

Visualizar conceptos científicos (gravedad, fuerzas) con cuerpos que chocan o se atraen.

Efectos visuales dinámicos

Lluvia de símbolos, textos que se rompen como vidrio al hacer clic, o logos que se desintegran.
