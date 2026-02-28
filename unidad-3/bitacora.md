# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 1
Me parecio impresionante como con el pasar del tiempo nos mostraba el avance que estaba teniendo con la ia, viendolo de un punto de vista mas artistico me parece impactante y por un lado un poco triste de pensar que la creatividad se la estamos dejando caada vez mas a una inteligencia artificial, pero al mismo tiempo asombrado por lo que se podria llegar a generar, siento que las obras generadas por ia pueden ser impactantes pero al mismo tiempo no me gustaaria comenzar a ver "obras" donde la intervencion humana es minima, siento que se perderia el alma de la obra, ya no esta interviniendo la imaginacion de un humano mas alla de simplemente escribir un prompt, entiendo que puede ser dificil conseguir ciertos resultados, y no quiero decir que lo que algunos artistas hacen con ia valgan menos pero a fin de cuentas le dejas todo el trabajo de creacion a una maquina que no entiende como vemos realmente el mundo.

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
Explicacion:
La idea que tenia detras de esta obra era poder ver un poco de la basura espacial presente alrededor de la tierra, en este caso no es una obra que busque como concientizar sobre este problema sino mas una forma de visualizar en una muy pequeña escala el como se puede ver y sentir como esta interaccion, a diferencia de como se podria comportar en la realidad aca podemos modificar variables que nos crean un caos mayor.

Historia: En una tierra donde los humanos ya se extinguieron dejaron rastros visibles, entre estos rastros queda la basura espacial, haciendo casi imposible que otra civilizacion obvie el claro avance tecnologico que una vez tuvo la raza humana, ahora ese caos constante mantiene las diferentes razas que quieren llegar a la tierraalejadas, por lo menos hasta que estas caigan a la superficie.

Codigo:
```
let bodies = [];
let G = 0.6;
let galaxyRadius;
let gravityDirection = 1;

let centralStar;

function setup() {
  createCanvas(900, 900);
  galaxyRadius = width * 0.42;
  resetSystem();
}

function resetSystem() {
  bodies = [];

  centralStar = new Body(0, 0, 2000, true);
  bodies.push(centralStar);

  for (let i = 0; i < 45; i++) {
    let angle = random(TWO_PI);
    let r = random(120, galaxyRadius - 40);

    let x = cos(angle) * r;
    let y = sin(angle) * r;

    let m = random(14, 40);
    let planet = new Body(x, y, m);

    // velocidad orbital estable
    let orbitalSpeed = sqrt((G * centralStar.mass) / r);
    let tangent = createVector(-sin(angle), cos(angle));
    tangent.setMag(orbitalSpeed);

    planet.vel = tangent;

    bodies.push(planet);
  }
}

function draw() {
  background(5, 5, 20);
  translate(width / 2, height / 2);

  drawBoundary();

  let frictionStrength = map(mouseX, 0, width, 0.0001, 0.01);

  for (let body of bodies) {

    if (!body.isStar) {

      // SOLO gravedad del sol
      let force = gravitationalForce(body, centralStar);
      body.applyForce(force);

      // fricción muy leve
      let friction = body.vel.copy();
      friction.mult(-1);
      friction.setMag(frictionStrength);
      body.applyForce(friction);

      body.update();
      contain(body);
    }

    body.display();
  }
}

function gravitationalForce(a, b) {
  let dir = p5.Vector.sub(b.pos, a.pos);
  let d = constrain(dir.mag(), 40, 600);

  let strength = (G * a.mass * b.mass) / (d * d);
  dir.setMag(strength * gravityDirection);

  return dir;
}

function contain(body) {
  let d = body.pos.mag();

  if (d > galaxyRadius) {
    let inward = body.pos.copy().mult(-1).setMag(0.5);
    body.applyForce(inward);
  }
}

function drawBoundary() {
  noFill();
  stroke(100, 120, 255, 40);
  circle(0, 0, galaxyRadius * 2);
}

function mousePressed() {
  gravityDirection *= -1;
}

function keyPressed() {
  if (key === 'r' || key === 'R') resetSystem();
}

class Body {
  constructor(x, y, m, isStar = false) {
    this.pos = createVector(x, y);
    this.vel = createVector();
    this.acc = createVector();
    this.mass = m;
    this.isStar = isStar;

    this.radius = sqrt(this.mass) * 1.4;
    this.baseColor = color(
      random(120, 255),
      random(120, 255),
      random(120, 255)
    );
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display() {
    noStroke();

if (this.isStar) {

  push();
  translate(this.pos.x, this.pos.y);
  noStroke();

  //  Atmósfera exterior
  for (let r = this.radius * 1.8; r > this.radius; r -= 3) {
    let alpha = map(r, this.radius, this.radius * 1.8, 120, 0);
    fill(100, 180, 255, alpha);
    circle(0, 0, r * 2);
  }

  //  Océano base con degradado
  for (let r = this.radius; r > 0; r -= 2) {
    let inter = map(r, 0, this.radius, 0, 1);
    let oceanColor = lerpColor(
      color(20, 60, 160),
      color(0, 20, 80),
      inter
    );
    fill(oceanColor);
    circle(0, 0, r * 2);
  }

  //  Continentes orgánicos usando ruido
  fill(40, 160, 70, 220);
  beginShape();
  let t = millis() * 0.0003;
  for (let a = 0; a < TWO_PI; a += 0.2) {
    let noiseFactor = noise(cos(a) + 1, sin(a) + 1, t);
    let r = this.radius * 0.75 * noiseFactor;
    vertex(cos(a) * r, sin(a) * r);
  }
  endShape(CLOSE);

  //  Nubes suaves
  fill(255, 40);
  ellipse(-this.radius * 0.3, -this.radius * 0.2,
          this.radius * 0.9, this.radius * 0.5);
  ellipse(this.radius * 0.2, this.radius * 0.3,
          this.radius * 0.7, this.radius * 0.4);

  pop();
} else {
      push();
      translate(this.pos.x, this.pos.y);

      for (let r = this.radius; r > 0; r -= 2) {
        let inter = map(r, 0, this.radius, 0, 1);
        let c = lerpColor(this.baseColor, color(0), inter);
        fill(c);
        circle(0, 0, r * 2);
      }

      fill(255, 40);
      ellipse(-this.radius * 0.3, -this.radius * 0.3,
              this.radius * 0.6, this.radius * 0.4);

      pop();
    }
  }
}
```
Link p5.js: [https://editor.p5js.org/juanpa1103/sketches/TH2qd6nfA](https://editor.p5js.org/juanpa1103/sketches/4CzwdgMjV)
![Unidad3_Act4Arte2](https://github.com/user-attachments/assets/fa7f85f0-0baa-4ef7-8482-186f929504d3)


## Bitácora de reflexión
### Actividad 5
1. En el marco de motion 101 nos sirve o mas bien nos explica a como simular el movimiento y las fisicas que lo compone, en este fundamento nos muestra como podemos usar los vectores, donde la posición de un objeto no se modifica directamente, sino que emerge como consecuencia de una cadena de relaciones físicas: las fuerzas aplicadas generan aceleración, la aceleración modifica la velocidad, y la velocidad actualiza la posición.

2. La obra que quiero tomar como referente de las que aparecen en el video es la siguiente:
<img width="1447" height="1299" alt="image" src="https://github.com/user-attachments/assets/f5fa0c79-5551-4dde-8c63-5ebe1c6b69c9" />

Tomo esta ya que me parece muy interesante como el movimiento de una orbita afecta a la orbita que esta en uno de los extremos de la bara y el efecto que se desencadena.

Codigo:
```
let root;
let wind;

function setup() {
  createCanvas(900, 700);
  root = new Orbiter(width / 2, height / 2, 120, 5);
}

function draw() {
  background(130);

  wind = map(mouseX, 0, width, -0.003, 0.003);

  root.applyWind(wind);
  root.update();
  root.display();
}


class Orbiter {
  constructor(x, y, radius, depth) {
    this.origin = createVector(x, y);
    this.radius = radius;

    this.angle = random(TWO_PI);
    this.aVelocity = random(-0.009, 0.001);
    this.aAcceleration = 0;

    this.mass = random(1, 3);
    this.child = null;

    if (depth > 0) {
      this.child = new Orbiter(0, 0, radius * 0.8, depth - 1);
    }
  }

  applyWind(force) {
    this.aAcceleration += force / this.mass;

    if (this.child) {
      this.child.applyWind(force * 1.4);
    }
  }

  update() {
    // gravedad angular suave
    this.aAcceleration += -sin(this.angle) * 0.00005;

    this.aVelocity += this.aAcceleration;
    this.aVelocity *= 0.95; // fricción leve
    this.angle += this.aVelocity;

    this.aAcceleration = 0;

    if (this.child) {
      this.child.update();
    }
  }

  display() {
    push();
    translate(this.origin.x, this.origin.y);

    let x = cos(this.angle) * this.radius;
    let y = sin(this.angle) * this.radius;

    stroke(0);
    line(0, 0, x, y);

    fill(40, 90, 180);
    noStroke();
    circle(x, y, 15);

    if (this.child) {
      this.child.origin = createVector(x, y);
      this.child.display();
    }

    pop();
  }
}
```
![Unidad3_Act5](https://github.com/user-attachments/assets/8c627057-11d4-4996-b149-9d92fea9d0a5)

Link p5.js: https://editor.p5js.org/juanpa1103/sketches/JH_QjCHbW

