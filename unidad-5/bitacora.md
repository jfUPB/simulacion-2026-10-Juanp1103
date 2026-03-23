# Unidad 5
## Bitácora de proceso de aprendizaje

### Actividad 1

#### Capa de comportamiento
1. Cada partícula tiene las siguientes propiedades: `this.position`, `this.velocity`, `this.acceleration`, `this.lifespan`.

Estado físico (movimiento):

- position: posición en pantalla.
- velocity: velocidad de movimiento.
- acceleration: fuerzas aplicadas.

Estado vital (vida):
- lifespan: tiempo de vida de la partícula.

2. La partícula muere cuando: `this.lifespan < 0.0`. Es una muerte gradual, no instantánea, porque: `this.lifespan -= 2;`, esto hace que: La partícula se desvanezca poco a poco y su transparencia disminuya progresivamente.

3. La actualizacion se hace por medio del método `run()`:
```
run() {
  let gravity = createVector(0, 0.05);
  this.applyForce(gravity);
  this.update();
  this.show();
}
```
- Patrón Motion 101:

Se cumple claramente el patrón:
```
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
this.acceleration.mult(0);
```
Flujo completo: Fuerza → Aceleración → Velocidad → Posición

Esto significa que: La gravedad afecta la aceleración, La aceleración modifica la velocidad, La velocidad modifica la posición.

#### Capa de estructura

4. Las partículas se crean en `draw()`: `particles.push(new Particle(width / 2, 20));` Se crea una partícula por frame, lo que genera un flujo continuo.

5. El programa en `draw()`:
```
if (particle.isDead()) {
  particles.splice(i, 1);
}
```
La partícula no se elimina sola, solo indica si está muerta.

6. Cuando eliminas un elemento (splice), los índices cambian. Si no se hiciera así: Se saltarían partículas y se generarían errores o comportamientos inesperados
Ejemplo :
[0][1][2][3]
Eliminar 1, ahora 2 pasa a ser 1
El loop sigue y se salta ese elemento

7. si se comenta `particles.splice(i, 1);`
Consecuencias: El array crece indefinidamente, aumenta el uso de memoria, baja el frame rate, eventualmente puede colgarse la simulación

#### Capa de visualizacion

8. Los elementos visuales que representan la particula son: `stroke(0, this.lifespan);`, `fill(127, this.lifespan);`, `circle(this.position.x, this.position.y, 8);`

Elementos:

- Círculo
- Color gris (fill)
- Borde (stroke)
- Transparencia basada en lifespan

9. El lifespan controla el alpha (transparencia):
```
stroke(0, this.lifespan);
fill(127, this.lifespan);
```
A medida que: lifespan disminuye la partícula se vuelve más transparente, esto crea un efecto de: desaparición progresiva (fade out)

10. si quisiera hacer un cambio visual por ejemplo: usar líneas en vez de círculos.

Cambiaría:
```
show() {
  stroke(0, this.lifespan);
  line(this.position.x, this.position.y,
       this.position.x + 10, this.position.y + 10);
}
```
No cambiaría:
```
update()
applyForce()
lifespan
isDead()
```
Porque eso pertenece a: la lógica del sistema, no a la visualización

### Actividad 2

#### Comparacion

1. En el Example 4.2, draw() se encargaba de:
- Crear partículas
- Actualizarlas
- Dibujarlas
- Eliminarlas

Ahora, en este ejemplo, esas responsabilidades se trasladan a la clase Emitter.

Antes (Example 4.2):
```
particles.push(new Particle(...));
particle.run();
if (particle.isDead()) { ... }
```
Ahora (Example 4.4):
```
emitter.addParticle();
emitter.run();
```
Ahora dentro de Emitter ocurre todo:
```
this.particles.push(new Particle(...));
this.particles[i].run();
this.particles.splice(i, 1);
```
Es decir, draw() ahora solo: Coordina el sistema e itera sobre los emisores

2. Encapsular en Emitter permite:

- Modularidad: Cada emisor funciona como un sistema independiente.

- Escalabilidad: Puedes tener múltiples emisores sin complicar draw().

- Reutilización: El mismo emisor se puede usar en diferentes contextos.

- Organización del código: Se separa la lógica del sistema (Emitter) y el control general (draw)

3. Los emitters se crean con interacción del usuario: `emitters.push(new Emitter(mouseX, mouseY));`, el usuario (click) decide dónde aparecen.

Las partículas Se crean dentro del emisor: `this.particles.push(new Particle(this.origin.x, this.origin.y));`, Cada emisor genera sus propias partículas.

4. Mapa de jerarquia
<img width="306" height="369" alt="Jerarquia de ejercicio 4 4" src="https://github.com/user-attachments/assets/4984b868-18a6-47bb-b9e1-d8ac1faceed4" />

Hay 2 niveles de colección:

- Array de emitters
- Array de partículas (dentro de cada emitter)

5. Un sistema principal contiene una colección de emisores. Cada emisor es una entidad que genera continuamente nuevas entidades más pequeñas. Estas entidades poseen un estado físico (posición, velocidad, aceleración) y un estado de vida (tiempo de existencia).

Cada entidad evoluciona en el tiempo mediante la aplicación de fuerzas, lo que modifica su movimiento. A medida que transcurre su ciclo de vida, su estado cambia hasta que alcanza una condición límite, momento en el cual es eliminada del sistema.

Los emisores funcionan como centros de generación que producen flujos constantes de entidades, creando un sistema dinámico donde múltiples colecciones interactúan de manera simultánea.

### Actividad 3

1. En común:

Todas las partículas (tanto Particle como Confetti) comparten:

- Propiedades físicas:
  - position
  - velocity
  - acceleration
- Estado de vida:
  - lifespan
- Comportamiento:
  - run()
  - update()
  - applyForce()
  - isDead()

Esto se debe a que Confetti hereda de Particle.

Diferencias:

La diferencia principal está en la visualización:

Particle:
```
circle(this.position.x, this.position.y, 8);
```
Confetti:
```
rotate(angle);
square(0, 0, 12);
```
Además: Confetti rota según su posición y Tiene una representación más dinámica.

2. Esto es importante porque permite que el sistema sea flexible y extensible, el emisor no necesita conocer los detalles de cada tipo de partícula, solo necesita saber que todas: se pueden actualizar (run()) y pueden morir (isDead()), esto evita acoplamiento fuerte.

El emisor gestiona comportamientos, no tipos específicos.

3. Solo se necesita crear una nueva clase, por ejemplo:
```
class TriangleParticle extends Particle {
  show() {
    triangle(...);
  }
}
```
Que no modificar:

- run() del Emitter
- isDead()
- update()
- lógica de movimiento
- estructura del sistema

4. ¿Cambió la lógica del Emitter?

Sí, pero no en este ejemplo directamente:

- Se introdujo la clase Emitter
- Se encapsuló la gestión de partículas

¿Cambió la lógica de muerte?

No cambió: `return this.lifespan < 0.0;`, todas las partículas mueren igual.

¿Qué capa del sistema se modificó?

- Capa de visualización
  - Diferentes formas (círculo vs cuadrado rotado)

- Capa de tipo/estructura (abstracción)
  - Introducción de herencia
  - Introducción de polimorfismo

¿Qué capas permanecieron intactas?

- Capa de comportamiento
  - Movimiento (Motion 101)
  - Fuerzas (gravedad)
  - Actualización
- Capa de ciclo de vida
  - lifespan
  - isDead()
- Capa de estructura (Emitter)
  -No cambia la forma de gestionar partículas

### Actividad 4


## Bitácora de aplicación 

### Actividad 5


## Bitácora de reflexión

### Actividad 6
