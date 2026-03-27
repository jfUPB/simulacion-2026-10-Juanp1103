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

1. Represento el ciclo de vida de una idea: nace como una entidad clara, se mueve buscando atención, y al perderla se fragmenta en pensamientos dispersos que finalmente desaparecen. La pieza busca transmitir cómo las ideas necesitan interacción para mantenerse vivas. Muchas veces tenemos buenas ideas, solo necesitamos un poco de concentracion para que estas ideas perduren en algo mejor aunque al final tambien terminemos olvidando estas ideas.

3. Mapa de decisiones:

- IdeaParticle (círculo) queria representar la claridad y unidad.
- FragmentParticle (líneas) Este queria que representara confusión, pérdida o desorientacion.
- Atracción al mouse busca simbolizar la atención humana.
- Transformación al morir, queria mostrar que una idea nunca desaparece completamente, se fragmenta.
- Gravedad la veía como un olvido inevitable, representando algo asi como un avismo.

3. Bocetos de idea:
<img width="720" height="1280" alt="boceto 1" src="https://github.com/user-attachments/assets/2faf59d9-5b1e-4932-8f23-5cc6c69e8789" />


4. Codigo:

```
let emitter;

function setup() {
  createCanvas(900, 600);
  emitter = new Emitter(width / 2, height / 4);
}

function draw() {
  background(10, 20);

  let gravity = createVector(0, 0.03);

  emitter.applyGravity(gravity);
  emitter.applyAttraction(mouseX, mouseY);

  if (frameCount % 10 === 0) {
    emitter.addIdea();
  }

  emitter.run();

  drawAttentionAura();
}

// ---------------- MOUSE CONCENTRACION ----------------
function drawAttentionAura() {
  noStroke();

  for (let r = 70; r > 0; r -= 10) {
    fill(255, 255, 255, map(r, 0, 120, 60, 0));
    circle(mouseX, mouseY, r * 2);
  }
}

// ---------------- PARTICLE BASE ----------------
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(0.5, 2));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2;
  }

  isDead() {
    return this.lifespan < 0;
  }
}

// ---------------- PARTICULA IDEA ----------------
class IdeaParticle extends Particle {
  constructor(x, y) {
    super(x, y);
  }

  run() {
    this.update();
    this.show();
  }

  show() {
    let d = dist(this.position.x, this.position.y, mouseX, mouseY);
    let radius = 120;

    let size = map(this.lifespan, 255, 0, 14, 4);

    if (d < radius) {
      fill(180, 255, 255, this.lifespan);
      size *= 1.8;
    } else {
      fill(100, 200, 255, this.lifespan);
    }

    noStroke();
    circle(this.position.x, this.position.y, size);
  }
}

// ---------------- FRAGMENTOS DE OLVIDO ----------------
class FragmentParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.velocity.mult(random(2, 4));
  }

  run() {
    this.update();
    this.show();
  }

  show() {
    stroke(255, this.lifespan);
    line(
      this.position.x,
      this.position.y,
      this.position.x + random(-6, 6),
      this.position.y + random(-6, 6)
    );
  }
}

// ---------------- EMITTER ----------------
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addIdea() {
    this.particles.push(new IdeaParticle(this.origin.x, this.origin.y));
  }

  applyGravity(force) {
    for (let p of this.particles) {
      p.applyForce(force);
    }
  }

  applyAttraction(mx, my) {
    let target = createVector(mx, my);

    for (let p of this.particles) {
      if (p instanceof IdeaParticle) {
        let dir = p5.Vector.sub(target, p.position);
        let d = dir.mag();

        let radius = 120;

        if (d < radius) {
          dir.setMag(0.8);
          p.applyForce(dir);

          p.lifespan += 1.5;
        } else {
          p.lifespan -= 1;
        }
      }
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();

      if (p.isDead()) {

        if (p instanceof IdeaParticle) {
          for (let j = 0; j < 6; j++) {
            this.particles.push(
              new FragmentParticle(p.position.x, p.position.y)
            );
          }
        }

        this.particles.splice(i, 1);
      }
    }
  }
}
```
Enlace p5.js: https://editor.p5js.org/juanpa1103/sketches/UYM_0duUU

5.Capturas:
- Nacimiento de las ideas:
<img width="1667" height="1047" alt="image" src="https://github.com/user-attachments/assets/d7ca538d-ccb5-46b1-acee-6e6a7b8142ac" />

- ideas siendo olvidadadas:
<img width="1558" height="886" alt="image" src="https://github.com/user-attachments/assets/6127a6aa-8669-4816-aef6-6fc3a57b4dcd" />

- Concentracion en las ideas:
<img width="1563" height="971" alt="image" src="https://github.com/user-attachments/assets/d80df3ad-ffc1-476b-9ff4-535dd10621b3" />

## Bitácora de reflexión

### Actividad 6
