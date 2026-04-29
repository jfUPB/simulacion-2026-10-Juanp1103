# Unidad 7

### Actividad 1

<img width="622" height="618" alt="image" src="https://github.com/user-attachments/assets/623d3636-04a5-455e-a21a-fc98ac13df5f" />

En este caso Ji Lee hace que las propias palabras de tetris formen el juego, agrega los propios sonidos del juego y ademas refuerza todo esto haciendo que la T baje para encajar en un espacio como si se estuviera jugando realmente.

<img width="672" height="484" alt="image" src="https://github.com/user-attachments/assets/bd735c8c-d3c3-477e-8e17-95d59bc90294" />

En este caso es simple pero ji lee nos refuerza semanticamente la palabra ill con una persona acostada tosiendo, se puede ver como la i se medio levanta y reacciona al mismo tiempo que se escucha la tos.

<img width="537" height="478" alt="image" src="https://github.com/user-attachments/assets/49e3004d-1465-486a-a0b1-737805fd8ef6" />

Aqui ji lee, aprovecha la letra E de la palabra zipper para darle forma visualmente a lo que seria el cierre, al mismo tiempo auditivamente se puede escuchar el distintivo sonido de este al cerrar reforzando asi la palabra.

<img width="707" height="487" alt="image" src="https://github.com/user-attachments/assets/ed3a5129-7a0d-4187-a347-fd25bf8b36eb" />

Aunque es muy sutil se puede apreciar como la S de superman esta siendo parcialmente visible como lo podria ser cuando superman se abre la camisa revelando su traje, este fue dificil de percibir en un primer momento pero cuando lo vi me parecio interesante esta forma de conectar la palabra visualmente con el personaje.

Propuestas:

`PEÑON/RISCO`: Estas palabras podrian usarse para que formen la estructura principal de la roca y hacer que la letra inicial de cada palabra esten escalando la piedra.

`MOLINO`: En esta palabra se me ocurre haacer que una de las o tenga aspas de colores, y con el sonido del viento hacer que este gire.

`ESTRUENDO`: Esta palabra se me ocurre hacerla un poco mas reactiva con el usuario, haciendo que cada que se reproduzca un sonido fuerte las letras de las palabras se desordenen y vuelvan para que luego vuelvan a su lugar.

La palabra que mas me llama la atencion puede ser estruendo ya que podemos crear una interaccion mas real donde la persona puede hacer el ruido que las afecta, o haacer que simplemente al presionar una tecla este reproduzca el sonido y que las letras reaccionen a este, seria igual para el molino pero un poco mas sencillo, por lo mismo siento que es mas interesante la palabra estruendo.


## Bitácora de proceso de aprendizaje

### Actividad 2

- Engine: Es el motor físico, el que calcula todo: gravedad, colisiones, velocidad, rebotes.
- World: El contenedor donde viven todos los objetos físicos.
- Bodies: Son los objetos físicos individuales:
  - Cajas.
  - Círculos.
  - Polígonos.

Cada body puede tener propiedades fisicas individuales.

- Constraint: Es una conexión entre cuerpos, sirve para:
  - Unir (como si fueran una cadena).
  - Simular resortes.
  - Mantener distancia entre elementos.
- MouseConstraint: Permite interactuar con el mouse, se puede:
  - arrastrar letras
  - lanzar objetos
  - manipular la palabra en tiempo real

- Experimento 1:

Codigo:
``` js
// Experimento 1: Letras que caen con gravedad
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine, world;
let letras = [];
let suelo, paredIzq, paredDer;
let mConstraint;

let palabra = "FÍSICA";

function setup() {
  createCanvas(600, 500);
  
  // 1. Crear el motor y el mundo
  engine = Engine.create();
  world = engine.world;
  
  // 2. Crear suelo y paredes (estáticos)
  suelo    = Bodies.rectangle(width/2, height-10, width, 20, { isStatic: true });
  paredIzq = Bodies.rectangle(10, height/2, 20, height, { isStatic: true });
  paredDer = Bodies.rectangle(width-10, height/2, 20, height, { isStatic: true });
  World.add(world, [suelo, paredIzq, paredDer]);
  
  // 3. Crear una caja por cada letra
  for (let i = 0; i < palabra.length; i++) {
    let x = 100 + i * 70;
    let y = 50;
    let caja = Bodies.rectangle(x, y, 60, 60, {
      restitution: 0.6,   // rebote
      friction: 0.3
    });
    caja.letra = palabra[i];
    letras.push(caja);
    World.add(world, caja);
  }
  
  // 4. Permitir arrastrar con el mouse
  let canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  mConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse,
    constraint: { stiffness: 0.2 }
  });
  World.add(world, mConstraint);
}

function draw() {
  background(20);
  Engine.update(engine);
  
  // Dibujar suelo y paredes
  fill(80);
  noStroke();
  rect(0, height-20, width, 20);
  rect(0, 0, 20, height);
  rect(width-20, 0, 20, height);
  
  // Dibujar cada letra dentro de su caja física
  for (let caja of letras) {
    push();
    translate(caja.position.x, caja.position.y);
    rotate(caja.angle);
    fill(255, 200, 60);
    rectMode(CENTER);
    rect(0, 0, 60, 60, 8);
    fill(20);
    textAlign(CENTER, CENTER);
    textSize(36);
    textStyle(BOLD);
    text(caja.letra, 0, 0);
    pop();
  }
}
```
<img width="705" height="535" alt="Unidad7_Act2 1" src="https://github.com/user-attachments/assets/c2bed58e-32bb-4d3a-a31a-f28da2a08fb0" />

Enlace: https://editor.p5js.org/juanpa1103/sketches/k5lQ9ZjQH

- Experimento 2:

Codigo:

``` js
// Experimento 2: Letras colgando con Constraint
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Constraint = Matter.Constraint,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine, world;
let letras = [];
let cuerdas = [];
let mConstraint;

let palabra = "MOVER";

function setup() {
  let canvas = createCanvas(600, 500);
  engine = Engine.create();
  world = engine.world;
  
  // Crear cada letra colgando del techo
  for (let i = 0; i < palabra.length; i++) {
    let x = 100 + i * 80;
    let yAnclaje = 30;
    let yLetra = 200;
    
    // Cuerpo de la letra
    let caja = Bodies.rectangle(x, yLetra, 50, 50, {
      restitution: 0.4
    });
    caja.letra = palabra[i];
    letras.push(caja);
    
    // Constraint que la une al techo
    let cuerda = Constraint.create({
      pointA: { x: x, y: yAnclaje },   // punto fijo en el techo
      bodyB: caja,                      // unido al cuerpo
      length: 170,
      stiffness: 0.05                   // elasticidad de la cuerda
    });
    cuerdas.push(cuerda);
    
    World.add(world, [caja, cuerda]);
  }
  
  // Mouse para empujar/jalar las letras
  let canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  mConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse,
    constraint: { stiffness: 0.2 }
  });
  World.add(world, mConstraint);
}

function draw() {
  background(15, 25, 40);
  Engine.update(engine);
  
  // Dibujar cuerdas
  stroke(180);
  strokeWeight(1);
  for (let c of cuerdas) {
    line(c.pointA.x, c.pointA.y, c.bodyB.position.x, c.bodyB.position.y);
  }
  
  // Dibujar puntos de anclaje
  noStroke();
  fill(200);
  for (let c of cuerdas) {
    ellipse(c.pointA.x, c.pointA.y, 8, 8);
  }
  
  // Dibujar letras
  for (let caja of letras) {
    push();
    translate(caja.position.x, caja.position.y);
    rotate(caja.angle);
    fill(120, 200, 255);
    rectMode(CENTER);
    rect(0, 0, 50, 50, 6);
    fill(15, 25, 40);
    textAlign(CENTER, CENTER);
    textSize(30);
    textStyle(BOLD);
    text(caja.letra, 0, 0);
    pop();
  }
}
```
<img width="705" height="535" alt="Unidad7_Act2 2" src="https://github.com/user-attachments/assets/c31bd725-797d-4304-a311-9bf70fd61054" />

Enlace: https://editor.p5js.org/juanpa1103/sketches/-5EHKxczx

Ya que mi palabra es `ESTRUENDO ` y la idea que tenia es que estas se des ordenen y vuelvan despues a su lugar, Para poder lograr esto usaría Bodies para crear cada letra como un cuerpo físico, Constraint con baja rigidez como un resorte invisible que jala cada letra a su posición original, y MouseConstraint para que el espectador pueda provocar el desorden con el mouse en caso de que no se pueda tener acceso al microfono, tambien agregando un nivel extra de interactividad. La gravedad del World iría en cero para que las letras floten y el efecto se concentre en la dispersión.

### Actividad 3

Experimento 1:
- Qué dato leo del audio: la amplitud (volumen general) usando mic.getLevel(), que devuelve un valor entre 0 y 1 según qué tan fuerte está sonando el micrófono en ese instante.
- Qué comportamiento activa: el tamaño y el color de un círculo en pantalla, en este experimento la respuesta es continua, el círculo cambia suavemente todo el tiempo, no espera eventos.

<img width="705" height="535" alt="Unidad7_Act3 1" src="https://github.com/user-attachments/assets/825646b0-5e72-4ac7-a2c9-7d1de4d62a6b" />


Experimento 2:

- Qué dato leo del audio: sigo leyendo amplitud, pero esta vez no la uso de forma continua sino que detecto eventos puntuales: cuando el volumen supera un umbral cuento un "pulso" (un golpe, un grito, un aplauso).
- Qué comportamiento activa: cada pulso detectado dispara un círculo nuevo en una posición aleatoria, que luego se desvanece, en este experimento la respuesta es puntual, el sistema reacciona solo en momentos específicos, no todo el tiempo.

<img width="705" height="535" alt="Unidad7_Act3 2" src="https://github.com/user-attachments/assets/4a927ca2-dc13-452d-ba70-c9b67e3033d3" />

Para mi palabra `ESTRUENDO` me sirve más una respuesta puntual basada en pulsos que una respuesta continua. Una respuesta continua haría que las letras estuvieran vibrando todo el tiempo según el ruido ambiente, lo cual diluye el concepto: un estruendo no es un fondo sonoro constante, es un evento. Lo que define a un estruendo es el golpe, la irrupción brusca que rompe la calma. Por eso me interesa detectar picos de amplitud que superen un umbral (como en el segundo experimento) y usar cada pico para disparar la dispersión de las letras de una sola vez, dejando que después se recompongan en silencio. La amplitud me alcanza como dato porque lo importante no es qué tipo de sonido es (grave, agudo, voz, golpe) sino qué tan fuerte es.

### Actividad 4

1. Prueba inicial:

<img width="1175" height="535" alt="Unidad7_Act4" src="https://github.com/user-attachments/assets/9823f654-a059-4a65-a7aa-4cf2e54d115d" />

Enlace: https://editor.p5js.org/juanpa1103/sketches/qS3He_cf3

2. Construí la palabra completa "ESTRUENDO", donde cada letra es un cuerpo físico independiente. Decidí hacerla entera y no solo un fragmento porque el efecto de dispersión y reorganización necesita varias letras para leerse claramente: con una sola letra no se entiende que la palabra "se rompe", se necesita ver cómo el conjunto pierde y recupera su forma legible.

3. Manipulé principalmente la fuerza aplicada a cada cuerpo `Body.applyForce` para provocar el desorden, y la elasticidad de un Constraint que une cada letra a su posición de reposo. Ese Constraint funciona como un resorte invisible con baja rigidez `stiffness: 0.02`, lo que permite que las letras se alejen al recibir el impulso pero sean jaladas de regreso poco a poco. También dejé la gravedad en cero para que el efecto se concentre en la dispersión y no en la caída, y agregué velocidad angular aleatoria para que las letras roten al dispersarse.

4. Uso la amplitud del micrófono (mic.getLevel()) como dato principal, pero no de forma continua sino como detector de eventos: cuando el nivel supera un umbral después de un momento de silencio, se cuenta como un "pulso" sonoro y se dispara la dispersión. Cada pulso aplica una fuerza aleatoria y una rotación a cada letra; el silencio entre pulsos permite que el resorte del Constraint las regrese a su sitio. La amplitud también afecta el color: las letras se ponen rojas mientras hay sonido fuerte y vuelven a blanco al recomponerse.

5. Lo que funciono y lo que no:
  - Lo que funcionó: la lógica de pulso → reacción → calma se entiende y es coherente con la palabra. Hay un momento de respuesta cuando suena un golpe y luego las letras vuelven hacia su sitio. La elección de detectar eventos de sonido en vez de hacer una respuesta continua fue acertada, porque un estruendo es por definición un evento puntual y no un fondo. Que las letras no caigan al suelo (gravedad cero) también ayuda, porque el desorden se siente como una onda expansiva más que como un derrumbe. Me gusta especialmente que las letras roten al recibir el impacto, porque refuerza la sensación de sacudida.

  - Lo que no funcionó: la dispersión de las letras es demasiado débil, las letras se mueven y rotan pero muy en su lugar, no llegan a desordenarse de verdad, así que la palabra nunca se "rompe" lo suficiente como para que se sienta el estruendo. Las letras también vuelven a su posición pero quedan rotadas, no se enderezan al recomponerse, lo que le quita limpieza al momento de calma y hace que la palabra no se lea bien después del impacto. Y noté un comportamiento extraño con el ruido sostenido: si hago un sonido fuerte y constante las letras pierden movimiento en vez de mantenerse agitadas, como si solo reaccionaran al instante inicial del sonido y luego se quedaran quietas aunque el ruido siga.

Para la pieza final pensaría en aumentar la fuerza del impulso para que la dispersión sea más violenta, corregir la rotación durante el silencio para que las letras vuelvan derechas a su sitio, y hacer que el sonido sostenido también mantenga la agitación, no solo el evento inicial.

## Bitácora de aplicación 

### Actividad 5

1. Palabra elegida: `ESTRUENDO`

2. Justificación conceptual: Elegí `ESTRUENDO` porque es una palabra que nombra un evento sonoro, lo cual la hace ideal para una pieza audiovisual: el medio (sonido del entorno) y el significado (un sonido fuerte y disruptivo) coinciden. La palabra no se limita a describir un ruido cualquiera, implica irrupción, sacudida, ruptura momentánea de la calma. Esa estructura es la que organiza toda la pieza, la pieza no dice "estruendo", la palabra se comporta como un estruendo: existe en reposo, es sacudida por el sonido real, se desordena violentamente y vuelve a recomponerse cuando regresa el silencio.

3. Análisis de su significado visual y comportamental: Un estruendo es breve, súbito y violento. No es un ruido constante: es un evento. Visualmente esto se traduce en estados claramente diferenciados, una transición abrupta entre ellos, y una recomposición que toma más tiempo que la ruptura, igual que en la realidad cuando algo se cae y luego hay que recogerlo.

4. MoodBoard
<img width="1280" height="720" alt="Diseño sin título" src="https://github.com/user-attachments/assets/2b53cf6a-a1c9-436f-8de2-fb1e8233f9e1" />

5. Boceto:

<img width="720" height="1280" alt="Boceto-Unidad7" src="https://github.com/user-attachments/assets/f9282eb0-7b5a-4bc6-902c-b5b161a7f56b" />

6. Mapa de decisiones:

- Tipografía bebas neue: Letras simples y un poco estilizadas, esto le da mas legibilidad a la palabra.
- Fondo negro: profundo, Vacío, silencio visual, el silencio del que irrumpe el estruendo.
- Color blanco en reposo: Da neutralidad, orden, estado de calma.
- Color rojo en impacto: Genera una alerta, urgencia, es un momento de violencia sonora.
- Gravedad cero: Las letras flotan, no caen haciendo que el estruendo sea una onda expansiva, no derrumbe.
- Constraint elástico a posición de reposo: Las letras tienden a volver, la palabra "quiere" reorganizarse.
- Detección de pulso por umbral: Da una reacción a eventos, no a ruido continuo, debido a que un estruendo es un evento puntual.
- Fuerza aleatoria al pulso: Cada letra se dispara distinto, generando un caos genuino, no coreografiado.
- Velocidad angular al pulso: Las letras rotan al volar, refuerza la sensación de sacudida.
- Enderezamiento en silencio: El ángulo vuelve a 0 al recomponerse, genrando una restauración del orden.

7. Mapa de interpretación
La pieza está pensada para ser ejecutada en vivo, haciendo dispocision de 3 entradas:
- Voz / sonido del cuerpo: gritar, aplaudir, golpear el suelo o la mesa cerca del micrófono produce el desorden principal. La intensidad del sonido modula la magnitud de la dispersión.
- Mouse: permite intervenir manualmente arrastrando letras individuales, lo que da control fino para componer momentos sin necesidad de hacer ruido.
- Teclado: la tecla R reinicia la palabra a su posición exacta de reposo (útil si una letra queda atascada), y la tecla F alterna pantalla completa.

8. Relación entre audio y comportamiento
El audio se procesa en dos modos simultáneos:
- Modo evento (pulso): el código detecta el momento en que el volumen pasa de estar bajo el umbral a superarlo. Ese instante específico dispara una fuerza aleatoria fuerte sobre cada letra y una velocidad angular aleatoria. Es el "golpe" del estruendo, ocurre una sola vez por evento sonoro.
- Modo continuo (sostenido): mientras el volumen siga por encima del umbral, se aplica una fuerza adicional, más débil pero constante, que mantiene a las letras agitadas. Esto resuelve el problema de que un sonido sostenido (un grito largo) hiciera que las letras se quedaran quietas.
- En silencio: el Constraint elástico jala cada letra hacia su posición original, y un factor de amortiguación reduce el ángulo gradualmente hacia cero, así las letras vuelven derechas, no rotadas.

9. Usé Claude como apoyo técnico durante todo el proceso: para entender los conceptos básicos de Matter.js (Engine, World, Bodies, Constraint, MouseConstraint), para diagnosticar errores y para iterar sobre el comportamiento del prototipo cuando la dispersión era débil o las letras no se enderezaban.
Decisiones que siguieron siendo mías: la elección de la palabra, la estructura conceptual (calma, golpe, caos, recomposición), la decisión de usar gravedad cero porque un estruendo es onda y no caída, la observación de los problemas del prototipo (dispersión débil, retorno rotado, comportamiento extraño con ruido sostenido), y el diseño de la interpretación performativa.
La IA aceleró la materialización; las decisiones de qué hacer y por qué fueron mías.





## Bitácora de reflexión

### Actividad 6
