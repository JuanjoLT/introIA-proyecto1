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

## Estructura

```
introIA-proyecto1/
├── main.ipynb    # Notebook principal: EDA + modelado
└── README.md
```

## Requisitos

- Python 3.x
- Jupyter Notebook
- pandas, numpy, matplotlib, seaborn, scikit-learn

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## Uso

```bash
jupyter notebook main.ipynb
```

## Miembros - Usuario GitHub
- Juan Lopez - JuanjoLT
- Felipe Scanda - FelipeScanda



## Planes

- Matriz de correlacion
- Aplicar PCA
- Investigar que modelo es el maś conveniente
- Pasar a metodología
- Si hay tiempo, anexar las demás datasets