# Guía de Instalación y Configuración — MorroWasi en Android Studio

> Verifiqué en julio de 2026: la serie estable actual de Android Studio es
> **"Quail" (2026.1.x)** y el BOM de Compose vigente es
> **2026.06.01**. Antes de instalar, confirma en
> `developer.android.com/studio` que sigan siendo las versiones más
> recientes — el ciclo de releases es muy rápido.

---

## 1. Requisitos previos

- **RAM:** mínimo 8 GB, recomendado 16 GB (el emulador consume bastante).
- **Espacio en disco:** ~10 GB libres (Android Studio + SDKs + emulador).
- **JDK:** no instalar uno aparte — Android Studio trae su propio JDK
  embebido (JBR), no lo reemplaces.
- **Cuenta de Google:** obligatoria para activar Gemini.
- **Un celular Android físico** (Android 8.0 / API 26 en adelante) con
  cable USB o depuración inalámbrica — **necesario para probar BLE**, ya
  que el emulador de Android Studio no soporta Bluetooth real.

---

## 2. Instalación de Android Studio

1. Descarga el instalador desde `developer.android.com/studio` (elige tu
   sistema operativo).
2. Ejecuta el instalador y deja que el asistente ("Setup Wizard") baje
   los componentes por defecto: Android SDK, Android SDK Platform-Tools,
   Android Virtual Device.
3. Al abrir por primera vez, ve a **More Actions → SDK Manager** y en la
   pestaña **SDK Platforms** marca al menos:
   - El API level más reciente estable (para `compileSdk`/`targetSdk`).
   - API 26 (Android 8.0) si vas a fijar `minSdk = 26` (mínimo razonable
     para BLE moderno con `BluetoothLeScanner`).
4. En la pestaña **SDK Tools**, confirma que estén marcados: *Android SDK
   Build-Tools*, *Android Emulator*, *Android SDK Platform-Tools*.

---

## 3. Activar Gemini en Android Studio

1. Abre el panel desde el ícono de Gemini en la barra lateral derecha, o
   **Tools → Gemini**.
2. Inicia sesión con tu cuenta de Google cuando se te pida.
3. En el flujo de onboarding, selecciona **"Gemini for individuals"**
   (nivel gratuito para desarrolladores individuales).
4. Elige **"Use all Gemini features"** para permitir que Gemini use el
   contexto de tu proyecto (esto es lo que le permite leer archivos como
   tu `AGENTS.md`).
5. Puedes ajustar esto después en **File → Settings → Tools → AI**
   (en macOS: **Android Studio → Settings → Tools → AI**).

---

## 4. Crear el proyecto

1. **File → New → New Project → Empty Activity (Compose)**.
2. Configuración:
   - **Name:** MorroWasi
   - **Package name:** `com.morrowasi.app`
   - **Minimum SDK:** API 26 (Android 8.0)
   - **Build configuration language:** Kotlin DSL (`.kts`) — recomendado,
     es lo que genera el asistente por defecto hoy.
3. Deja que Android Studio termine el "Gradle Sync" inicial antes de
   tocar nada.
4. Copia el archivo `AGENTS.md` (que ya tienes) a la **raíz del
   proyecto**, junto a `build.gradle.kts` — así Gemini lo ve como parte
   del contexto del repositorio.

---

## 5. Organización de carpetas

Recrea esta estructura dentro de `app/src/main/java/com/morrowasi/app/`
(ya la detallamos antes, aquí el resumen para ir creando las carpetas):

```
data/local/  (Room, DAOs, entidades, DataStore)
data/ble/    (conexión con Mr. Gota)
data/repository/
data/model/
ui/theme/    (Color.kt, Theme.kt — ya los tienes)
ui/navigation/
ui/components/illustrations/
ui/screens/{auth,splash,tutorial,login,dashboard,misiones,cursos,noticias,ahorro,menu}/
util/
```

Sugerencia: crea las carpetas vacías primero (clic derecho en `app` →
New → Package) y luego pide a Gemini que genere los archivos dentro de
cada una, un paquete a la vez — es más fácil de revisar que pedirle todo
el proyecto de un solo prompt.

---

## 6. Dependencias de Gradle

Usa el **Version Catalog** (`gradle/libs.versions.toml`), que ya viene
por defecto en proyectos nuevos, en vez de pegar versiones sueltas en
cada `build.gradle.kts`. Ejemplo de bloques a añadir/editar:

```toml
[versions]
composeBom = "2026.06.01"
room = "2.7.1"
datastore = "1.1.1"
navigationCompose = "2.9.0"
workManager = "2.10.0"
firebaseBom = "34.0.0"
splashscreen = "1.0.1"
ksp = "2.1.0-1.0.29"

[libraries]
compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
room-runtime = { group = "androidx.room", name = "room-runtime", version.ref = "room" }
room-ktx = { group = "androidx.room", name = "room-ktx", version.ref = "room" }
room-compiler = { group = "androidx.room", name = "room-compiler", version.ref = "room" }
datastore-preferences = { group = "androidx.datastore", name = "datastore-preferences", version.ref = "datastore" }
navigation-compose = { group = "androidx.navigation", name = "navigation-compose", version.ref = "navigationCompose" }
work-runtime-ktx = { group = "androidx.work", name = "work-runtime-ktx", version.ref = "workManager" }
firebase-bom = { group = "com.google.firebase", name = "firebase-bom", version.ref = "firebaseBom" }
core-splashscreen = { group = "androidx.core", name = "core-splashscreen", version.ref = "splashscreen" }
```

> Las versiones exactas cambian seguido — antes de fijar la tuya, revisa
> `developer.android.com/jetpack/androidx/versions` para la última
> estable de cada librería.

En `app/build.gradle.kts`, dentro de `dependencies { ... }`:

```kotlin
implementation(platform(libs.compose.bom))
implementation("androidx.compose.material3:material3")
implementation("androidx.compose.material:material-icons-extended")
implementation(libs.navigation.compose)
implementation(libs.room.runtime)
implementation(libs.room.ktx)
ksp(libs.room.compiler)
implementation(libs.datastore.preferences)
implementation(libs.work.runtime.ktx)
implementation(libs.core.splashscreen)
implementation(platform(libs.firebase.bom))
implementation("com.google.firebase:firebase-firestore-ktx")
implementation("com.google.firebase:firebase-auth-ktx")
```

Y aplica el plugin de KSP (necesario para Room) en el bloque `plugins`
del mismo archivo:

```kotlin
plugins {
    id("com.google.devtools.ksp")
}
```

---

## 7. Tecnologías adicionales — qué son, para qué y cómo se usan

| Tecnología | Para qué la necesitas | Requiere | Cómo se usa (resumen) |
|---|---|---|---|
| **Jetpack Compose + Material 3** | Toda la UI de la app | Ya viene con el proyecto | Composables declarativos; ya tienes `Color.kt`/`Theme.kt` |
| **Navigation-Compose** | Moverte entre Auth/Splash/Tutorial/Login/tabs | Dependencia Gradle | `NavHost` + `NavController`, rutas como `sealed class Screen` |
| **Room** | Reemplazar `localStorage`: tareas, cursos, historial | Dependencia + plugin KSP | Entidades `@Entity`, `@Dao` con queries, `RoomDatabase.Builder` |
| **DataStore (Preferences)** | Config simple: nombre familiar, avatar, tema, puntos | Dependencia Gradle | `Flow<Preferences>` con `dataStore.data`, escritura con `edit {}` |
| **Material Icons Extended** | Reemplazo de `lucide-react` del MVP | Dependencia Gradle | `Icons.Filled.X` / `Icons.Outlined.X` en vez de SVGs sueltos |
| **Permisos en tiempo de ejecución** | Bluetooth y ubicación para BLE | API nativa de Compose | `rememberPermissionState` (Accompanist) o `ActivityResultContracts.RequestPermission` nativo |
| **BLE (Bluetooth Low Energy)** | Conexión real con Mr. Gota (ESP32-CAM) | Permisos en el Manifest + librería BLE | Recomendado: **Nordic Android-Kotlin-BLE-Library** (`no.nordicsemi.android.kotlin-ble`), maneja reconexión y GATT de forma mucho más estable que la API nativa de `BluetoothGatt` a pelo |
| **WorkManager** | Sincronizar datos con Firebase cuando vuelva la conexión (offline-first real) | Dependencia Gradle | `CoroutineWorker` + `WorkManager.enqueueUniquePeriodicWork` |
| **Firebase (Firestore + Auth)** | Sincronización en la nube (fase futura, opcional en el MVP nativo) | Proyecto en Firebase Console + `google-services.json` en `app/` + plugin `com.google.gms.google-services` | Firestore como espejo remoto; Room sigue siendo la fuente de verdad local |
| **Core SplashScreen API** | Splash screen nativo (reemplaza el `setTimeout` del MVP web) | Dependencia Gradle + tema `Theme.SplashScreen` | `installSplashScreen()` en `onCreate()` de `MainActivity` |
| **Coroutines & Flow** | Programación asíncrona (BLE, Room, DataStore) | Vienen con Kotlin/Compose | `viewModelScope.launch { }`, `StateFlow` en los ViewModels |
| **Compose UI Test / JUnit** | Pruebas de pantallas y lógica de gamificación | Dependencia (`androidTestImplementation`) | `createComposeRule()` para tests de UI |

### Permisos a declarar en `AndroidManifest.xml` (para BLE)

```xml
<uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```
(`ACCESS_FINE_LOCATION` solo es obligatorio en versiones de Android
anteriores a la 12; en Android 12+ basta con los dos permisos de
Bluetooth si declaras `neverForLocation` en el manifest).

---

## 8. Dispositivo de prueba

- El **emulador** te sirve para casi todo (UI, Room, DataStore,
  navegación), pero **no soporta Bluetooth real**.
- Para probar la conexión con Mr. Gota necesitas un **celular físico**:
  1. Activa **Opciones de desarrollador** (7 toques sobre "Número de
     compilación" en Ajustes del celular).
  2. Activa **Depuración USB**.
  3. Conéctalo por cable (o configura depuración inalámbrica) y
     selecciónalo como dispositivo de destino en Android Studio.

---

## 9. Checklist final antes de programar

- [ ] Android Studio instalado y con Gradle Sync exitoso.
- [ ] Gemini activado y con "Use all Gemini features" habilitado.
- [ ] `AGENTS.md` en la raíz del proyecto.
- [ ] Paquetes vacíos creados según la estructura de la sección 5.
- [ ] Dependencias del `libs.versions.toml` agregadas y sincronizadas.
- [ ] `Color.kt` / `Theme.kt` ya copiados en `ui/theme/`.
- [ ] Un celular físico disponible para las pruebas de BLE.
