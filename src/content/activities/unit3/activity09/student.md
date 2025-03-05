## Creando fuerzas en tu mundo de pixeles
### ¿Qué pasos hay que seguir?
#### 1.Comprender el concepto de fuerza
Temás como la fricción y resistencia del entorno los cuales son factores que le agregan más realismo y factores más realistas a nuestras simulaciónes, ya se al usar la fricción la cual se presenta cuando
2 superficies estan en contacto la cual es una fuerza disipativa o cuando se tiene resistencia del entorno como viento o un liquido en el cual la velocidad o impulso se ven reducidos al entar en contacto.
#### 2.Descomponer la fórmula de la fuerza en dos partes:
  - ¿Cómo se calcula la dirección de la fuerza?
  -¿Cómo se calcula la magnitud de la fuerza?

``` js
let distance = force.mag();
//La longitud (magnitud) de ese vector es la distancia entre los dos objetos.

let magnitude = (G * mass1 * mass2) / (distance * distance);
//Utiliza la fórmula de la gravedad para calcular la intensidad de la fuerza.
```
#### 3. Traducir esa fórmula en código p5.js que calcule un vector para pasarlo a través del método applyForce() de un objeto Mover.

``` js
function draw() {
  background(255);

  let force = attractor.attract(mover);
  mover.applyForce(force);
//Cálcula la fuerza de atracción y la aplica

  mover.update();

  attractor.show();
  mover.show();
}
```
