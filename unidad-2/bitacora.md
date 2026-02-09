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

## Bitácora de aplicación 



## Bitácora de reflexión

