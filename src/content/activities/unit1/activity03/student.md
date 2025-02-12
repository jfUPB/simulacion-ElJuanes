## Describe el experimento que vas a realizar.
Despues de analizar el codigo unos minutos me planteo hacer una variable en la cantidad de desplazamiento que se realiza en cada frame, por ejemplo hacer que el rango aumente a -2 a 2  o de -3 a 3
## ¿Qué pregunta quieres responder con este experimento?
Que tanto puede variar el generador de caminata al tener unos valores más exagerados 
## ¿Qué resultados esperas obtener? 
Con suerte la figura dibujada sera más distinta probablemente más erratica y con suerte creara en algun momento alguna figura más interesante.
## ¿Qué resultados obtuviste?
Con este codido 
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
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

    step() {
    //{!2} Any floating-point number from –1 to 1
    let xstep = random(-3, 3);
    let ystep = random(-3, 3);
    this.x += xstep;
    this.y += ystep;
  }
}

```

## ¿Qué aprendiste de este experimento?
Como lo habia pensado al aftectar el rango de movimiento se genera algo mucho más exagerado con unas formas mucho más grandes, menos definidas y en menor tiempo.

<img src="/1-3.png" style="height: 80%; width:80%;" />

