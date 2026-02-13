# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 2

- La suma de vectores en p5 se hace por medio del comando `add()`, este comando me hace las operaciones matematicas necesarias para hacer la correcta suma de vectores.
- La linea `position = position + velocity` no funciona ya que los vectores no se pueden sumar tan directamente, ya que los vectores utilizan magnitud y direccion, este proceso de suma de vectores se debe hacer de forma matematica donde se separa la suma por valores en este caso `x` y `y`, el operador `+` solo funciona para valores fijos como serian enteros y flotantes.

### Actividad 3

En este caso se esta modificando el random walker, lo que se hizo para poder usar vectores fue crear un vector de posicion, este nos marca donde inicia el punto en la pantalla, en el metodo step se crea un vector random con el operador `p5.Vector.Random2D()`, el cual nos modifica la posicion inicial, podemos controlar la velocidad en que se modifica y de esta forma ya no utilizamos solo la posiciones de `x` y `y` para darle el movimento.

Codigo
```
let walker;

function setup() {
  createCanvas(640, 240);
  background(255);
  walker = new Walker();
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2);
  }

  step() {
    // vector aleatorio
    let step = p5.Vector.random2D();
    step.mult(2); // controla qué tan rápido se mueve
    this.position.add(step);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }
}
```

### Actividad 4

- Espero que en este codigo no haga mucho, solo nos pinta la pantalla de un color y nos imprime una posicion, esta posicion luego se modifica y nos muestra como cambia las coordenadas de `(6,9)` a `(20,30)`.
- El resultado obtenido es el esperado.
- En el paso por valor, se da una valor fijo, al momento de entregarlo a una funcion este valor se mantiene y se crea una copia que no modifica el valor original, en el paso por referencia se toma la referencia del objeto como tal en el caso del codigo el vector, esto en memoria nos envia al mismo espacio sin crearnos una copia por lo que los valores originales se modifican.
- en el codigo se hace paso por referencia ya que nos envia a la posicion del objeto que seria el vector.
- Aprendí que los objetos en JavaScript, como los vectores, se pasan por referencia, por lo que las funciones pueden modificar directamente su estado.

### Actividad 5

- ¿Para qué sirve mag()?:

  Devuelve la longitud (magnitud) de un vector, la diferencia con magSq() es que esta devuelve la magnitud al cuadrado.
- ¿Para qué sirve normalize()?

  Convierte el vector en uno de longitud 1, manteniendo su dirección.
- ¿para qué sirve dot()?

  Sirve para saber qué tan alineados están dos vectores.
- Diferencia entre dot() estático y de instancia

  No hay diferencia matemática; solo cambia la forma de llamarlo.
-  ¿Cuál es la interpretación geométrica del producto cruz de dos vectores?

  Produce un vector perpendicular a los dos originales; su magnitud representa el área del paralelogramo y su dirección depende del orden (orientación).
- ¿Para qué sirve dist()?

  Para medir la distancia entre dos puntos o vectores.
- ¿Para qué sirve limit()?

  Sirve para restringir la magnitud máxima del vector

### Avtividad 6

Codigo:

```
let t = 0;
let speed = 0.005;

function setup() {
    createCanvas(660, 550);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(530, 0);
    let v2 = createVector(0, 430);
  
    // t va de 0 a 1 y vuelve
  t += speed;
  if (t > 1 || t < 0) {
    speed *= -1;
  }
  
  let v3 = p5.Vector.lerp(v1, v2, t);
  
  let c = lerpColor(color(255, 0,0), color(0, 0, 255), t);
  
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, c);
    drawArrow(  p5.Vector.add(v0, v1),  p5.Vector.sub(v2, v1), 'green')
  
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
link a p5.js: https://editor.p5js.org/juanpa1103/sketches/fl9HqNAxz
- ¿Cómo funciona `lerp()`?

`lerp()` calcula un valor intermedio entre dos valores o vectores usando un parámetro t entre 0 y 1.

- ¿Cómo funciona `lerpColor()`?

`lerpColor()` mezcla dos colores de forma lineal según un parámetro t entre 0 y 1.

- ¿Cómo se dibuja una flecha usando drawArrow()?

La flecha se dibuja trasladando el origen al punto base, dibujando una línea con la longitud del vector y rotando un triángulo para indicar su dirección.

### Actividad 7

El concepto fundamental de motion 101 es que un objeto se mueve actualizando su posición según su velocidad en cada instante. Es decir, en cada paso del tiempo:

- El objeto tiene una posición.
- Tiene una velocidad (que indica hacia dónde y qué tan rápido se mueve).
- La nueva posición se obtiene “sumando” la velocidad a la posición anterior.

Geométricamente, todo se entiende en el plano como flechas (vectores):

- La posición: La posición es un punto en el plano, que puede imaginarse como una flecha que va desde el origen (0,0) hasta el lugar donde está el objeto.

- La velocidad es otra flecha: Apunta en la dirección del movimiento y su longitud representa qué tan rápido se mueve el objeto.

- ¿Qué significa “sumar la velocidad a la posición”?

Geométricamente, significa: Que tomas el punto donde está el objeto, desde ese punto, avanzas en la dirección de la flecha de velocidad, la punta de esa flecha marca la nueva posición.

En el ejemplo 1.7 cada objeto tiene dos vectores:

- Un vector que representa dónde está el objeto.

- Un vector que representa cómo se mueve (su velocidad).

Actualización de posición: En cada paso del tiempo, se toma la posición actual y se “desplaza” agregándole la velocidad.
Geométricamente esto significa mover el punto actual una cierta distancia en la dirección indicada por la flecha de velocidad.

Dibujar el objeto: Después de moverlo, el objeto se dibuja en esa nueva posición, así ves que se desplaza en la pantalla.

### Actividad 8

- En el caso de la aceleracion constante se el movimento es mas predecible y constante, a medida que pasa el tiempo la velocidad aumenta constantemente.
- En el caso de la aceleracion aleatoria nos da un movimento muy erratico sin sentido aparente, haciendo que el movimento no se pueda predecir, similar a un random walk
- En el caso de la aceleracion con el mouse el movimento no solo es predecible sino que tambien es manipulable, dependiendo de la distancia en la que me encuentre la aceleracion cambia de forma diferente

Se puede observar que la aceleracion es una variable importante que nos sirve para variar el comportamiento sin mucho problema.

## Bitácora de aplicación 

### Actividad 9

- Mi obra la desarrolle pensando en la fluidez, por eso decidi utilizar una aceleracion constante, de esta forma la obra se siente un poco mas natural pero impredecible como un fluido, esta imprediccion la hice utilizando el concepto de ruido perlin de la unidad anterior, al ver la obra siento que logre darle ese sentimiento como si fuera agua o un fluido similar. adicional para las normas interactivas utilizo el mouse para que la posicion me cambie los angulos de como se mueven las particulas, y cuando presiono hago que las particulas se repelen, del mismo modo con el boton shift las particulas son atraidas.

Codigo:
```
let particles = [];
let zoff = 0;

function setup() {
  createCanvas(740, 640);
  background(0);

  for (let i = 0; i < 5000; i++) {
    particles.push({
      pos: createVector(random(width), random(height)),
      vel: createVector(0, 0),
      acc: createVector(0, 0)
    });
  }
}

function draw() {

  fill(0,10);
  rect(0, 0, width, height);

 let noiseScale = map(mouseY, 0, height, 0.002, 0.02);
  let fieldStrength = map(mouseX, 0, width, 0.5, 3);

  for (let p of particles) {

    // ---------- FLOW FIELD ----------
    let n = noise(p.pos.x * noiseScale, p.pos.y * noiseScale, zoff);
    let angle = n * TWO_PI * fieldStrength;

    let flowForce = p5.Vector.fromAngle(angle);
    flowForce.setMag(0.1);
    p.acc.add(flowForce);

    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, p.pos);
    let d = dir.mag();

    // ---------- REPULSIÓN (mouse click) ----------
    if (mouseIsPressed && d < 150) {
      dir.normalize();
      dir.mult(-0.5);
      p.acc.add(dir);
    }

    // ---------- ATRACCIÓN (SHIFT) ----------
    if (keyIsDown(SHIFT)) {
      dir.normalize();
      dir.mult(0.3);
      p.acc.add(dir);
    }


    // ---------- MOTION 101 ----------
    p.vel.add(p.acc);
    p.vel.limit(3);
    p.pos.add(p.vel);
    p.acc.mult(0);

    // ---------- BORDES ----------
    p.pos.x = (p.pos.x + width) % width;
    p.pos.y = (p.pos.y + height) % height;

    // ---------- COLOR DEPENDIENTE DE VELOCIDAD ----------
    let Actspeed = p.vel.mag();
    let hueValue = map(Actspeed, 0, 3, 70, 150);

    stroke(hueValue, 100, 255 - hueValue, 140);
    point(p.pos.x, p.pos.y);
  }

  zoff += 0.005;

}
```
Enlace a p5.js: https://editor.p5js.org/juanpa1103/sketches/4otoYv1VK


https://github.com/user-attachments/assets/b37a5210-c1a1-46c2-807d-c64c76a98bbb


## Bitácora de reflexión

### Actividad 10

- Para esta actividad me gusto los ecosistemas que eran mas caoticos, lo que buscaba para esta obra era precisamente eso, lo logre creando 3 especies, cada una de las especies se veia atraida por una especie diferente pero esta especie diferente repelia esa especie, asi mismo cada especie se repelia levemente entre ellos para que no se volvieran cumulos de particulas

Codigo:
```
let particles = [];
let interactionRadius = 80;

function setup() {
  createCanvas(900, 700);
  background(0);

  // 🔴 Especie A (200)
  for (let i = 0; i < 200; i++) {
    particles.push(new Particle(random(width), random(height), "A"));
  }

  // 🔵 Especie B (250)
  for (let i = 0; i < 250; i++) {
    particles.push(new Particle(random(width), random(height), "B"));
  }

  // 🟢 Especie C (200)
  for (let i = 0; i < 200; i++) {
    particles.push(new Particle(random(width), random(height), "C"));
  }
}

function draw() {
  // Trails
  fill(0, 25);
  noStroke();
  rect(0, 0, width, height);

  for (let p of particles) {
    p.applyInteractions(particles);
    p.update();
    p.display();
  }
}

class Particle {
  constructor(x, y, species) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.species = species;
    this.size = 5;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  applyInteractions(others) {
    for (let other of others) {
      if (other === this) continue;

      let dir = p5.Vector.sub(other.pos, this.pos);
      let d = dir.mag();

      if (d < interactionRadius && d > 5) {

        let forceStrength = getInteraction(this.species, other.species);

        if (forceStrength !== 0) {
          dir.normalize();

          // fuerza depende de distancia
          let strength = forceStrength * (1 - d / interactionRadius);
          dir.mult(strength);

          this.applyForce(dir);
        }
      }
    }
  }

  update() {
    // Motion 101
    this.vel.add(this.acc);
    this.vel.limit(3);
    this.pos.add(this.vel);

    // Fricción leve
    this.vel.mult(0.98);

    this.acc.mult(0);

    // Bordes envolventes
    this.pos.x = (this.pos.x + width) % width;
    this.pos.y = (this.pos.y + height) % height;
  }

  display() {
    noStroke();

    if (this.species === "A") fill(255, 80, 80, 180);
    if (this.species === "B") fill(80, 150, 255, 180);
    if (this.species === "C") fill(100, 255, 140, 180);

    circle(this.pos.x, this.pos.y, this.size);
  }
}

// MATRIZ ASIMÉTRICA
function getInteraction(from, to) {

  // 🔴 A
  if (from === "A" && to === "A") return -0.1;
  if (from === "A" && to === "B") return 0.6;     // fuerte atracción
  if (from === "A" && to === "C") return -0.15;   // leve repulsión

  // 🔵 B
  if (from === "B" && to === "A") return -0.6;    // fuerte repulsión
  if (from === "B" && to === "B") return 0.2;     // agrupamiento leve
  if (from === "B" && to === "C") return 0;       // ignora

  // 🟢 C
  if (from === "C" && to === "A") return 0.3;    // leve atracción
  if (from === "C" && to === "B") return -0.6;    // fuerte repulsión
  if (from === "C" && to === "C") return -0.06;    // leve atracción

  return 0;
}
```
Link p5.js: https://editor.p5js.org/juanpa1103/sketches/wh3YKnOVc

<img width="1785" height="1384" alt="image" src="https://github.com/user-attachments/assets/d4e755c9-578b-4c47-96a1-e8b41701aa46" />


