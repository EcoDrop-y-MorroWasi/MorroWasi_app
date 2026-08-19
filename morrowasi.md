Aplicación Móvil de Monitoreo y Gamificación (Android)

Descripción: Aplicación móvil nativa diseñada en Sitch/Figma y desarrollada en Android Studio (Kotlin/Java)con soporte del modelo fundacional Gemini 3.1 Pro. Su función es doble: por un lado, recibe notificaciones push instantáneas enviadas desde el backend cuando se detecta un goteo en el hogar; por otro lado, gamifica el consumo diario del agua mediante el registro de "Eco-Retos", visualización de tablas de clasificación comunitaria y la evolución del avatar interactivo de la familia.

Insumos Requeridos:
1. Plataforma de Prototipado UX/UI: Stitch o Figma (Planes educativos gratuitos) para el flujo de interfaces.
2. Entorno de Desarrollo (IDE móvil): Android Studio Jellyfish (o superior) con Gemini integrado.
3. Servicios de Red y Notificaciones:Firebase Cloud Messaging (FCM) para el envío de alertas push sin costo.
4. Cuenta de Desarrollador de Google Play (Opcional):** Requerida si se desea publicar la app oficialmente en la Play Store.
5. Dispositivos de Prueba:** Smartphones Android de las familias participantes (Android 8.0 o superior).

1. PROPÓSITO Y CONTEXTO DE LA APLICACIÓN

MorroWasi es una plataforma educativa y gamificada diseñada para combatir la crisis hídrica en la región de Piura, Perú. Su nombre combina el término "Morropón" (zona desértica piurana de alta vulnerabilidad hídrica) y la palabra quechua "Wasi" (casa), simbolizando la transformación de un hogar convencional en una casa ecológica autosostenible.

La aplicación tiene como objetivo empoderar a las familias para que tomen control de su consumo de agua, aprendan técnicas de purificación y reuso, y adopten hábitos responsables mediante un sistema dinámico de recompensas basadas en Hydro-Puntos y Rachas de Ahorro.
2. FLUJO DE TRABAJO DEL USUARIO (WORKFLOW)

El flujo de navegación de la aplicación ha sido estructurado para ofrecer una introducción intuitiva y garantizar la configuración correcta de cada hogar:

    Pantalla de Inicio de Sesión (auth): Es el punto de partida donde el usuario inicia su sesión mediante correo electrónico o cuenta de Google.

    Pantalla Splash de Bienvenida (splash): Una transición animada de 3 segundos protagonizada por la mascota Mr. Gota, la cual establece la identidad visual de la aplicación.

    Tutorial Interactivo (tutorial): Un deslizador de 3 diapositivas donde se enseña al usuario la importancia del agua en zonas de desierto y cómo acumular puntos cuidando el recurso.

    Personalización del Perfil (login): El usuario introduce el Nombre de su Familia y selecciona un Avatar interactivo. Esto personaliza toda la experiencia y las metas del hogar.

    Menú Principal de la App (app): Una vez autenticado, el usuario accede al panel de control central para gestionar su consumo, realizar misiones y capacitarse.

    Cerrar Sesión: Al hacer clic en este botón dentro del panel de ajustes, los datos se restablecen de forma segura y el flujo retorna limpiamente a la pantalla de Inicio de Sesión (auth) para permitir un nuevo ciclo de aprendizaje.

3. ARQUITECTURA DE PESTAÑAS (TABS PRINCIPALES)

La interfaz principal utiliza un sistema de navegación inferior que segmenta las funciones del aplicativo:

    Menú Principal (Dashboard):

        Simulador de Cisterna y Tanque Elevado: Permite a las familias monitorear la capacidad real de agua acumulada (simulando los cortes programados comunes en Piura) y configurar alertas de desabastecimiento.

        Métricas del Hogar: Visualización en tiempo real de la racha diaria de ahorro, Hydro-Puntos familiares acumulados y volumen de agua protegido hoy.

        Historial de Consumo: Un gráfico de barras interactivo que muestra el uso estimado de agua de lunes a viernes, alertando con colores cálidos si se superan los límites saludables.

        Conexión Mr. Gota: Panel de sincronización Bluetooth ficticio que simula la vinculación con sensores reales de medición de cañerías.

    Cursos (Academia del Agua):
    Un catálogo interactivo de módulos educativos enfocados en ecotecnologías de bajo costo. Cada curso incluye una ficha técnica, tutor asignado, duración, contenido detallado y una recompensa en puntos al finalizar:

        Método SODIS: Desinfección solar del agua utilizando botellas PET.

        Reuso de Aguas Grises: Construcción de biofiltros caseros para agua de lavadoras y duchas.

        Filtros Caseros: Filtrado físico con grava, arena y carbón activo.

        Riego por Goteo Casero: Optimización para biohuertos utilizando capilaridad y condensación solar.

        Recolección de Lluvia y Tratamiento: Aprovechamiento óptimo de precipitaciones estacionales.

    Misiones (Desafíos de Ahorro):
    Un gestor de tareas pendientes dividido en categorías Diarias, Semanales y Mensuales.

        Contiene misiones por defecto (como cerrar el caño al cepillarse o usar baldes en lugar de mangueras).

        Permite a la familia crear misiones personalizadas definiendo el nombre, los litros estimados que ahorrarán y el miembro responsable.

        Al completar cada tarea, se suman los litros al total ahorrado del día y se acumulan Hydro-Puntos.

    Noticias (Oasis Piurano):
    Un canal informativo con artículos de concientización y boletines oficiales del Ministerio del Agua de Piura. Permite leer reportajes detallados sobre conservación y el estado de los reservorios regionales (como Poechos).

    Ahorro (Calculadora de Desperdicio):
    Una herramienta analítica avanzada donde el usuario ingresa las pérdidas de agua estimadas de su hogar. La calculadora analiza el tipo de vivienda (vivienda multifamiliar, casa con cisterna, sin tanque) y calcula el impacto ecológico y financiero de las fugas detectadas, entregando recomendaciones inmediatas.

4. COMPONENTES CLAVE Y TECNOLOGÍAS

    Animaciones Fluidas: Desarrollado con motion/react para lograr un deslizamiento natural entre las diapositivas del tutorial, micro-transiciones al interactuar con las pestañas y efectos táctiles en los botones principales.

    Gestión de Estado Centralizada: Implementado con React Hooks (useState, useEffect) garantizando la sincronización bidireccional entre las misiones completadas, las lecturas del simulador de cisterna y las estadísticas globales.

    Persistencia Local: Utiliza localStorage de forma optimizada para almacenar de forma segura la racha, el avatar seleccionado, el nombre de la familia, las misiones personalizadas y los cursos aprobados. Esto asegura que el progreso no se pierda al cerrar el navegador.

    UI/UX con Estilo Desértico: Diseño visual con temática cálida desértica piurana utilizando paletas de colores arena, tierra y azul oasis, estructurados de forma moderna y limpia mediante Tailwind CSS.




1. ¿Qué es MorroWasi?

MorroWasi es una aplicación con enfoque ecológico y gamificado, pensada
para familias y comunidades de Piura — una región afectada por crisis
hídrica y anomalías climáticas como el Fenómeno El Niño. Su propósito
central es fomentar, medir y educar sobre el ahorro de agua en el
hogar, premiando los buenos hábitos con puntos, rachas y la evolución
visual de una casa virtual ("Wasi").

La lógica de la app se apoya en tres pilares:


Medición: cuántos litros se ahorran o desperdician.
Gamificación: puntos, rachas y evolución de la vivienda virtual.
Educación: cursos y noticias sobre manejo sostenible del agua,
adaptados al contexto peruano.



2. Flujo de usuario (User Journey)


Autenticación (Auth): pantalla de entrada. Muestra el logo y a
"Mr. Gota". Permite iniciar sesión con correo o con Google.
Splash Screen: transición de carga de ~3 segundos que refuerza la
identidad visual de la app.
Tutorial interactivo (3 pasos):

Retos diarios de ahorro ("Gota a gota: cuidar el agua en Piura").
Ganar puntos para la comunidad ("Evoluciona tu Wasi").
Impacto vecinal y preparación comunitaria ("Comunidad Activa").



Configuración de perfil: el usuario ingresa un nombre de familia
(ej. "Familia Ramírez") y elige un avatar. Si no completa el nombre,
se asigna automáticamente "Familia MorroWasi" con un avatar por
defecto.
Panel principal: acceso libre mediante la barra de navegación
inferior.


Cierre de sesión: al pulsar "Cerrar sesión" (dentro de Ajustes), la
app reinicia el progreso a cero (puntos, litros, racha) y devuelve al
usuario a la pantalla de Auth, reiniciando todo el flujo descrito arriba.


3. Navegación principal (5 pestañas)

ÍconoPestañaFunción principal🏠Menú (Dashboard)Cuartel general: progreso, tanque, registro rápido📖Cursos (Academia del Agua)Educación técnica en módulos🏆Misiones (Eco-Retos)Motor de hábitos: retos diarios/semanales/mensuales🐷AhorroCalculadora de impacto, control de fugas y ajustes del sistema📅NoticiasBoletín comunitario y contenido cultural/ambiental


4. Detalle por pestaña

🏠 Menú (Dashboard central)


Monitor de Reservorio/Tanque: visualización animada del nivel de
llenado del tanque de agua del hogar, con una interfaz simulada de
conectividad "Mr. Gota IoT" que representa la recepción de datos
volumétricos del hogar.
Barra de progreso: litros acumulados de ahorro frente a la meta
global familiar, y racha de días consecutivos de uso sostenible.
Registro rápido de ahorro (Acciones Rápidas): botones para sumar
litros manualmente:

+20 L — Cerrar la ducha
+10 L — Lavado de dientes (con el caño cerrado)
+40 L — Reuso de agua de la lavadora
+15 L — Riego nocturno



Nivel del Wasi: muestra la evolución visual de la casa ecológica,
desde "Pequeño Brote" (nivel 1) hasta "Oasis Sagrado"
(nivel 10).


📖 Cursos (Academia del Agua)

Biblioteca educativa en módulos breves de alto impacto, con ilustraciones
temáticas dinámicas (ej. ☀️ para SODIS, 🚿 para biofiltros) sobre tarjetas
con colores diferenciados (ámbar, azul, etc.).

Catálogo de cursos:


Método SODIS (desinfección solar con botellas PET)
Construcción de biofiltros caseros
Riego por goteo
Cosecha/recolección de agua de lluvia
Reuso de aguas grises
Tratamiento biológico


Mecánica: cada curso muestra tutor, duración, puntos que otorga y
temario. Algunos requieren acumular Hydro-Puntos o alcanzar cierto nivel
del Wasi para desbloquearse. Al completarlo, se otorga una recompensa
grande de Hydro-Puntos (entre 150 y 250 puntos) y experiencia (XP).

🏆 Misiones (Eco-Retos)

Núcleo de la retención de hábitos, organizado en tres horizontes de
tiempo:


Diarias: acciones simples (cerrar el caño, regar de noche).
Semanales: tareas de mayor esfuerzo (revisar fugas en inodoros).
Mensuales: metas grandes (ahorrar 2000 L en el mes).
Tareas personalizadas: el usuario puede crear y añadir misiones
propias adaptadas a la rutina del hogar.


Marcar una misión como completada suma litros al contador global y
otorga experiencia inmediata.

🐷 Ahorro (Calculadora de Impacto y Ajustes)

Panel utilitario que traduce el ahorro/desperdicio a términos tangibles
y concentra la configuración del sistema.


Simulador de fugas: el usuario ingresa litros perdidos por un caño
dañado y la app calcula su equivalencia según el tipo de instalación
del hogar (con cisterna, tanque elevado, o multifamiliar), mostrando
alertas cuando el desperdicio supera niveles críticos.
Métricas de impacto familiar:

Litros totales ahorrados en el historial de la cuenta.
Progreso hacia la meta escolar (1000 L).
Equivalencia en bidones de 20 L.
Emisiones de CO₂ evitadas.
Ahorro estimado en soles (S/.), comparando la tarifa de red pública
(EPS Grau) frente al costo de camiones cisterna.



Ajustes del sistema:

Modificar la capacidad base del tanque/reservorio (ej. 1000 L o
2500 L).
Activar "Modo Noche Desértica" (modo oscuro).
Editar nombre de familia y avatar.
Ver historial de consumo en gráfico de barras.
Cerrar sesión.





📅 Noticias (Boletín comunitario)

Sección de lectura con artículos de contexto ambiental, cultural e
histórico, referenciados con fuentes oficiales (MINAM, SUNASS, ENFEN).

Ejemplos de contenido:


Preparación preventiva del hogar ante temporales/Fenómeno El Niño.
El algarrobo y sus lecciones de resiliencia hídrica.
Ingeniería del agua de las civilizaciones preincas de la costa norte
del Perú.
Guías prácticas para detectar fugas invisibles.



5. Sistema de gamificación


Hydro-Puntos: moneda de progreso que se gana completando misiones,
registrando ahorro rápido, y terminando cursos.
Rachas: días consecutivos de uso/cumplimiento sostenible.
Nivel del Wasi: la casa virtual evoluciona en 10 niveles, del
"Pequeño Brote" (nivel 1) al "Oasis Sagrado" (nivel 10), a medida que
la familia acumula Hydro-Puntos. Al subir de nivel se muestra un modal
donde la familia visualiza el siguiente nivel y "adquiere" implementos
sustentables teóricos para la vivienda.
Avatares familiares: en lugar de personas genéricas, se usa una
paleta de íconos temáticos de naturaleza y agua: 💧 🌱 🌿 🪴 🌳 🌻 🍎
🌎 🌧️ 🌊 🐢 🌵.
Mr. Gota: personaje guía y mascota. Acompaña el logo, aparece en la
pantalla de Auth y brinda tips interactivos durante las transiciones
de pantalla.



Nota: en versiones previas del diseño (Stitch) el nivel máximo se
llamaba "Guardián del Agua". La versión actual del MVP lo nombra
"Oasis Sagrado". Conviene definir un único nombre oficial antes de
pasar a la implementación en Android Studio, para no generar
inconsistencia entre pantallas.




6. Arquitectura técnica del MVP


Stack: React + Tailwind CSS, con un sistema de tarjetas (cards) de
componente único reutilizado en toda la app.
Enfoque de diseño: Mobile-first, responsivo, sensación táctil y
nativa sin recargas de página.
Persistencia: estado local mediante localStorage, que simula un
comportamiento Offline-First — los datos sobreviven aunque el
usuario recargue el navegador (esto es una simulación del MVP; la app
nativa en Android deberá reemplazar esto por Room/SQLite +
sincronización real con Firebase, según el diseño original).
Identidad visual: colores cálidos del desierto piurano, tipografía
limpia, soporte de modo oscuro ("Modo Noche Desértica").



7. Mejoras recientes implementadas en el MVP


Flujo de cierre de sesión corregido: reinicia progreso y retorna a
Auth, respetando el recorrido completo (Splash → Tutorial → Perfil →
Menú).
Botones de registro rápido ajustados a los valores exactos requeridos
(+20 L, +10 L, +40 L, +15 L).
Avatares familiares reemplazados por íconos temáticos de naturaleza y
agua (en vez de personas genéricas).
Ilustraciones de cursos corregidas: sistema dinámico de íconos grandes
por temática, sobre tarjetas con color diferenciado.
Botón "Editar Perfil Familiar" reconstruido con bordes marcados,
soporte de modo oscuro y mejor transición al pasar el cursor.
Perfil rápido: si no se ingresa nombre, se asigna "Familia MorroWasi"
con avatar por defecto.