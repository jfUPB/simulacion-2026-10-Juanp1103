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


## Bitácora de aplicación 
### Actividad 4


## Bitácora de reflexión
### Actividad 5
