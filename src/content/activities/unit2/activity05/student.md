### Interpolamos?
#### Codigo
``` js
let t = 0; // Parámetro de interpolación
let dir = 1; // Dirección de la interpolación (1 para ir hacia adelante, -1 para ir hacia atrás)

function setup() {
    createCanvas(200, 200);
}

function draw() {
    background(400);

    let v0 = createVector(100, 100);
    let v1 = createVector(60, 0); // Flecha roja
    let v2 = createVector(0, 60); // Flecha azul

    // Interpolación de posición (mueve la flecha morada)
    let v3 = p5.Vector.lerp(v1, v2, t);

    // Interpolación de color entre rojo y azul
    let lerpedColor = lerpColor(color('red'), color('blue'), t);

    // Dibuja las flechas
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, lerpedColor); // Flecha morada con color interpolado

    // Dibuja la línea que conecta las puntas de las flechas roja y azul
    stroke(0); // Línea de color negro
    strokeWeight(2);
    line(v1.x + v0.x, v1.y + v0.y, v2.x + v0.x, v2.y + v0.y); // Conecta las puntas de las flechas

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

#### ¿Cómo funciona lerp() y lerpColor().
La función lerp() realiza una interpolación lineal entre dos valores. Recibe tres parámetros: el valor inicial, el valor final y un valor t entre 0 y 1 que determina la fracción de interpolación entre esos dos valores. La fórmula es:
lerp(start, end, t) = start + (end - start) * t.

lerpColor() interpola entre dos colores. Toma los colores inicial y final, junto con un valor t que controla la interpolación (0 para el color inicial y 1 para el color final). La fórmula es similar a la de lerp(), pero aplicada a los componentes RGB de los colores.
#### ¿Cómo se dibuja una flecha usando drawArrow()?
La función drawArrow() en P5.js se usa para dibujar una flecha. Se pasa como parámetros las coordenadas del punto inicial y final de la flecha, por ejemplo:
drawArrow(x1, y1, x2, y2) dibuja una flecha desde (x1, y1) hasta (x2, y2), agregando una cabeza en el extremo.
