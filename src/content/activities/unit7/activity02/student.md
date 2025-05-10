## 1.Codigo
Box
``` js  
function Box(x, y, w, h) {
  var options = {
    friction: 0.5,
    restitution: 0.8,
    angle: PI
  }
  this.body = Bodies.rectangle(x, y, w, h, options);
  this.w = w;
  this.h = h;
  World.add(world, this.body);

  this.show = function() {
    var pos = this.body.position;
    var angle = this.body.angle;

    push();
    stroke(200);
    strokeWeight(2);
    fill(255, 255, 255, 100);
    translate(pos.x, pos.y);
    rotate(angle);
    rectMode(CENTER);
    rect(0, 0, this.w, this.h);
    pop();
  }
}
```
Circle
```js
function Circle(x, y, r) {
  var options = {
    friction: 0.3,
    restitution: 0.8,
    frictionAir: 0.02
  }
  this.body = Bodies.circle(x, y, r, options);
  this.r = r;
  World.add(world, this.body);

  this.show = function() {
    var pos = this.body.position;

    push();
    stroke(200);
    strokeWeight(2);
    fill(100, 255, 255, 100);
    translate(pos.x, pos.y);
    ellipseMode(RADIUS);
    ellipse(0, 0, this.r);
    // Pequeña línea para mostrar rotación
    line(0, 0, this.r, 0);
    pop();
  }
}
```
Sketch.js
``` js
// module aliases
var Engine = Matter.Engine,
  World = Matter.World,
  Bodies = Matter.Bodies;

var engine;
var world;
var boxes = [];
var circles = [];
var ground;
var platform;
var counter = 0; // Contador para alternar

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();
  world = engine.world;
  Engine.run(engine);
  
  // Suelo
  var options = {
    isStatic: true
  }
  ground = Bodies.rectangle(200, height, width, 10, options);
  
  // Plataforma diagonal
  platform = Bodies.rectangle(width/2, height/2 + 50, width*0.7, 10, {
    isStatic: true,
    angle: PI/6 // 30 grados de inclinación
  });
  
  World.add(world, [ground, platform]);
}

function mouseDragged() {
  // Alternamos usando un contador
  if (counter % 2 === 0) {
    boxes.push(new Box(mouseX, mouseY, 20, 20)); // Tamaño aumentado para mejor visibilidad
  } else {
    circles.push(new Circle(mouseX, mouseY, 10)); // Radio de 10 (diámetro 20)
  }
  counter++;
}

function draw() {
  background(10);
  
  // Dibujar cajas
  for (var i = 0; i < boxes.length; i++) {
    boxes[i].show();
  }
  
  // Dibujar círculos
  for (var i = 0; i < circles.length; i++) {
    circles[i].show();
  }
  
  // Dibujar suelo
  stroke(255);
  strokeWeight(5);
  rectMode(CENTER);
  rect(200, height, width, 10);
  
  // Dibujar plataforma diagonal
  push();
  translate(platform.position.x, platform.position.y);
  rotate(platform.angle);
  rect(0, 0, width*0.7, 10);
  pop();
}
```
### Imagen de la simulación
![image](https://github.com/user-attachments/assets/607fa015-ad04-4105-a957-46d2cc72c037)

## Resumen de conceptos
### 1. Engine (Motor)
El núcleo de Matter.js. Controla:

La simulación física (actualizaciones)

El paso del tiempo (timing)

La coordinación entre todos los componentes

### 2. World (Mundo)
Contenedor de todos los objetos físicos:

Almacena cuerpos (Bodies), restricciones (Constraints) y composites

Define propiedades globales como la gravedad (por defecto: { x: 0, y: 1 })

### 3. Bodies (Cuerpos)
Objetos físicos con masa, forma y propiedades:

Tipos comunes:

Bodies.rectangle(x, y, width, height, [options])

Bodies.circle(x, y, radius, [options])

Bodies.polygon(x, y, sides, radius, [options])

### 4. Constraint (Restricción)
Conexiones entre cuerpos o puntos fijos:

Usos comunes:

Cadenas/péndulos

Amarrar objetos a puntos fijos

Crear "joints" (articulaciones)

### 5. MouseConstraint (Control con Ratón)
Interacción usuario-mundo físico:

Permite arrastrar objetos

Detecta clics sobre cuerpos

Requiere renderizado para visualizar

