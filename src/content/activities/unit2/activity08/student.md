### ¿Qué pasa si…?
¿Qué pasaría si cambiamos la velocidad de la pelota al hacer clic en la pantalla?

### ¿Qué te imaginas que pasará?
Imaginamos que la velocidad de la pelota cambiará cada vez que se haga clic, lo que hará que se mueva más rápido o más lento dependiendo del clic.

### ¿Qué pasó?
Al hacer clic en la pantalla, la velocidad del objeto mover cambia, y este se mueve con una nueva velocidad cada vez que se haga clic.

### ¿Por qué?
Esto sucede porque se modifican los valores de velocity.x y velocity.y dentro del método de clic, lo que afecta directamente el movimiento del objeto.

### Conclusión
Cambiando la velocidad y haciendo una variación de color con la interacción del usuario, podemos controlar el movimiento del objeto dinámicamente, haciéndolo más interactivo y adaptable a las acciones del usuario.

``` js 
let mover;

function setup() {
  createCanvas(640, 240);
  mover = new Mover();
}

function draw() {
  background(255);
  mover.update();
  mover.checkEdges();
  mover.show();
}

function mousePressed() {
  // Cambia la velocidad y el color de la bola al hacer clic
  mover.velocity = createVector(random(-5, 5), random(-5, 5));
  mover.color = color(random(255), random(255), random(255)); // Color aleatorio
}

class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(random(-2, 2), random(-2, 2));
    this.color = color(127); // Color inicial de la bola
  }

  update() {
    this.position.add(this.velocity);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(this.color); // Usa el color aleatorio
    circle(this.position.x, this.position.y, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}

```
https://editor.p5js.org/ElJuanes/sketches/tSAS36rWz
