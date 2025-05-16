## Cómo configuraste p5.sound
Fue relativamente sencillo, importar las librerias necesarias, agregar el archivo mp3 a p5.js y ya el resto es codigo usando de base la documentación de las librerias.

## Puntos clave del codigo
```  js
fft.analyze();
let level = amplitude.getLevel();
let bass = fft.getEnergy("bass");
```
¿Por qué es importante?
Estas líneas son el corazón de la visualización. Analizan el audio en tiempo real:

fft.analyze() descompone el sonido en frecuencias.

level mide el volumen general (para mover el walker).

bass detecta los graves (para activar partículas y raíces).
```  js
function drawRootTrail(r) {
  trailCanvas.fill(r.color);
  trailCanvas.noStroke();
  drawShapeOnTrail(r.x, r.y, r.size, r.shape);
}
```
¿Por qué es importante?

Crea el efecto de "raíces" que dejan un rastro permanente en trailCanvas.

Usa el color actual del walker para mantener coherencia visual.

El rastro se acumula frame a frame (trailCanvas no se borra completamente).

```  js
function keyPressed() {
  if (keyCode === UP_ARROW) velocityMultiplier += 0.1;
  if (keyCode === DOWN_ARROW) velocityMultiplier -= 0.1;
}
```
¿Por qué es importante?

Permite ajustar en vivo la fuerza de salto de las partículas.

El valor velocityMultiplier se refleja inmediatamente en el texto de debug.

Hace la experiencia más dinámica y customizable.

## Codigo completo
```  js
// Variables globales para la canción, análisis de audio y elementos visuales
let song, amplitude, fft;
let walker; // Elemento principal que se mueve
let particles = []; // Partículas que reaccionan al bajo
let roots = []; // "Raíces" que se generan y dejan un rastro
let shapes = ['circle', 'square', 'triangle']; // Formas posibles para el walker y las raíces
let lastEventTime = 0; // Control de tiempo para eventos de bajo
let isAudioReady = false; // Flag para saber si el audio está listo
let trailCanvas; // Canvas separado para dibujar los rastros
let velocityMultiplier = 2; // Valor inicial
let showDebugInfo = true;

// Ruta al archivo de audio. Asegúrate de que este archivo esté disponible.
const audioFilePath = 'Jacob Tillberg - Mantra.mp3'; // O una URL si es externa

function preload() {
  // Carga la canción y establece isAudioReady a true cuando finalice
  song = loadSound(audioFilePath, () => {
    isAudioReady = true;
    console.log("Audio cargado exitosamente.");
  }, (err) => {
    console.error("Error cargando el audio:", err);
    // Opcional: Muestra un mensaje al usuario en el canvas si falla la carga
    // alert("No se pudo cargar el archivo de audio. Verifica la ruta o el archivo.");
  });
}

function setup() {
  createCanvas(800, 600);
  colorMode(HSB, 360, 100, 100, 100); // HSB con alpha

  // Canvas para el rastro (fondo oscuro)
  trailCanvas = createGraphics(width, height);
  trailCanvas.colorMode(HSB, 360, 100, 100, 100); // HSB con alpha para el trailCanvas también
  trailCanvas.background(0, 0, 10); // Fondo inicial oscuro para el rastro

  // Inicializa objetos de p5.sound
  amplitude = new p5.Amplitude();
  fft = new p5.FFT();

  // Configuración inicial del walker
  walker = {
    x: width / 2,
    y: height / 2,
    size: 30,
    shape: random(shapes),
    color: color(random(360), 80, 100), // Color HSB aleatorio
    xoff: random(1000), // Offset para Perlin noise (movimiento)
    yoff: random(1000)
  };

  // Creación de partículas iniciales
  for (let i = 0; i < 50; i++) {
    particles.push({
      x: random(width),
      y: height,
      velY: 0,
      size: random(5, 15),
      color: color(random(360), 60, 80, 80), // Color HSB con alpha
      sensitivity: random(0.02, 0.05), // Sensibilidad al bajo
      canJump: true,
      lastJumpTime: 0
    });
  }
}

function draw() {
  // Dibuja el trailCanvas (que contiene los rastros acumulados) en el canvas principal
  
  image(trailCanvas, 0, 0);
  
  

  // Fondo semi-transparente en el canvas principal para efecto de desvanecimiento global
  // Esto hace que tanto el trailCanvas como los elementos actuales se desvanezcan lentamente
  background(0, 0, 10, 20); // HSB: Negro/gris oscuro, muy transparente

  // Si el audio no está listo, no hacer nada más
  if (!isAudioReady) {
    fill(255);
    textAlign(CENTER, CENTER);
    text("Cargando audio...", width / 2, height / 2);
    return;
  }

  // Analiza el audio
  fft.analyze();
  let level = amplitude.getLevel(); // Nivel general de amplitud
  let bass = fft.getEnergy("bass");  // Energía en las frecuencias bajas (bajos)

   //Debug: Mostrar valores de audio (opcional)
   fill(255);
   noStroke();
   text("Bass: " + nf(bass, 0, 2), 20, 20);
  text("Level: " + nf(level, 0, 2), 20, 40);
  if (showDebugInfo) {
  fill(255);
  noStroke();
  text("Bass: " + nf(bass, 0, 2), 20, 20);
  text("Level: " + nf(level, 0, 2), 20, 40);
  text("Vel Multiplier: " + nf(velocityMultiplier, 0, 1) + " (↑/↓ to adjust)", 20, 60); // Nueva línea
}

  // ---- WALKER ----
  // Movimiento del walker usando Perlin noise, velocidad basada en el nivel de audio
  let maxSpeed = map(level, 0, 1, 0.005, 0.03) * 0.5;
  walker.xoff += maxSpeed;
  walker.yoff += maxSpeed;
  walker.x = noise(walker.xoff) * width;
  walker.y = noise(walker.yoff) * height;

  // Si hay un pico de bajo y ha pasado tiempo suficiente, cambia el walker y crea una raíz
  if (bass > 150 && millis() - lastEventTime > 1000) {
    walker.shape = random(shapes); // Cambia la forma del walker
    walker.color = color(random(360), 80, 100); // Cambia el color del walker
    createRoot(); // Crea una nueva raíz
    lastEventTime = millis(); // Actualiza el tiempo del último evento
  }

  drawWalker(); // Dibuja el walker en el canvas principal

  // ---- RAÍCES ----
  // Actualiza y dibuja cada raíz. Elimina las raíces que se han encogido demasiado.
  for (let i = roots.length - 1; i >= 0; i--) {
    let r = roots[i];
    updateRoot(r);    // Actualiza posición y tamaño de la raíz
    drawRootTrail(r); // Dibuja la raíz y su rastro
    
    if (r.size < 2) { // Si la raíz es muy pequeña, elimínala
      roots.splice(i, 1);
    }
  }

  // ---- PARTÍCULAS ----
  // Actualiza y dibuja las partículas que reaccionan al bajo
  for (let p of particles) {
    p.velY += 0.8; // Gravedad simulada
    // Si hay un pico de bajo fuerte y la partícula puede saltar
    if (bass > 242 && p.canJump && millis() - p.lastJumpTime > 50) {
      p.velY = -bass * p.sensitivity * velocityMultiplier; // Impulso hacia arriba
      p.canJump = false;
      p.lastJumpTime = millis();
    }
    p.y += p.velY; // Actualiza posición vertical

    // Rebote en el suelo
    if (p.y > height) {
      p.y = height;
      p.velY *= -0.2; // Pierde energía en el rebote
      p.canJump = true; // Puede volver a saltar
    }
    // Evita que se salga por arriba
    if (p.y < 0) {
      p.y = 0;
      p.velY = 0;
    }
    // Dibuja la partícula
    fill(p.color);
    noStroke();
    ellipse(p.x, p.y, p.size);
  }
}

function createRoot() {
  // Decide de qué lado de la pantalla aparecerá la raíz
  let side = floor(random(4));
  let x, y;

  if (side === 0) { x = random(width); y = -150; } // Arriba
  else if (side === 1) { x = width + 150; y = random(height); } // Derecha
  else if (side === 2) { x = random(width); y = height + 150; } // Abajo
  else { x = -150; y = random(height); } // Izquierda

  // Añade la nueva raíz al array de raíces
  roots.push({
    x: x, y: y,
    prevX: x, prevY: y, // Posición anterior (podría usarse para dibujar líneas de rastro)
    size: random(150, 250), // Tamaño inicial
    shape: walker.shape,    // Forma actual del walker
    color: walker.color,    // ***CAMBIO CLAVE: Almacena el color actual del walker***
    targetX: width / 2 + random(-100, 100), // Punto objetivo (centro con variación)
    targetY: height / 2 + random(-100, 100),
    speed: random(0.3, 1.0), // Velocidad de movimiento
    shrinkFactor: random(0.985, 0.995), // Factor de encogimiento
    opacity: 100 // Opacidad inicial (para el borde en el canvas principal)
  });
}

function updateRoot(r) {
  // Guarda la posición actual como la anterior
  r.prevX = r.x;
  r.prevY = r.y;

  // Calcula el vector de dirección hacia el punto objetivo
  let dx = r.targetX - r.x;
  let dy = r.targetY - r.y;
  let distance = sqrt(dx * dx + dy * dy);

  // Mueve la raíz hacia el objetivo si está lejos
  if (distance > 10) {
    let angle = atan2(dy, dx);
    r.x += cos(angle) * r.speed;
    r.y += sin(angle) * r.speed;
  }

  // Encoge la raíz y reduce su opacidad (para el borde)
  r.size *= r.shrinkFactor;
  r.opacity *= 0.995;
}

// Función para dibujar formas en el canvas principal
function drawShape(x, y, size, shape, currentCanvas = this) {
  currentCanvas.push();
  currentCanvas.translate(x, y);
  switch (shape) {
    case 'circle':
      currentCanvas.ellipse(0, 0, size);
      break;
    case 'square':
      currentCanvas.rectMode(CENTER);
      currentCanvas.rect(0, 0, size, size);
      break;
    case 'triangle':
      currentCanvas.triangle(
        0, -size / 2,
        -size / 2, size / 2,
        size / 2, size / 2
      );
      break;
  }
  currentCanvas.pop();
}

// Función para dibujar formas en el trailCanvas
// (Modificada para ser más genérica, aunque drawShape podría usarse pasándole trailCanvas)
function drawShapeOnTrail(x, y, size, shape) {
  trailCanvas.push();
  trailCanvas.translate(x, y);
  switch (shape) {
    case 'circle':
      trailCanvas.ellipse(0, 0, size);
      break;
    case 'square':
      trailCanvas.rectMode(CENTER);
      trailCanvas.rect(0, 0, size, size);
      break;
    case 'triangle':
      trailCanvas.triangle(
        0, -size / 2,
        -size / 2, size / 2,
        size / 2, size / 2
      );
      break;
  }
  trailCanvas.pop();
}


function drawRootTrail(r) {
  // Dibuja la forma de la raíz en el trailCanvas con su color almacenado
  // ***CAMBIO CLAVE: Usa r.color para el relleno en trailCanvas***
  trailCanvas.fill(r.color); // Usar el color específico de esta raíz
  trailCanvas.noStroke();
  // Dibuja la forma de la raíz en su posición actual en el trailCanvas
  // Esto acumula las formas en trailCanvas, creando el rastro.
  drawShapeOnTrail(r.x, r.y, r.size, r.shape);

  // Dibuja un borde blanco para la "cabeza" de la raíz en el canvas principal
  // Esto hace que la raíz actual sea visible sobre su propio rastro.
  noFill();
  stroke(0, 0, 100, r.opacity); // HSB: Blanco, con opacidad decreciente
  strokeWeight(2);
  drawShape(r.x, r.y, r.size, r.shape, this); // Dibuja en el canvas principal
}

function drawWalker() {
  // Dibuja el walker en el canvas principal
  fill(walker.color);
  noStroke();
  drawShape(walker.x, walker.y, walker.size * 2, walker.shape, this); // Dibuja en el canvas principal
}

function mousePressed() {
  // Permite pausar/reanudar la canción con un clic del mouse
  if (!isAudioReady) return; // No hacer nada si el audio no está listo

  if (song.isPlaying()) {
    song.pause();
  } else {
    // Intenta obtener el contexto de audio en la interacción del usuario si es necesario
    if (getAudioContext().state !== 'running') {
      getAudioContext().resume();
    }
    song.play();
  }
  
}

function keyPressed() {
  // Flecha arriba: aumentar multiplicador
  if (keyCode === UP_ARROW) {
    velocityMultiplier = min(velocityMultiplier + 0.1, 5); // Límite máximo de 5
    return false; // Previene comportamiento por defecto
  }
  // Flecha abajo: disminuir multiplicador
  if (keyCode === DOWN_ARROW) {
    velocityMultiplier = max(velocityMultiplier - 0.1, 0.1); // Límite mínimo de 0.1
    return false;
  }
  // Tecla 'D' para toggle debug info
  if (key === 'd' || key === 'D') {
    showDebugInfo = !showDebugInfo;
    return false;
   }
 } 







// Opcional: Manejo del redimensionamiento de la ventana
// function windowResized() {
//   resizeCanvas(windowWidth, windowHeight);
//   // Podrías necesitar recrear trailCanvas si el tamaño cambia drásticamente
//   // trailCanvas = createGraphics(width, height);
//   // trailCanvas.colorMode(HSB, 360, 100, 100, 100);
//   // trailCanvas.background(0, 0, 10);
// }

```
## Video
https://github.com/user-attachments/assets/124c0c13-2d93-4575-a499-3d5da26b14f1

https://editor.p5js.org/ElJuanes/full/xeVVn-xLi

