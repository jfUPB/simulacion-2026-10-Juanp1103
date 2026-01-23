# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 2
En la siguiente seccion de codigo se modificara los valores de choice que se evaluan para dar una direccion de movimiento

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

Hipotesis:
- Los valores se cambian a "0", se espera que el movimiento sea estatico ya que los valores se estarian cancelando entre si
Resultado:
- El movimiento fue diagonal acendente hacia la derecha, 


## Bitácora de aplicación 



## Bitácora de reflexión
