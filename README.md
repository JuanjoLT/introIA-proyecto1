# Proyecto 1 — Análisis del desempeño de jugadores de League of Legends

Proyecto del curso **Introducción a la Inteligencia Artificial**.

## Descripción

Análisis estadístico, exploratorio y predictivo de los factores que determinan
la habilidad y el desempeño de jugadores de *League of Legends*. El objetivo es
identificar qué variables influyen en el posicionamiento calificatorio de un
jugador y evaluar la capacidad de **predecir su ELO**.

> **ELO:** término usado para representar el rango calificado de un jugador.
> Ejemplo: *high elo* = jugador de alto desempeño.

## Objetivos

El objetivo principal de esta invertigación es identificar las variables que más afectan en el rango/ELO de los jugadores, para que de esta manera puedan centrarse en mejorar dichas variables y así lograr subir su rango de manera más rápida y eficiente. Con esto, se proponen los siguientes objetivos específicos:

- Análisis exploratorio de datos (EDA) sobre estadísticas de jugadores y partidas.
- Identificar los factores que más influyen en el rango/ELO.
- Construir un modelo predictivo del ELO de un jugador.

## Dataset

Fuentes (Kaggle):

[League of Legends player statistics](https://www.kaggle.com/datasets/makspl/league-of-legends-player-statistics)

El dataset incluye los datos de las últimas 25 partidas jugadas de aproximadamente 2200 jugadores, extraidos de la API pública de Riot Games (Empresa creadora de League of Legends). Los datos del dataset incluyen información relevante para analizar el nivel de los jugadores, determinar cuáles son las estadisticas que más influyen dentro de las partidas y finalmente, poder predecir el ELO.

## Atributos del Dataset

- **summonerName**: Nombre de usuario del jugador.

- **summonerLevel**: Experiencia acumulada a lo largo de múltiples partidas de League of Legends. Es diferente al nivel del campeón dentro de una partida; puede considerarse como una medida del tiempo total que el jugador ha dedicado al juego.

- **rank**: Sistema de clasificación que separa a los jugadores en diferentes divisiones o rangos. Es un indicador del nivel de habilidad del jugador.

- **wins**: Cantidad de partidas ganadas de las últimas 25 partidas jugadas.

- **losses**: Cantidad de partidas perdidas de las últimas 25 partidas jugadas.

- **winRate**: Número de victorias dividido por el total de partidas jugadas (25).

- **kills**: Promedio de asesinatos obtenidos durante las últimas 25 partidas.

- **deaths**: Promedio de muertes registradas durante las últimas 25 partidas.

- **assists**: Promedio de asistencias, es decir, la cantidad de veces que el jugador ayudó a eliminar a un enemigo.

- **prefLane**: Línea o rol más jugado durante las últimas 25 partidas. (ADC y SUPPORT juegan juntos en la línea inferior o *bottom lane*).

- **campsKilled**: Cantidad promedio de monstruos de la jungla eliminados.

- **minionsKilled**: Cantidad promedio de súbditos de línea eliminados.

- **goldEarned**: Promedio de oro acumulado por partida.

- **turretTakedowns**: Promedio de torretas destruidas por el jugador en cada partida. No corresponde al total de torretas destruidas en una partida completa.

- **visionScore**: Puntuación promedio relacionada con revelar zonas ocultas del mapa y destruir guardianes de visión enemigos.

- **dragonKills**: Promedio de dragones eliminados por el jugador. Derrotar dragones otorga mejoras adicionales al equipo, como más daño o más vida.

- **longestTimeSpentLiving**: Mayor tiempo que el jugador permaneció con vida, medido en segundos.

- **totalDamageDealt**: Promedio del daño total infligido a campeones enemigos.

- **totalDamageTaken**: Promedio del daño total recibido de campeones enemigos.

- **gameDuration**: Duración promedio de una partida, medida en minutos.

- **gameStart**: Hora promedio en la que el jugador comienza sus partidas, expresada en horas. Por ejemplo, `15.06` representa aproximadamente las `15:04`.

## Estructura del repositorio

```
introIA-proyecto1/
├── main.ipynb              # Notebook principal: EDA, análisis y modelado
├── LeaguePlayerStats.csv   # Dataset (estadísticas de jugadores)
├── environment.yml         # Dependencias del entorno (conda)
├── requirements.txt        # Dependencias del entorno (pip)
├── .gitignore
└── README.md
```

## Metodología

El flujo de trabajo completo está documentado en [main.ipynb](main.ipynb) y sigue
estas etapas:

### 1. EDA (Análisis Exploratorio de Datos)
- Revisión de dimensiones, tipos de datos y estadísticas descriptivas.
- `countplot` de la variable objetivo (`rank`) para verificar el balance de clases.
- Promedios de estadísticas clave (`minionsKilled`, `gameDuration`, `visionScore`)
  segmentados por rango, visualizados con *scatterplots*.

### 2. Feature engineering
- **Codificación ordinal** de `rank` → `rank_encoded` (IRON=0 … MASTER=7),
  respetando el orden real de los rangos del juego.
- **One-hot encoding** de `prefLane` (rol), ya que influye en las estadísticas.
- Eliminación de `summonerName` por no aportar información predictiva.
- Creación de una variable agrupada en **3 tiers** (Bajo / Medio / Alto) como
  objetivo alternativo.

### 3. Feature selection
- **Matriz de correlación** para detectar relaciones entre variables y con el
  objetivo, identificando redundancias (p. ej. `winRate`↔`wins`↔`losses`).
- Análisis de las variables más correlacionadas con el rango, en general y
  **por rol** (clave para entender qué importa en cada posición).

### 4. Reducción de dimensionalidad
- **PCA** sobre las variables estandarizadas (`StandardScaler`) para visualizar la
  separación de rangos y medir cuánta varianza explica cada componente.

### 5. Entrenamiento
- Modelo principal: **Random Forest** (`RandomForestClassifier`).
- Búsqueda de hiperparámetros con **`GridSearchCV`** (`n_estimators`, `max_depth`,
  `criterion`) usando un `Pipeline`.
- Se evaluaron además HistGradientBoosting, GradientBoosting, Regresión Logística
  y SVM para comparar; ninguno superó significativamente al Random Forest.

### 6. Control de overfitting
- **Validación cruzada** (`cv=5`) dentro de `GridSearchCV`.
- Limitación de la profundidad de los árboles (`max_depth`).
- Evaluación siempre sobre un **conjunto de prueba independiente**
  (`train_test_split`, 80/20) no visto durante el entrenamiento.

### 7. Testeo y métricas
- **Accuracy**, **classification report** (precision / recall / f1 por clase) y
  **matriz de confusión**.
- Métrica complementaria **±1 rango** (aciertos dentro de un rango de distancia),
  adecuada por la naturaleza ordinal del problema.

### 8. Visualización de resultados
- Scatterplots, heatmaps de correlación, scree plot del PCA, proyección 2D,
  matrices de confusión y predicciones individuales con probabilidades.

## Resultados

| Enfoque | Accuracy | Observación |
|---|---|---|
| Random Forest — 8 clases | **0.42** | Acierto ±1 rango ≈ **0.71** |
| Random Forest — 3 tiers | **0.68** | Bajo / Medio / Alto |
| Modelos por rol | ≤ global | No superan al modelo global |

**Lectura de las métricas:**

- Con **8 clases** la precisión es limitada (~0.42), pero no aleatoria (azar = 0.125).
  El *classification report* muestra que los extremos (IRON, MASTER) se predicen muy
  bien y los rangos intermedios se confunden entre sí. La métrica **±1 rango ≈ 0.71**
  confirma que el modelo captura la tendencia aunque no clave el rango exacto.
- Agrupar en **3 tiers** eleva la precisión a **0.68**, replanteando el problema sin
  cambiar los datos.
- El **PCA** reveló que la mayor fuente de varianza es el **rol** del jugador, no su
  rango (se necesitan 8 componentes para explicar el 80% de la varianza).
- El análisis **por rol** mostró variables específicas: `visionScore` domina en
  SUPPORT, `minionsKilled` en MIDDLE/ADC/TOP y `totalDamageDealt` en JUNGLE, mientras
  que `winRate`/`wins`/`losses` son transversales a todos los roles.
- **Entrenar un modelo por rol no mejoró** los resultados: la pérdida de muestra al
  segmentar (~400 filas por rol vs 2285) pesa más que la ganancia de especificidad, y
  el modelo global ya incorpora el rol vía one-hot.

## Conclusiones

- **Cada rol tiene su propio camino para subir.** El rol condiciona qué estadísticas
  importan: visión para SUPPORT, control de oleadas (minions) para los carriles y daño
  para JUNGLE. Un consejo de mejora debe ser específico al rol del jugador.
- **Predecir el rango exacto es especialmente difícil** con estas variables. El
  `winRate` se calcula sobre solo 25 partidas (alta varianza) y depende de factores
  externos no medibles (MMR, jugadores AFK/troll, lag). Por eso ningún modelo supera
  ~0.42 en 8 clases; por tanto se puede haber tenido unas ultimas partidas muy malas, pero pertenecer a un rango más bajo del que debeŕia o más alto si fueron muy buenas partidas.
- **Agrupar en tiers (rangos) es la estrategia más útil** cuando se busca una predicción
  confiable (0.68), mientras que el modelo de 8 clases sirve para el análisis
  explicativo de la tendencia. Entrega una información más aproximada del nivel del jugador basado en sus ultimas partidas, ya que el perfomance puede cambiar bastante.


## Posibles mejoras
- **Mayor numero de partidas** Nos hubiera gustado tener una muestra que tenga un nivel más amplio de los promedio del jugador, en vez de las ultimas 25, las ultimas 50, o todas las partidas ranked de la temporada.

- **Acceso a la API Riot** Tambien se perdio tiempo tratando de acceder y comprender como funcionaba la Api para generar el propio dataset con mejores muestras, pero dificultó en exceso.

- **Campeones utilizados** Es bien sabido que ciertos campeones funcionan mejor en ciertos ELO's que en otros, tener la estadistica de los campeones utilizados, podria apuntar a un analisis más proundo

- **Modos de juego** Si bien el dataset corresponde a el modo de juego Clasificatoria Solo/Duo, se podría ver si los jugadores juegan normalmente en Duo o si juegan en Clasficatoria Flexible, donde puede jugarse de 1,2,3,5 jugadores, siendo 5 normalmente el modo habitual.

- **Sesiones de juego** Tambien podría ser util saber cuanto tiempo un jugador esta conectado habitualmente, normalmente jugadores de alto rendimiento tienden a estar más tiempo esperando para jugar y acaban las partidas entre 20-30 minutos, rangos medios entran más rapido, pero suelen alargar sus partidas

- **Objetivos capturados** Es bien sabido que las diversas cantidades de objetivos por partida tambien pueden determinar que equipo ganará, entre estos torretas, dragones, Baron Nashor, etc.




## Miembros - Usuario GitHub
- Juan Lopez - JuanjoLT
- Felipe Scanda - FelipeScanda