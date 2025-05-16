##  Materialización

### Variación?
En este caso al estar pensando en la idea y desarrollándola al mismo tiempo no hubo muchos cambios en la implementación fuera del cambio en código para solucionar errores.

### Codigo
``` js
flowfield.js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Flow Field Following

class FlowField {

  constructor(r, x, y, size) {
    this.resolution = r;
    this.originX = x;  // Coordenada X del punto de clic
    this.originY = y;  // Coordenada Y del punto de clic
    this.size = size;  // Tamaño del campo de flujo en píxeles
    //{!2} Determinamos las columnas y filas del campo de flujo en base a la resolución y el tamaño del área
    this.cols = Math.floor(this.size / this.resolution);
    this.rows = Math.floor(this.size / this.resolution);
    //{!1} Creamos un campo de flujo de tamaño limitado en el área alrededor del clic
    this.field = make2Darray(this.cols, this.rows);
    this.init();
  }

  // El init() llena nuestro array 2D con vectores de flujo
  init() {
    // Reseed noise para un nuevo flujo cada vez
    noiseSeed(random(10000));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        // Usamos ruido Perlin para crear los vectores de flujo
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }

  // Dibujamos los vectores del campo de flujo
  display() {
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        drawVector(this.field[i][j], this.originX + i * this.resolution, this.originY + j * this.resolution, this.resolution - 2);
      }
    }
  }

  // Función que retorna un p5.Vector basado en la posición
  lookup(lookup) {
    let column = constrain(floor((lookup.x - this.originX) / this.resolution), 0, this.cols - 1);
    let row = constrain(floor((lookup.y - this.originY) / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

// Helper function to make a 2D array
function make2Darray(cols, rows) {
  let array = new Array(cols);
  for (let i = 0; i < cols; i++) {
    array[i] = new Array(rows);
  }
  return array;
}

// Renders a vector 'v' as an arrow at a location 'x,y'
function drawVector(v, x, y, scayl) {
  push();
  let arrowsize = 4;
  // Translate to location to render vector
  translate(x, y);
  strokeWeight(1);
  stroke(127);
  // Call vector heading function to get direction (note that pointing to the right is a heading of 0) and rotate
  rotate(v.heading());
  // Calculate length of vector & scale it to be bigger or smaller if necessary
  let len = v.mag() * scayl;
  // Draw three lines to make an arrow (draw pointing up since we've rotate to the proper direction)
  line(0, 0, len, 0);
  pop();
}

sketch.js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Usamos esta variable para decidir si mostrar el campo de flujo en modo debug
let debug = true;

// Arreglo de objetos FlowField
let flowfields = [];
// Arreglo de vehículos
let vehicles = [];

function setup() {
  createCanvas(360, 240);
  // Crear algunos vehículos con velocidad y fuerza aleatorias
  for (let i = 0; i < 120; i++) {
    vehicles.push(new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5)));
  }
}

// Crear un nuevo campo de flujo con una resolución de 20 cada vez que se hace clic
function mousePressed() {
  let resolution = 20;  // Resolución de cada celda del flujo
  let flowSize = 100;   // Tamaño del campo de flujo (en píxeles) alrededor del clic
  
  // Se crea un campo de flujo centrado en el punto donde se hace clic
  flowfields.push(new FlowField(resolution, mouseX, mouseY, flowSize));
}

function draw() {
  background(255);

  // Dibujar todos los campos de flujo en modo debug (si está activado)
  if (debug) {
    for (let i = 0; i < flowfields.length; i++) {
      flowfields[i].display();
    }
  }

  // Hacer que todos los vehículos sigan todos los campos de flujo generados
  for (let i = 0; i < vehicles.length; i++) {
    for (let j = 0; j < flowfields.length; j++) {
      vehicles[i].follow(flowfields[j]);
    }
    vehicles[i].run();
  }
}

// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Usamos esta variable para decidir si mostrar el campo de flujo en modo debug
let debug = true;

// Arreglo de objetos FlowField
let flowfields = [];
// Arreglo de vehículos
let vehicles = [];

function setup() {
  createCanvas(360, 240);
  // Crear algunos vehículos con velocidad y fuerza aleatorias
  for (let i = 0; i < 120; i++) {
    vehicles.push(new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5)));
  }
}

// Crear un nuevo campo de flujo con una resolución de 20 cada vez que se hace clic
function mousePressed() {
  let resolution = 20;  // Resolución de cada celda del flujo
  let flowSize = 100;   // Tamaño del campo de flujo (en píxeles) alrededor del clic
  
  // Se crea un campo de flujo centrado en el punto donde se hace clic
  flowfields.push(new FlowField(resolution, mouseX, mouseY, flowSize));
}

function draw() {
  background(255);

  // Dibujar todos los campos de flujo en modo debug (si está activado)
  if (debug) {
    for (let i = 0; i < flowfields.length; i++) {
      flowfields[i].display();
    }
  }

  // Hacer que todos los vehículos sigan todos los campos de flujo generados
  for (let i = 0; i < vehicles.length; i++) {
    for (let j = 0; j < flowfields.length; j++) {
      vehicles[i].follow(flowfields[j]);
    }
    vehicles[i].run();
  }
}

// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// The "Vehicle" class

class Vehicle {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(0, 0);
    this.r = 4;
    this.maxspeed = ms || 4;
    this.maxforce = mf || 0.1;
  }

  run() {
    this.update();
    this.borders();
    this.display();
  }

  // Implementing Reynolds' flow field following algorithm
  // http://www.red3d.com/cwr/steer/FlowFollow.html
  follow(flow) {
    // What is the vector at that spot in the flow field?
    let desired = flow.lookup(this.position);
    // Scale it up by maxspeed
    desired.mult(this.maxspeed);
    // Steering is desired minus velocity
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce); // Limit to maximum steering force
    this.applyForce(steer);
  }

  applyForce(force) {
    // We could add mass here if we want A = F / M
    this.acceleration.add(force);
  }

  // Method to update location
  update() {
    // Update velocity
    this.velocity.add(this.acceleration);
    // Limit speed
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    // Reset accelerationelertion to 0 each cycle
    this.acceleration.mult(0);
  }

  // Wraparound
  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  display() {
    // Draw a triangle rotated in the direction of velocity
    let theta = this.velocity.heading() + PI / 2;
    fill(127);
    stroke(0);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(theta);
    beginShape();
    vertex(0, -this.r * 2);
    vertex(-this.r, this.r * 2);
    vertex(this.r, this.r * 2);
    endShape(CLOSE);
    pop();
  }
}
```
``` js 
let vehicles = [];
let paths = [];
let debug = false;

function setup() {
  createCanvas(640, 480);
  paths.push(createPath());
  vehicles.push(new Vehicle(random(width), random(height), random(2, 5), random(0.1, 0.5)));
}

function draw() {
  background(255);
  
  
  if (debug) {
    for (let path of paths) {
      path.display();
    }
  }
  
  
  for (let vehicle of vehicles) {
    vehicle.follow(paths[vehicle.pathIndex]);
    vehicle.run();
  }
}

function mousePressed() {
  paths.push(createPath());
  vehicles.push(new Vehicle(mouseX, mouseY, random(2, 5), random(0.1, 0.5)));
}

function createPath() {
  let path = [];
  let xOff = random(0, 1000); // Inicio del ruido Perlin
  let yOff = random(0, 1000); 
  for (let x = 0; x < width; x++) {
    
    let y = map(noise(xOff, yOff), 0, 1, 0, height);

    
    y += gaussianNoise(x) * 20; 

    path.push(createVector(x, y));
    xOff += 0.05;
    yOff += 0.05;
  }
  return new Path(path);
}


function gaussianNoise(x) {
  return randomGaussian(); 
}


class Vehicle {
  constructor(x, y, maxSpeed, maxForce) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxSpeed = maxSpeed;
    this.maxForce = maxForce;
    this.pathIndex = paths.length - 1; 
    this.pathProgress = 0; 
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  follow(path) {
    
    this.pathProgress += 1;
    if (this.pathProgress >= path.points.length) {
      this.pathProgress = 0; 
    }

  
    let target = path.points[this.pathProgress];
    let desired = p5.Vector.sub(target, this.position);
    desired.normalize();
    desired.mult(this.maxSpeed);
    
    let steering = p5.Vector.sub(desired, this.velocity);
    steering.limit(this.maxForce);
    this.applyForce(steering);
  }

  run() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0); 
    this.display();
  }

  display() {
    fill(127);
    stroke(0);
    ellipse(this.position.x, this.position.y, 10, 10); 
  }
}


class Path {
  constructor(points) {
    this.points = points;
  }

  display() {
    noFill();
    stroke(200);
    beginShape();
    for (let pt of this.points) {
      vertex(pt.x, pt.y);
    }
    endShape();
  }
}
```
### Captura del contenido generado
<img src="../../../../assets/1-9-1.jpg" style="height: 80%; width:80%;" />
<img src="../../../../assets/1-9-2.jpg" style="height: 80%; width:80%;" />
