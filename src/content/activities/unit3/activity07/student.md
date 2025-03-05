## En mi mundo los pixeles si tienen masa
### Problema:
 El principal problema es que el vector force se pasa por referencia, y al modificarlo dentro de applyForce(), se afecta el vector original. Esto es un problema si se desean 
mantener las fuerzas originales sin alterarlas.

### Solución: 
Debemos hacer una copia del vector force antes de modificarlo, para evitar que se modifique el vector original. Esto se logra utilizando el método .copy() para crear una copia
del vector antes de realizar la operación.

### Implementación en p5.js:
``` js

javascript
Copiar
applyForce(force) {
    let f = force.copy();  // Crear una copia del vector
    f.div(10);  // Dividir por la masa
    this.acceleration.add(f);  // Sumar la aceleración
}
```

### Explicación: 
Al hacer una copia de force, no alteramos el vector original y solo modificamos la copia para que la aceleración se acumule correctamente sin afectar las fuerzas aplicadas.
