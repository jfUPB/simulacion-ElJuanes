## ¿Y qué relación tiene esto de las leyes de Newton con al arte generativo?
###  ¿Qué problema le ves a este planteamiento?
El planteamiento actual sobrescribe la aceleración en cada aplicación de fuerza, lo que impide que se acumulen las fuerzas correctamente, ya que se está 
sustituyendo la aceleración en lugar de sumarla.

### ¿Qué solución propones?
 En lugar de sobrescribir la aceleración, se debe acumular la fuerza en la aceleración, asegurando que todas las fuerzas actúen juntas en cada frame. Para esto, 
se debe modificar el método applyForce para sumar las fuerzas a la aceleración y no reemplazarla.

¿Cómo lo implementarías en p5.js?
``` js
class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = 1;
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);  // Acumular fuerzas
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);  // Resetear aceleración para el siguiente frame
  }

  display() {
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

let mover, wind, gravity;

function setup() {
  createCanvas(400, 400);
  mover = new Mover();
  wind = createVector(0.1, 0);
  gravity = createVector(0, 0.1);
}

function draw() {
  background(220);
  mover.applyForce(wind);
  mover.applyForce(gravity);
  mover.update();
  mover.display();
}

```
