# Actividad Medio Término

Proyecto de la actividad de medio término de la materia **Modelación de sistemas multiagentes con gráficas computacionales**, desarrollado en **Unity**.

Se trata de una pequeña escena 3D de tipo *bullet hell*: un cactus controlado por el jugador se mueve por un escenario de bosque *low poly* mientras un generador lanza oleadas de abejas siguiendo distintos patrones geométricos. Un contador en pantalla muestra cuántas abejas hay activas en cada momento.

## Contenido

- [Descripción](#descripción)
- [Requisitos](#requisitos)
- [Cómo ejecutarlo](#cómo-ejecutarlo)
- [Controles](#controles)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Scripts](#scripts)
- [Patrones de abejas](#patrones-de-abejas)
- [Estado actual y pendientes](#estado-actual-y-pendientes)
- [Créditos de recursos externos](#créditos-de-recursos-externos)
- [Licencia](#licencia)

## Descripción

El proyecto modela un sistema de múltiples agentes simples (las abejas). Cada abeja es una instancia del prefab `Larry` que avanza en línea recta a velocidad constante en la dirección en la que fue generada, y se destruye 3 segundos después. El comportamiento global emerge del patrón con el que el `GameController` las genera: espirales, ráfagas circulares y barridos simétricos.

## Requisitos

- **Unity 6** (`6000.1.15f1`), la versión con la que se creó el proyecto. Se recomienda usar exactamente esa versión desde Unity Hub.
- **Universal Render Pipeline (URP) 17.1.0**.
- Paquetes principales (ya declarados en `Packages/manifest.json`, se instalan solos al abrir el proyecto):
  - Input System `1.14.0`
  - TextMeshPro (vía `com.unity.ugui` `2.0.0`)
  - AI Navigation `2.0.8`
  - Timeline `1.8.7`

## Cómo ejecutarlo

1. Clona el repositorio:

   ```bash
   git clone https://github.com/JuanManuelMurilloLopez/Actividad-Medio-Termino.git
   ```

2. Abre **Unity Hub**, pulsa **Add → Add project from disk** y selecciona la carpeta `Medio_Termino_v2`.
3. Abre el proyecto con Unity `6000.1.15f1` y espera a que se importen los recursos (la primera vez puede tardar).
4. Abre la escena `Assets/SimpleLowPolyNature/Scenes/DemoDay.unity`. Es la escena donde están conectados el generador de abejas, el contador y el cactus.
5. Pulsa **Play**.

> **Nota:** la escena que figura en *Build Settings* es `Assets/Scenes/SampleScene.unity`, que no tiene los scripts del juego asignados. Si quieres generar un *build*, añade `DemoDay` a la lista de escenas o conecta los scripts en `SampleScene`.

## Controles

| Acción | Entrada |
| --- | --- |
| Moverse adelante / atrás | `W` / `S` o flechas ↑ / ↓ |
| Moverse a los lados | `A` / `D` o flechas ← / → |

El movimiento usa los ejes `Horizontal` y `Vertical` del `Input Manager` clásico (`Input.GetAxis`).

## Estructura del repositorio

```
.
├── LICENSE
├── .gitignore
└── Medio_Termino_v2/            # Proyecto de Unity
    ├── Assets/
    │   ├── Scripts/             # Código propio del proyecto
    │   ├── Prefabs/             # Larry.prefab (la abeja)
    │   ├── Scenes/              # SampleScene
    │   ├── Settings/            # Configuración de URP (PC y móvil)
    │   ├── RPGMonsterBuddiesPBRPA/  # Recurso externo: cactus y hongo animados
    │   ├── amusedART/               # Recurso externo: Fantasy Bee
    │   ├── SimpleLowPolyNature/     # Recurso externo: escenario low poly (incluye DemoDay)
    │   ├── TextMesh Pro/            # Fuentes y shaders de TextMeshPro
    │   └── TutorialInfo/            # Plantilla de URP de Unity
    ├── Packages/
    └── ProjectSettings/
```

## Scripts

Todos los scripts propios están en `Medio_Termino_v2/Assets/Scripts/`.

| Script | Función |
| --- | --- |
| `GameController.cs` | Genera las abejas según el patrón activo y mantiene el contador estático `beesAlive`. Recibe el prefab de la abeja por `beePrefab`. |
| `BeeHappy.cs` | Mueve cada abeja hacia adelante (`Vector3.forward`) a una velocidad configurable (`speed`, por defecto `5`). Está en el prefab `Larry`. |
| `BeesBeeing.cs` | Actualiza un `TMP_Text` con el texto `Número de abejas: N` leyendo `GameController.beesAlive`. |
| `cactusVictor.cs` | Controla el movimiento del jugador con los ejes `Horizontal` y `Vertical` (`forwardSpeed` y `sideSpeed`, por defecto `5`). |
| `evilMushroom.cs` | Plantilla vacía de `MonoBehaviour`; todavía no implementa comportamiento. |

## Patrones de abejas

El `GameController` avanza un temporizador y cambia de patrón cada 10 segundos. Todas las abejas nacen en la posición del generador y se destruyen a los 3 segundos.

| Tiempo | Patrón | Comportamiento |
| --- | --- | --- |
| 0 – 10 s | `pattern1` | Cada 0.3 s lanza 5 abejas separadas 72° entre sí (un pentágono), y rota el ángulo base 10° en cada disparo, formando una espiral. |
| 10 – 20 s | `pattern2` | Cada 2 s lanza una ráfaga de 50 abejas repartidas uniformemente en 360°. |
| 20 – 30 s | `pattern3` | Cada 0.3 s lanza 2 abejas en direcciones espejo y avanza el ángulo base 15° en cada disparo. |

Pasados los 30 segundos no se generan más abejas.

## Estado actual y pendientes

Esto es lo que el código tiene hoy, para que sea claro qué está hecho y qué no:

- [x] Movimiento del jugador (cactus).
- [x] Generación de abejas en tres patrones temporizados.
- [x] Contador de abejas activas en pantalla.
- [ ] Detección de colisiones entre abejas y jugador (el prefab `Larry` no tiene colisionadores).
- [ ] Comportamiento del hongo (`evilMushroom.cs` está vacío).
- [ ] Reiniciar el ciclo de patrones (actualmente, tras 30 s la generación se detiene).
- [ ] Escena principal propia en *Build Settings*.

## Créditos de recursos externos

El proyecto incluye recursos de terceros dentro de `Assets/`. Cada uno conserva sus propios términos de uso; revísalos antes de redistribuir o publicar un *build*.

- **RPG Monster Buddies (PBR / PolyArt)** – modelos y animaciones del cactus y del hongo (`Assets/RPGMonsterBuddiesPBRPA`).
- **Fantasy Bee** – modelo y animaciones de la abeja (`Assets/amusedART/Fantasy Bee`).
- **Simple Low Poly Nature** – escenario del bosque (`Assets/SimpleLowPolyNature`).
- **TextMesh Pro** – fuentes y shaders de texto (`Assets/TextMesh Pro`).
