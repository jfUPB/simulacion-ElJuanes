### Hagamos que todo se mueva
``` js
let t = 0; // Parámetro de interpolación
let dir = 1; // Dirección de la interpolación (1 para ir hacia adelante, -1 para ir hacia atrás)

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(255);

    let base = createVector(mouseX, mouseY); // La base de las flechas sigue al ratón

    let v1 = createVector(60, 0);  // Flecha roja
    let v2 = createVector(0, 60);  // Flecha azul

    // Escala de los vectores depende de la distancia desde la base del ratón
    let scaleFactor = dist(mouseX, mouseY, width / 2, height / 2) / 100;  // Factor de escala
    v1.mult(scaleFactor);
    v2.mult(scaleFactor);

    // Interpolación de posición (mueve la flecha morada)
    let v3 = p5.Vector.lerp(v1, v2, t);

    // Interpolación de color entre rojo y azul
    let lerpedColor = lerpColor(color('red'), color('blue'), t);

    // Dibuja las flechas
    drawArrow(base, v1, 'red');
    drawArrow(base, v2, 'blue');
    drawArrow(base, v3, lerpedColor); // Flecha morada con color interpolado

    // Dibuja la línea que conecta las puntas de las flechas roja y azul
    stroke(0); // Línea de color negro
    strokeWeight(2);
    line(v1.x + base.x, v1.y + base.y, v2.x + base.x, v2.y + base.y); // Conecta las puntas de las flechas

    // Actualiza el parámetro `t` para animar la flecha morada
    t += 0.01 * dir; // Incrementa o decrementa `t` según la dirección
    if (t >= 1 || t <= 0) {
        dir *= -1; // Invierte la dirección cuando `t` alcanza los límites
    }
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}

```

### Que hice?
El metedo usado más importante para poder realizar el cambio es mouseX y mouseY en P5.js, las cuales proporcionan las posiciones actuales del ratón en la ventana del canvas
