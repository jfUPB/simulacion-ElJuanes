## Ondas
### Enlace a la simulación en el editor de p5.js.
https://editor.p5js.org/ElJuanes/full/F8DLRafei
### Código de la simulación.
``` js
let angle = 0;
let angleVelocity = 0.05; // Velocidad de la onda
let amplitude = 100;
let frequency = 0.02; // Este valor controla cuántas ondas hay en la pantalla

function setup() {
  createCanvas(640, 240);
  background(255);

  stroke(0);
  strokeWeight(2);
  fill(127, 127);
}

function draw() {
  background(255);  

  for (let x = 0; x <= width; x += 24) {
    // 1) Calcula la posición y según la amplitud y el seno del ángulo.
    let y = amplitude * sin(angle + (x * frequency)); // Reduce la frecuencia para tener menos ondas

    // 2) Interpola el color entre rojo (abajo) y azul (arriba)
    let colorValue = map(y, -amplitude, amplitude, 0, 1); // Mapea el valor de y en el rango [0, 1]
    let c = lerpColor(color(255, 0, 0), color(0, 0, 255), colorValue); // Interpola entre rojo y azul

    // 3) Dibuja un círculo en la posición (x, y) con el color interpolado
    fill(c);
    circle(x, y + height / 2, 48);
  }

  // 4) Incrementa el ángulo para mover la ola.
  angle += angleVelocity;
}

``` 
### Captura de pantalla de la simulación.
![image](https://github.com/user-attachments/assets/42c7487e-f038-482d-9229-ef32b48b89d3)
![image](https://github.com/user-attachments/assets/48271bd1-0155-4928-970b-7a6d5afdee78)


