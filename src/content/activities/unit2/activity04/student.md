## p5.Vector
#### ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
Mag calcula la magnitud de un vector, mientras que magSq() Calcula la magnitud (longitud) del vector al cuadrado. 
magSq() es más eficiente, ya que no requiere el cálculo de la raíz cuadrada. Si solo es necesario comparar magnitudes, usar magSq() es una forma más rápida de hacerlo, ya que evita una operación costosa (la raíz cuadrada).
#### Normalize
Escala los componentes de un objeto p5.Vector para que su magnitud sea 1.
#### Dot
Calcula el producto punto entre 2 vectores.
#### Periodista pregunton:
El producto cruz de dos vectores genera un vector perpendicular al plano formado por ellos. 
Su magnitud es igual al área del paralelogramo formado por los vectores y depende del ángulo entre ellos. La dirección se determina por la regla de la mano derecha. 
Si los vectores son paralelos, el resultado es el vector cero.

#### Dist()
Calcula la distancia entre dos puntos representados por vectores.

#### normalize() y limit()
normalize(): Escala los componentes de un objeto p5.Vector para que su magnitud sea 1.
La versión estática de normalize(), como en p5.Vector.normalize(v), devuelve un nuevo objeto p5.Vector y no modifica el original.
limit(): Limita la magnitud de un vector a un valor máximo.
``` js
limit(max,v, max, [target])
```
La versión estática de limit(), como en p5.Vector.limit(v, 5), devuelve un nuevo objeto p5.Vector y no modifica el original.
