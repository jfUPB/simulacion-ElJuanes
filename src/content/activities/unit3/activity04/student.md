### Marco Motion 101
En la unidad anterior, el cálculo de la aceleración se basaba en aplicar una fórmula derivada de las leyes de movimiento de Newton, específicamente la segunda ley de Newton, que establece que la aceleración de un 
objeto es directamente proporcional a la fuerza neta aplicada sobre él e inversamente proporcional a su masa. Esta ley se puede expresar como:
F=ma
Donde:
F es la fuerza aplicada,
m es la masa del objeto,
a es la aceleración.
Ejemplos de código de la unidad anterior:
En la unidad anterior, calculábamos la aceleración utilizando algoritmos que tenían en cuenta las fuerzas aplicadas, como la gravedad, la fricción o cualquier otra fuerza que queríamos simular.
El cálculo de la aceleración se hacía como una modificación de la velocidad en función de la fuerza aplicada. Por ejemplo:
``` js 
// Ejemplo con gravedad
let acceleration;
let velocity = createVector(0, 0); // velocidad inicial
let gravity = createVector(0, 0.1); // fuerza de gravedad

function update() {
  acceleration = gravity; // la aceleración es la gravedad
  velocity.add(acceleration); // velocidad aumentada por la aceleración
  position.add(velocity); // la posición se actualiza con la nueva velocidad
}

```
En este ejemplo, la aceleración es simplemente la gravedad. Al sumarla a la velocidad, estamos calculando cómo cambia la velocidad del objeto a lo largo del tiempo bajo la influencia de la gravedad.

Otro ejemplo, si quisiéramos calcular la aceleración en función de una fuerza externa (por ejemplo, una fuerza de empuje), podríamos usar algo similar a esto:

``` js
let mass = 1; // masa del objeto
let force = createVector(0.1, 0); // fuerza aplicada
let acceleration;

function update() {
  acceleration = force.div(mass); // a = F / m (fuerza sobre masa)
  velocity.add(acceleration);
  position.add(velocity);
}

```

Entonces, ¿qué tiene que ver esto con las leyes de movimiento de Newton?
En este contexto, el cálculo de la aceleración está directamente relacionado con la segunda ley de Newton, que nos indica cómo se moverá un objeto en respuesta a las fuerzas que se le apliquen. 
La aceleración es el cambio de velocidad a lo largo del tiempo y, en los ejemplos anteriores, las fuerzas aplicadas (como la gravedad o el empuje) determinan cómo cambia la velocidad del objeto en función de su masa.

Si la fuerza sobre el objeto es mayor, la aceleración también lo será, por lo que el objeto acelerará más rápidamente.
Si el objeto es más pesado (tiene más masa), necesitará más fuerza para producir la misma aceleración.
