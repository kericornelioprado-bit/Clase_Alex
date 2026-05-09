# Sesión 3: Context Engineering — La meta-habilidad

> **Lectura previa.** Lee esto antes de la clase. En la sesión vamos directo a la práctica.

---

## El problema del prompt suelto

Abre Claude o ChatGPT y escribe: "Hazme una app de gestión de proyectos." ¿Qué pasa? El agente genera algo. Pero ese algo probablemente no se parece a lo que tú tenías en la cabeza. Tal vez usó React cuando tú querías algo más simple. Tal vez puso los proyectos en una lista cuando tú querías un tablero tipo Kanban. Tal vez inventó campos que no necesitas y no incluyó los que sí. Tal vez metió colores que no van con la marca de tu empresa.

¿De quién es la culpa? Del agente, no. Tuya, tampoco. El problema es que un prompt suelto no tiene suficiente información para que el agente haga algo útil. Es como si le dijeras a un arquitecto "hazme una casa" sin decirle cuántos cuartos, en qué terreno, con qué presupuesto, para cuántas personas, en qué clima. El arquitecto va a diseñar algo, pero probablemente no lo que necesitas.

Hasta ahora en el curso instalamos las herramientas (sesión 1) y la red de seguridad (sesión 2). Hoy vamos a aprender la habilidad que determina la calidad de todo lo que construyas de aquí en adelante: **context engineering**. Es la diferencia entre darle a tu agente una instrucción vaga y darle un brief profesional completo.

---

## Prompts estructurados — El punto de partida

Antes de llegar al context engineering completo, hay un paso intermedio que ya es mucho mejor que el prompt suelto: **estructurar lo que le dices al agente**.

Hay un template simple que funciona muy bien para cualquier conversación con un agente de IA, ya sea en claude.ai, ChatGPT o donde sea:

```
Tu rol:
[Quién es el agente en esta conversación]

Contexto:
[Información de fondo que el agente necesita para entender la situación]

Tu tarea:
[La instrucción concreta de lo que debe hacer]

Requisitos:
[Restricciones o condiciones específicas — solo cuando la ocasión lo requiere]
```

Vamos a desarmarlo:

### Tu rol

Cuando le dices a un agente quién es, cambia la calidad de su respuesta. No es lo mismo "escríbeme un email" que "Eres un project manager senior en una empresa de market research. Escríbeme un email." El agente ajusta su tono, su vocabulario y sus decisiones en función del rol que le asignas.

Buenos roles para tus proyectos:
- "Eres un desarrollador fullstack senior especializado en aplicaciones web con React y Supabase."
- "Eres un arquitecto de software que diseña herramientas internas para equipos pequeños."
- "Eres un experto en integración de APIs y automatización de procesos de negocio."

El rol no tiene que ser verdad — es una instrucción que moldea el comportamiento del agente.

### Contexto

Esta es la parte que la mayoría de la gente se salta, y es la que más impacto tiene. El contexto es todo lo que el agente necesita saber sobre tu situación para no tomar decisiones en el vacío.

Ejemplo sin contexto:
> "Crea un dashboard de proyectos."

Ejemplo con contexto:
> "Estoy construyendo una herramienta interna para mi equipo de 12 personas en una empresa de market research. El equipo gestiona entre 30 y 40 proyectos simultáneos. Actualmente usamos un Excel que yo mantengo manualmente, pero necesitamos algo donde todos puedan ver la carga de trabajo. Solo yo debo poder crear y editar proyectos; el resto del equipo solo necesita ver."

¿Ves la diferencia? Con contexto, el agente sabe cuántos usuarios hay, qué problema resuelves, qué permisos necesitas y qué estás reemplazando. Sus decisiones van a ser radicalmente mejores.

### Tu tarea

La instrucción concreta. Aquí la clave es ser específico sin ser excesivo. Dile qué quieres que haga, no cómo hacerlo (a menos que tengas una razón técnica para preferir una solución sobre otra).

Buena tarea:
> "Crea el componente de vista de proyectos con columnas para: nombre del proyecto, PM asignado, estado (activo/pausado/completado), fecha de entrega y peso del proyecto (1-5)."

Mala tarea:
> "Haz la parte de los proyectos."

### Requisitos

Este bloque es opcional. Lo usas cuando hay restricciones importantes que el agente debe respetar. No lo metas siempre — solo cuando la ocasión lo requiere.

Ejemplos de cuándo sí usarlo:
- "No uses TypeScript, solo JavaScript."
- "Los colores deben ser: primario #1E3A5F, secundario #4A90D9, fondo #F5F7FA."
- "El componente no debe hacer llamadas a la API — recibe los datos por props."
- "Responde en español."

Ejemplos de cuándo NO hace falta:
- Cuando la tarea es simple y no tiene restricciones especiales.
- Cuando el requisito es obvio del contexto (no necesitas decirle "que funcione").

### ¿Cuándo usas este template?

Para conversaciones puntuales. Cuando abres claude.ai o ChatGPT y necesitas que el agente haga algo bien a la primera. Es tu herramienta para el día a día.

Pero tiene un límite: cuando trabajas en un proyecto durante días o semanas con Claude Code, no puedes repetir este contexto en cada mensaje. Sería como si cada mañana le explicaras a tu equipo desde cero quién eres, qué hace la empresa y en qué proyecto están trabajando. Necesitas algo más permanente. Ahí entra el context engineering de verdad.

---

## ¿Qué es context engineering?

El término viene de una idea simple: la calidad de lo que genera la IA depende directamente de la calidad del contexto que recibe. "Context engineering" es el arte de diseñar y estructurar toda la información que un agente necesita para hacer bien su trabajo.

La diferencia con un prompt es de alcance:

- Un **prompt** es lo que le dices al agente en un mensaje. Es temporal, se usa una vez.
- **Context engineering** es todo el sistema de información que rodea al agente mientras trabaja en tu proyecto. Es permanente — está ahí en cada interacción.

Piénsalo así: el prompt es lo que le dices a un nuevo empleado cuando le asignas una tarea. El context engineering es el onboarding completo: el manual de la empresa, las reglas de estilo, la documentación de procesos, el acceso a los sistemas. Un buen onboarding hace que el empleado sea productivo desde el día uno. Un mal onboarding (o ninguno) hace que tenga que adivinar y pregunte cada cinco minutos.

Para entender por qué esto importa tanto, necesitas saber un concepto técnico: la **ventana de contexto**.

### La ventana de contexto (context window)

Cuando hablas con un agente de IA, el agente no tiene memoria permanente. Lo único que sabe es lo que está dentro de su **ventana de contexto** — la cantidad de texto que puede "ver" en un momento dado. Piénsalo como una mesa de trabajo: solo puede tener encima un número limitado de documentos. Si la mesa se llena, los documentos más viejos se caen.

Cuando usas Claude Code, la ventana de contexto incluye:
- Tu mensaje más reciente
- El historial de la conversación actual
- Los archivos de tu proyecto que el agente haya leído
- El archivo CLAUDE.md (si existe)

Todo eso compite por espacio en la ventana. Si la conversación se hace muy larga, el agente empieza a "olvidar" lo que le dijiste al principio. A esto le llamamos **drift** — la deriva del agente.

### El drift: cuando el agente se olvida

¿Te ha pasado que empiezas una conversación con Claude o ChatGPT, todo va bien, y después de 20 mensajes el agente empieza a hacer cosas raras? Cambia de estilo, olvida restricciones que le diste, toma decisiones que contradicen lo que habían acordado. Eso es drift.

El drift pasa porque la información del inicio de la conversación se va quedando atrás a medida que la conversación crece. El agente le da más peso a lo reciente que a lo antiguo.

¿Cómo lo combates? Con un archivo de contexto que el agente lee al inicio de cada interacción. No importa qué tan larga sea la conversación — las reglas del archivo siempre están ahí. Ese archivo es el CLAUDE.md.

---

## CLAUDE.md — El manual de instrucciones de tu agente

El CLAUDE.md es un archivo de texto que pones en la raíz de tu proyecto (la carpeta principal). Cuando Claude Code se abre en un proyecto que tiene CLAUDE.md, lo primero que hace es leerlo. Es como si le entregaras un manual de instrucciones antes de empezar a trabajar.

Este archivo es el núcleo de tu context engineering. Todo lo que el agente necesita saber sobre tu proyecto va aquí. Vamos a ver las secciones que debe tener un buen CLAUDE.md.

### Sección 1: Descripción del proyecto

¿Qué es este proyecto? ¿Para quién es? ¿Qué problema resuelve? No necesitas escribir un ensayo — unas pocas líneas claras bastan.

```markdown
# Project Pipeline Tool

Herramienta interna de gestión de proyectos para un equipo de 12 personas
en una empresa de market research. Reemplaza un Excel manual.
Los PMs asignan proyectos y gestionan la carga de trabajo del equipo.
El resto del equipo puede consultar pero no editar.
```

¿Por qué importa esto? Porque sin esta sección, el agente no sabe si está construyendo una app para 10 usuarios o para 10,000. No sabe si es una herramienta interna o un producto público. Esas diferencias cambian las decisiones técnicas que toma.

### Sección 2: Stack tecnológico

Le dices al agente qué tecnologías usar. Esto evita que tome decisiones por su cuenta (que a veces son buenas y a veces son desastrosas).

```markdown
## Stack

- Frontend: React con Vite
- Estilos: Tailwind CSS
- Backend: Supabase (PostgreSQL + Auth + API)
- Despliegue: Vercel (frontend), Supabase (backend)
- Lenguaje: JavaScript (NO TypeScript)
- Package manager: npm
```

La línea "NO TypeScript" es un ejemplo de restricción explícita. Si no se lo dices, Claude Code a veces decide usar TypeScript porque técnicamente es "mejor práctica". Pero si tú no sabes TypeScript y no quieres aprender ahora, es una decisión que te va a generar problemas. Mejor ser explícito.

### Sección 3: Estructura del proyecto

¿Cómo se organizan los archivos? Esto le dice al agente dónde poner las cosas y evita que cree carpetas por todas partes sin orden.

```markdown
## Estructura

src/
  components/    → Componentes de React reutilizables
  pages/         → Páginas principales de la app
  lib/           → Conexión con Supabase y utilidades
  hooks/         → Custom hooks de React
public/          → Archivos estáticos (imágenes, favicon)
```

### Sección 4: Reglas de estilo y convenciones

Aquí defines cómo quieres que se vea y se comporte el código. No necesitas saber programar para definir esto — son decisiones de organización.

```markdown
## Convenciones

- Nombres de archivos: kebab-case (ejemplo: project-list.jsx, no ProjectList.jsx)
- Nombres de componentes: PascalCase (ejemplo: ProjectList, TeamView)
- Un componente por archivo
- Los comentarios en el código van en español
- No crear archivos de más de 200 líneas — si un componente crece mucho, dividirlo
```

### Sección 5: Restricciones (lo que NO debe hacer)

Esta es una de las secciones más importantes. Los agentes de IA a veces deciden "mejorar" cosas que no les pediste. Tal vez decides cambiar una librería por otra que considera mejor, o refactoriza un archivo que ya estaba funcionando. Las restricciones previenen esto.

```markdown
## Restricciones

- NO cambiar de stack sin permiso explícito
- NO instalar dependencias nuevas sin consultarme antes
- NO borrar archivos existentes
- NO refactorizar código que ya funciona a menos que se lo pida
- NO agregar funcionalidades que no estén en la especificación
- NO usar console.log para debugging — usar un logger apropiado
```

### Sección 6: Contexto de negocio

Información que ayuda al agente a tomar mejores decisiones sobre UX y lógica.

```markdown
## Contexto de negocio

- El equipo trabaja en horario US (EST) desde España
- Los proyectos tienen un peso de 1-5 que indica complejidad/esfuerzo
- Cada persona del equipo tiene una capacidad máxima de 10 puntos de peso por semana
- Los estados de un proyecto son: Pipeline, Active, On Hold, Completed
- Los PMs pueden ver y editar todo. El resto del equipo solo puede ver.
- El equipo tiene PTO y holidays que afectan la capacidad disponible
```

### ¿El CLAUDE.md es un system prompt?

Sí y no. Si has usado la API de OpenAI o Anthropic, sabes que existe algo llamado **system prompt**: un mensaje invisible que le da instrucciones al modelo antes de que el usuario diga nada. El system prompt moldea el comportamiento del agente desde el inicio.

El CLAUDE.md funciona de forma parecida — es una instrucción que el agente lee antes de hacer cualquier cosa. La diferencia es que el system prompt de la API está oculto, mientras que el CLAUDE.md es un archivo visible en tu proyecto que puedes editar, versionar con Git y compartir con otros. Es tu system prompt, pero portable y transparente.

Si usas el template de prompts estructurados que vimos arriba (Tu rol / Contexto / Tu tarea / Requisitos), el CLAUDE.md es como tener el "Tu rol" y el "Contexto" pregrabados permanentemente. Cada vez que le hablas a Claude Code, solo necesitas darle la tarea. El contexto ya está ahí.

---

## Seguridad — Lo que nunca va en el código

¿Te acuerdas de que tu colega te advirtió sobre tener cuidado con las credenciales? Vamos a explicar exactamente qué significa eso y cómo manejarlo correctamente.

### ¿Qué son las credenciales?

Son las llaves que le dan a tu aplicación acceso a servicios externos. Algunos ejemplos:

- **API keys:** Una API key es como una contraseña que le permite a tu app conectarse con un servicio externo. Por ejemplo, para conectar el Opportunity Radar con NewsAPI (para obtener noticias), necesitas una API key de NewsAPI. Para conectar con HubSpot, necesitas una API key de HubSpot.
- **Contraseñas de base de datos:** La contraseña que tu app usa para conectarse a Supabase.
- **Tokens de acceso:** Permisos temporales para acceder a servicios como Google, Slack, etc.

### ¿Por qué no van en el código?

Imagínate que pones tu API key de HubSpot directamente en un archivo de tu proyecto:

```javascript
// ❌ NUNCA hagas esto
const hubspotKey = "sk-abc123456789secreto";
```

Ahora haces `git push` y eso sube a GitHub. ¿Qué pasa?

1. **Si el repo es público:** Cualquier persona en internet puede ver tu clave. Hay bots que escanean GitHub las 24 horas buscando exactamente esto. En minutos, alguien puede estar usando tu cuenta de HubSpot, consumiendo tu plan, accediendo a datos de tus clientes, o peor.

2. **Si el repo es privado:** Cualquier persona con acceso al repo (compañeros, colaboradores futuros) puede ver tu clave. Y aunque confíes en ellos hoy, el historial de Git guarda todo — incluso si borras la clave después, el commit viejo la sigue teniendo.

Esto no es paranoia. Es uno de los errores de seguridad más comunes en la industria. Pasa todos los días, incluso en empresas grandes. Es exactamente lo que tu colega te estaba advirtiendo.

A meter credenciales directamente en el código le llamamos **hardcodear**. Es el pecado capital del desarrollo. No lo hagas nunca.

### Variables de entorno — La solución

La solución es separar las credenciales del código usando **variables de entorno**. Una variable de entorno es un valor que existe en tu computadora (o en tu servidor de despliegue) pero que no está escrito en ningún archivo de tu proyecto.

En la práctica, usamos un archivo especial llamado `.env` que vive en la raíz de tu proyecto:

```
# .env — Este archivo NUNCA se sube a GitHub
SUPABASE_URL=https://tuproyecto.supabase.co
SUPABASE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
NEWS_API_KEY=abc123456789
HUBSPOT_API_KEY=pat-na1-secret123
```

Y en tu código, en vez de poner el valor directamente, lees la variable de entorno:

```javascript
// ✅ Esto sí
const supabaseUrl = process.env.SUPABASE_URL;
const supabaseKey = process.env.SUPABASE_KEY;
```

¿El resultado? Tu código no tiene ninguna clave visible. Si alguien ve tu repositorio en GitHub, solo ve `process.env.SUPABASE_URL` — no el valor real. La clave solo existe en tu computadora y en el servidor donde despliegas.

### El combo .env + .gitignore

¿Recuerdas el `.gitignore` de la sesión pasada? Ahí pusimos `.env` como uno de los archivos que Git debe ignorar. Ese es el mecanismo de protección:

1. Creas tu archivo `.env` con las credenciales reales.
2. El `.gitignore` le dice a Git que ignore ese archivo.
3. Cuando haces `git push`, el `.env` no sube a GitHub.
4. Las credenciales se quedan solo en tu computadora.

### .env.example — El mapa sin los secretos

Hay un problema práctico: si el `.env` no sube a GitHub, ¿cómo sabe alguien (o tú mismo en otra computadora) qué variables necesita el proyecto? Para eso existe el `.env.example`:

```
# .env.example — Este SÍ se sube a GitHub
# Copia este archivo como .env y rellena los valores reales
SUPABASE_URL=
SUPABASE_KEY=
NEWS_API_KEY=
HUBSPOT_API_KEY=
```

Es la misma estructura pero sin los valores. Es un mapa que dice "necesitas estas llaves" sin revelar las llaves mismas. Este archivo sí se sube a GitHub porque no contiene información sensible.

### ¿Y en producción?

Cuando despliegues tu app (en Vercel, Netlify, Railway o donde sea), estos servicios tienen una sección de "Environment Variables" en su panel de configuración. Ahí pones los mismos valores que tienes en tu `.env` local. La app en producción lee las variables desde el servidor, nunca desde un archivo.

No necesitas entender los detalles técnicos de cómo funciona. Solo necesitas saber el flujo:

1. En desarrollo (tu computadora): las variables están en `.env`.
2. En producción (el servidor): las variables están en el panel del servicio de despliegue.
3. En GitHub: no hay variables. Solo está el `.env.example` como referencia.

### ¿Qué le dices a Claude Code?

Cuando trabajes con Claude Code, el agente necesita saber que usas variables de entorno. Esto va en tu CLAUDE.md:

```markdown
## Seguridad

- NUNCA hardcodear credenciales, API keys o secretos en el código
- Todas las credenciales van en variables de entorno (.env)
- El archivo .env está en .gitignore — nunca debe subirse a Git
- Si necesitas una nueva credencial, agrégala al .env.example con el nombre pero sin el valor
- Antes de hacer commit, verificar que no haya secretos en el código
```

Con esta sección, el agente sabe que debe usar `process.env.VARIABLE` en vez de poner valores directamente. Y si por alguna razón mete una credencial en el código, tú la vas a detectar en la revisión (paso 4 del loop fundamental) y le vas a pedir que la mueva a una variable de entorno.

---

## Poniendo todo junto — CLAUDE.md completo

Esto es lo que un CLAUDE.md real se ve cuando combinas todas las secciones. No tienes que copiarlo tal cual — lo vamos a escribir juntos en clase adaptado a tu proyecto. Pero para que tengas la referencia:

```markdown
# Project Pipeline Tool

Herramienta interna de gestión de proyectos para un equipo de 12 personas
en una empresa de market research. Reemplaza un Excel manual.
Los PMs asignan proyectos y gestionan la carga de trabajo del equipo.
El resto del equipo puede consultar pero no editar.

## Stack

- Frontend: React con Vite
- Estilos: Tailwind CSS
- Backend: Supabase (PostgreSQL + Auth + API)
- Despliegue: Vercel (frontend), Supabase (backend)
- Lenguaje: JavaScript (NO TypeScript)
- Package manager: npm

## Estructura

src/
  components/    → Componentes de React reutilizables
  pages/         → Páginas principales de la app
  lib/           → Conexión con Supabase y utilidades
  hooks/         → Custom hooks de React
public/          → Archivos estáticos

## Convenciones

- Nombres de archivos: kebab-case (ejemplo: project-list.jsx)
- Nombres de componentes: PascalCase (ejemplo: ProjectList)
- Un componente por archivo
- Comentarios en español
- Archivos de máximo 200 líneas
- Mensajes de commit descriptivos en español

## Restricciones

- NO cambiar de stack sin permiso explícito
- NO instalar dependencias nuevas sin consultarme antes
- NO borrar archivos existentes
- NO refactorizar código que funciona sin que se lo pida
- NO agregar funcionalidades fuera de la especificación
- NO usar console.log — usar un logger apropiado

## Seguridad

- NUNCA hardcodear credenciales o API keys en el código
- Todas las credenciales van en variables de entorno (.env)
- El .env está en .gitignore
- Si se necesita una nueva credencial, agregarla al .env.example sin valor
- Antes de cada commit, verificar que no haya secretos en el código

## Contexto de negocio

- Equipo de ~12 personas, horario US (EST) desde España
- Proyectos con peso 1-5 (complejidad/esfuerzo)
- Capacidad máxima por persona: 10 puntos de peso por semana
- Estados: Pipeline, Active, On Hold, Completed
- PMs: ver y editar todo. Equipo: solo lectura.
- Considerar PTO y holidays para capacidad disponible
```

¿Ves cómo este archivo captura exactamente lo que pondrías en un prompt estructurado (Tu rol + Contexto + Requisitos), pero de forma permanente? Cada vez que abras Claude Code en este proyecto, el agente ya sabe todo esto. No tienes que repetirlo nunca.

---

## Errores comunes con CLAUDE.md

Antes de que escribas el tuyo, unos errores que vale la pena evitar:

**1. Hacerlo demasiado largo.** El CLAUDE.md compite por espacio en la ventana de contexto. Si escribes 5 páginas, estás ocupando espacio que el agente necesita para leer tu código. Sé conciso. Si algo se puede decir en una línea, no uses tres.

**2. Ser demasiado vago.** "Hacer código limpio" no le dice nada al agente. "Archivos de máximo 200 líneas, un componente por archivo, nombres en kebab-case" sí le dice exactamente qué hacer.

**3. No incluir restricciones.** Sin restricciones, el agente toma decisiones por su cuenta. A veces buenas, a veces desastrosas. Las restricciones son tu mecanismo de control.

**4. Olvidar actualizarlo.** El CLAUDE.md debe evolucionar con tu proyecto. Si decides agregar una nueva tecnología, o cambias la estructura, actualiza el archivo. Un CLAUDE.md desactualizado es peor que no tener uno — le da instrucciones incorrectas al agente.

**5. Poner credenciales en el CLAUDE.md.** El CLAUDE.md sí se sube a GitHub (es parte de tu proyecto). Nunca pongas API keys, contraseñas o tokens aquí. Para eso está el `.env`.

---

## El flujo completo de context engineering

Para que veas cómo se conecta todo lo que hemos cubierto en las tres sesiones:

1. **Defines tu intención** — qué quieres construir y por qué.
2. **Escribes tu CLAUDE.md** — el manual permanente del agente para tu proyecto.
3. **Creas tu .env** — las credenciales separadas del código.
4. **Configuras tu .gitignore** — para que el .env nunca suba a Git.
5. **Inicializas Git** — tu red de seguridad.
6. **Abres Claude Code** — el agente lee el CLAUDE.md y ya tiene todo el contexto.
7. **Le das tareas** — usando prompts concretos, porque el contexto general ya lo tiene.
8. **Revisas, iteras, haces commits** — el loop fundamental.

Esto es vibecoding profesional. A partir de la próxima sesión, cada vez que abramos Claude Code, este flujo ya va a estar en su lugar.

---

## Lo que haremos en clase

1. Revisar el template de prompts estructurados (Tu rol / Contexto / Tu tarea / Requisitos) con un ejemplo en vivo.
2. Escribir juntos el CLAUDE.md del Project Pipeline Tool, sección por sección.
3. Crear el archivo `.env` con las variables que necesitaremos (Supabase, etc.).
4. Crear el `.env.example` y verificar que el `.gitignore` protege el `.env`.
5. Probar la diferencia: pedirle algo a Claude Code sin CLAUDE.md, y luego lo mismo con CLAUDE.md. Ver cómo cambia el resultado.
6. Hacer commit del CLAUDE.md al repositorio.
7. Hablar sobre el CLAUDE.md del Opportunity Radar — qué secciones tendrá y en qué se diferencia del Pipeline Tool.

Llega con tu proyecto de las sesiones anteriores abierto y tu cuenta de Supabase creada (si no la tienes, créala en [supabase.com](https://supabase.com) — es gratis). En la siguiente sesión vamos a empezar a construir en serio con Claude Code, y el CLAUDE.md que escribamos hoy va a ser la base de todo.

---

## Glosario rápido

Términos nuevos de esta sesión.

| Término | Qué significa |
|---|---|
| **Context engineering** | El arte de diseñar y estructurar toda la información que un agente necesita para hacer bien su trabajo en un proyecto. Va más allá de un prompt: incluye archivos de configuración, reglas, restricciones y contexto de negocio. |
| **CLAUDE.md** | Archivo de texto en la raíz de tu proyecto que Claude Code lee automáticamente al iniciar. Es tu "manual de instrucciones" permanente para el agente. |
| **Ventana de contexto (context window)** | La cantidad de texto que un agente de IA puede "ver" al mismo tiempo. Todo lo que le dices, el historial de la conversación y los archivos que lee compiten por este espacio. |
| **System prompt** | Un mensaje de instrucciones que se le da al agente antes de que el usuario diga nada. Moldea su comportamiento. El CLAUDE.md funciona de forma parecida pero como archivo visible y editable. |
| **Drift (deriva)** | Cuando un agente de IA "se olvida" de instrucciones que le diste al principio de una conversación larga. Pasa porque la información vieja pierde relevancia a medida que la conversación crece. |
| **API key** | Una clave única que le permite a tu aplicación conectarse con un servicio externo (como NewsAPI, HubSpot, Supabase). Es como una contraseña para tu app. |
| **Variable de entorno** | Un valor (como una API key o contraseña) que existe en tu computadora o servidor, pero no en el código de tu proyecto. Se accede con `process.env.NOMBRE`. |
| **Hardcodear** | Escribir un valor directamente en el código en vez de usar una variable de entorno. Es el error de seguridad más común. Nunca lo hagas con credenciales. |
| **.env** | Archivo donde guardas tus variables de entorno. Vive en la raíz del proyecto y NUNCA se sube a GitHub (está en el .gitignore). |
| **.env.example** | Copia del .env pero sin los valores reales. Sirve como mapa para que otros sepan qué variables necesita el proyecto. Este sí se sube a GitHub. |
| **Credenciales / secretos** | Cualquier información que da acceso a un servicio: API keys, contraseñas, tokens de acceso. Deben mantenerse fuera del código y fuera de Git. |

---

## Cheatsheet de context engineering

Referencia rápida para construir tu CLAUDE.md y manejar secretos.

| Qué quieres hacer | Cómo |
|---|---|
| Crear el CLAUDE.md | Crear un archivo llamado `CLAUDE.md` en la raíz de tu proyecto |
| Definir el stack | Sección "Stack" con tecnologías específicas y las que NO quieres |
| Establecer reglas | Sección "Convenciones" con nombres, estructura y límites |
| Poner límites al agente | Sección "Restricciones" con lo que NO debe hacer |
| Crear variables de entorno | Crear archivo `.env` en la raíz con formato `NOMBRE=valor` |
| Proteger el .env | Verificar que `.env` está en el `.gitignore` |
| Documentar las variables | Crear `.env.example` con los nombres sin valores |
| Verificar que no hay secretos | Antes de cada commit: `git diff` y buscar valores sospechosos |

---

> **Nos vemos en clase.** Crea una cuenta en [supabase.com](https://supabase.com) si no la tienes. Y si te animas, empieza a escribir tu propio CLAUDE.md antes de la sesión — lo revisamos y mejoramos juntos.
