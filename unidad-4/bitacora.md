# Unidad 4

## Bitácora de proceso de aprendizaje

### Actividad 2
- En la primera simulación se observa un sistema donde un objeto rota constantemente alrededor de un punto central. El ángulo de rotación cambia en cada frame, lo que provoca que el objeto se mueva circularmente. La interacción principal es la modificación continua de ese ángulo, lo que genera la animación de rotación.
- En cada frame se traslada el origen del sistema de coordenadas al centro de la pantalla, Esto se hace porque el sistema de coordenadas tiene su origen en la esquina superior izquierda del canvas. Si la rotación se realizara desde ese punto, el objeto giraría alrededor de esa esquina.
- La función `rotate()` rota el sistema de coordenadas completo. Esto significa que todos los elementos dibujados después de aplicar esta función se orientan según ese nuevo sistema rotado. Por lo tanto, el sistema de coordenadas define la orientación del espacio donde se dibujan los objetos, y `rotate()` modifica esa orientación.

- los elementos gráficos se dibujan alrededor del punto (0,0) del sistema de coordenadas. Esto se hace porque es más fácil construir las formas en relación con su propio centro que usando posiciones absolutas del canvas. Aunque en cada frame se dibuja exactamente lo mismo, los objetos rotan porque el sistema de coordenadas ha sido rotado previamente con `rotate()`. Es decir, los objetos no cambian su posición relativa, pero el espacio en el que existen sí cambia de orientación.

- La función `heading()` calcula el ángulo del vector de velocidad. Es decir, devuelve la dirección en la que apunta ese vector con respecto al eje horizontal. Ese ángulo se utiliza luego para rotar el objeto de modo que su orientación coincida con la dirección de su movimiento.
- Las funciones `push()` y `pop()` se utilizan para guardar y restaurar el estado del sistema de coordenadas y de las transformaciones gráficas. Cuando se usa `push()`, se guarda la configuración actual (traslaciones, rotaciones, escalas, estilos). Luego se pueden aplicar transformaciones sin afectar el resto del dibujo. Finalmente, `pop()` restaura el estado anterior. Esto permite aislar transformaciones para objetos específicos.
- El vector de velocidad representa la dirección y la magnitud del movimiento del objeto. El ángulo de ese vector indica hacia dónde se está desplazando el objeto. Cuando se utiliza ese ángulo en la función `rotate()`, el objeto se orienta en la misma dirección en la que se mueve. Primero se traslada el sistema de coordenadas a la posición del objeto con `translate()`, y luego se rota según el ángulo del vector de velocidad. De esta forma, el objeto no solo se mueve por el espacio, sino que también apunta hacia la dirección de su desplazamiento, generando una representación más natural del movimiento.

### Actividad 3

![Unidad4_Act3](https://github.com/user-attachments/assets/32177d6a-ac65-41a5-9a59-8628c3988b8d)

Codigo p5.js: https://editor.p5js.org/juanpa1103/sketches/inZJ4-EL_

### Actividad 4

En la simulación se puede identificar claramente el marco Motion 101 dentro del método `update()` de la clase Mover. Este marco describe cómo se actualiza el movimiento de un objeto utilizando tres vectores fundamentales: aceleración, velocidad y posición.

En el código aparece en estas líneas:
```
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
```
Primero la aceleración modifica la velocidad, y luego la velocidad modifica la posición. Este orden permite que el movimiento del objeto sea continuo y dinámico dentro del sistema.

Cuando se introducen fuerzas en el sistema, la aceleración deja de ser una regla fija y pasa a ser la suma de todas las fuerzas que actúan sobre el objeto en ese frame. En el código esto ocurre en el método:
```
applyForce(force) {
  let f = p5.Vector.div(force, this.mass);
  this.acceleration.add(f);
}
```
Aquí cada fuerza que actúa sobre el objeto se divide por la masa (siguiendo la segunda ley de Newton) y luego se suma al vector de aceleración. Sin embargo, para que las fuerzas funcionen correctamente es necesario hacer una modificación importante al marco Motion 101: reiniciar la aceleración al final de cada frame. Esto ocurre en la línea: `this.acceleration.mult(0);` Esto se hace porque la aceleración solo debe representar las fuerzas que actúan en el frame actual. Si no se reiniciara, las fuerzas se seguirían acumulando indefinidamente y el movimiento sería incorrecto.

para la modificacion del color del attractor, toca modificar la funcion de display que hay en esta clase, siendo esta la parte del codigo modificada:
```
 display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(50, 180, 90);
    } else if (this.rollover) {
      fill(140, 180, 90);
    } else {
      fill(175, 180, 90);
    }
```
Para el funcionamiento de rollover y dragging primero se puede detectar si el mouse está sobre el attractor calculando la distancia entre el mouse y su posición. Esto se hace usando la función `dist()`, que permite medir la distancia entre dos puntos en el plano.
```
function draw() {    
  background(255);    
  
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);  
  attractor.rollover = d < attractor.mass;  
  
  attractor.display();  
  
  for (let i = 0; i < movers.length; i++) {  
    let force = attractor.attract(movers[i]);  
    movers[i].applyForce(force);  
    movers[i].update();  
    movers[i].show();  
  }  
}
```
En este fragmento se calcula la distancia entre el mouse y el attractor. Si la distancia es menor que el radio del attractor `attractor.mass`, entonces la variable rollover se vuelve verdadera. Esto permite saber cuándo el mouse está encima del attractor y cambiar su apariencia visual.

Luego se puede permitir arrastrar el attractor utilizando las funciones `mousePressed()` y `mouseReleased()` para interactuar con el mouse.
```
function mousePressed() {
  if (attractor.rollover) {
    attractor.dragging = true;
  }
}

function mouseReleased() {
  attractor.dragging = false;
}
```
Cuando el usuario presiona el botón del mouse, se verifica si el cursor está sobre el attractor. Si es así, la variable dragging se vuelve verdadera, lo que indica que el objeto puede ser arrastrado. Cuando el botón del mouse se libera, dragging vuelve a ser falso y el attractor deja de moverse.

Finalmente, dentro de `draw()` se revisa si el attractor está siendo arrastrado. Si dragging es verdadero, su posición se actualiza con la posición del mouse.
```
if (attractor.dragging) {
  attractor.position.x = mouseX;
  attractor.position.y = mouseY;
}
```
De esta manera el attractor sigue el movimiento del mouse mientras el usuario mantiene presionado el botón, permitiendo moverlo por la pantalla y modificar el comportamiento de las fuerzas que afectan a los movers.


### Actividad 5

En la simulación se utilizan coordenadas polares para calcular la posición de un punto que rota alrededor del origen. En este sistema la posición de un punto se define mediante dos valores:

r: la distancia desde el origen hasta el punto (radio).

theta: el ángulo que forma el punto con respecto al eje horizontal.

Para poder dibujar el punto en la pantalla es necesario convertir estas coordenadas polares a coordenadas cartesianas (x, y). Esto se hace con las siguientes ecuaciones: x=r⋅cos(θ), y=r⋅sin(θ)

En el código esto aparece como:
```
let x = r * cos(theta);
let y = r * sin(theta);
```
Estas fórmulas permiten calcular la posición del punto en el plano. Como theta aumenta en cada frame (theta += 0.02), el punto se mueve en un movimiento circular alrededor del origen. Además, el origen del sistema de coordenadas se traslada al centro del canvas usando: `translate(width / 2, height / 2);`, Esto hace que el movimiento circular ocurra alrededor del centro de la pantalla.

En la segunda modificacion del código se utiliza la función: `let v = p5.Vector.fromAngle(theta);`

Esta función crea un vector que apunta en la dirección del ángulo theta. Sin embargo, este vector tiene magnitud 1, es decir, es un vector unitario. Por esta razón, el punto ya no se mueve en un círculo grande sino en un círculo muy pequeño cerca del origen, ya que las coordenadas v.x y v.y están limitadas a valores entre -1 y 1.

En la tercera modificación se utiliza: `let v = p5.Vector.fromAngle(theta, r);`

Aquí el segundo parámetro indica la magnitud del vector, por lo que el vector tendrá longitud r. Esto hace que la distancia entre el centro y el círculo exterior vuelva a ser constante y equivalente al radio r. Como resultado, el círculo se mueve en una trayectoria circular estable alrededor del centro.

### Actividad 6

![Unidad4_Act6](https://github.com/user-attachments/assets/d8db682a-a132-4a8e-a91a-6a42fe57d011)

Link p5.js: https://editor.p5js.org/juanpa1103/sketches/XqpTc65VI

### Actividad 7

![Unidad4_Act7](https://github.com/user-attachments/assets/1ab4a7ca-bff6-4531-86b7-3b5259dc5787)

Link p5.js: https://editor.p5js.org/juanpa1103/sketches/MIseqpsVo

De la unidad 1 se modifico el codigo para que hiciera uso del ruido de perlin para que los movimientos fueran suaves, este se usa por medio de `noise()`. de la unidad 3 se agrega la aceleracion y una fuerza que empuje a los osciladores, para evitar que este movimiento se salga de control se usa `limit()`.

Codigo oscillator:
```
class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));

    this.amplitude = createVector(
      random(20, width / 2),
      random(20, height / 2)
    );

    // MOTION 101
    this.acceleration = createVector(0, 0);

    // offset para noise
    this.noiseOffset = random(1000);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {

    // variación orgánica usando noise
    let n = noise(this.noiseOffset);
    this.noiseOffset += 0.01;

    let variation = map(n, 0, 1, -0.01, 0.01);
    let noiseForce = createVector(variation, variation);

    this.applyForce(noiseForce);

    // MOTION 101
    this.angleVelocity.add(this.acceleration);

    // limitar velocidad angular para evitar que se descontrole
    this.angleVelocity.limit(0.1);

    this.angle.add(this.angleVelocity);

    // reset aceleración
    this.acceleration.mult(0);
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);
    stroke(0);
    strokeWeight(2);
    fill(127);
    line(0, 0, x, y);
    circle(x, y, 32);
    pop();
  }
}
```
En este codigo se modifica la funcion `draw()` para agregarle las fuerzas y mostrarlas a los oscillators.

Codigo Sketch:
```
let oscillators = [];

function setup() {
  createCanvas(640, 240);
  // Initialize all objects
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator());
  }
}

function draw() {
  background(255);

  // fuerza externa suave usando noise
  let windValue = map(noise(frameCount * 0.01), 0, 1, -0.02, 0.02);
  let wind = createVector(windValue, windValue);

  for (let i = 0; i < oscillators.length; i++) {
    oscillators[i].applyForce(wind); // aplicar fuerza
    oscillators[i].update();
    oscillators[i].show();
  }
}
```

### Actividad 8

El código se ejecuta únicamente dentro de la función setup(), por lo que la onda se dibuja una sola vez y permanece estática, para lograr que la onda se comporte como una ola en movimiento, el primer cambio fue trasladar el código de dibujo a la función draw(). Esto permite que la escena se actualice constantemente en cada frame.

Luego se añadió un desplazamiento progresivo del ángulo utilizado en la función seno. Se creó una variable `angleOffset` que se incrementa dentro del ciclo for para generar la forma de la onda a lo largo del eje horizontal. Después, el valor de `angle` se incrementa ligeramente en cada frame, lo que produce que toda la onda se desplace en el tiempo.

![Unidad4_Act8](https://github.com/user-attachments/assets/f01f1b79-84b4-4907-9139-d619a38120c0)
Link p5.js : https://editor.p5js.org/juanpa1103/sketches/wLuZllZpP

Codigo:
```
let angle = 0;
let angleVelocity = 0.2;
let amplitude = 100;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  let angleOffset = angle;

  for (let x = 0; x <= width; x += 24) {
    
    let y = amplitude * sin(angleOffset);

    circle(x, y + height / 2, 48);

    angleOffset += angleVelocity;
  }

  // hace que la onda se desplace como una ola
  angle += 0.05;
}
```

### Actividad 9

En esta actividad los cambios realizados suceden principalmente en la seccion de codigo donde esta el setup, se modifican las funciones de `draw()` y se modifican las 2 funciones para el movimiento del mouse.
Codigo modificado:
```
let bob1;
let bob2;

let spring1;
let spring2;

function setup() {
  createCanvas(640, 240);

  spring1 = new Spring(width / 2, 10, 80);
  spring2 = new Spring(width / 2, 100, 80);

  bob1 = new Bob(width / 2, 100);
  bob2 = new Bob(width / 2, 180);
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);

  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  bob1.update();
  bob2.update();

  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);

  // primer resorte
  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);

  // segundo resorte usa bob1 como ancla
  spring2.anchor = bob1.position.copy();
  spring2.connect(bob2);
  spring2.constrainLength(bob2, 30, 200);

  // dibujar resortes
  spring1.showLine(bob1);
  spring2.showLine(bob2);

  // dibujar bobs
  bob1.show();
  bob2.show();

  spring1.show();
}
function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```
Link p5.js: https://editor.p5js.org/juanpa1103/sketches/t2VYrxzTf

### Actividad 10

Para esta actividad se presenta un solo péndulo compuesto por un pivote fijo, un brazo y una masa que oscila debido a la gravedad. Para crear un sistema de dos péndulos conectados en serie, el primer paso fue duplicar el objeto Pendulum, generando dos instancias independientes.
El primer péndulo mantiene su funcionamiento original, utilizando un punto fijo en la parte superior del lienzo como pivote. El segundo péndulo se conecta al primero haciendo que su pivote se actualice continuamente con la posición de la masa del primer péndulo. De esta forma, el segundo péndulo siempre cuelga del extremo del primero.
Durante cada frame se actualiza el movimiento de ambos péndulos utilizando las mismas ecuaciones de movimiento angular basadas en la gravedad, la aceleración angular y un factor de amortiguamiento. Además, se mantiene la interacción con el mouse para permitir arrastrar cualquiera de las masas.

Codigo:
```
let pendulum1;
let pendulum2;

function setup() {
  createCanvas(640, 240);

  pendulum1 = new Pendulum(width / 2, 0, 120);
  pendulum2 = new Pendulum(width / 2, 120, 120);
}

function draw() {
  background(255);

  pendulum1.update();
  pendulum1.show();

  // el segundo pivote sigue al primer bob
  pendulum2.pivot = pendulum1.bob.copy();

  pendulum2.update();
  pendulum2.show();

  pendulum1.drag();
  pendulum2.drag();
}

function mousePressed() {
  pendulum1.clicked(mouseX, mouseY);
  pendulum2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum1.stopDragging();
  pendulum2.stopDragging();
}
```
link p5.js: https://editor.p5js.org/juanpa1103/sketches/APrBkNFLt

## Bitácora de aplicación 

### Actividad 11

Concepto: La obra representa un cielo en el que las constelaciones antiguas han perdido su forma original. Las estrellas continúan moviéndose por el espacio, pero todavía conservan una ligera “memoria” que las impulsa a conectarse con otras estrellas cercanas. Cuando dos estrellas se encuentran dentro de cierta distancia, se forma una conexión entre ellas, generando temporalmente una constelación.

Sin embargo, estas constelaciones no son permanentes. Debido al movimiento continuo de las estrellas, las conexiones se rompen y vuelven a formarse constantemente, generando nuevas configuraciones del cielo.

Codigo:
```
let particles = [];

let connectionDistance = 100;

function setup() {
  createCanvas(640, 400);

  for (let i = 0; i < 70; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  background(10,10,25);

  // actualizar partículas
  for (let p of particles) {
    p.update();
    p.show();
  }

  // comprobar conexiones
  for (let i = 0; i < particles.length; i++) {
    particles[i].connected = false;
  }

  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {

      let d = p5.Vector.dist(particles[i].pos, particles[j].pos);

      if (d < connectionDistance) {

        stroke(180,180,255,150);
        line(
          particles[i].pos.x,
          particles[i].pos.y,
          particles[j].pos.x,
          particles[j].pos.y
        );

        particles[i].connected = true;
        particles[j].connected = true;
      }
    }
  }
}

function mousePressed(){

  // supernova con click izquierdo
  if(mouseButton === LEFT){

    let center = createVector(mouseX,mouseY);

    for(let p of particles){

      let force = p5.Vector.sub(p.pos,center);

      let d = force.mag();

      if(d < 150){
        force.normalize();
        force.mult(3);
        p.vel.add(force);
      }

    }

  }

}

function keyPressed(){

  // aumentar distancia de constelación
  if(key === 'a' || key === 'A'){
    connectionDistance += 10;
  }

  // disminuir distancia
  if(key === 's' || key === 'S'){
    connectionDistance -= 10;
  }

}

class Particle{

  constructor(){

    this.pos = createVector(random(width),random(height));

    // centro de oscilación
    this.center = this.pos.copy();

    // movimiento sinusoidal
    this.angle = random(TWO_PI);
    this.speed = random(0.02,0.05);
    this.amplitude = random(10,30);

    this.vel = p5.Vector.random2D();
    this.vel.mult(random(0.5,1.5));

    this.connected = false;

  }

  update(){

  // ralentizar movimiento cuando está conectada
  if(this.connected){
    this.angle += this.speed * 0.4;
    this.vel.mult(0.98);
  }else{
    this.angle += this.speed;
  }

  // movimiento sinusoidal
  this.pos.x = this.center.x + sin(this.angle) * this.amplitude;
  this.pos.y = this.center.y + cos(this.angle) * this.amplitude;

  // movimiento base
  this.vel.limit(2);
  this.center.add(this.vel);

  // rebote en bordes
  if(this.center.x < 0 || this.center.x > width){
    this.vel.x *= -1;
  }

  if(this.center.y < 0 || this.center.y > height){
    this.vel.y *= -1;
  }

}

  show(){

    noStroke();
    fill(255);

    if(this.connected){
      fill(255,220,150);
    }

    circle(this.pos.x,this.pos.y,4);

  }

}
```

![Unidad4_Act11](https://github.com/user-attachments/assets/294f8458-a9a8-45d4-bdf3-f701e4b7cca3)

Link p5.js: https://editor.p5js.org/juanpa1103/sketches/agtSIBXIq_


## Bitácora de reflexión

### Actividad 12

