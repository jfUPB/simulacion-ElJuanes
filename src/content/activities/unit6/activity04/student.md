## Conceptualiza la aplicación inesperada
### Brainstorming:
En lugar de simular pájaros o peces, voy a usar la lógica del algoritmo de flocking para representar partículas de agua dentro de una
caja muy reducida en el espacio, casi en 2D, como si fuera una pecera o recipiente transparente.

Cada agente será una gota microscópica de agua. El movimiento colectivo representará cómo reaccionan esas partículas ante un estímulo
externo inesperado: un movimiento brusco de la caja. Este movimiento vendrá representado por un líder invisible o una fuerza repentina
que empuja las partículas hacia un lado, como si alguien agitara la caja en un mundo 3D.

### Concepto definido:
“Agua Inestable en un Recipiente 2D”

El sistema simula cómo las moléculas de agua, contenidas en un espacio reducido y casi plano, se comportan como una unidad dinámica y 
sensible ante movimientos del entorno. El "líder" representa un cambio abrupto del mundo 3D sobre este plano: una vibración, un temblor, una sacudida.

## Diseña la implementación
Adaptación del algoritmo base (Flocking):
Mantendremos las reglas de Separación, Alineación y Cohesión, para simular el comportamiento molecular del agua: mantenerse juntas, moverse de
 forma fluida y evitar colisiones.

Añadiremos una fuerza externa fuerte: “shock” o impulso repentino (como si se moviera la caja). Esta fuerza actuará como el "líder", generando un
 efecto en masa.

Esta fuerza será controlada por el usuario (con el mouse) o generada aleatoriamente en momentos inesperados.

### Agentes:
Representan moléculas de agua. Se comportan como un enjambre líquido, manteniéndose en conjunto y reaccionando a perturbaciones.

### Campo/Reglas:
El campo es el espacio limitado de la caja. Las reglas del enjambre simulan propiedades físicas del agua: cohesión, movimiento fluido, pero con 
reacción inmediata a estímulos bruscos.

### Aspecto visual:
Las partículas (agentes) serán círculos semi-transparentes, azules o turquesa.

El fondo será una caja rectangular simple, con líneas para definir los límites del recipiente.

Cuando el impulso externo actúe, se verá un efecto de oleaje o dispersión, simulando cómo el agua reacciona dentro del recipiente.

### Interacción:
El mouse representa un impulso externo: si se mueve bruscamente o se hace clic, genera una sacudida que empuja a las partículas en esa dirección.

Teclas pueden modificar la intensidad del impulso, o el grado de cohesión entre partículas.

Clic puede reiniciar el sistema o cambiar la dirección del impulso.
``` js 
let flock;
let externalForce;
let showForce = false;
let forceDir = 'right'; // Puede ser 'right', 'left', 'up', 'down'

function setup() {
  createCanvas(800, 500);
  flock = new Flock();
  externalForce = createVector(0, 0);
  for (let i = 0; i < 120; i++) {
    let boid = new Boid(random(width), random(height));
    flock.addBoid(boid);
  }
  noStroke();
}

function draw() {
  background(10, 20, 40, 40); // Azul oscuro translúcido
  drawTankBorders();

  if (showForce) drawExternalForceArrow();

  flock.run();
}

// === Visualiza la pecera (bordes translúcidos)
function drawTankBorders() {
  stroke(100, 150, 200, 100);
  noFill();
  strokeWeight(4);
  rect(0, 0, width, height);
}

// === Visualiza la flecha de fuerza externa
function drawExternalForceArrow() {
  push();
  stroke(255, 50, 50);
  strokeWeight(4);
  fill(255, 100, 100, 100);
  translate(width / 2, height / 2);

  let dir;
  switch (forceDir) {
    case 'right': dir = createVector(100, 0); break;
    case 'left': dir = createVector(-100, 0); break;
    case 'up': dir = createVector(0, -100); break;
    case 'down': dir = createVector(0, 100); break;
  }

  line(0, 0, dir.x, dir.y);
  let arrowSize = 10;
  let angle = atan2(dir.y, dir.x);
  translate(dir.x, dir.y);
  rotate(angle);
  triangle(0, 0, -arrowSize, arrowSize / 2, -arrowSize, -arrowSize / 2);
  pop();
}

// === Control con teclado
function keyPressed() {
  if (key === 's' || key === 'S') {
    showForce = true;
    // Dirección aleatoria de la fuerza
    let dirs = ['left', 'right', 'up', 'down'];
    forceDir = random(dirs);

    switch (forceDir) {
      case 'right': externalForce = createVector(1.5, 0); break;
      case 'left': externalForce = createVector(-1.5, 0); break;
      case 'up': externalForce = createVector(0, -1.5); break;
      case 'down': externalForce = createVector(0, 1.5); break;
    }

    // Apaga la fuerza luego de un momento
    setTimeout(() => {
      externalForce = createVector(0, 0);
      showForce = false;
    }, 1500);
  }
}

// === Clases Flock y Boid
class Flock {
  constructor() {
    this.boids = [];
  }

  run() {
    for (let boid of this.boids) {
      boid.run(this.boids);
    }
  }

  addBoid(b) {
    this.boids.push(b);
  }
}

class Boid {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(1, 2));
    this.acceleration = createVector(0, 0);
    this.r = 3.0;
    this.maxforce = 0.05;
    this.maxspeed = 2;
  }

  run(boids) {
    this.flock(boids);
    this.update();
    this.borders();
    this.render();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  flock(boids) {
    let sep = this.separate(boids);
    let ali = this.align(boids);
    let coh = this.cohesion(boids);

    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);

    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
    this.applyForce(externalForce); // Aplica la fuerza externa
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.position);
    desired.normalize();
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    return steer;
  }

  render() {
    fill(100, 200, 255, 180);
    ellipse(this.position.x, this.position.y, 6, 6);
  }

  borders() {
    if (this.position.x < 0 || this.position.x > width) {
      this.velocity.x *= -0.9;
    }
    if (this.position.y < 0 || this.position.y > height) {
      this.velocity.y *= -0.9;
    }
    this.position.x = constrain(this.position.x, 1, width - 1);
    this.position.y = constrain(this.position.y, 1, height - 1);
  }

  separate(boids) {
    let desiredSeparation = 25;
    let steer = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < desiredSeparation)) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.normalize();
        diff.div(d);
        steer.add(diff);
        count++;
      }
    }
    if (count > 0) {
      steer.div(count);
    }
    if (steer.mag() > 0) {
      steer.normalize();
      steer.mult(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }

  align(boids) {
    let neighborDist = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < neighborDist)) {
        sum.add(other.velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.normalize();
      sum.mult(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector(0, 0);
    }
  }

  cohesion(boids) {
    let neighborDist = 50;
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < neighborDist)) {
        sum.add(other.position);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum);
    } else {
      return createVector(0, 0);
    }
  }
}
```
https://editor.p5js.org/ElJuanes/full/w2vqo2A8W

![image](https://github.com/user-attachments/assets/013ef795-1c61-4097-9551-0d9f187d9856)

