# DEV25-G03-P2
Repositorio creado por el gruopo G03 para el proyecto de Aprendiz de Brujo

# Punto de partida
Este proyecto está basado con el Unreal Engine 5.6 partiendo de la plantilla Top Down (sin variantes) que ofrece Unreal Engine, que recrea un mundo 3D con vista cenital en tercera persona, con el movimiento del avatar controlado por el ratón. Además hemos usado contenido del paquete Starter Content y de la tienda Fab, y hemos usado la biblioteca de Quixel Megascans para añadir objetos o el recurso gratuito [Stylized Fantasy Provencal](https://www.fab.com/listings/ced19ea1-31ed-437f-ae64-2b6b1561fede)

# Aprendiz de Brujo
Este proyecto consiste en desarrollar el prototipo ejecutable de un videojuego para un sólo jugador en forma de aventura con pequeños puzles físicos basados en realizar «mágicamente» tareas domésticas, tomando la inspiración de la película de Fantasía. Es un juego de puzles, sin enemigos ni posibilidad de recibir daño o morir.

# Instalación y uso

Los ficheros más importantes del proyecto están disponible en este repositorio, aunque puede que algunos binarios potencialmente grandes estén en el almacén GitHub LFS y se requiera tener activa la extensión Git LFS. El resto de los ficheros, generalmente de contenido más pesado o creado por terceros y sin intención de ser modificado en este proyecto, tendrá que descargarse de carpetas compartidas en [Google Drive](https://drive.google.com/drive/folders/1TfoB5S3yQw49-onoFfn0q79PTfk2RoSE) con ficheros ZIP, para después descomprirlos directamente en la carpeta Content.

Para este proyecto hace falta descargar los ficheros ZIP:

------------------------------------------------------------------cut from here--------------------------------------

- Characters
- LevelPrototyping
- ThirdPerson
- Variant_Platforming

Y luego extraerlos dentro del archivo **Content**
 
# Preproducción

El diseño tiene estas secciones:

- [Estetica](#Estetica)
    - [Grafico](#Grafico)
    - [Sonido](#Sonido)
- [Dinamica](#Dinamica)
    - [Objetivo](#Objetivo)
    - [Castigo](#Castigo)
- [Mecanica](#Mecanica)
- [Contenido](#Contenido)
    - [Zona A](#Zona-A)
    - [Zona B](#Zona-B)
    - [Zona C](#Zona-C)

# Estetica

El entorno virtual se basa en la plantilla Third Person que ofrece Unreal Engine, que recrea un mundo 3D con vista en tercera persona, a través de una cámara que sigue al avatar; además se incluye el contenido del paquete Starter Content. 

## Grafico

El juego usa solamente el contenido de la plantilla proporcinada por el profesor.

## Sonido

Debido a la falta de tiempo, hemos decidido a no implenmentar sonido en este poyecto.

# Dinamica

La dinámica del juego se basa en resolver pequeños puzles dentro de un juego con simulación física: manejamos al aprendiz en las inmediaciones del caserón del brujo y tendremos que usar nuestros poderes para realizar todas las tareas domésticas que nos ha encargado el maestro.

```mermaid
flowchart TD
    %% --- Nodos principales ---
    Start([start])
    T1[Tarea 1]
    T2[Tarea 2]
    T3[Tarea 3]
    Victory([Victory])

    %% --- Rama izquierda ---
    Start --> T1
    T1 --> ZH1[Zona Hunted]
    ZH1 --> ST1[[step on tails<br>red + green]]
    ST1 --> ZM1[Zona Meadow]
    ZM1 --> LS[[Levitation spell<br>Repair car]]
    LS --> TC1([Task completed])

    %% --- Rama central ---
    T1 --> T2
    T2 --> ZH2[Zona Hunted]
    ZH2 --> ST2[[step on tails<br>red + blue]]
    ST2 --> ZM2[Zona Mountain]
    ZM2 --> PS[[Propulsion spell<br>Load barrels]]
    PS --> BC{Barrel colision<br>+3}
    BC --> |SI| ZM2
    BC --> |NO| TC2([Task completed])

    %% --- Rama derecha ---
    T2 --> T3
    T3 --> ZH3[Zona Hunted]
    ZH3 --> ST3[[step on tails<br>Blue + green]]
    ST3 --> ZM3[Zona Meadow]
    ZM3 --> DS[[Driving spell<br>Transport barrels]]
    DS --> GW{Get out of the way}
    GW --> |SI| ZM2
    GW --> |NO| TC3([Task completed])
    T3 --> Victory

    %% --- Estilo (blanco y negro) ---
    classDef default stroke:#000,fill:#fff,color:#000,stroke-width:1px;

    %% Flechas del flujo principal
    linkStyle 0 stroke:red,stroke-width:5px;
    linkStyle 6 stroke:red,stroke-width:5px;
    linkStyle 14 stroke:red,stroke-width:5px;
    linkStyle 22 stroke:red,stroke-width:5px;

    %% Flechas del tarea terminada o condicional si/no
    linkStyle 12 stroke:orange,stroke-width:2px;
    linkStyle 20 stroke:orange,stroke-width:2px;

    linkStyle 5 stroke:green,stroke-width:2px;
    linkStyle 13 stroke:green,stroke-width:2px;
    linkStyle 21 stroke:green,stroke-width:2px;

    %% Flechas del flujo principal
    style ST1 fill:yellow,stroke:black,stroke-width:2px
    style ST2 fill:purple,stroke:black,stroke-width:2px
    style ST3 fill:cyan,stroke:black,stroke-width:2px
```

## Objetivo

El objetivo del juego es pasar por todas las pruebas hasta llegar a la corazón del castillo.

## Castigo

El jugador puede morirse en el caso de ser golpeado por barriles o balas de cañon, o caido en la agua. Cuando esto pase, volverá al inicio del nivel o al checkpoint depende de la zona que está el jugador.
Posible mejora (a evaluar)
Suponiendo parametro de vida, si el avatar muere supondria "game over" y deberia reiniciar desde el principio.

# Contenido

A continuación se muestra los componentes del juego.

## Avatar(caracter controlado por el jugador)

El clásico maniquí de Unreal Engine que se puede mover y saltar es el avatar que controla el jugador.

## Pastillas energéticas

Son unos objetos de forma pastilla, cual dará una habilidad de super salto, y se desaparece una vez sido tomada.

## El gran lingote dorado

Es el objeto con cual se gana el jugador.

# Contenido

A continuación se muestra las escenas del juego.

## Zona A

La primera zona del juego es un exterior con agua y consiste en cruzar el foso del castillo dando saltos sobre gruesos troncos que flotan sobre el agua, moviéndose lentamente bajo nuestros pies. Si el avatar cae al agua, muere pero reaparece al principio

```mermaid
flowchart LR
    A(["Start"]) --> C@{ label: "<span style=\"box-sizing:\">El foso del castillo</span>" }
    C --> D["La muralla del castillo"]
    C@{ shape: rect}
```

## Zona B

La segunda zona consiste en escalar la muralla del castillo aprovechando sus salientes, siendo necesario el uso de pastillas para saltar entre ciertas plataformas. Aunque prime la verticalidad, conviene dividir la escalada en partes, de modo que si el avatar cae, sólo tiene que repetir la escalada de la última parte. Algún saliente estará dañado y se romperá si el avatar lo pisa.

```mermaid
flowchart LR
    D["La muralla del castillo"] --> n1["Escalares o truncos que direge hacia la correzáon el castillo"]
    n1 --> n2["El corrazón del castillo"]
```

## Zona C

La tercera y última zona es un interior y consiste en atravesar las estancias interiores hasta el corazón del castillo, con varias puertas y algunas trampas. No hay “enemigos” como tales, sólo objetos con movimiento que resultan molestos e incluso mortales para el jugador, y las llaves de las puertas mencionadas anteriormente. Si el avatar muere, será necesario repetir la zona. Finalmente al coger el gran lingote dorado en el interior del castillo, la partida termina.

```mermaid
flowchart LR
    n2["El corrazón del castillo"] --> n3["Laberinto"]
    n3 --> n4["El gran lingote dorado, dentro de todo"]
```

# Referencia

[Fūun! Takeshi Jō]( https://narratech.com/es/desarrollo-de-videojuegos-25-26/)

[Castle - Base](https://github.com/narratech/castle-base)

# Video demo
https://drive.google.com/drive/folders/14ALIV9pRV7xGzeA53S0Z2yKjn5nXuOZI?usp=sharing

El enlace contiene videos de visibilidad de todos los escenarios y videos de jugabilidad

