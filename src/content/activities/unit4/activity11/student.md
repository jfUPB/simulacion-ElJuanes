## Actividad 11
![image](https://github.com/user-attachments/assets/f0920921-7da8-48b1-94cc-d767d67e996c)

![image](https://github.com/user-attachments/assets/45f46ef8-1384-4572-8ee4-7210669c6277)
El concepto que seleccione para la obra interactiva se basa en la interacción entre objetos que simulan un resorte (con el comportamiento físico de atracción y repulsión)
y las ondas (a través de la interpolación de colores y movimiento). Esta idea combina física simple con interacción visual.
Idea Inicial:
La obra interactiva se basa en dos elementos: bob1 y bob2. Estos elementos están conectados por un sistema de resortes. Los usuarios pueden interactuar con los objetos arrastrando 
a uno de ellos (bob1) con el mouse, mientras que el otro objeto (bob2) sigue una simulación de "resorte", moviéndose en respuesta a la posición de bob1. Además, la posición vertical 
de bob2 genera un efecto visual en el que su color cambia a través de una interpolación entre dos colores (rojo y azul), lo que simboliza el comportamiento de una onda.
La obra se enriquece con la interacción del teclado: cuando el usuario presiona la tecla E, el sistema se traslada a una nueva posición aleatoria en el lienzo, y al mismo tiempo, 
se genera un círculo en la posición de bob2, con el color correspondiente en ese momento. Además de que se genera con el tiempo una onda colorida si se presiona la e muchas veces.

### codigo
``` js
Bob.js
class Bob {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = 24;
    this.damping = 0.98;
    this.dragOffset = createVector();
    this.dragging = false;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.mult(this.damping);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  show(c) {
    stroke(0);
    strokeWeight(2);
    fill(c || 127); // Si no se pasa color, usa 127 como predeterminado
    if (this.dragging) {
      fill(200);
    }
    circle(this.position.x, this.position.y, this.mass * 2);
  }

  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  stopDragging() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}

```
``` js
let bob1, bob2;
let spring1, spring2;
let angle = 0;
let angleVelocity = 0.05; // Velocidad de la onda
let amplitude = 100;
let frequency = 0.01; // Este valor controla cuántas ondas hay en la pantalla
let savedCircles = []; // Arreglo para almacenar los círculos creados

function setup() {
  createCanvas(640, 240);
  spring1 = new Spring(width / 2, 10, 100);
  spring2 = new Spring(width / 2, 100, 100);
  bob1 = new Bob(width / 2, 100);
  bob2 = new Bob(width / 2, 200); // Inicializamos bob2
}

function draw() {
  background(255);
  let gravity = createVector(0, 2);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  // Movimiento de la onda para bob2 (opcional, si quieres combinar la onda con el resorte)
  let y2 = amplitude * sin(angle + (bob2.position.x * frequency)); // Movimiento de bob2 basado en onda
  bob2.position.y = y2 + height / 2; // Ajustamos la posición en Y

  // Interpolamos el color basado en la posición Y
  let colorValue = map(bob2.position.y, height / 2 - amplitude, height / 2 + amplitude, 0, 1);
  let circleColor = lerpColor(color(255, 0, 0), color(0, 0, 255), colorValue); // Interpolación de colores

  // Actualizamos y manejamos bob1
  bob1.update();
  bob1.handleDrag(mouseX, mouseY);

  // Para bob2, calculamos la fuerza del resorte entre bob1 y bob2
  let springForce = p5.Vector.sub(bob1.position, bob2.position); // Dirección de la fuerza
  let distance = springForce.mag(); // Distancia entre bob1 y bob2
  let springStrength = 0.05; // Fuerza del resorte (ajustable)
  let restLength = 100; // Longitud en reposo del resorte entre ambos bobs

  // Si la distancia es mayor que la longitud en reposo, aplicamos una fuerza de atracción
  let stretch = distance - restLength; // Cuánto se estira o comprime el resorte
  springForce.setMag(-springStrength * stretch); // Ajustamos la magnitud de la fuerza

  // Aplicamos la fuerza de resorte sobre bob2
  bob2.applyForce(springForce);

  // Actualizamos la posición de bob2 y lo hacemos interactuar con el mouse si se arrastra
  bob2.update();
  bob2.handleDrag(mouseX, mouseY);

  // Conectamos los resortes
  spring1.connect(bob1);
  spring2.anchor = bob1.position.copy();
  spring2.connect(bob2);

  // Mostramos todo
  spring1.constrainLength(bob1, 30, 200);
  spring2.constrainLength(bob2, 30, 200);
  spring1.showLine(bob1);
  spring2.showLine(bob2);
  bob1.show();
  bob2.show(circleColor); // Usamos el color interpolado

  spring1.show();

  // Dibujamos todos los círculos guardados
  for (let i = 0; i < savedCircles.length; i++) {
    let circle = savedCircles[i];
    fill(circle.color);
    noStroke();
    ellipse(circle.x, circle.y, 48);
  }
}

function keyPressed() {
  if (key === 'e' || key === 'E') {
    // Guardamos la posición y el color de bob2 cuando se presiona la tecla "e"
    let colorValue = map(bob2.position.y, height / 2 - amplitude, height / 2 + amplitude, 0, 1);
    let circleColor = lerpColor(color(255, 0, 0), color(0, 0, 255), colorValue);

    // Creamos un nuevo círculo y lo agregamos al arreglo
    savedCircles.push({
      x: bob2.position.x,
      y: bob2.position.y,
      color: circleColor
    });

    // Movemos todo el sistema a una nueva posición aleatoria
    let newX = random(50, width - 50);
    let newY = random(50, height - 50);

    // Actualizamos las posiciones de los bobs y los resortes
    bob1.position = createVector(newX, newY);
    bob2.position = createVector(newX, newY + 100); // Aseguramos que bob2 esté debajo de bob1
    spring1.anchor = bob1.position.copy();
    spring2.anchor = bob1.position.copy();
  }
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}

```

``` js
spring.js
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2;
  }
  // Calculate and apply spring force
  connect(bob) {
    // Vector pointing from anchor to bob location
    let force = p5.Vector.sub(bob.position, this.anchor);
    // What is distance
    let currentLength = force.mag();
    // Stretch is difference between current distance and rest length
    let stretch = currentLength - this.restLength;

    //{!2 .bold} Direction and magnitude together!
    force.setMag(-1 * this.k * stretch);

    //{!1} Call applyForce() right here!
    bob.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {
    //{!1} Vector pointing from Bob to Anchor
    let direction = p5.Vector.sub(bob.position, this.anchor);
    let length = direction.mag();

    //{!1} Is it too short?
    if (length < minlen) {
      direction.setMag(minlen);
      //{!1} Keep position within constraint.
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
      //{!1} Is it too long?
    } else if (length > maxlen) {
      direction.setMag(maxlen);
      //{!1} Keep position within constraint.
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  //{!5} Draw the anchor.
  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  //{!4} Draw the spring connection between Bob position and anchor.
  showLine(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y);
  }
}

```
