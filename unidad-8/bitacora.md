# Unidad 8

## Bitácora de proceso de aprendizaje

### Actividad 1

1. Herramienta elegida: `Unity`

La elijo porque es la herramienta con la que ya tengo continuidad y porque me permite reconstruir los sistemas del curso en un entorno donde la lógica de "cada frame el sistema se actualiza" es nativa al motor. Lo que en p5.js es draw() en Unity es Update().

2. Relación con mi línea de énfasis:

Mi énfasis es videojuegos y Unity es el motor con el que ya he trabajado. Esta unidad me sirve para llevar lo que aprendí del curso al entorno donde realmente voy a trabajar profesionalmente. En videojuegos, los sistemas generativos no son decoración: son efectos visuales, comportamientos de enemigos, partículas reactivas al gameplay, materiales procedurales. Hacer este puente ahora me deja un proyecto que es a la vez pieza de la unidad y demo técnica para portafolio.

3. Referentes:

- Keijiro Takahashi (Unity Technologies Japan): Su GitHub tiene cientos de experimentos en Unity centrados en VFX Graph, Compute Shaders y audio reactivo, proyectos como Smrvfx (emisión de partículas desde mallas animadas), SdfVfxSamples (campos de distancia con partículas) y SplatVFX (Gaussian Splatting).

- Tundra Collective: Aunque trabajan con TouchDesigner, los incluyo porque su forma de entender la pieza generativa como "instrumento interpretable" y no como video pregrabado me sirve como brújula conceptual.

- NONOTAK Studio: Demuestran que un vocabulario visual mínimo bien parametrizado sostiene una pieza completa, lo cual es relevante porque en Unity la tentación de sobrecargar la escena es alta.

4. Qué me interesa de ellos: De Keijiro me interesa la forma de experimentar: piezas pequeñas, muy enfocadas, cada una explorando un solo problema técnico a fondo. Es lo opuesto al proyecto grande que se cae bajo su propio peso, y es probablemente la mejor estrategia para esta unidad. De Tundra y NONOTAK me interesa la actitud frente a la pieza y la disciplina compositiva con pocos elementos.

5. Contexto profesional: Portafolio. La pieza final será una demo técnica/visual en Unity que reconstruye uno de los sistemas del curso usando las herramientas del motor VFX Graph y/o shader Graph para mostrar tanto la comprensión del sistema como la capacidad de implementarlo en el entorno donde voy a trabajar profesionalmente.


### Actividad 2

1. Sistema a transferir:

Flow field con agentes autónomos, en modalidad audio-reactiva, traducido a Unity (VFX Graph + LASP).

2. Cómo funcionaba en p5.js

El sistema tenía dos capas comunicadas a través de una rejilla:

  - La capa del campo era una rejilla bidimensional donde cada celda guardaba un vector que apuntaba en alguna dirección. Esos vectores se generaban a partir de ruido de Perlin: para cada celda `(col, fila)` se pedía un valor de `noise(col * escala, fila * escala, tiempo)` y ese valor se interpretaba como un ángulo, del que se obtenía un vector unitario con `p5.Vector.fromAngle()`. El parámetro tiempo se incrementaba cada frame, lo que hacía que el campo entero respirara y mutara en lugar de quedarse congelado.

  - La capa de los agentes era un arreglo de objetos cada uno con posición, velocidad, aceleración y velocidad máxima que en cada frame hacían lo mismo: mirar en qué celda del campo están, leer el vector de esa celda, usarlo como fuerza de dirección `steering = deseado - velocidad`, aplicarlo como aceleración, actualizar velocidad y posición, y dibujarse.

El resultado es ese comportamiento de "líneas que fluyen como humo o como corrientes de viento": muchos agentes individualmente tontos producen una imagen global muy orgánica porque comparten un campo que varía suavemente en el espacio.

4. Por qué transferirlo a Unity

Por tres razones, una conceptual y dos técnicas:

  - Conceptualmente, el flow field separa con claridad "estructura del mundo" de "comportamiento del agente". Esa separación es la misma que usan los videojuegos todo el tiempo, el mundo expone información, los agentes la leen, así que reconstruirlo en Unity no es solo traducirlo, es aprender a pensarlo en el lenguaje del motor.

  - Técnicamente, es el sistema del curso que mejor aprovecha lo que Unity hace bien y p5.js hace mal: meter el campo en una textura, dejar que decenas de miles de partículas lo lean en paralelo en GPU, y olvidarse del bucle de CPU. En p5.js, 500 agentes ya hacían caer los FPS; en Unity con VFX Graph leyendo una RenderTexture puedo apuntar a decenas de miles sin pelear.

  - Específicamente para el contexto de pieza para concierto, el flow field es ideal porque tiene dos puntos de entrada para el audio muy claros y separados: el campo mismo y el comportamiento de los agentes. Esto permite que la pieza tenga "graves que mueven el mundo" y "agudos que excitan a los agentes" como dos canales expresivos independientes, en lugar de un único parámetro reaccionando a todo el audio.

4. Pieza visual que imagino

Una visual para un set de música electrónica, con estética inspirada en Tron: fondo negro, partículas como trails brillantes en cian, magenta y naranja-rojo, bloom fuerte tipo neón.
El sistema: un flow field 3D con curl noise dentro de un volumen, recorrido por entre 20.000 y 50.000 partículas que siguen el campo. Reacciona al audio en tres canales con LASP (Plugin desarrollado por keijiro):

Graves: mueven el campo, el mundo se contrae y expande con el kick.
Medios: modulan la velocidad de las partículas.
Agudos: disparan emisión y brillo.

Estructura en 4 secciones de ~1 minuto: 
- despertar, campo quieto, solo cian.
- densidad, turbulencia, entra el magenta.
- colapso alrededor de un atractor, genera tensión, aparece el naranja.
- liberación, todo abierto, máxima energía y fade final.

5. Dificultades técnicas que anticipo

- Generar el campo y meterlo en una textura 3D. En p5.js el campo era un arreglo de vectores 2D. En Unity necesito una Texture3D donde cada texel codifica un vector en RGB.
- Tengo que decidir si la genero en CPU con C#, o en GPU con un compute shader. El curl noise tampoco está como nodo directo en Shader Graph: hay que construirlo a partir de derivadas del Perlin.
- Mapeo audiovisual sin que se sienta trivial: El error fácil es conectar amplitud a tamaño y que todo "lata al ritmo" de forma obvia y aburrida. Lo difícil y donde se juega la calidad de la pieza es que el audio modifique parámetros estructurales en lugar de parámetros cosméticos.
- Rendimiento con bloom + trails + 30k partículas. Cada uno de estos tres es barato; los tres juntos en HDRP pueden bajar los FPS. Voy a tener que perfilar y probablemente bajar trails antes que bajar cantidad de partículas, porque visualmente las líneas son la pieza.


### Actividad 3

1. Componentes y módulos que necesito aprender:

Para reconstruir el sistema en Unity identifiqué cinco bloques mínimos:

- Render pipeline: HDRP. Lo elijo sobre URP porque la pieza depende fuertemente de bloom y postprocesado para la estética Tron, y porque VFX Graph está totalmente soportado en HDRP, mientras que en URP sigue marcado como preview con un subconjunto de plataformas y limitaciones.

- VFX Graph: Es el equivalente de mi sistema de partículas de p5.js, pero corriendo en GPU. La diferencia clave: en lugar de un bucle `for` que recorre agentes, hay tres contexts `Initialize`, `Update`, `Output` donde describo las reglas que aplican a todas las partículas en paralelo. Necesito aprender los bloques Set Position, Set Velocity, Turbulence, Kill, Output Particle Line para tener el sistema mínimo.

- Bloque Turbulence: Resuelve la generación del flow field y el movimiento de las partículas en un solo paso. En p5.js yo generaba la rejilla y los agentes la leían en dos pasos separados; aquí está unificado. Esto es bueno y malo para empezar lo voy a usar como caja negra y si necesito el campo separado más adelante, lo construyo con compute shader.

- LASP + LaspVfx: Es el puente entre audio y visual. LASP captura audio del sistema con baja latencia y lo descompone en bandas (graves, medios, agudos); LaspVfx provee los Property Binders que conectan esos valores con parámetros expuestos del VFX Graph. Sin esto, la pieza no sería audio-reactiva.




## Bitácora de aplicación 


## Bitácora de reflexión
