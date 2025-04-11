## La fuerza neta debe ser acumulativa
### ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
Es necesario multiplicar la aceleración por cero en cada frame para resetearla, ya que solo queremos que la aceleración acumulada en ese frame influya en la velocidad y 
posición del objeto. Si no lo hiciéramos, la aceleración seguiría acumulándose de frames anteriores, lo que causaría movimientos incorrectos.
### ¿Por qué se multiplica por cero justo al final de update()?
Se multiplica por cero justo al final de update() porque ya hemos utilizado la aceleración para actualizar la velocidad y la posición. De esta forma, evitamos que 
las fuerzas de frames pasados sigan afectando el objeto.
