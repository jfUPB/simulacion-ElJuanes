## Repasa
### Ejercicio 1.1
Toma uno de los ejemplos de caminante del capítulo 0 y conviértelo para que utilice vectores.
``` js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2); 
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    const step = createVector(0, 0); 
    
    
    if (choice == 0) {
      step.x = 1; 
    } else if (choice == 1) {
      step.x = -1; 
    } else if (choice == 2) {
      step.y = 1;
    } else {
      step.y = -1; 
    }
    
    this.position.add(step);
  }
}

```
