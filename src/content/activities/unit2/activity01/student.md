## Analiza una aplicación interactiva
### ¿Cómo funciona la suma dos vectores?
Antes que hablar de la suma se nos menciona que la suma sigue las mismas reglas algebraicas que con los números reales. Resumiendolo en que el resultado es siempre el mismo sin importar el orden en el que los vectores
fueron agregados.
La función busca los componentes x de los dos vectores y los suma por separado. Así es como está escrito el método add() de la clase p5.Vector

### ¿Por qué esta línea position = position + velocity; no funciona?
position = position + velocity;
No funcionará a menos que el operador + esté sobrecargado o definido específicamente para los objetos con los que trabajas.

position.add(velocity);
Funciona porque el método add está explícitamente definido para manejar la adición de objetos y modificará el objeto position en consecuencia.
