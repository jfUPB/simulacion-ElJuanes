## El problema de los n-cuerpos
### Código
``` js
body.js
class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.color = color(127);
    this.size = this.mass * 16;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.checkEdges();
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(this.color);
    circle(this.position.x, this.position.y, this.size);
  }

  changeProperties() {
    this.mass += 0.5;
    this.size = this.mass * 16;
    this.color = color(random(255), random(255), random(255));
  }

  checkEdges() {
    if (this.position.x < 0 || this.position.x > width) {
      this.velocity.x *= -1;
    }
    if (this.position.y < 0 || this.position.y > height) {
      this.velocity.y *= -1;
    }
  }

  attract(other) {
    let force = p5.Vector.sub(this.position, other.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);

    let strength = (G * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }
}

``` 

``` js
sketch.js
let bodies = [];
let G = 1;
let checkInterval = 4000; // Tiempo en milisegundos (2 segundos)
let lastCheckTime = 0;
let maxSize = 80; // Tamaño máximo antes de dividirse

function setup() {
  createCanvas(640, 240);
  for (let i = 0; i < 10; i++) {
    bodies[i] = new Body(random(width), random(height), random(0.1, 2));
  }
}

function draw() {
  background(255);

  for (let i = bodies.length - 1; i >= 0; i--) { // Iteramos al revés para evitar errores al eliminar
    for (let j = 0; j < bodies.length; j++) {
      if (i !== j) {
        let force = bodies[j].attract(bodies[i]);
        bodies[i].applyForce(force);
      }
    }

    bodies[i].update();
    bodies[i].show();

    // Si el objeto supera el tamaño máximo, se divide
    if (bodies[i].size > maxSize) {
      splitBody(i);
    }
  }

  // Cada 2 segundos, verificamos colisiones y aplicamos cambios
  if (millis() - lastCheckTime > checkInterval) {
    checkCollisionsAndChange();
    lastCheckTime = millis();
  }
}

// Función para verificar colisiones y cambiar propiedades
function checkCollisionsAndChange() {
  for (let i = 0; i < bodies.length; i++) {
    for (let j = i + 1; j < bodies.length; j++) {
      let distance = p5.Vector.dist(bodies[i].position, bodies[j].position);
      let minDistance = (bodies[i].size / 2) + (bodies[j].size / 2);

      if (distance < minDistance) {
        bodies[i].changeProperties();
        bodies[j].changeProperties();
      }
    }
  }
}

// Función para dividir una bola en 2 más pequeñas
function splitBody(index) {
  let parent = bodies[index];
  let newMass = parent.mass / 2; // Cada nueva bola tendrá la mitad de la masa original
  let newSize = parent.size / 2; // Se reducen de tamaño
  let newColor = parent.color; // Mantienen el mismo color

  // Crear dos nuevas bolas con direcciones opuestas
  let offset = createVector(random(-5, 5), random(-5, 5)); // Pequeño empuje aleatorio
  let body1 = new Body(parent.position.x + offset.x, parent.position.y + offset.y, newMass);
  let body2 = new Body(parent.position.x - offset.x, parent.position.y - offset.y, newMass);

  // Asignamos el mismo color
  body1.color = newColor;
  body2.color = newColor;

  // Asignamos velocidad opuesta
  body1.velocity = parent.velocity.copy().rotate(PI / 4);
  body2.velocity = parent.velocity.copy().rotate(-PI / 4);

  // Agregamos los nuevos cuerpos y eliminamos el original
  bodies.push(body1);
  bodies.push(body2);
  bodies.splice(index, 1);
}

```
### Explicación
Esta simulación no solo ilustra el problema de los n-cuerpos desde una perspectiva física, sino que también añade un elemento visual inspirado 
en la estética de Alexander Calder. La combinación de leyes gravitacionales con movimientos artísticos permite que la simulación sea tanto un experimento científico como una experiencia visual y creativa.

### Editor
https://editor.p5js.org/ElJuanes/full/OmKm_KpW6

### Imagen
![image](https://github.com/user-attachments/assets/93606ce5-a2c9-4dc4-a210-2e64401d41e4)
