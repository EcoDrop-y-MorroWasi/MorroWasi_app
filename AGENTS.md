# AGENTS.md — MorroWasi (Android)

Este archivo le da contexto a Gemini (u otro agente de IA) dentro de Android
Studio para ayudar a construir la app nativa de **MorroWasi**, parte del
ecosistema HydroSmart-Morropón (Desafío Amauta 2026, Piura, Perú).

Léelo completo antes de generar código. Si una instrucción del usuario
contradice este archivo, pregunta antes de asumir.

---

## 1. Contexto del proyecto

MorroWasi es una app familiar de ahorro de agua y gamificación para Piura.
Ya existe un **MVP funcional en React + Tailwind** (Google AI Studio,
`App.tsx`, ~2700 líneas) que sirve como **fuente de verdad de la lógica de
producto**. La tarea es portar ese MVP a una app Android nativa, no
reinventar el diseño ni las reglas de negocio desde cero.

El ecosistema completo tiene 3 partes: esta app Android, un sitio web
Flask/Firebase (HydroSmart-Morropón), y un dispositivo IoT físico
("Mr. Gota": ESP32-CAM + DFPlayer Mini) que detecta goteos por visión
artificial y se comunica por BLE.

---

## 2. Stack tecnológico (decisiones fijas, no cambiar sin avisar)

- **Kotlin + Jetpack Compose** (Material 3), un solo `Activity`.
- **Navigation-Compose** para las rutas.
- **Room** para datos estructurados (tareas, cursos completados, historial).
- **DataStore (Preferences)** para configuración simple (nombre familiar,
  avatar, puntos, racha, tema, capacidad de tanque) — reemplaza las claves
  `oasis_v3_*` de `localStorage` del MVP.
- **Sin Hilt.** Para mantener el proyecto simple (un solo desarrollador),
  usar un `AppContainer` manual o `ViewModelFactory` simple para proveer
  repositorios. Si el proyecto crece mucho, se puede migrar a Hilt después.
- **Sin backend obligatorio en esta fase.** Firebase Firestore queda como
  sincronización opcional/futura (offline-first: Room es la verdad local).

---

## 3. Sistema de diseño (paleta REAL, no la de `DESIGN.md`)

`DESIGN.md` (turquesa) fue una propuesta de Stitch que **no se usó** en el
MVP final. El código real (`index.css` del MVP) usa esta paleta —
**úsala tal cual, es la definitiva**:

| Token | Hex | Uso |
|---|---|---|
| primary | `#99B4D8` | acciones principales, agua |
| secondary | `#FFB793` | gamificación, alertas suaves |
| tertiary | `#E26D5C` | énfasis, botones destacados, texto de acento |
| background (claro) | `#fdfae7` | fondo general |
| background (oscuro) | `#0a0a0a` | modo oscuro |
| on-surface | `#1c1c11` | texto sobre fondo claro |

Tipografía: **Plus Jakarta Sans** (headings/display) + **Atkinson
Hyperlegible** (cuerpo, por legibilidad para niños y adultos mayores).
Bordes marcados (`keyline-border`, 2px sólido) en vez de sombras —
replicar con `Modifier.border()` en Compose, no `elevation`.

---

## 4. Modelo de datos (portar desde `types.ts`)

```kotlin
data class Task(
    val id: String,
    val text: String,
    val litersSaved: Int,
    val completed: Boolean,
    val xp: Int,
    val category: TaskCategory, // ducha, lavanderia, riego, cocina, fugas, otros
    val familyMember: String?
)

data class ReservoirConfig(
    val capacityLiters: Int,
    val currentPercentage: Int,
    val daysOfWaterCut: Int,
    val familyMembersCount: Int
)

data class WasiStage(val number: Int, val name: String, val description: String)
```

**Las 10 etapas del Wasi (nombres reales, no inventar otros):**

1. Pequeño Brote 2. Semilla Germinada 3. Jardín de Duna
4. Arbusto Resiliente 5. Oasis Temprano 6. Refugio Verde
7. Flujo del Chira 8. Bosque Seco 9. Santuario Hídrico
10. Oasis Sagrado

Fórmula real: `etapa = min(10, (hydroPuntos / 500) + 1)`. Progreso a la
siguiente etapa = `hydroPuntos % 500` sobre 500.

---

## 5. Pantallas y navegación

Flujo: `Auth → Splash (3s) → Tutorial (3 slides) → Login (nombre + avatar) → App`

App principal (bottom nav, 5 tabs): **Cursos, Misiones, Menú (Dashboard,
tab central), Noticias, Ahorro**. El perfil/ajustes ("menu" en el MVP) se
abre desde el ícono de avatar en el header superior, no desde el bottom
nav.

- **Dashboard**: agua ahorrada hoy, Hydro-Puntos, racha, tarjeta Wasi
  (abre modal de evolución con las 10 etapas), monitor de tanque/reservorio.
- **Misiones**: sub-tabs Diarias / Semanales / Mensuales + formulario de
  misión personalizada. La calculadora de desperdicio (costo en soles)
  vive dentro de Mensuales en el MVP — evaluar si moverla a Ahorro para
  mayor claridad (decisión de UX pendiente, no bloqueante).
- **Cursos**: SODIS, Reuso de Aguas Grises, Filtros Caseros, Riego por
  Goteo, Cosecha de Lluvia, Tratamiento Biológico. Desbloqueo por puntos
  o por nivel de Wasi (ver `unlockRequirements` en `App.tsx`).
- **Noticias**: artículos (algarrobo, fugas invisibles, El Niño, cultura
  del agua preincaica), con fuentes citadas (MINAM, SUNASS, ENFEN).
- **Ahorro**: contador total, meta familiar de 1000L, botones de registro
  rápido (+20L ducha, +10L dientes, +40L lavadora, +15L riego nocturno).
- **Menú/Perfil**: avatar (12 emojis temáticos de naturaleza/agua),
  nombre familiar, ajustes (tema, conexión Mr. Gota, historial, cerrar
  sesión).

---

## 6. Qué corregir respecto al MVP web (no replicar estos bugs)

1. El badge **"Guardián del Agua"** en Perfil está *hardcodeado* — debería
   reflejar dinámicamente el nombre de la etapa actual del Wasi (o
   definir si es un título familiar aparte; decidir antes de implementar).
2. **"340L Ahorrados"** en Perfil es un valor fijo — debe calcularse desde
   el estado real (`totalSavedLiters`), no un placeholder.
3. Los nombres de nivel mostrados en el Dashboard (`Pequeño Brote` /
   `Jardín de Duna` / `Flujo del Chira` / `Oasis Sagrado` cada 3 niveles)
   no coinciden con los 10 nombres completos del modal de evolución —
   usar siempre la lista completa de 10 nombres de la sección 4.
4. El toggle de Bluetooth es **simulado** (`isBluetoothEnabled` solo
   cambia un texto) — en Android debe ser una conexión BLE real (ver
   sección 7).
5. `localStorage` → reemplazar completamente por Room + DataStore, con
   claves equivalentes bien tipadas (nada de `JSON.parse` manual).

---

## 7. Integración con Mr. Gota (BLE)

Mr. Gota es un ESP32-CAM con TinyML (Edge Impulse) que detecta goteos y
envía una alerta. **El contrato BLE (UUIDs de servicio/característica)
todavía no está definido en el firmware** — no inventar UUIDs como si
fueran reales. Al implementar `MrGotaBleManager.kt`:

- Dejar una interfaz clara (`connect()`, `observeAlerts(): Flow<Boolean>`,
  `disconnect()`) para que el contrato real se conecte después sin
  rediseñar la capa de UI.
- Diseñar la UI asumiendo reconexión intermitente (el dispositivo es
  offline-first y corre a baterías).
- Persistir cada alerta recibida en Room para el historial, aunque no
  haya internet.

---

## 8. Reglas de código

- Comentarios de código en **español**, pensados como material educativo
  (este es un proyecto escolar).
- Todos los textos de UI van en `strings.xml`, nunca hardcodeados en los
  Composables (la app es 100% en español, pero facilita mantenimiento).
- Un `ViewModel` por pantalla con estado; los Composables son "tontos"
  (reciben estado + lambdas, no acceden a repositorios directamente).
- Reutilizar componentes (`MissionCard`, `CourseCard`, `ArticleCard`) en
  vez de duplicar layouts entre pantallas.
- Accesibilidad: tamaños de toque mínimo 48dp, contraste alto, `contentDescription`
  en íconos y en las ilustraciones de Mr. Gota / Wasi.

---

## 9. Checklist antes de dar por terminada una tarea

- [ ] ¿Los valores numéricos (litros, puntos, tarifas) coinciden con los
      del MVP (`App.tsx`), salvo que se haya decidido cambiarlos a propósito?
- [ ] ¿Los strings están en `strings.xml`, no hardcodeados?
- [ ] ¿La pantalla funciona sin conexión (offline-first)?
- [ ] ¿Se usó la paleta de la sección 3, no la de `DESIGN.md`?
- [ ] ¿Se evitó introducir alguno de los bugs listados en la sección 6?


Estructura del proyecto en Android Studio
Arquitectura recomendada: Kotlin + Jetpack Compose + MVVM, sin Hilt (para no añadir complejidad de DI innecesaria en un proyecto escolar de un solo desarrollador) — Room + DataStore reemplazan el localStorage del MVP web.
app/src/main/java/com/morrowasi/app/
├── MainActivity.kt
├── MorroWasiApp.kt                    // Composable raíz + NavHost
├── data/
│   ├── local/
│   │   ├── AppDatabase.kt             // Room DB
│   │   ├── dao/TaskDao.kt
│   │   ├── entity/TaskEntity.kt
│   │   └── datastore/UserPrefsDataStore.kt   // familyName, avatar, puntos, racha, darkMode, tankConfig...
│   ├── ble/
│   │   ├── MrGotaBleManager.kt        // cliente BLE real hacia el ESP32-CAM
│   │   └── MrGotaBleContract.kt       // UUIDs de servicio/característica (pendiente de definir con firmware)
│   ├── remote/firebase/FirestoreSync.kt   // sync opcional/futura
│   ├── repository/
│   │   ├── TaskRepository.kt
│   │   ├── ProgressRepository.kt      // puntos, racha, etapa Wasi
│   │   ├── CourseRepository.kt
│   │   └── ReservoirRepository.kt
│   └── model/  (Task, Achievement, ReservoirConfig, Course, Article, WasiStage)
├── ui/
│   ├── theme/ (Color.kt, Theme.kt, Type.kt)
│   ├── navigation/ (Screen.kt, MorroWasiNavHost.kt)
│   ├── components/
│   │   ├── illustrations/ (MrGotaIllustration.kt, WasiIllustration.kt)
│   │   ├── MorroWasiBottomBar.kt, MorroWasiTopBar.kt
│   │   └── MissionCard.kt, CourseCard.kt, ArticleCard.kt
│   └── screens/
│       ├── auth/ · splash/ · tutorial/ · login/
│       ├── dashboard/ (+ DashboardViewModel)
│       ├── misiones/ (+ MisionesViewModel)
│       ├── cursos/ (+ CursosViewModel)
│       ├── noticias/
│       ├── ahorro/ (+ AhorroViewModel)
│       └── menu/   (perfil + ajustes)
└── util/Constants.kt
Plan de desarrollo (por fases)

Setup: Gradle (Compose, Navigation, Room, DataStore), tema base.
Datos y persistencia: entidades Room que reemplazan cada clave oasis_v3_* de localStorage.
Navegación: flujo Auth → Splash → Tutorial → Login → App con NavHost.
Pantallas con datos estáticos: las 6 pestañas (dashboard, misiones, cursos, noticias, ahorro, menú) usando los datos reales del MVP (tareas, cursos, artículos, tarifas).
Lógica de gamificación: ViewModels con cálculo de etapa Wasi, desbloqueo de cursos, calculadora de desperdicio.
Ilustraciones: Mr. Gota y las 10 etapas del Wasi como Composables (Canvas o vectores).
BLE real con el ESP32-CAM: reemplaza el toggle Bluetooth simulado del MVP.
Sincronización Firebase (offline-first, opcional/futuro).
Pulido: modo oscuro, accesibilidad, corrección de bugs detectados (ver AGENTS.md).

Una nota importante antes del archivo: revisando el código real (index.css), la paleta que sí se implementó y validó es la pastel (#99B4D8 / #FFB793 / #E26D5C), no la turquesa de DESIGN.md que te generé antes. El AGENTS.md usa la paleta real como definitiva.
