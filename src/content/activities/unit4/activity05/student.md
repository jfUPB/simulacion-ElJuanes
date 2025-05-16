## Coordenadas polares
### ¿Cuál es la relación entre r y theta con las posiciones x y y? 
En este código, r es la distancia desde el origen y theta es el ángulo en radianes. Las coordenadas x y y se calculan con las fórmulas:

x = r * cos(theta)
y = r * sin(theta)
Esto convierte las coordenadas polares a cartesianas, permitiendo que el punto se mueva en un círculo. A medida que theta aumenta, el punto rota alrededor del origen (el centro de la pantalla).

### Primer cambio
El código modificado usa p5.Vector.fromAngle(theta), que crea un vector unitario (de magnitud 1), lo que hace que el círculo se dibuje en una circunferencia de radio 1, no en el radio r esperado.
Por lo que la bola generada no tiene un movimiento deseado.

### Segundo cambio 
p5.Vector.fromAngle(theta, r) crea un vector con un ángulo theta y una magnitud r, lo cual es exactamente lo que necesitamos. La función fromAngle() tiene un segundo parámetro que establece la 
magnitud del vector, y al pasar r, el vector tendrá la longitud del radio deseado.
Resultado: El círculo se moverá correctamente a lo largo de un círculo con radio r desde el centro de la pantalla, como en la versión original con coordenadas polares.
