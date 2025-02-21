## Paso por valor y paso por referencia
### Diferencia entre paso por valor y paso por referencia:
#### Paso por valor: 
Se crea una copia del valor original. Cualquier cambio realizado sobre la copia no afecta al valor original.

#### Paso por referencia:
 Se pasa una referencia al objeto original, por lo que cualquier cambio realizado sobre la referencia afectará al objeto original.

### En el fragmento de código:
let friction = this.velocity.copy();:

#### Paso por valor:
 Aquí, copy() crea una nueva copia de this.velocity. Esto significa que cualquier modificación de friction no afectará a this.velocity, ya que son dos objetos independientes.
let friction = this.velocity;:

#### Paso por referencia:
 En este caso, friction es solo una referencia a this.velocity. Si se cambia friction, se estará modificando directamente a this.velocity, ya que ambos apuntan al mismo objeto.
