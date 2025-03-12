## Funciones sinusoides
### Editor Web
https://editor.p5js.org/ElJuanes/full/Al2vQunWc
### Codigo 

``` js
// Variables para la sinusoide
let amplitude = 100;  // Amplitud
let frequency = 1;    // Frecuencia (ciclos por segundo)
let phase = 0;        // Fase
let angularVelocity;  // Velocidad angular

// Deslizadores para modificar los parámetros
let amplitudeSlider, frequencySlider, phaseSlider;

function setup() {
  createCanvas(640, 480);
  
  // Deslizadores para los parámetros
  amplitudeSlider = createSlider(20, 200, 100);
  amplitudeSlider.position(20, height - 120);  // Posición más arriba
  
  frequencySlider = createSlider(0.1, 5, 1, 0.1);
  frequencySlider.position(20, height - 90);  // Posición más arriba
  
  phaseSlider = createSlider(0, TWO_PI, 0, 0.1);
  phaseSlider.position(20, height - 60);  // Posición más arriba
}

function draw() {
  background(255);

  // Actualizamos los parámetros con los valores de los sliders
  amplitude = amplitudeSlider.value();
  frequency = frequencySlider.value();
  phase = phaseSlider.value();
  
  // Calculamos la velocidad angular
  angularVelocity = 2 * PI * frequency;
  
  // Dibuja la onda sinusoide (sin usar translate)
  stroke(0);
  noFill();
  
  beginShape();
  for (let x = -width / 2; x < width / 2; x++) {
    // Calculamos el valor de la sinusoide en cada punto
    let y = amplitude * sin(angularVelocity * (x / width) + phase);
    vertex(x + width / 2, y + height / 2);  // Ajustar las posiciones para centrar la onda
  }
  endShape();
  
  // Dibuja los textos descriptivos en el centro del canvas
  fill(0);
  textSize(16);
  textAlign(CENTER, CENTER);  // Centrar el texto
  text("Amplitud", width / 2 - 120, height / 2 + 130);  // Texto de Amplitud
  text("Frecuencia", width / 2 - 120, height / 2 + 160);      // Texto de Frecuencia
  text("Fase", width / 2 - 120, height / 2 + 190);       // Texto de Fase
}

```

### Captura
![image](https://github.com/user-attachments/assets/adfb927d-13e4-432f-9406-94857d07b4e0)
