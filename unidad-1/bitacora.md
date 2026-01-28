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







