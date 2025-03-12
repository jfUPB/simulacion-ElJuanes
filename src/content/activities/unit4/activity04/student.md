## Relación con el marco motion 101
###  ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas?
Para agregar fuerzas acumulativas, debes acumular las fuerzas aplicadas a cada mover en cada cuadro. En el código actual, la fuerza se aplica en cada paso del draw(), pero se aplica directamente sin considerar fuerzas anteriores. Para implementar fuerzas acumulativas, se debe modificar el código en el Mover para que la aceleración no se reinicie en cada iteración. En lugar de:

``` js

this.acceleration.mult(0);
```

Se puede acumular las fuerzas a lo largo de los cuadros sumando las aceleraciones de cada paso, manteniendo el valor de this.acceleration para la siguiente iteración, en lugar de resetearla.
### Identifica dónde está el Attractor en la simulación. Cambia el color de este.
El Attractor está definido en el código en la clase Attractor. En la función display() de la clase, el color de relleno del Attractor se define dependiendo de las condiciones this.dragging y this.rollover.

Para cambiar el color del Attractor, puedes modificar la línea:
``` js
fill(175, 200);

cambiarlo a 

fill(255, 0, 0); //Codigo para colro rojo
``` 


###  Observa que el Attractor tiene dos atributos this.dragging y this.rollover
Para modificar el código y permitir que el Attractor se mueva con el mouse y cambie de color cuando el mouse está sobre él, necesitas agregar dos cosas:

Detectar si el mouse está sobre el Attractor (esto cambiaría el valor de this.rollover).
Detectar si el mouse está arrastrando el Attractor (esto cambiaría el valor de this.dragging y movería el Attractor con el mouse).
Se pueden hacer estas modificaciones en el sketch.js agregando funciones como mousePressed(), mouseReleased(), y mouseMoved().
``` js
function mousePressed() {
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);
  if (d < attractor.mass) {
    attractor.dragging = true;
  }
}

function mouseReleased() {
  attractor.dragging = false;
}

function mouseMoved() {
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);
  if (d < attractor.mass) {
    attractor.rollover = true;
  } else {
    attractor.rollover = false;
  }
}

function draw() {
  background(255);

  // Move the attractor if it's being dragged
  if (attractor.dragging) {
    attractor.position.set(mouseX, mouseY);
  }

  attractor.display();

  for (let i = 0; i < movers.length; i++) {
    let force = attractor.attract(movers[i]);
    movers[i].applyForce(force);

    movers[i].update();
    movers[i].show();
  }
}

```

mousePressed() detecta si el mouse está presionando sobre el Attractor y cambia dragging a true.
mouseReleased() se asegura de que el Attractor deje de moverse cuando se suelta el mouse.
mouseMoved() detecta si el mouse está sobre el Attractor y cambia rollover a true o false, dependiendo de si el mouse está dentro del área del Attractor.
