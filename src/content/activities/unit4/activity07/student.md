## Repasa conceptos de las unidades anteriores

### Editor
https://editor.p5js.org/ElJuanes/full/UaiuxFFKr

### Codigo
#### oscillator.js
``` js
class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));  // Velocidad angular inicial
    this.amplitude = createVector(
      random(20, width / 2),
      random(20, height / 2)
    );
    this.noiseOffset = createVector(random(1000), random(1000));  // Usamos un offset único para cada oscilador
    this.color = color(random(255), random(255), random(255));  // Color inicial aleatorio
  }

  update() {
    // Actualiza el ángulo con ruido perlin
    let noiseX = noise(this.noiseOffset.x);
    let noiseY = noise(this.noiseOffset.y);

    this.angleVelocity.x = map(noiseX, 0, 1, -0.05, 0.05);  // Mover de manera más suave
    this.angleVelocity.y = map(noiseY, 0, 1, -0.05, 0.05);  // Mover de manera más suave

    this.angle.add(this.angleVelocity);  // Aplicamos el cambio al ángulo
    this.noiseOffset.add(0.01, 0.01);  // Movemos el offset para continuar el ruido Perlin
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);
    stroke(0);
    strokeWeight(2);
    fill(this.color);  // Usamos el color asignado al oscilador
    line(0, 0, x, y);
    circle(x, y, 32);
    pop();
  }

  // Método para alterar el comportamiento cuando se presiona una tecla
  randomizeMovement() {
    this.noiseOffset = createVector(random(1000), random(1000));  // Nuevo offset aleatorio
    this.color = color(random(255), random(255), random(255));  // Asignamos un nuevo color aleatorio
  }
}

```

#### Sketch.js
``` js
// Arreglo de osciladores
let oscillators = [];

function setup() {
  createCanvas(640, 240);
  // Inicializar todos los objetos osciladores
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator());
  }
}

function draw() {
  background(255);

  // Ejecutamos todos los osciladores
  for (let i = 0; i < oscillators.length; i++) {
    oscillators[i].update();  // Actualizamos el movimiento
    oscillators[i].show();    // Mostramos el oscilador
  }
}

// Función que se activa cuando se presiona una tecla
function keyPressed() {
  // Cuando se presiona cualquier tecla, aleatorizamos el movimiento y el color de todos los osciladores
  for (let i = 0; i < oscillators.length; i++) {
    oscillators[i].randomizeMovement();  // Cambiar el comportamiento y color de cada oscilador
  }
}

``` 
![image](https://github.com/user-attachments/assets/ae4a6b08-9ec0-4185-b162-dc3ecba20518)
