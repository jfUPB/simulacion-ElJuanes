## Practica un poco
https://editor.p5js.org/ElJuanes/full/q_goGTQtc
``` js
class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 4;
    this.angle = 0;  // Dirección del vehículo
    this.r = 16;
  }

  update() {
    // Movimiento con las teclas de flecha
    if (keyIsDown(LEFT_ARROW)) {
      this.acceleration = createVector(-0.1, 0);  // Acelera hacia la izquierda
    } else if (keyIsDown(RIGHT_ARROW)) {
      this.acceleration = createVector(0.1, 0);   // Acelera hacia la derecha
    } else {
      this.acceleration = createVector(0, 0);  // Sin aceleración cuando no se presionan las flechas
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
  }

  display() {
    // Calculamos la dirección de movimiento del vehículo
    let angle = this.velocity.heading();
    
    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle + HALF_PI); // Restamos HALF_PI para corregir la orientación
    // Dibuja un triángulo para representar el vehículo
    beginShape();
    vertex(0, -this.r); // parte superior del triángulo
    vertex(-this.r, this.r); // esquina inferior izquierda
    vertex(this.r, this.r); // esquina inferior derecha
    endShape(CLOSE);
    pop();
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}

```

![image](https://github.com/user-attachments/assets/8087a2b7-9acc-4077-bc3a-c0d90e5ec111)
