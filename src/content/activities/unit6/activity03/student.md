## Explica las Reglas (según el código):

### Separación (separate(boids)):
Objetivo: Evitar que el boid se acerque demasiado a otros boids. Mantener una distancia mínima.
Cálculo: Revisa todos los otros boids. Si alguno está dentro de una distancia desiredSeparation (25 píxeles en el código), 
calcula un vector que apunta en dirección contraria a ese vecino cercano. La fuerza de este vector es mayor cuanto más cerca está 
el vecino (diff.div(d)). Suma todos estos vectores de repulsión de los vecinos cercanos, calcula un promedio, y convierte este 
vector resultante en una fuerza de dirección (steer) limitada por maxforce, que empuja al boid lejos de la concentración de vecinos cercanos.
### Alineación (align(boids)):
Objetivo: Que el boid ajuste su dirección de vuelo para coincidir con la dirección promedio de sus vecinos.
Cálculo: Busca todos los boids vecinos dentro de una distancia neighborDistance (50 píxeles). Calcula el vector velocidad promedio de todos estos 
vecinos encontrados. Este vector promedio representa la dirección general del grupo. Luego, calcula la fuerza de dirección (steer) necesaria 
para que el boid ajuste su propia velocidad actual hacia esa velocidad promedio, limitada por maxforce.
### Cohesión (cohere(boids)):
Objetivo: Que el boid se mueva hacia el centro de masa (la posición promedio) de sus vecinos. Mantener el grupo unido.
Cálculo: Busca todos los boids vecinos dentro de una distancia neighborDistance (50 píxeles). Calcula la posición promedio (sum) de todos
 estos vecinos. Una vez que tiene esta posición central, utiliza la función seek(sum) para calcular la fuerza de dirección (steer) necesaria
 para moverse hacia esa posición promedio, limitada por maxforce.
### Identifica parámetros clave:

#### Radio/Distancia de Percepción:
Para Separación: desiredSeparation = 25 (dentro de separate()). Define qué tan cerca debe estar otro boid para ser repelido.
Para Alineación y Cohesión: neighborDistance = 50 (dentro de align() y cohere()). Define el radio para considerar a otros boids como "vecinos" para
calcular la dirección y el centro promedio.
Pesos/Multiplicadores de Reglas: Se aplican en la función flock():
Peso de Separación: 1.5 (sep.mult(1.5);)
Peso de Alineación: 1.0 (ali.mult(1.0);)
Peso de Cohesión: 1.0 (coh.mult(1.0);) Estos valores determinan cuánta influencia tiene cada regla en el comportamiento final. La separación tiene
un peso mayor en este código.
Velocidad Máxima (maxspeed): Definida en el constructor: this.maxspeed = 3. Limita la velocidad total del boid.
Fuerza Máxima (maxforce): Definida en el constructor: this.maxforce = 0.05. Limita cuánto puede cambiar la velocidad del boid en cada fotograma
(la magnitud de la fuerza de dirección total).

##  Experimenta con modificaciones:
Opción: Cambiar drásticamente el peso de una de las reglas.
Modificación: En la función flock(boids), cambia el peso de la cohesión a cero. Modifica la línea coh.mult(1.0); a coh.mult(0.0);.
Ejecuta y describe el efecto observado: Al eliminar la fuerza de cohesión (coh.mult(0.0);), se observa que los boids ya no sienten atracción 
hacia el centro del grupo. Aunque todavía intentan alinearse y separarse, la falta de cohesión hace que el enjambre tienda a dispersarse y 
separarse en grupos más pequeños o individuos aislados con el tiempo. Pierden la tendencia a agruparse fuertemente. Si un grupo se separa por
alguna razón (ej: al esquivar un obstáculo o por condiciones iniciales), no tendrá la fuerza necesaria para volver a unirse fácilmente. 
El comportamiento colectivo pierde su característica de "mantenerse juntos".
