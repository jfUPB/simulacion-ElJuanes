### Proceso de creación
https://editor.p5js.org/ElJuanes/full/t268G_HSS

Me base en una de las actividades hechas en el puntor anterior y además le agregue el tema de un repeller pero queria cambiar como se interactuba con este, enves de usar el teclado o ratón de nuevo preferi hacer algo con el sonido. ya sea por el microfono o el sonido del sistema. De aqui me plantee usar el motion 101 de la unidad 1, para la unidad 2 uso el sistema de bouncing ball, para la unidad 3 gravedad y algo de fricción.
Actualización, el editor no me quiso agarrar el sonido del sistema entonces al final si lo hice con teclado pero agregandole escala y fuerza al crecer el repller. Además de esto hice que las particulas extendieran
un poco su vida al rebotar y que cambiaran su forma.
``` js
Repeller.js
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.power = 150; // Fuerza inicial
    this.size = 32; // Tamaño inicial
    this.speed = 5; // Velocidad de movimiento
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, this.size);
  }

  repel(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    let strength = (-1 * this.power) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  update() {
    // Movimiento con A y D
    if (keyIsDown(65)) { // A (izquierda)
      this.position.x = max(this.position.x - this.speed, 0);
    }
    if (keyIsDown(68)) { // D (derecha)
      this.position.x = min(this.position.x + this.speed, width);
    }

    // Cambio de tamaño y potencia con W y S
    if (keyIsDown(87)) { // W (aumentar)
      this.size = min(this.size + 2, 100);
      this.power = min(this.power + 10, 300);
    }
    if (keyIsDown(83)) { // S (reducir)
      this.size = max(this.size - 2, 20);
      this.power = max(this.power - 10, 50);
    }
  }
}

```
``` js
Confetti.js
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    rectMode(CENTER);
    fill(this.color);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}

```
``` js
Emitter.js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.repeller = new Repeller(width / 2, height - 50); // Posición inicial del repulsor
  }

  addParticle() {
    let r = random(1);
    if (r < 0.33) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else if (r < 0.66) {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new TriangleParticle(this.origin.x, this.origin.y));
    }
  }

  run() {
    this.repeller.update(); 
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      let repelForce = this.repeller.repel(p); // Calcula la fuerza de repulsión sobre la partícula
      p.applyForce(repelForce); // Aplica la fuerza de repulsión
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

```
``` js
particle.js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.color = color(random(255), random(255), random(255)); // Color inicial aleatorio
  }

  run() {
    let gravity = createVector(0, 0.1);
    this.applyForce(gravity);
    this.update();
    this.checkEdges(); // Nuevo: Rebote en los bordes
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  checkEdges() {
  let bounceFactor = -0.8;

  if (this.position.y >= height) {
    this.position.y = height;
    this.velocity.y *= bounceFactor;
    this.changeColor(); 
    this.extendLife(); // Aumenta la vida
    this.changeShape(); // Cambia de forma
  }
  
  if (this.position.x <= 0 || this.position.x >= width) {
    this.velocity.x *= bounceFactor;
    this.changeColor();
    this.extendLife();
    this.changeShape();
  }
}

  changeColor() {
    this.color = color(random(255), random(255), random(255)); // Nuevo color aleatorio
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.color);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
extendLife() {
  this.lifespan += 120; // Añadir 3 segundos (~60 FPS * 3)
}

changeShape() {
  let r = random(1);
  if (r < 0.33) {
    Object.setPrototypeOf(this, Particle.prototype);
  } else if (r < 0.66) {
    Object.setPrototypeOf(this, Confetti.prototype);
  } else {
    Object.setPrototypeOf(this, TriangleParticle.prototype);
  }
}

}

```
``` js
sketch.js
let emitter;
let mic;
let amplitude;

function setup() {
  createCanvas(600, 400);
  emitter = new Emitter(width / 2, height - 150);
  
  mic = new p5.AudioIn();
  mic.start();
  amplitude = new p5.Amplitude();
  
  // Activa el contexto de audio con una interacción del usuario
  userStartAudio();
}


function draw() {
  background(255);
  
  emitter.repeller.update(); // Actualiza la posición y tamaño del repeller
  emitter.addParticle();
  emitter.run();
  emitter.repeller.show();
}

```
``` js
triangle.js
class TriangleParticle extends Particle {
  show() {
    fill(this.color);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    beginShape();
    vertex(-6, 6);
    vertex(6, 6);
    vertex(0, -6);
    endShape(CLOSE);
    pop();
  }
}

```

