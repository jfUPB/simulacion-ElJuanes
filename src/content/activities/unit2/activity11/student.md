## Materialización
``` js
let objetos = [];  // Lista para almacenar los objetos
let fotos = [];    // Lista para almacenar las "fotos" (figuras estáticas)

function setup() {
  createCanvas(600, 600);
  
  // Crear algunos círculos y cubos con velocidad aleatoria
  for (let i = 0; i < 5; i++) {
    let tipo = random(1) > 0.5 ? "circle" : "square";
    let obj = new Objeto(random(width), random(height), tipo);
    objetos.push(obj);
  }
}

function draw() {
  background(255);
  
  // Dibujar las fotos (figuras estáticas)
  for (let foto of fotos) {
    foto.show();
  }
  
  // Actualizar y dibujar todos los objetos dinámicos
  for (let obj of objetos) {
    obj.update();
    obj.show();
  }
}

function mousePressed() {
  // Al hacer clic en el canvas, crear una "foto" de los objetos en sus posiciones actuales
  for (let obj of objetos) {
    // Crear una "foto" estática antes de que el objeto cambie de color
    let foto = new Foto(obj.x, obj.y, obj.tipo, obj.color, obj.size);
    fotos.push(foto);
  }
  
  // Hacer que todos los objetos cambien de color y registren la posición del clic
  for (let obj of objetos) {
    obj.cambiarColor();
    obj.registrarPosicion(mouseX, mouseY);
    obj.reducirVelocidad();
  }
}

// Clase para los objetos (círculos o cubos)
class Objeto {
  constructor(x, y, tipo) {
    this.x = x;
    this.y = y;
    this.velX = random(-3, 3);
    this.velY = random(-3, 3);
    this.acelX = 0.01;
    this.acelY = 0.01;
    this.tipo = tipo;
    this.color = color(0, 0, 255); // Color inicial (azul)
    this.historialPosiciones = [];  // Para almacenar las posiciones donde se hace clic
    this.size = 50;  // Tamaño inicial del objeto
  }
  
  update() {
  // Aplicar aceleración a la velocidad
  this.velX += this.acelX;
  this.velY += this.acelY;

  // Limitar la velocidad para que no se haga demasiado rápida
  this.velX = constrain(this.velX, -5, 5); // Limitar velocidad en el eje X
  this.velY = constrain(this.velY, -5, 5); // Limitar velocidad en el eje Y

  // Actualizar la posición
  this.x += this.velX;
  this.y += this.velY;

  // Rebotar en los bordes de la pantalla con un cambio de dirección más suave
  if (this.x < 0) {
    this.velX *= -1;  // Invertir dirección en X
    this.x = 0;  // Asegurarse de que el objeto no quede fuera de la pantalla
  } else if (this.x > width) {
    this.velX *= -1;  // Invertir dirección en X
    this.x = width;  // Asegurarse de que el objeto no quede fuera de la pantalla
  }

  if (this.y < 0) {
    this.velY *= -1;  // Invertir dirección en Y
    this.y = 0;  // Asegurarse de que el objeto no quede fuera de la pantalla
  } else if (this.y > height) {
    this.velY *= -1;  // Invertir dirección en Y
    this.y = height;  // Asegurarse de que el objeto no quede fuera de la pantalla
  }
}

  
  show() {
    // Dibujar el objeto (círculo o cuadrado)
    fill(this.color);
    if (this.tipo === "circle") {
      ellipse(this.x, this.y, this.size, this.size);
    } else {
      rect(this.x - this.size / 2, this.y - this.size / 2, this.size, this.size);
    }
    
    // Mostrar el historial de posiciones donde se hizo clic
    for (let pos of this.historialPosiciones) {
      fill(pos.color);
      // Dibujar el punto con la mitad del tamaño del objeto original
      if (this.tipo === "circle") {
        ellipse(pos.x, pos.y, this.size / 2, this.size / 2);
      } else {
        rect(pos.x - this.size / 4, pos.y - this.size / 4, this.size / 2, this.size / 2);
      }
    }
  }
  
  isClicked(px, py) {
    // Verificar si se hizo clic dentro del área del objeto
    let distancia;
    if (this.tipo === "circle") {
      distancia = dist(px, py, this.x, this.y);
      return distancia < this.size / 2;  // Radio del círculo
    } else {
      return px > this.x - this.size / 2 && px < this.x + this.size / 2 && py > this.y - this.size / 2 && py < this.y + this.size / 2;
    }
  }
  
  cambiarColor() {
    // Cambiar el color del objeto a un color aleatorio
    this.color = color(random(255), random(255), random(255));
  }
  
  registrarPosicion(px, py) {
    // Registrar la posición donde se hizo clic
    this.historialPosiciones.push({x: px, y: py, color: this.color});
  }
  
  reducirVelocidad() {
    // Reducir la velocidad en un 20% al hacer clic, pero ahora de forma escalable
    this.velX *= 10.1;
    this.velY *= 10.1;
  }
}

// Clase para la "foto" de un objeto (figura estática)
class Foto {
  constructor(x, y, tipo, color, size) {
    this.x = x;
    this.y = y;
    this.tipo = tipo;
    this.color = color;
    this.size = size;
  }
  
  show() {
    // Dibujar la foto (figura estática) en la posición donde se hizo clic
    fill(this.color);
    if (this.tipo === "circle") {
      ellipse(this.x, this.y, this.size, this.size);
    } else {
      rect(this.x - this.size / 2, this.y - this.size / 2, this.size, this.size);
    }
  }
}

```
https://editor.p5js.org/ElJuanes/full/mhnmeTFqo

### Imagen
![image](https://github.com/user-attachments/assets/0f1d7837-700b-4000-86fe-8c2a64e222d9)

