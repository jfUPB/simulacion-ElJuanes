## Experimentando con la aceleración
``` js
let blueBall, yellowBall, redBall;

function setup() {
  createCanvas(640, 240);
  
  // Crear las 3 bolas con diferentes tipos de aceleración
  blueBall = new Mover(color(0, 0, 255), "constant");
  yellowBall = new Mover(color(255, 255, 0), "random");
  redBall = new Mover(color(255, 0, 0), "towardsMouse");
}

function draw() {
  background(255);

  // Actualiza y dibuja las bolas
  blueBall.update();
  blueBall.checkEdges();
  blueBall.show();

  yellowBall.update();
  yellowBall.checkEdges();
  yellowBall.show();

  redBall.update();
  redBall.checkEdges();
  redBall.show();
}

class Mover {
  constructor(ballColor, accelerationType) {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.color = ballColor; // Color de la bola
    this.accelerationType = accelerationType; // Tipo de aceleración
  }

  update() {
    // Aceleración constante
    if (this.accelerationType === "constant") {
      this.acceleration = createVector(0.1, 0.1); // Aceleración constante
    }
    
    // Aceleración aleatoria
    else if (this.accelerationType === "random") {
      this.acceleration = createVector(random(-0.5, 0.5), random(-0.5, 0.5)); // Aceleración aleatoria
    }
    
    // Aceleración hacia el mouse
    else if (this.accelerationType === "towardsMouse") {
      let force = createVector(mouseX - this.position.x, mouseY - this.position.y);
      force.normalize();
      force.mult(0.05); // Aceleración hacia el mouse
      this.acceleration = force;
    }

    // Actualiza la velocidad y la posición
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(this.color); // Usar el color de la bola
    circle(this.position.x, this.position.y, 48);
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

https://editor.p5js.org/ElJuanes/sketches/t2i1btU5M

#### Aceleración constante 
proporciona un movimiento predecible y suave, ideal para simulaciones con movimiento continuo.
#### Aceleración aleatoria 
genera movimientos impredecibles y caóticos, lo que puede ser útil para efectos o simulaciones estocásticas.
#### Aceleración hacia el mouse
 crea un movimiento controlado y dirigido, lo que permite que el objeto se acerque a un punto específico (el mouse) de manera natural.
