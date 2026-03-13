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



### Actividad 6


### Actividad 7


### Actividad 8


### Actividad 9


### Actividad 10



## Bitácora de aplicación 

### Actividad 11



## Bitácora de reflexión

### Actividad 12
