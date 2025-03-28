## Resortes
### Enlace a la simulación en el editor de p5.js.
https://editor.p5js.org/ElJuanes/full/Xu1iW9IGb
### Código de la simulación.
``` js
let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 240);
  spring1 = new Spring(width / 2, 10, 100);
  spring2 = new Spring(width / 2, 100, 100);
  bob1 = new Bob(width / 2, 100);
  bob2 = new Bob(width / 2, 200);
}

function draw() {
  background(255);
  let gravity = createVector(0, 2);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);
  bob1.update();
  bob2.update();
  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);
  spring1.connect(bob1);
  spring2.anchor = bob1.position.copy();
  spring2.connect(bob2);
  spring1.constrainLength(bob1, 30, 200);
  spring2.constrainLength(bob2, 30, 200);
  spring1.showLine(bob1);
  spring2.showLine(bob2);
  bob1.show();
  bob2.show();
  spring1.show();
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
### Captura de pantalla de la simulación.
![image](https://github.com/user-attachments/assets/a510231b-7b1d-453c-a63f-2d779a2c1917)

