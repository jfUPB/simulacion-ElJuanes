## 1. 4.2: an Array of Particles.
``` js
class Particle {
  constructor(x, y) {
    this.anchor = createVector(x, y); // Punto al que el resorte atrae la partícula
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.k = 0.1; // Constante del resorte
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.applySpringForce();
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  applySpringForce() {
    let force = p5.Vector.sub(this.anchor, this.position); // Dirección hacia el anclaje
    let distance = force.mag(); // Distancia actual
    let stretch = distance - 20; // Elongación del resorte (20 es la longitud reposo)
    force.setMag(this.k * stretch); // F = k * x (Ley de Hooke)
    this.applyForce(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
    
    // Dibuja la línea del resorte
    stroke(0, 100);
    line(this.anchor.x, this.anchor.y, this.position.x, this.position.y);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

```
###  Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Explicación General de los Cambios
Concepto aplicado:

Ley de Hooke (Resorte): Se agregó una fuerza elástica que atrae cada partícula a un punto de anclaje.

Aleatorización controlada: Se ajustó el punto de generación de las partículas para que nacieran en un rango alrededor del centro.

Optimización de la tasa de generación: Se redujo la cantidad de partículas generadas para evitar ruido visual.

Cómo se aplicó:

Resorte: Se calculó la fuerza de Hooke usando F = k * x, donde k es la constante de elasticidad y x es la elongación del resorte. Esto se aplicó como una aceleración adicional en la partícula.

Punto de generación: En lugar de un solo punto fijo, se generaron partículas dentro de un rango de ±50 píxeles alrededor del centro (width / 2 + random(-50, 50)).

Menos partículas: Se aplicó if (frameCount % 2 === 0) para generar partículas cada dos fotogramas en lugar de cada uno.

Por qué:

El resorte añade oscilaciones naturales, haciendo que las partículas no solo caigan, sino que vibren y vuelvan al punto de anclaje, lo que da un efecto más dinámico.

Centrar el punto de generación distribuye mejor las partículas en la pantalla, evitando que se acumulen en una sola línea.

Reducir la tasa de generación evita el exceso de partículas, mejorando la legibilidad de la simulación y optimizando el rendimiento.

https://editor.p5js.org/ElJuanes/full/GB9l_40Q6
## 2.   4.4: a System of Systems.
![image](https://github.com/user-attachments/assets/3924d7ca-df30-4282-9a8d-4426c372ed1c)
https://editor.p5js.org/ElJuanes/full/vCjmXXxvN

Explicación de los cambios
Concepto aplicado:

Se usó una función seno (sin) para que las partículas oscilen de un lado a otro mientras caen, creando un efecto de onda.

Cómo se aplicó:

Se guardó la posición inicial en originX para que la oscilación no se acumule con el movimiento normal.

Se usa sin(this.time) * waveStrength para calcular el desplazamiento en X, variando con el tiempo.

this.time aumenta con cada fotograma (this.time += waveSpeed), lo que permite el movimiento ondulatorio.

Por qué:

Este efecto simula el movimiento de partículas en un medio fluido o en el aire, haciéndolas más dinámicas y menos predecibles.

Permite generar efectos interesantes como humo, agua o partículas con un comportamiento más realista.
``` js
class Particle {
  constructor(x, y) {
    this.originX = x;
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.time = random(0, TWO_PI); // Fase aleatoria para la oscilación
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.applyWaveEffect();
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  applyWaveEffect() {
    let waveStrength = 10;
    let waveSpeed = 0.1;
    this.position.x = this.originX + sin(this.time) * waveStrength;
    this.time += waveSpeed;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    let col = this.getInterpolatedColor();
    stroke(col[0], col[1], col[2], this.lifespan);
    strokeWeight(2);
    fill(col[0], col[1], col[2], this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  getInterpolatedColor() {
    let t = this.position.y / height; // Normalizar entre 0 y 1
    let r = lerp(0, 135, t);    // De 0 (azul oscuro) a 135 (azul claro)
    let g = lerp(0, 206, t);    // De 0 (azul oscuro) a 206 (azul claro)
    let b = lerp(139, 250, t);  // De 139 (azul oscuro) a 250 (azul claro)
    return [r, g, b];
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

```

## 3.  4.5: a Particle System with Inheritance and Polymorphism.

![image](https://github.com/user-attachments/assets/6cb12338-d297-450c-9d35-9ac97c040dd2)

https://editor.p5js.org/ElJuanes/sketches/tikqtWDFK
Modifique mucho el codigo entonces es mejor ver todo directamente en el editor para evitar que esta entrega quede más larga de lo que ya va a ser.
Probabilidad en la generación de partículas

Concepto: Uso de valores aleatorios para determinar el tipo de partícula generada.

Aplicación: Se utilizó random(1) para asignar un 33% de probabilidad a cada tipo de partícula (círculo, cuadrado o triángulo).

Motivo: Introducir variedad visual y hacer el sistema más dinámico.

Simulación de rebote (física de colisiones)

Concepto: Inversión de la velocidad al tocar un borde con un factor de pérdida de energía.

Aplicación: Se detecta el contacto con los bordes y se multiplica la velocidad por -0.8 para simular pérdida de energía en cada rebote.

Motivo: Evitar que las partículas atraviesen los límites y dar un comportamiento más realista.

Cambio de color dinámico

Concepto: Asignación de un color aleatorio cuando ocurre un evento específico (rebote).

Aplicación: Al detectar un rebote, se genera un nuevo color con random(255).

Motivo: Agregar un efecto visual atractivo y resaltar la interacción con los bordes.

## 4. 4.6: a Particle System with Forces.

https://editor.p5js.org/ElJuanes/full/Zwos2QYzy

Lógica de eliminación eficiente:

Cada partícula tiene una propiedad lifespan que disminuye con el tiempo.

Si lifespan llega a 0, la partícula se marca como eliminada (isDead() devuelve true).

Se usa un bucle inverso para recorrer la lista y eliminar partículas de forma segura.

Eliminación cuando la velocidad es mínima:

Si la partícula se desacelera casi por completo (|velocity| < 0.2), se fuerza su eliminación.

Esto evita que partículas "muertas" consuman memoria innecesariamente.

Lo agregado es una aceleración en este caso desaceleración y que asi cuando se llegue a 0 se borre la particula, generando un efecto de fuente rebote y ya desaparecen poco despues de esto.

## 5. 4.7
https://editor.p5js.org/ElJuanes/full/NnWGVatEc
Interacción entre fuerzas y partículas

Concepto: En la simulación física, las partículas pueden ser afectadas por fuerzas externas, como la gravedad y repulsión.

Cómo se aplicó:

Se agregó una fuerza de repulsión en applyRepeller(), que modifica la aceleración de cada partícula. Si la magnitud de la fuerza aplicada es suficiente (f.mag() > 0.1), se cambia su color a rojo.

Por qué: Esto permite visualizar qué partículas fueron afectadas por el repeller, mejorando la claridad del sistema. 
Movimiento del repeller con el mouse

Concepto: Un objeto en una simulación puede actualizar su posición según la entrada del usuario.

Cómo se aplicó:

Se añadió el método move(x, y) en Repeller, que actualiza su position.

En mouseDragged(), se llama a repeller.move(mouseX, mouseY).

Por qué: Permite al usuario manipular la simulación en tiempo real, haciendo la interacción más dinámica.

Aumento de la fuerza del repeller

Concepto: La repulsión se calcula con una fuerza proporcional a la distancia inversa al cuadrado, similar a la ley de gravitación.


