# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 2
En la siguiente seccion de codigo se modificara los valores de choice que se evaluan para dar una direccion de movimiento

``` 
step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
```
Hipotesis:
- Los valores se cambian a "0", se espera que el movimiento sea estatico ya que los valores se estarian cancelando entre si
Resultado:
- El movimiento fue diagonal acendente hacia la derecha, esto se da porque cada `else if` se convierte simplemente en `if` por lo que todos se ejecutaran, haciendo que se sume cada valor


### Actividad 3
 -la diferencia esta en que la distribucion uniforme hace que todos los valores dentro de un intervalo tengan la misma probababilidad de ocurrir, a diferencia que la distribucion no uniforme que hace que todos los valores tengan diferentes probabilidades de ocurrir.

 -En el codigo se agrego una funcion random que ayuda a crear el porcentaje para favorecer hacia que lado se mueve:

```  step() {
  let r = random(1);

  if (r < 0.45) {
    // 45% derecha
    this.x++;
  } else if (r < 0.7) {
    // 25% izquierda
    this.x--;
  } else if (r < 0.85) {
    // 15% arriba
    this.y--;
  } else {
    // 15% abajo
    this.y++;
  }
}
```
Esta funcion me da contantemente un numero entre o y 1, esta funcion como tal no es el que me favorece hacia que lado se mueve ya que tiene las mismas probabilidades de caer en el mismo numero, lo que me favorece hacia que lado se mueve es el intervalo el tamaño del intervalo de numeros que escoja como el intervalo mas grande en este caso es de o.45 hacia la derecha podemos ver que el camino recorrido mayormente es hacia la derecha:
 <img width="634" height="236" alt="image" src="https://github.com/user-attachments/assets/156dfd2a-7a9d-4064-8faa-c3a7e9c033c6" />

 ### Actividad 4
 
Codigo de la desviacion estandar visualizada `X` y `Y`
```
function setup() {
  createCanvas(640, 550);
  background(163, 227, 178);
}

function draw() {
  let x = randomGaussian(320, 80);
  let y = randomGaussian(220, 80);
  noStroke();
  fill(0, 100);
  circle(x, y, 10);
}

```
<img width="1592" height="1285" alt="image" src="https://github.com/user-attachments/assets/44c97bf9-85e3-480d-a616-5a2cd16c5921" />

Enlace a p5.js: https://editor.p5js.org/juanpa1103/sketches/yGfUvzje9

### Actividad 5

Seleccione la caminata random normal y aplique esta tecnica de salto para que el punto se se moviera aleatoriamente como antes lo hacia pero en esta ocasion dando saltos para que haga un recorrido mas amplio.

```
let walker;

function setup() {
  createCanvas(640, 550);
  walker = new Walker();
  background(118, 149, 168);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    noStroke();
    circle(this.x, this.y, 3);
  }

  step() {
    let stepSize = levy() * 20; // controla qué tan lejos puede saltar
    let angle = random(TWO_PI);

    this.x += cos(angle) * stepSize;
    this.y += sin(angle) * stepSize;

    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }
}

function levy() {
  while (true) {
    let r1 = random(1);
    let r2 = random(1);
    if (r2 < r1) {
      return r1;
    }
  }
}
```
![Unidad1_Act5](https://github.com/user-attachments/assets/05784cc4-6347-499f-9e7d-31ccf047cd58)

Enlace a p5.js: https://editor.p5js.org/juanpa1103/sketches/0SF-UybIO

### Actividad 6

La idea que tenia en mente es modificar el codigo de random walk con el ruido de perlin creando un nuevo ruido, este ruido modifica el tamaño y color del circulo, dependiendo de la posicion en y que se encuentre, esto hace que los ciculos en la parte superior sean mas pequeños y en la inferior mas grandes, al mismo tiempo crea un degradado de color verticalmente.

Codigo:
```
let walker;

function setup() {
  createCanvas(640, 580);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.tx = 0;
    this.ty = 10000;
  }

  step() {
    //{!2} x- and y-position mapped from noise
    this.x = map(noise(this.tx), 0, 1, 0, width);
    this.y = map(noise(this.ty), 0, 1, 0, height);

    //{!2} Move forward through “time.”
    this.tx += 0.01;
    this.ty += 0.01;
  }

  show() {
  let c = map(noise(this.ty), 0, 1, 50, 200);
  stroke(0,30);
  fill(c, 100, 150, 70);
  circle(this.x, this.y, c);
}
}
```
![Unidad1_Act6](https://github.com/user-attachments/assets/8f8c769f-10fd-479f-b47e-dc0e69070e0b)

Enlace a p5.js: https://editor.p5js.org/juanpa1103/sketches/_cj5wGosk

## Aplicacion

### Actividad 7

- El arte generativo es cuando se crea una obra, pero a diferencia del arte tradicional no se sabe o se puede predecir el resultado, ademas el resultado siempre es variable, lo mas importante del arte generativo son las reglas y normas que se usan, para crear la obra, en la mayoria de las veces se utiliza el azar como un factor importante en esta generacion, sin embargo no se le entrega todo el control sino que el artista debe pensar como y cuando va a usar este azar para que sea controlado y se obtengan resultados deseados

Codigo:
```
let particles = [];
let zoff = 0;

function setup() {
  createCanvas(740, 540);
  background(0);
  

  // crear partículas
  for (let i = 0; i < 2000; i++) {
    particles.push(createVector(random(width), random(height)));
  }
}

function draw() {
  // fondo con alpha para dejar rastro
  stroke(50, 98, 162);
  fill(149, 181, 62,20);
  rect(0, 0, width, height);

  let noiseScale = map(mouseY, 0, height, 0.002, 0.02);
  let fieldStrength = map(mouseX, 0, width, 0.5, 3);

  for (let p of particles) {
    // ruido Perlin
    let n = noise(p.x * noiseScale, p.y * noiseScale, zoff);
    let angle = n * TWO_PI * fieldStrength;

    // distribución normal
    angle += randomGaussian() * 0.01;

    // movimiento base
    p.x += cos(angle);
    p.y += sin(angle);

    // Lévy flight
    if (random(1) < 0.007) {
      let jumpSize = levy() * 100;
      p.x += cos(angle) * jumpSize;
      p.y += sin(angle) * jumpSize;
    }

    // envolver bordes
    p.x = (p.x + width) % width;
    p.y = (p.y + height) % height;

    // dibujar
    stroke(96, 28, 156, 40);
    fill(96, 28, 156,10)
    circle(p.x, p.y,1.5);
  }

  zoff += 0.005;
}

// Accept–Reject → Lévy flight
function levy() {
  while (true) {
    let r1 = random(1);
    let r2 = random(1);
    if (r2 < r1) {
      return r1;
    }
  }
}
```
<img width="1816" height="1337" alt="image" src="https://github.com/user-attachments/assets/9f1c23dd-97dd-4f0b-a107-ac02ac293e2e" />

Enlace a P5.js: https://editor.p5js.org/juanpa1103/sketches/x6Ut7NzsX

## Reflexion

### Actividad 8

- Describe la diferencia fundamental entre la aleatoriedad generada por `random()` y la apariencia de aleatoriedad del Ruido Perlin `noise()`. ¿En qué tipo de situación usarías cada una?

La diferencia mas importante es que la alietoriedad generada por random no tiene un orden aparente ni control, mientras la generada por noise se controla dandole un poco de continuidad al siguiente numero limitandolo a que sea aleatorio si pero cercano al numero anterior.

- Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?

En una caminata aleatoria con distribución uniforme, todos los posibles pasos tienen la misma probabilidad de ocurrir. Visualmente, esto produce un movimiento más errático y disperso. En cambio, en una caminata aleatoria con distribución normal, la mayoría de los pasos se concentran alrededor de un valor promedio y los pasos extremos son poco frecuentes. Visualmente, el movimiento se percibe más suave y orgánico.

- ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple

En el arte generativo, la aleatoriedad no se usa como caos puro, sino como un material creativo controlado dentro de un sistema de reglas. Una de las funciones es introducir variación. La aleatoriedad evita que el sistema produzca siempre el mismo resultado, permitiendo que cada ejecución de la obra sea distinta. Otra funcionalidad es romper patrones y producir sorpresa. Incluso en sistemas muy estructurados, la aleatoriedad permite que ocurran eventos inesperados.

- Piensa en tu obra final (Actividad 07). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.

utilicé el Lévy flight como uno de los conceptos de aleatoriedad principales. Esta forma de aleatoriedad se caracteriza porque la mayoría de los movimientos son pequeños y locales, pero de manera ocasional ocurren saltos grandes y poco probables. Elegí este tipo de aleatoriedad porque me permitió evitar un comportamiento repetitivo y demasiado predecible. Si el sistema solo se movía con pasos pequeños y constantes, la imagen tendía a concentrarse siempre en las mismas zonas del espacio. Con el Lévy flight, el sistema podía explorar nuevas áreas de forma inesperada

-¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?

Una caminata (walk) es un modelo de movimiento en el que una entidad cambia su posición paso a paso siguiendo ciertas reglas. En cada iteración, el nuevo estado depende del estado anterior y de algún proceso de decisión, que puede ser determinista, aleatorio o una combinación de ambos.
Una caminata de tipo Lévy flight se caracteriza por usar una distribución de probabilidad no uniforme para el tamaño de los pasos. La mayoría de los movimientos son cortos y locales, pero de manera ocasional aparecen saltos muy largos. Estos saltos no son errores, sino una parte esencial del modelo: aunque son poco probables, tienen un impacto grande en la trayectoria.
