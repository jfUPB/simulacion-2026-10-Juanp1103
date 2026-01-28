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
 


