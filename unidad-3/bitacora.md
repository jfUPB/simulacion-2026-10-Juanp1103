# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 1


### Actividad 2
Lo que pude aprender en esta actividad y lo que cambia de la unidad anterior es que en esta unidad ya se hace un calculo de fuerzas que estan actuando o que yo le estoy agregando y de esta forma calculamos la aceleracion, en la unidad anterior lo que haciamos era darle un valor definido por mi, sin ningun tipo de calculo.

Respondiendo a la pregunta de porque al final del update se multiplica por 0 es porque la aceleración representa solo las fuerzas que actúan en ese frame específico.

Si no la reiniciamos:

Las fuerzas se seguirían acumulando indefinidamente, haciendo que el objeto acelere sin control por lo que el sistema perdería coherencia física.

Se pone al final de update() porque:

Primero usamos la aceleración para modificar velocidad, luego actualizamos la posición y finalmente limpiamos la aceleración para el siguiente frame.

Es como “borrar las fuerzas” después de aplicarlas.

si la masa es 1 no hay mucho problema ya que segun la formula de la aceleracion las fuerzas se dividirian por 1 por lo que no nos afectaria las fuerzas, pero si hacemos

```
applyForce(force) {
    // Asume que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```

Entonces el calculo es modificaria las fuerzas globales que afectan al objeto, cambiandonos la referencia original por lo que tendriamos Crear una copia del vector:
```
applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
}
```
Así:

No modificamos la fuerza original, aplicamos correctamente la segunda ley de Newton y cada objeto puede tener masa diferente.

### Actividad 3
## `friccion:`
```
let movers = [];

function setup() {
  createCanvas(800, 600);
  background(0);

  for (let i = 0; i < 40; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  fill(0, 30);
  noStroke();
  rect(0, 0, width, height);

  for (let m of movers) {
    m.applyFriction();
    m.update();
    m.display();
  }
}

class Mover {
  constructor() {
    this.pos = createVector(width/2, height/2);
    this.vel = p5.Vector.random2D().mult(random(5,10));
    this.acc = createVector(0,0);
    this.mass = 1;
  }

  applyForce(force){
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  applyFriction(){
    let mu = 0.05;
    let friction = this.vel.copy();
    friction.normalize();
    friction.mult(-mu);
    this.applyForce(friction);
  }

  update(){
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display(){
    fill(255);
    circle(this.pos.x, this.pos.y, 6);
  }
}
```
![Unidad3_Act3F](https://github.com/user-attachments/assets/9e5856ef-a415-4d5c-a874-8b2bfacd8108)
Link p5.js: https://editor.p5js.org/juanpa1103/sketches/L4b3qeyB3

## `Resistencia a fluidos:`
```
let movers = [];
let windOn = false;

function setup(){
  createCanvas(800,600);

  for(let i=0;i<40;i++){
    movers.push(new Mover(random(width), random(-200,0)));
  }
}

function draw(){
  background(230);

  for(let m of movers){

    // GRAVEDAD
    let gravity = createVector(0, 0.2 * m.mass);
    m.applyForce(gravity);

    // VIENTO (solo si se presiona W)
    if(windOn){
      let wind = createVector(0.3, 0);
      m.applyForce(wind);
    }

    // RESISTENCIA DEL FLUIDO
    m.applyDrag();

    m.update();
    m.display();
  }
}

function keyPressed(){
  if(key === 'w' || key === 'W'){
    windOn = !windOn;
  }
}

class Mover{
  constructor(x,y){
    this.pos = createVector(x,y);
    this.vel = createVector(0,0);
    this.acc = createVector(0,0);
    this.mass = random(0.5,4);
  }

  applyForce(force){
    let f = p5.Vector.div(force,this.mass);
    this.acc.add(f);
  }

  applyDrag(){
    let c = 0.02; // coeficiente del fluido
    let speed = this.vel.mag();

    let dragMag = c * speed * speed;

    let drag = this.vel.copy();
    drag.normalize();
    drag.mult(-dragMag);

    this.applyForce(drag);
  }

  update(){
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    if(this.pos.y > height){
      this.pos.y = 0;
      this.vel.mult(0);
    }
  }

  display(){
    fill(100,150,255,180);
    circle(this.pos.x,this.pos.y,this.mass*8);
  }
}
```
![Unidad3_Act3ResFlu](https://github.com/user-attachments/assets/8228d486-ad7d-4703-843b-895db234d5dc)
Link p5.js: https://editor.p5js.org/juanpa1103/sketches/wsp9kOPHh


## `Atraccion gravitacional:`
```
let sun;
let planets = [];

function setup(){
  createCanvas(800,600);
  sun = new Attractor(width/2,height/2,40);

  for(let i=0;i<20;i++){
    planets.push(new Mover(random(width),random(height)));
  }
}

function draw(){
  background(0);

  sun.display();

  for(let p of planets){
    let force = sun.attract(p);
    p.applyForce(force);

    p.update();
    p.display();
  }
}

class Attractor{
  constructor(x,y,m){
    this.pos = createVector(x,y);
    this.mass = m;
  }

  attract(mover){
    let force = p5.Vector.sub(this.pos,mover.pos);
    let d = constrain(force.mag(),10,50);
    force.normalize();

    let G = 1;
    let strength = (G * this.mass * mover.mass)/(d*d);
    force.mult(strength);

    return force;
  }

  display(){
    fill(255,200,0);
    circle(this.pos.x,this.pos.y,this.mass);
  }
}

class Mover{
  constructor(x,y){
    this.pos = createVector(x,y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0,0);
    this.mass = random(1,4);
  }

  applyForce(force){
    let f = p5.Vector.div(force,this.mass);
    this.acc.add(f);
  }

  update(){
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display(){
    fill(150,200,255);
    circle(this.pos.x,this.pos.y,this.mass*4);
  }
}
```
![Unidad3_Act3G](https://github.com/user-attachments/assets/ec55d1f4-8d2f-4b7b-9a10-9de3fe430151)
Link p5.js: https://editor.p5js.org/juanpa1103/sketches/Vk3dJ-0HN

## Bitácora de aplicación 
### Actividad 4


## Bitácora de reflexión
### Actividad 5

