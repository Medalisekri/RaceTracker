# Race Tracker 

Une application Android de simulation de course entre deux joueurs,
développée avec **Jetpack Compose** et les **Coroutines Kotlin**.

## Fonctionnalités

-  Course simultanée entre **Joueur 1** et **Joueur 2**
-  Barres de progression animées en temps réel (`LinearProgressIndicator`)
-  Boutons **Start / Pause / Reset**
-  Joueur 2 progresse **2x plus vite** que Joueur 1
-  Lancement concurrent avec `coroutineScope` + `launch`


## Technologies utilisées

- **Langage :** Kotlin
- **Framework UI :** Jetpack Compose & Material 3
- **Concurrence :** Coroutines Kotlin (`LaunchedEffect`, `coroutineScope`, `launch`, `delay`)
- **État :** `mutableStateOf`, `remember`, `rememberSaveable`
- **SDK minimum :** 21+

## Comment ça fonctionne ?

1. Appuyez sur **Start** pour lancer la course
2. Les deux joueurs progressent simultanément grâce aux coroutines
3. Appuyez sur **Pause** pour suspendre la course
4. Appuyez sur **Reset** pour recommencer

## Logique de progression

| Joueur | Incrément | Délai |
|---|---|---|
| Player 1 | +1% | 500ms |
| Player 2 | +2% | 500ms |

> Player 2 atteint 100% en 2x moins de temps que Player 1.

## Structure du projet
com.example.racetracker/
├── MainActivity.kt
└── ui/
├── RaceTrackerApp.kt       # UI principale + logique de course
├── RaceParticipant.kt      # Modèle d'état d'un participant
└── theme/
└── RaceTrackerTheme.kt

## Composables principaux

| Composable | Description |
|---|---|
| `RaceTrackerApp` | Gère l'état global et lance les coroutines |
| `RaceTrackerScreen` | Affichage de la course |
| `StatusIndicator` | Barre de progression d'un joueur |
| `RaceControls` | Boutons Start / Pause / Reset |

## Modèle `RaceParticipant`
```kotlin
class RaceParticipant(
    val name: String,
    val maxProgress: Int = 100,         // Objectif à atteindre
    val progressDelayMillis: Long = 500L, // Délai entre chaque pas
    private val progressIncrement: Int = 1, // Pas de progression
    private val initialProgress: Int = 0
)
```

| Méthode / Propriété | Description |
|---|---|
| `run()` | Fonction suspend — progresse jusqu'à `maxProgress` |
| `reset()` | Remet `currentProgress` à 0 |
| `progressFactor` | Ratio 0.0 → 1.0 pour `LinearProgressIndicator` |

## Architecture des Coroutines*
LaunchedEffect(playerOne, playerTwo)
└── coroutineScope
├── launch → playerOne.run()   // concurrent
└── launch → playerTwo.run()   // concurrent

> Les deux coureurs s'exécutent **en parallèle** dans le même scope.
> La course se termine quand les deux coroutines sont complètes.

## Captures d'écran
<img width="1732" height="961" alt="Screenshot (265)" src="https://github.com/user-attachments/assets/47ac3fd3-b835-4556-8542-cc4c5a2e3643" />
<img width="1714" height="961" alt="Screenshot (266)" src="https://github.com/user-attachments/assets/983789f2-90e5-4ffe-b4cb-2a0bf45cee14" />
<img width="1721" height="942" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/13356725-38ba-4ed9-9cc0-c0167a7125b8" />
<img width="1748" height="949" alt="Screenshot (264)" src="https://github.com/user-attachments/assets/3c35bdee-22d3-4582-9c96-d83584fe439e" />

