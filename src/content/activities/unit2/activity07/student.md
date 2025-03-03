### Motion 101 
Es un concepto básico para programar movimiento en gráficos computacionales, particularmente en el contexto de simulaciones de objetos en movimiento. 
La idea central es usar vectores para describir dos aspectos fundamentales de un objeto en movimiento:

-Posición: El lugar donde se encuentra el objeto en el espacio (generalmente en 2D o 3D).
- Velocidad: La dirección y rapidez con la que el objeto se mueve. Esto se describe mediante un vector de velocidad, que indica tanto la dirección en que se mueve el objeto como la rapidez de ese movimiento.

La idea principal de Motion 101:
Sumar la velocidad a la posición: Cada vez que se actualiza el estado del objeto (en cada fotograma), se añade el vector de velocidad a la posición del objeto.
Esto hace que el objeto se desplace a lo largo de su trayectoria.

Dibujar el objeto en su nueva posición: Después de actualizar la posición, el objeto se dibuja en el lienzo en su nueva ubicación.

Ejemplo de Motion 101:
Por ejemplo un círculo que se mueve en una pantalla. La posición del círculo se actualiza continuamente mediante la adición de su velocidad a su posición actual.
