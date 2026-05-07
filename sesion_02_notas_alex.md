# Sesión 2: Git — Tu red de seguridad

> **Lectura previa.** Lee esto antes de la clase. En la sesión vamos directo a la práctica.

---

## El problema sin Git

Imagínate esto: llevas dos horas trabajando con Claude Code en el Pipeline Tool. Has avanzado bien — el layout se ve perfecto, los estados de los proyectos funcionan, las asignaciones del equipo cargan correctamente. Entonces le pides al agente que agregue un filtro por fecha. El agente toca varios archivos, algo se rompe, y de pronto ya nada funciona. El layout se descuadró, los datos no cargan, y el filtro tampoco funciona.

¿Qué haces?

Sin Git, tus opciones son: intentar explicarle al agente qué se rompió y rezar para que lo arregle sin romper otra cosa, o empezar de cero. Las dos opciones son malas. La primera es poco confiable porque el agente no siempre sabe qué rompió. La segunda es un desperdicio de tiempo brutal.

Con Git, la solución toma 10 segundos: vuelves a la versión que funcionaba antes de pedir el filtro. Literalmente un comando. Tu proyecto regresa al estado exacto en el que estaba hace dos horas. Desde ahí, puedes volver a pedir el filtro, pero con mejor contexto, y si se vuelve a romper, vuelves a revertir. Sin drama, sin perder trabajo.

¿Te acuerdas de los tres problemas que mencionamos en la sesión 1? No hay fuente de verdad, no hay red de seguridad, no hay especificación. Git resuelve el segundo. Es tu red de seguridad. Todo lo demás se apoya en que Git exista.

---

## ¿Qué es Git?

Git es un **sistema de control de versiones**. En lenguaje humano: es un programa que guarda el historial completo de tu proyecto. Cada vez que decides "guardar" (en Git se llama hacer un **commit**), Git toma una foto de todos los archivos de tu proyecto en ese momento. Esa foto queda guardada para siempre. Puedes tener 10 fotos, 100, 500 — cada una es un punto al que puedes regresar.

**La analogía más útil: guardar partida en un videojuego.**

Cuando juegas un juego difícil, guardas partida antes de una pelea de jefe. Si pierdes, cargas la partida guardada y lo intentas de nuevo. No empiezas el juego entero desde cero. Git es exactamente eso para tu proyecto.

Cada commit es un punto de guardado. Tú decides cuándo guardar y qué nombre le pones a cada guardado (el **mensaje del commit**). Si algo sale mal después, cargas el guardado anterior.

Lo que hace a Git especialmente poderoso es que no solo guarda la foto — guarda **qué cambió** entre una foto y otra. Puedes ver exactamente qué archivos se modificaron, qué líneas se agregaron y cuáles se borraron. Eso es muy útil cuando necesitas entender por qué algo se rompió.

---

## Git no es para programadores — es para cualquiera que construya algo que evoluciona

Hay un mito de que Git es "cosa de programadores" y que es complicadísimo. Es verdad que Git tiene funcionalidades avanzadas que son complejas. Pero tú no necesitas esas funcionalidades. Necesitas el 10% de Git que resuelve el 90% de los problemas. Y ese 10% es tan simple como copiar y pegar en Excel.

Además, un dato importante: **Claude Code sabe usar Git**. Puedes pedirle al agente que haga commits por ti, que cree branches, que revise el historial. No necesitas memorizar comandos — necesitas entender los conceptos para poder darle instrucciones al agente. Igual que no necesitas saber construir una pared, pero sí necesitas saber qué es un muro de carga para no pedirle al arquitecto que lo quite.

Dicho esto, en esta sesión sí vamos a usar los comandos directamente. ¿Por qué? Porque quiero que entiendas qué está pasando debajo. Cuando le pidas al agente "haz un commit", quiero que sepas exactamente qué es un commit. Cuando el agente diga "creé una branch", quiero que sepas qué significa eso. Después de hoy, podrás elegir: hacerlo tú o pedírselo al agente. Las dos opciones son válidas.

---

## El flujo básico: init → add → commit → push

Todo el trabajo con Git se reduce a un ciclo de cuatro pasos. Vamos uno por uno.

### 1. `git init` — Crear el punto de guardado inicial

Este comando le dice a Git: "esta carpeta es un proyecto que quiero rastrear." Solo lo haces una vez, al inicio. A partir de ese momento, Git está observando todo lo que pasa en esa carpeta.

```
git init
```

Cuando tú hiciste `uv init` en la sesión pasada para crear el proyecto, eso ya creó una carpeta de proyecto. Ahora con `git init` le agregas el superpoder de tener historial.

### 2. `git add` — Elegir qué incluir en la foto

Antes de guardar partida, tienes que decirle a Git qué archivos quieres incluir en la foto. Esto se llama el **staging area** (área de preparación). Piénsalo como poner cosas en una caja antes de sellarla.

```
git add .
```

El punto (`.`) significa "todo lo que haya cambiado". La mayoría del tiempo, eso es lo que quieres. Pero también puedes agregar archivos específicos si solo quieres guardar algunos cambios:

```
git add archivo1.py archivo2.py
```

¿Por qué existe este paso intermedio? ¿Por qué no se guarda todo directamente? Porque a veces haces cambios en varios archivos pero solo algunos están listos. El staging area te da control. En la práctica, como vibecoder, casi siempre vas a usar `git add .` para agregar todo. Pero es bueno que sepas que la opción existe.

### 3. `git commit` — Tomar la foto

Este es el momento de guardar partida. Un commit toma una foto de todo lo que pusiste en el staging area y la guarda con un mensaje que la describe.

```
git commit -m "Agregar layout principal del Pipeline Tool"
```

El `-m` es por "message" (mensaje). Lo que va entre comillas es la descripción de qué hiciste. Esto es importante: seis meses después, cuando veas tu historial, ese mensaje es lo que te va a decir qué pasó en cada punto de guardado.

**Buenos mensajes de commit:**
- "Agregar vista de carga de trabajo por semana"
- "Corregir bug donde los proyectos no se guardaban al refrescar"
- "Implementar filtro por estado del proyecto"

**Malos mensajes de commit:**
- "Cambios"
- "Arreglos"
- "WIP"
- "asdfasdf"

Un buen mensaje responde la pregunta: "¿qué cambió y por qué?"

### 4. `git push` — Subir la foto a la nube

Hasta este punto, todo está en tu computadora. Si se te cae un café encima del laptop, pierdes todo. `git push` sube tus commits a un servidor remoto (casi siempre GitHub) donde quedan respaldados.

```
git push
```

Además de ser tu backup, esto permite que otras personas (o tú desde otra computadora) puedan acceder al proyecto. Cuando despliegues el Pipeline Tool, el servidor de despliegue va a leer tu código desde GitHub.

### El ciclo completo

Esto es lo que vas a repetir docenas de veces en cada sesión:

```
git add .
git commit -m "Descripción de lo que cambié"
git push
```

Tres comandos. Eso es. El `init` solo se hace una vez. Después de eso, tu vida con Git es: hago cambios → los agrego → los guardo → los subo. Repetir.

---

## GitHub — Tu proyecto en la nube

Git y GitHub no son lo mismo, aunque la gente los confunde constantemente.

- **Git** es la herramienta que corre en tu computadora y maneja el historial de versiones.
- **GitHub** es un servicio en internet (github.com) donde puedes subir tus repositorios de Git para tenerlos en la nube.

Piénsalo así: Git es Microsoft Word (la herramienta para crear documentos) y GitHub es Google Drive (el lugar donde los guardas para tener backup y poder compartirlos).

### Crear un repositorio en GitHub

El flujo es:

1. Entras a github.com y creas un **repositorio nuevo** (un repo). Le pones nombre, por ejemplo `project-pipeline-tool`.
2. GitHub te da una URL única para ese repo.
3. En tu computadora, le dices a Git que tu repo local está conectado a ese repo remoto:
   ```
   git remote add origin https://github.com/tu-usuario/project-pipeline-tool.git
   ```
4. Subes tus commits:
   ```
   git push -u origin main
   ```

A partir de ahí, cada vez que hagas `git push`, tus cambios suben a GitHub automáticamente.

### ¿Necesitas una cuenta de GitHub?

Sí. Si no tienes una, créala antes de la clase en [github.com](https://github.com). Es gratis. Usa tu email personal, no el corporativo — así el repo es tuyo y no de la empresa.

---

## Branches — Trabajar sin miedo a romper

Este es el concepto que más potencia le da a Git cuando trabajas con agentes de IA.

Una **branch** (rama) es una línea paralela de tu proyecto. Imagina que tu proyecto es un camino recto. En algún punto, creas una bifurcación: un camino alternativo donde puedes experimentar sin afectar el camino principal. Si lo que haces en el camino alternativo funciona, lo fusionas de vuelta al camino principal. Si no funciona, simplemente lo abandonas y el camino principal sigue intacto.

En Git, el camino principal se llama **main** (antes se llamaba "master", por si ves ese nombre en algún tutorial viejo). Cada branch que creas es un camino alternativo.

### ¿Cuándo crear una branch?

La regla simple: **antes de cada cambio grande o experimento**. Algunos ejemplos:

- Vas a pedirle al agente que implemente el sistema de permisos → crea una branch `feature/permisos`.
- Quieres probar si Supabase funciona mejor que Firebase para la base de datos → crea una branch `experiment/supabase`.
- Necesitas arreglar un bug urgente sin perder el trabajo en progreso → crea una branch `fix/bug-refresh`.

### El flujo con branches

```
git checkout -b feature/nueva-funcionalidad
```

Este comando crea una branch nueva Y te mueve a ella. A partir de ese momento, todos los cambios que hagas (y todos los commits que hagas) quedan en esa branch. La branch `main` sigue exactamente como estaba.

Cuando terminas y todo funciona:

```
git checkout main
git merge feature/nueva-funcionalidad
```

El primer comando te regresa a `main`. El segundo trae los cambios de tu branch a `main`. Si todo va bien, los cambios se integran y listo.

Si en cambio el experimento salió mal y quieres descartarlo:

```
git checkout main
```

Vuelves a `main` y la branch experimental queda ahí abandonada. Tu proyecto principal nunca se enteró de que algo salió mal.

### ¿Cuántas branches necesitas?

Para tus proyectos, un flujo simple es suficiente: `main` es tu versión estable (la que funciona) y creas una branch temporal cada vez que vas a hacer un cambio grande. No necesitas flujos complicados con múltiples branches permanentes. Eso es para equipos de 20 desarrolladores, no para un vibecoder trabajando con un agente.

---

## Git + Claude Code — El combo poderoso

Ahora viene la parte que más te importa: cómo se conecta todo esto con tu flujo de vibecoding.

Claude Code entiende Git. Puedes pedirle cosas como:

- *"Haz un commit con lo que llevamos"*
- *"Crea una branch para implementar el filtro de fechas"*
- *"Muéstrame qué archivos cambié desde el último commit"*
- *"Revierte el último cambio, algo se rompió"*

Pero más allá de la conveniencia, la razón por la que Git es fundamental para vibecoding es esta: **Git te permite ser agresivo con los cambios sin miedo**.

Sin Git, cada vez que le pides algo al agente, estás apostando. Si el cambio sale bien, ganaste. Si sale mal, perdiste todo lo anterior. Eso te hace conservador — pides cambios pequeños, pruebas con miedo, y avanzas lento.

Con Git, la apuesta desaparece. Antes de pedirle algo arriesgado al agente, haces un commit. Si el agente lo rompe todo, reviertes. Si funciona, sigues adelante. Puedes pedirle cambios grandes, refactorizaciones completas, experimentos locos — porque siempre puedes volver atrás.

**El flujo profesional con Claude Code:**

1. Haces un commit de lo que ya funciona.
2. Le pides al agente el siguiente cambio.
3. Pruebas si funciona.
4. Si funciona → commit → siguiente cambio.
5. Si no funciona → reviertes → reformulas tu instrucción → vuelves a intentar.

Ese ciclo es rápido, seguro y te da confianza para avanzar sin miedo.

---

## Comandos de supervivencia — Ver qué pasó y cómo volver atrás

Además del ciclo básico (add → commit → push), hay unos cuantos comandos que vas a usar constantemente para ver el estado de las cosas y para deshacerte de cambios que no quieres.

### Ver en qué estado estás

```
git status
```

Te dice: en qué branch estás, qué archivos cambiaron, cuáles están en el staging area y cuáles no. Es como preguntarle a Git "¿qué está pasando ahorita?" Úsalo siempre que no estés seguro de dónde estás parado.

### Ver el historial de commits

```
git log --oneline
```

Te muestra la lista de todos tus commits, del más reciente al más viejo, en una línea cada uno. Algo así:

```
a3f7b2c Implementar filtro por fecha
e9d1a4f Agregar vista de carga de trabajo
7c2b8e1 Layout principal del Pipeline Tool
b4a9f3d Commit inicial
```

Cada commit tiene un identificador único (esa combinación de letras y números a la izquierda). Lo vas a necesitar si quieres volver a un punto específico.

### Ver qué cambió

```
git diff
```

Te muestra exactamente qué líneas se agregaron y cuáles se borraron desde el último commit. Es útil para revisar qué hizo el agente antes de hacer un commit.

### Deshacer cambios que aún no has guardado

Si el agente hizo algo que no te gustó y aún no hiciste commit:

```
git checkout .
```

Esto borra todos los cambios no guardados y regresa los archivos al estado del último commit. Es el equivalente de "deshacer todo desde la última vez que guardé."

### Volver a un commit anterior

Si ya hiciste commit pero quieres volver al commit anterior:

```
git revert HEAD
```

Esto crea un nuevo commit que "deshace" lo que hiciste en el último. No borra historial — agrega un commit nuevo que revierte los cambios. Es la forma segura de echarte para atrás.

Si necesitas volver a un commit más viejo (no el inmediatamente anterior), usas el identificador del commit:

```
git revert a3f7b2c
```

---

## .gitignore — Lo que Git NO debe rastrear

No todo en tu proyecto debe estar en Git. Hay archivos que se generan automáticamente (como las dependencias que instala UV) y archivos que contienen información sensible (como tus claves de API y contraseñas). Estos no deben subirse a GitHub.

El archivo `.gitignore` le dice a Git: "ignora estos archivos, no los incluyas en las fotos." Es un archivo de texto simple que pones en la raíz de tu proyecto.

Un `.gitignore` básico para tus proyectos se ve así:

```
# Dependencias (las instala UV, no necesitan estar en Git)
node_modules/
.venv/
__pycache__/

# Variables de entorno (NUNCA deben subirse)
.env
.env.local

# Archivos del sistema operativo
.DS_Store
Thumbs.db
```

La línea más importante es `.env`. En la sesión 3 vamos a hablar de variables de entorno y por qué nunca, nunca pones credenciales en el código. El `.gitignore` es tu primera línea de defensa contra eso.

---

## Lo que haremos en clase

1. Crear una cuenta de GitHub (si no la tienes).
2. Crear un repositorio en GitHub para el Project Pipeline Tool.
3. Inicializar Git en el proyecto que creamos en la sesión 1.
4. Hacer 3 commits con cambios distintos para practicar el ciclo add → commit → push.
5. Ver el historial con `git log` y las diferencias con `git diff`.
6. Romper algo a propósito, revertirlo y verificar que el proyecto volvió a funcionar.
7. Crear una branch, hacer cambios en ella, y fusionarla de vuelta a `main`.
8. Practicar el flujo con Claude Code: pedirle al agente un cambio, hacer commit, pedirle otro cambio, revertir si es necesario.

No te preocupes si algún comando no te queda claro de la lectura. La clase es para hacerlo juntos. Lo importante es que llegues con el modelo mental claro: Git es tu sistema de guardado, commits son puntos a los que puedes volver, y branches son caminos paralelos para experimentar sin riesgo.

---

## Glosario rápido

Términos nuevos de esta sesión. Junto con los de la sesión 1, este es tu vocabulario completo hasta ahora.

| Término | Qué significa |
|---|---|
| **Git** | Programa que guarda el historial de versiones de tu proyecto. Corre en tu computadora. |
| **GitHub** | Servicio en la nube donde subes tus repositorios de Git para tener backup y poder compartirlos. |
| **Repositorio (repo)** | Una carpeta de proyecto controlada por Git. Tiene todo el historial de cambios desde el primer commit. |
| **Commit** | Una "foto" de tu proyecto en un momento dado. Un punto de guardado al que puedes volver. |
| **Staging area** | El área de preparación donde pones los archivos que quieres incluir en el próximo commit. Se llena con `git add`. |
| **Branch (rama)** | Una línea paralela de desarrollo. Te permite experimentar sin afectar la versión principal del proyecto. |
| **Main** | La branch principal de tu proyecto. La versión estable, la que funciona. |
| **Merge (fusionar)** | Traer los cambios de una branch a otra. Normalmente fusionas una branch de feature de vuelta a `main`. |
| **Push** | Subir tus commits de tu computadora a GitHub. |
| **Pull** | Bajar los cambios de GitHub a tu computadora. Lo opuesto de push. |
| **Clone (clonar)** | Descargar un repositorio de GitHub a tu computadora por primera vez. |
| **Revert** | Crear un commit que deshace los cambios de un commit anterior. La forma segura de echarte para atrás. |
| **Diff** | La diferencia entre dos versiones de un archivo. Muestra qué líneas se agregaron y cuáles se borraron. |
| **.gitignore** | Archivo que le dice a Git qué archivos o carpetas ignorar (no rastrear ni subir). |
| **Remote (remoto)** | El repositorio en la nube (GitHub). Lo opuesto de tu repositorio local en tu computadora. |

---

## Cheatsheet de comandos

Referencia rápida. No necesitas memorizarlos — tenlos a la mano mientras trabajas.

| Qué quieres hacer | Comando |
|---|---|
| Inicializar Git en un proyecto | `git init` |
| Ver el estado actual | `git status` |
| Agregar todos los cambios al staging | `git add .` |
| Guardar un commit | `git commit -m "mensaje"` |
| Subir cambios a GitHub | `git push` |
| Ver historial de commits | `git log --oneline` |
| Ver qué cambió desde el último commit | `git diff` |
| Deshacer cambios no guardados | `git checkout .` |
| Revertir el último commit | `git revert HEAD` |
| Crear y moverte a una branch nueva | `git checkout -b nombre-de-branch` |
| Volver a main | `git checkout main` |
| Fusionar una branch a main | `git merge nombre-de-branch` |
| Conectar un repo remoto | `git remote add origin URL` |
| Clonar un repo de GitHub | `git clone URL` |

---

> **Nos vemos en clase.** Si no tienes cuenta de GitHub, créala antes en [github.com](https://github.com). Si ya la tienes, asegúrate de que recuerdas tu contraseña. No necesitas nada más.
