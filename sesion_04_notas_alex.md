# Sesión 4: Claude Code — El coding agent de terminal

> **Lectura previa.** Lee esto antes de la clase. En la sesión vamos directo a la práctica.

---

## De la teoría a la construcción

Las primeras tres sesiones fueron cimientos: aprendiste el framework (sesión 1), la red de seguridad (sesión 2) y cómo comunicarte con el agente (sesión 3). Todo eso era preparación para lo que empieza hoy.

A partir de esta sesión, construimos. Y la herramienta con la que vas a construir todo — el Pipeline Tool, el Opportunity Radar, y cualquier proyecto futuro — es **Claude Code**.

Ya lo instalaste en la sesión 1 y le hiciste un "hola mundo". Hoy vamos a usarlo en serio. Vas a aprender el flujo profesional que repiten los vibecoders que producen software real: describir lo que quieres, revisar lo que genera, aprobar o rechazar, iterar. Y al final de la clase, vas a tener la estructura base del Pipeline Tool montada — desde cero, hecha correctamente.

---

## ¿Por qué rehacer el Pipeline Tool desde cero?

Sé lo que estás pensando: "Ya tengo un prototipo. ¿Por qué no partimos de ahí?"

Tu prototipo tiene dos problemas fundamentales:

1. **Fue construido sin contexto.** Lo hiciste en Claude Design, que genera HTML bonito pero sin estructura profesional. No hay componentes separados, no hay organización de archivos, no hay convenciones. Intentar agregarle funcionalidades encima sería como construir un segundo piso sobre cimientos de cartón.

2. **No tiene backend.** El prototipo guarda los datos en el navegador (o ni eso — por eso desaparecen al refrescar). Lo que vamos a construir tiene una arquitectura real: frontend que muestra datos, backend que los guarda. Eso requiere empezar con una estructura limpia.

Rehacer no significa perder trabajo. Tu prototipo cumplió su función: te ayudó a visualizar lo que querías. Ahora que tienes claro el "qué", vamos a construir el "cómo" correctamente. Es como tener un sketch en servilleta y ahora pasarlo a plano profesional.

---

## Claude Code por dentro

Cuando abres la terminal y escribes `claude`, entras en una sesión interactiva. Parece un chat, pero es mucho más que eso. Claude Code tiene acceso directo a tu proyecto: puede leer todos tus archivos, crear nuevos, modificar los existentes, instalar dependencias, correr comandos del sistema, ejecutar pruebas y hacer commits en Git. Todo desde la conversación.

### Lo que Claude Code puede ver

Cuando inicias una sesión, lo primero que hace el agente es orientarse. Lee el CLAUDE.md (si existe), revisa la estructura de archivos y entiende dónde está parado. A partir de ahí, cada vez que le pides algo, puede:

- **Leer archivos** de tu proyecto para entender el código existente.
- **Crear archivos nuevos** donde tú le indiques (o donde él considere apropiado según la estructura).
- **Modificar archivos existentes** — agregar funciones, corregir errores, cambiar estilos.
- **Ejecutar comandos** en tu terminal: instalar paquetes, correr el servidor de desarrollo, ejecutar pruebas.
- **Usar Git** directamente: hacer commits, crear branches, ver el historial.

### Lo que Claude Code NO puede ver

- **No ve tu pantalla.** Si algo se ve raro en el navegador, no basta con decirle "se ve mal". Tienes que describir qué ves: "El sidebar se sobrepone al contenido principal" o "Los proyectos aparecen en una sola columna en vez de en un grid."
- **No recuerda conversaciones anteriores.** Cada vez que abres Claude Code, es una sesión nueva. Por eso el CLAUDE.md es tan importante — es la memoria persistente.
- **No navega internet por su cuenta.** No puede buscar documentación ni verificar cosas online. Todo lo que sabe viene de su entrenamiento y de lo que hay en tu proyecto.

---

## El flujo profesional

Este es el ciclo que vas a repetir decenas de veces en cada sesión de trabajo. Es la versión práctica del "loop fundamental" que vimos en la sesión 1, aplicada específicamente a Claude Code.

### 1. Describir — Dile qué quieres

Le das una instrucción al agente. Dado que ya tienes el CLAUDE.md con todo el contexto del proyecto, tus instrucciones pueden ser directas y específicas. No necesitas repetir el stack ni las convenciones — el agente ya lo sabe.

**Buen prompt para Claude Code:**
> "Crea el componente ProjectList que muestre una tabla con los proyectos. Columnas: nombre, PM asignado, estado, fecha de entrega y peso. Usa datos hardcodeados por ahora — la conexión a base de datos la hacemos después."

**Mal prompt para Claude Code:**
> "Haz la lista de proyectos."

¿Notas que no necesitaste decirle "usa React con Vite y Tailwind"? Eso ya está en el CLAUDE.md. Tu prompt solo necesita la tarea concreta.

### 2. Generar — El agente trabaja

Claude Code empieza a generar. Vas a ver en la terminal cómo crea archivos, escribe código y modifica la estructura. A veces te va a preguntar cosas antes de actuar ("¿Quieres que instale esta dependencia?" o "¿Prefieres que el componente reciba los datos por props o que los obtenga directamente?"). Contéstale como le contestarías a un compañero de equipo.

### 3. Revisar el diff — Qué cambió

Cuando el agente termina, te muestra un **diff**: la lista de cambios que hizo. No necesitas leer cada línea de código, pero sí necesitas entender qué archivos creó o modificó y por qué. El diff tiene un formato sencillo:

- Las líneas en **verde** (con un `+` al inicio) son código nuevo que se agregó.
- Las líneas en **rojo** (con un `-` al inicio) son código que se eliminó.
- El resto no cambió.

Tu checklist de revisión rápida:

- ¿Creó los archivos donde debía, según la estructura del CLAUDE.md?
- ¿Los nombres de archivos siguen la convención (kebab-case)?
- ¿Instaló alguna dependencia que no esperabas?
- ¿Tocó archivos que no debería haber tocado?
- ¿Hay credenciales hardcodeadas en algún lado?

No necesitas verificar si el código es "bueno" en el sentido técnico. Eso viene con el testing (sesión 14). Por ahora, verifica que hizo lo que le pediste y que no se pasó de la raya.

### 4. Aprobar o rechazar

Si lo que ves se ve bien, le dices que continúe. Si no, le dices qué no te gustó y le pides que lo corrija. Claude Code te va a pedir confirmación antes de ejecutar comandos que pueden cambiar cosas (como instalar paquetes o borrar archivos). Cuando veas ese prompt de confirmación, es tu oportunidad de decir "sí" o "no, espera."

### 5. Probar — ¿Funciona?

Después de aprobar los cambios, prueba. Si el agente creó un componente visual, abre el navegador y verifica que se ve como esperabas. Si agregó una funcionalidad, úsala y asegúrate de que funciona. El agente puede correr el servidor de desarrollo por ti:

> "Levanta el servidor de desarrollo para que pueda ver los cambios en el navegador."

### 6. Iterar o hacer commit

Si funciona: haz commit. Ya sabes cómo — `git add . && git commit -m "mensaje"`. O mejor: pídele al agente que lo haga.

Si no funciona: descríbele el problema al agente. Sé específico. "No funciona" no es útil. "Al hacer click en el botón de crear proyecto, no pasa nada — esperaba que apareciera un formulario" sí es útil.

---

## Cómo hablarle a Claude Code

Con el tiempo vas a desarrollar tu propio estilo, pero hay patrones que funcionan mejor que otros. Aquí van los más importantes.

### Para crear algo nuevo

> "Crea un componente `TeamView` que muestre una grilla con los miembros del equipo. Cada tarjeta debe mostrar el nombre, los proyectos asignados y los puntos de carga actuales vs. la capacidad máxima (10 puntos)."

La clave: dile **qué** crear, **qué datos** mostrar y **cómo** se ve a grandes rasgos. No le digas cómo programarlo — deja que él tome esas decisiones dentro de las reglas del CLAUDE.md.

### Para modificar algo existente

> "En el componente `ProjectList`, agrega una columna de 'Prioridad' después de la columna de 'Estado'. Los valores posibles son: Alta, Media, Baja. Muéstralos con un badge de color: rojo para Alta, amarillo para Media, verde para Baja."

La clave: dile **qué archivo** tocar, **qué cambio** exactamente y **cómo** debe verse o comportarse.

### Para corregir algo

> "El sidebar se está sobreponiendo al contenido cuando la pantalla es menor a 1200px de ancho. Necesito que en pantallas pequeñas el sidebar se colapse y se pueda abrir con un botón de menú."

La clave: describe **qué está pasando**, **qué esperabas** que pasara y, si puedes, **en qué condiciones** ocurre el problema (tamaño de pantalla, navegador, acción específica del usuario).

### Para que te explique algo

Claude Code no solo escribe código — también lo explica. Esto es muy útil cuando ves algo en tu proyecto que no entiendes.

> "Explícame qué hace el archivo `supabase-client.js`. No necesito los detalles técnicos, solo quiero entender a alto nivel qué hace y por qué existe."

> "¿Qué pasa cuando un usuario hace clic en el botón de 'Guardar proyecto'? Recorre el flujo completo, desde el click hasta que el dato se guarda."

Estas preguntas son legítimas y valiosas. No necesitas entender todo el código, pero sí necesitas poder seguir el flujo a alto nivel para tomar decisiones como PM del proyecto.

### Para navegar el código

A medida que el proyecto crezca, vas a tener decenas de archivos. No necesitas recordar dónde está cada cosa — Claude Code puede buscarlo por ti.

> "¿En qué archivo se define la lógica de asignación de proyectos?"

> "Busca todos los lugares donde se usa el estado 'On Hold'. Necesito cambiarlo a 'Paused'."

> "Dame un resumen de la estructura actual del proyecto — qué archivos hay y qué hace cada uno."

---

## Git + Claude Code — El flujo integrado

En la sesión 2 aprendiste los comandos de Git. Ahora la buena noticia: puedes pedirle a Claude Code que haga la mayoría por ti. Pero lo importante es que entiendes qué está pasando debajo — por eso la sesión 2 era necesaria.

### Commits desde Claude Code

En vez de escribir los comandos tú, puedes decirle:

> "Haz un commit con todo lo que llevamos. Mensaje: 'Estructura base del Pipeline Tool con componentes iniciales'."

El agente ejecuta `git add .` y `git commit -m "..."` por ti. Verás los comandos en la terminal.

### Branches desde Claude Code

Antes de un cambio grande:

> "Crea una branch llamada 'feature/filtro-por-fecha' y muévete a ella."

Si quieres volver a main:

> "Vuelve a la branch main."

Si quieres fusionar:

> "Fusiona la branch 'feature/filtro-por-fecha' a main."

### El flujo completo en la práctica

Así se ve una sesión típica de trabajo con Claude Code:

```
TÚ:     "Haz un commit con lo que tenemos antes de empezar."
AGENTE: [ejecuta git add . && git commit]

TÚ:     "Crea una branch 'feature/sidebar' para trabajar el menú lateral."
AGENTE: [ejecuta git checkout -b feature/sidebar]

TÚ:     "Crea un componente Sidebar con links a: Dashboard, Proyectos, Equipo, Calendario."
AGENTE: [crea archivos, modifica layout]

TÚ:     [Revisas en el navegador — se ve bien]
TÚ:     "Se ve bien. Haz commit."
AGENTE: [ejecuta git add . && git commit]

TÚ:     "Ahora quiero que al hacer click en cada link, cambie la vista principal."
AGENTE: [implementa routing]

TÚ:     [Revisas — funciona, pero el link activo no se resalta]
TÚ:     "Funciona, pero necesito que el link de la página actual se vea resaltado — con un fondo más oscuro o un borde lateral."
AGENTE: [ajusta estilos]

TÚ:     "Perfecto. Commit y fusiona a main."
AGENTE: [commit, checkout main, merge]
```

¿Ves el patrón? Es una conversación fluida donde tú diriges y el agente ejecuta. Cada cambio queda guardado con Git. Si algo se rompe, reviertes. Si todo va bien, avanzas. Sin miedo, sin drama.

---

## Comandos útiles de Claude Code

Además de la conversación normal, Claude Code tiene algunos comandos especiales que se ejecutan directamente (no son para el agente, son para la herramienta).

| Comando | Qué hace |
|---|---|
| `/help` | Muestra la lista de comandos disponibles |
| `/clear` | Limpia la conversación actual (útil cuando la ventana de contexto se llena) |
| `/compact` | Compacta la conversación para ahorrar espacio en la ventana de contexto, manteniendo un resumen |
| `/cost` | Muestra cuántos tokens has usado en la sesión |

El más importante es `/compact`. Cuando llevas un rato trabajando y la conversación se hace larga, el agente empieza a sufrir drift (se olvida de cosas). En ese momento, usas `/compact` y el agente resume la conversación, liberando espacio en la ventana de contexto. Es como hacer limpieza en tu mesa de trabajo sin tirar lo importante.

---

## Cuándo el agente se equivoca

Se va a equivocar. Mucho. Acéptalo como parte del proceso. Lo importante no es que el agente sea perfecto — es que tú sepas qué hacer cuando falla.

### Errores comunes y cómo manejarlos

**El agente cambia cosas que no le pediste.** A veces decide "mejorar" algo que ya estaba funcionando. Si ves en el diff que tocó archivos que no mencionaste, recházalo: "No modifiques el archivo X — solo te pedí cambios en Y. Revierte lo que hiciste en X."

**El agente instala dependencias innecesarias.** Si de pronto aparece una librería nueva que no esperabas, pregúntale por qué la necesita. A veces hay una razón legítima. A veces no, y puedes pedirle que use lo que ya hay.

**El agente pierde el hilo.** Después de muchas instrucciones, el agente puede empezar a contradecirse o a olvidar lo que le dijiste. Ahí usas `/compact` para limpiar la conversación, o cierras la sesión y abres una nueva (el CLAUDE.md se vuelve a cargar y el agente arranca fresco).

**El agente genera código que no compila.** A veces el código que genera tiene errores de sintaxis. Simplemente dile: "El servidor da error. Revisa y corrige." El agente puede leer los mensajes de error de la terminal y generalmente se autocorrige.

**El agente "alucina" funcionalidades.** Inventa APIs que no existen o usa funciones de librerías que no funcionan como él dice. Esto es más raro con Claude Code que con el chat, pero pasa. Si sospechas que algo no tiene sentido, pregúntale: "¿Estás seguro de que esa función existe en esta versión de la librería?"

### La regla de oro

Si después de 3 intentos el agente no logra resolver un problema, cambia de estrategia. No le repitas lo mismo esperando un resultado diferente. Opciones:

1. **Reformula la instrucción** con más contexto o de forma diferente.
2. **Divide el problema** en partes más pequeñas y resuélvelas una por una.
3. **Revierte y empieza de nuevo** desde el último commit que funcionaba.
4. **Usa `/compact`** o abre una sesión nueva para que el agente arranque limpio.

---

## Lo que vamos a construir hoy

En la clase de hoy vamos a rehacer el Pipeline Tool desde cero con Claude Code. Específicamente, vamos a crear:

1. **La estructura del proyecto** — `npm create vite@latest` con React, instalar Tailwind, organizar las carpetas según el CLAUDE.md.
2. **El layout principal** — Sidebar con navegación, header con el nombre de la app, área de contenido principal.
3. **El componente de lista de proyectos** — Tabla con datos hardcodeados (la conexión a base de datos la hacemos en la sesión 5).
4. **El componente de vista de equipo** — Grilla con los miembros del equipo y su carga de trabajo simulada.

Todo con el CLAUDE.md que escribimos en la sesión 3 guiando al agente. Vas a ver en tiempo real cómo cambia la calidad del output cuando el agente tiene contexto vs. cuando no lo tiene.

Al final de la clase, vas a tener una app que corre en tu navegador, con navegación funcional, datos de ejemplo y una estructura limpia lista para conectarle un backend mañana.

---

## Glosario rápido

Términos nuevos de esta sesión.

| Término | Qué significa |
|---|---|
| **Diff** | La vista que muestra exactamente qué cambió entre dos versiones de un archivo. Líneas en verde = agregadas, líneas en rojo = eliminadas. Claude Code lo muestra después de cada cambio. |
| **Sesión** | Una conversación continua con Claude Code. Cada sesión tiene su propia ventana de contexto. Cuando cierras y abres otra, el agente empieza limpio (pero lee el CLAUDE.md de nuevo). |
| **Datos hardcodeados** | Datos de ejemplo escritos directamente en el código para probar la interfaz antes de conectar una base de datos real. Son temporales — los reemplazamos en la sesión 5. |
| **Routing** | El sistema que permite navegar entre distintas "páginas" dentro de una app web sin que el navegador recargue. Cuando haces clic en "Proyectos" y cambia la vista sin recargar, eso es routing. |
| **Compilar** | El proceso de convertir el código que escribes en algo que el navegador puede ejecutar. Si hay errores de sintaxis, la compilación falla y la app no corre. |
| **Servidor de desarrollo** | Un servidor que corre en tu computadora mientras desarrollas. Te permite ver tu app en el navegador y se actualiza automáticamente cuando haces cambios. Se levanta con un comando como `npm run dev`. |
| **Refactorizar** | Reorganizar o reescribir código existente sin cambiar lo que hace. El objetivo es que quede más limpio o más eficiente. Solo se hace cuando es necesario, no por deporte. |
| **/compact** | Comando de Claude Code que resume la conversación para liberar espacio en la ventana de contexto. Úsalo cuando el agente empiece a perder el hilo. |

---

> **Nos vemos en clase.** Asegúrate de tener el proyecto de las sesiones anteriores abierto, con el CLAUDE.md listo y tu terminal disponible. Hoy construimos.
