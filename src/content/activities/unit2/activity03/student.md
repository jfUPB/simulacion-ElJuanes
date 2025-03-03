## Experimenta
### ¿Qué resultado esperas obtener?
Luego de analizar el codigo se entiende que el objetivo principal es modificar las componentes del vector posicion 
dentro de la función playingVector. Luego, con el uso de console.log(), podrías intentar ver el valor de este vector después de ser modificado. Le agregue esto 
console.log(position); para ver la posición luego del cambio

### ¿Qué resultado obtuviste? 
``` js
n {isPInst: true, _fromRadians: ƒ bound (), _toRadians: ƒ bound (), x: 20, y: 30…}
isPInst: true
_fromRadians: ƒ bound () {}
_toRadians: ƒ bound () {}
x: 20
y: 30
z: 0
```
### Recuerda los conceptos...
#### Paso por Valor:
El paso por valor significa que cuando pasamos una variable a una función, se pasa una copia del valor de esa variable. Si la variable es modificada dentro de la función, el valor original no cambia fuera de la función.
``` js
function modifyNumber(n) {
  n = 10; 
  console.log("Dentro de la función:", n);
}

let number = 5;
modifyNumber(number);
console.log("Fuera de la función:", number);  

//SALIDA
Dentro de la función: 10
Fuera de la función: 5
```
#### Paso por Referencia:
El paso por referencia significa que cuando pasamos una variable que es un objeto o un array a una función, estamos pasando una referencia al objeto original.
 Si modificamos el objeto dentro de la función, los cambios se reflejan también fuera de la función, ya que estamos trabajando con la misma referencia al objeto.
``` js
function modifyObject(obj) {
  obj.name = "Juan";  
  console.log("Dentro de la función:", obj);
}

let person = { name: "Ana" };
modifyObject(person);
console.log("Fuera de la función:", person);  

//SALIDA
Dentro de la función: { name: "Juan" }
Fuera de la función: { name: "Juan" }

```

### ¿Qué tipo de paso se está realizando en el código?
El paso se realiza por referencia. Esto es porque position es un objeto de tipo p5.Vector, y cuando se pasa por ese objeto a la función playingVector(v), se está pasando una referencia al objeto original.

### ¿Qué aprendiste?
Los tipos primitivos se pasan por valor. Esto significa que cualquier cambio dentro de una función no afecta el valor fuera de la función.
Los objetos y arrays se pasan por referencia, lo que significa que los cambios dentro de la función pueden afectar al objeto original fuera de ella.
