## Ejecuta el ejemplo: 
Listo y analizado
## Identifica las tres reglas y explicalas:
### Separación:
Objetivo: No chocar. Evitar amontonarse.
Cómo: Calcula una fuerza para alejarse de los vecinos que están demasiado cerca.
### Alineación:
Objetivo: Moverse en la misma dirección que el grupo.
Cómo: Calcula la dirección promedio de los vecinos e intenta igualarla.
### Cohesión:
Objetivo: Mantenerse cerca del grupo. Ir hacia el centro.
Cómo: Calcula el centro promedio de los vecinos e intenta moverse hacia él.

## Identifica parámetros clave:

### Resolución del campo de flujo:
 Se define en sketch.js al crear el objeto FlowField: flowfield = new FlowField(20);. Aquí, la resolución es 20. Este valor se pasa al constructor de FlowField y
 se almacena en this.resolution. Determina el tamaño (en píxeles) de cada celda de la cuadrícula.

### Velocidad máxima (maxspeed):
Se define para cada vehículo en sketch.js al crearlos: new Vehicle(..., random(2, 5), ...). Se pasa al constructor de Vehicle y se almacena en this.maxspeed. Limita
la magnitud del vector de velocidad del agente.
### Fuerza máxima (maxforce):
Se define para cada vehículo en sketch.js al crearlos: new Vehicle(..., random(0.1, 0.5)). Se pasa al constructor de Vehicle y se almacena en this.maxforce. Limita la
magnitud de la fuerza de dirección (steer) que se puede aplicar en cada fotograma.

## Experimenta con modificaciones:

## Opción: Modificar sustancialmente la resolución del campo de flujo.
Modificación: Cambia la línea flowfield = new FlowField(20) en sketch.js   

Prueba 1 (Resolución más gruesa): Cámbiarla a flowfield = new FlowField(80);

Prueba 2 (Resolución más fina): Cámbiarla a flowfield = new FlowField(5);

## Ejecuta y describe el efecto:
Con resolución gruesa (ej: 80): Las celdas de la cuadrícula son muy grandes. Los agentes dentro de la misma celda grande seguirán exactamente el mismo vector de dirección. 
El movimiento general parecerá más "en bloque" o cuantizado. Los agentes tardarán más en reaccionar a los cambios curvos del campo porque el vector que siguen no cambia hasta que
cruzan a una celda diferente y grande. El campo visualizado (si debug es true) mostrará menos vectores, más espaciados.
Con resolución fina (ej: 5): Las celdas son muy pequeñas. Los agentes reaccionarán a cambios mucho más sutiles y locales en el campo de flujo. Su movimiento parecerá más suave
 y orgánico, siguiendo las curvas del ruido Perlin con mayor fidelidad. El campo visualizado (si debug es true) mostrará muchos vectores pequeños y densos, lo que podría ralentizar 
un poco la ejecución si hay muchos vectores que dibujar.
