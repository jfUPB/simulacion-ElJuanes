## Diferencias fundamentales:
### Flow Fields
Inteligencia en el entorno:
Los agentes se mueven siguiendo un campo vectorial (el flow field), que es como un mapa invisible de flechas que les indica en cada punto hacia dónde moverse.

Este campo puede estar diseñado o generado algorítmicamente (por ejemplo, apuntando hacia un objetivo).

Los agentes simplemente siguen estas direcciones locales, sin necesidad de considerar a otros agentes.

 Coordinación ocurre porque todos siguen el mismo mapa, no porque interactúen entre sí.

###  Flocking 
Cada agente se mueve de acuerdo con reglas locales basadas en la posición y velocidad de sus vecinos:
-Separación (evitar colisiones),

-Alineación (ajustar su dirección a la de los vecinos),

-Cohesión (moverse hacia el centro del grupo).

No hay un mapa externo ni una dirección predeterminada.

Coordinación emerge del comportamiento colectivo local, como en bandadas de aves o bancos de peces.

## Tipos de comportamiento emergente
###  Flow Fields: patrones suaves, dirigidos y espaciales
Más fácil o natural con Flow Fields:

Movimiento dirigido y suave a lo largo de trayectorias definidas.

Ideal para representar flujo de información, tráfico, o energía en un espacio.

Ejemplos:

Simulación de viento o humo: las partículas siguen curvas suaves y fluidas.

Rutas hacia un objetivo: como enjambres de drones que siguen caminos marcados hacia puntos de recolección o ataque.

Efectos visuales estilizados: como partículas que se arremolinan en torno a campos magnéticos o patrones artísticos animados.

### Flocking: patrones emergentes, dinámicos y sociales
Más fácil o natural:

Movimiento emergente que parece vivo, reactivo y social.

Ideal para representar comportamientos de grupo, adaptativos y de evasión/persecución.

Ejemplos:

Bandadas de pájaros o bancos de peces: con movimientos orgánicos y coordinados que cambian rápidamente.


Simulación de multitudes: personas moviéndose por un espacio sin chocar, adaptándose al grupo.

Comportamientos evasivos o de pastoreo: como presas huyendo de un depredador, manteniéndose juntas.

### Ventajas y desventajas
###  Flow Fields
Ventajas:

Control total del movimiento (dirección, forma, ritmo).

Ideal para efectos suaves y predecibles (humo, viento, trayectorias).

Escalable: cada agente solo necesita consultar el campo.

Desventajas:

Poco realismo social o grupal.

No responde bien a cambios dinámicos (agentes no se adaptan entre sí).

### Flocking
Ventajas:

Movimiento emergente y natural (perfecto para simular vida).

Adaptativo: los agentes reaccionan entre sí y al entorno.

Muy expresivo para multitudes o comportamientos sociales.

Desventajas:

Menos control directo: comportamiento difícil de predecir.

Más costoso computacionalmente (requiere cálculo de vecinos).

### ¿Cuál usar?
¿Quieres orden, dirección o trayectorias suaves? → Flow Field

¿Quieres vida, interacción o comportamiento grupal? → Flocking

## El agente autonomo 
Ambos sistemas muestran que un agente autónomo:

Toma decisiones localmente, según reglas simples.

No necesita un “jefe” o controlador global.

Contribuye a un comportamiento colectivo mayor, a partir de acciones individuales.

#### Característica             -   	Descripción.

Autonomía	Actúa por sí mismo, sin instrucciones específicas en cada momento.

Percepción local	Solo usa información del entorno cercano (vecinos o el campo).

Simplicidad	Sus reglas son simples, pero pueden generar dinámicas complejas.


Reactividad	Cambia su comportamiento en respuesta al entorno.

Coherencia	Su acción sigue una lógica (reglas del field o del grupo).

## Emergencia 
El comportamiento emergente aparece cuando se tiene orden, adaptabilidad o estructura compleja que no fue escrita línea por línea. Solo se tienen que definir reglas locales, y lo demás emerge.

