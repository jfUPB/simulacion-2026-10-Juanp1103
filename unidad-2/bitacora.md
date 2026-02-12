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



## Bitácora de aplicación 



## Bitácora de reflexión



