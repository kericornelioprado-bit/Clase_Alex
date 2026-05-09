# Sesión 5: Project Pipeline Tool — Backend y persistencia

> **Lectura previa.** Lee esto antes de la clase. En la sesión vamos directo a la práctica.

---

## El bug que te trajo aquí

Vamos a resolver el problema que empezó todo: tu prototipo del Pipeline Tool se veía bien, pero los datos desaparecían al refrescar la página. Ese no era un bug menor — era un síntoma de un problema de arquitectura. Tu prototipo no tenía backend.

Hoy vamos a entender qué significa eso, por qué importa y cómo solucionarlo. Al final de la clase, vas a poder agregar un proyecto en el Pipeline Tool, cerrar el navegador, abrirlo al día siguiente, y el proyecto va a seguir ahí. Ese es el momento en el que tu herramienta deja de ser un prototipo y se convierte en algo que tu equipo puede usar de verdad.

---

## ¿Qué es un backend?

Para entender el backend, primero hay que entender cómo funciona una aplicación web. Toda app tiene dos partes:

**Frontend:** Es lo que el usuario ve y toca. Los botones, las tablas, los colores, los formularios. Es lo que tú generaste con Claude Design y lo que reconstruimos ayer con Claude Code. El frontend corre en el **navegador** del usuario (Chrome, Firefox, Safari).

**Backend:** Es lo que pasa detrás de escena. Donde se guardan los datos, donde se ejecuta la lógica de negocio, donde se verifican permisos. El backend corre en un **servidor** — una computadora que está prendida 24/7 en algún data center.

La analogía más clara: piensa en un restaurante. El **frontend** es el salón — las mesas, el menú, los meseros, la decoración. Es lo que el cliente ve y con lo que interactúa. El **backend** es la cocina — donde se prepara la comida, donde se guardan los ingredientes, donde se aplican las recetas. El cliente no ve la cocina, pero sin ella el restaurante no funciona.

Tu prototipo era un restaurante con salón pero sin cocina. Tenías mesas bonitas y menú impreso, pero cuando el cliente pedía algo, no había dónde prepararlo ni dónde guardarlo.

### ¿Qué pasaba con tus datos?

Cuando tu prototipo mostraba proyectos, esos datos vivían en la **memoria del navegador**. Específicamente, en algo llamado "estado" (state) de la aplicación. El problema es que la memoria del navegador es temporal — existe solo mientras la pestaña está abierta. En el momento en que cierras la pestaña o refrescas la página, el navegador borra esa memoria y empieza de cero. Es como escribir notas en un pizarrón blanco: si alguien borra el pizarrón, se pierde todo.

Lo que necesitas es que los datos estén en un lugar permanente — una **base de datos**. La base de datos es como un archivero: guardas cosas ahí y siguen ahí mañana, la semana que viene y el año que viene. No importa quién cierre el navegador o apague la computadora.

El flujo correcto es:

1. El usuario abre la app (frontend).
2. El frontend le pide los datos al backend: "Dame la lista de proyectos."
3. El backend busca los proyectos en la base de datos y se los envía al frontend.
4. El frontend los muestra en pantalla.
5. El usuario crea un proyecto nuevo.
6. El frontend le dice al backend: "Guarda este proyecto."
7. El backend lo guarda en la base de datos.
8. El usuario refresca la página.
9. El frontend vuelve a pedirle al backend los datos — y ahí están, intactos.

Eso es lo que vamos a implementar hoy.

---

## Opciones de backend para un vibecoder

Si le preguntas a un desarrollador profesional qué backend usar, te va a dar 47 opciones y una conferencia de 2 horas sobre las ventajas y desventajas de cada una. Tú no necesitas eso. Necesitas algo que funcione, que sea fácil de conectar con Claude Code y que no requiera que te conviertas en experto en bases de datos.

Hay tres opciones principales que funcionan bien para vibecoding:

### Supabase — La que vamos a usar

Supabase es un servicio que te da backend completo sin que tú tengas que configurar servidores. Cuando creas un proyecto en Supabase, te dan:

- **Una base de datos** (PostgreSQL) donde guardar tus datos.
- **Una API automática** para que tu frontend pueda leer y escribir datos.
- **Autenticación** para manejar login y permisos de usuarios.
- **Almacenamiento de archivos** por si necesitas subir imágenes o documentos.

¿Por qué Supabase y no otra cosa? Porque es la mejor combinación de simplicidad y potencia para lo que necesitas. Tiene un plan gratuito generoso, funciona bien con React y Claude Code lo conoce muy bien — cuando le pidas que conecte tu app a Supabase, va a saber exactamente qué hacer.

### Firebase — La alternativa de Google

Firebase hace algo parecido a Supabase. La diferencia principal es que Firebase usa una base de datos diferente (NoSQL en vez de SQL). Para tu caso, Supabase es mejor porque los datos de tu Pipeline Tool son muy estructurados (proyectos con campos fijos, personas con roles, asignaciones con fechas) y SQL es ideal para datos estructurados.

### SQLite — La opción local

SQLite es una base de datos que vive en un archivo dentro de tu proyecto. No necesita un servidor externo. Es simple y rápida, pero tiene una limitación importante: si la app corre en dos computadoras, cada una tiene su propia copia de los datos. Para una herramienta que va a usar tu equipo, necesitas que todos vean los mismos datos, y para eso necesitas algo en la nube como Supabase.

Para el Pipeline Tool: **Supabase**. Para el Opportunity Radar: probablemente también Supabase, pero lo decidimos cuando lleguemos ahí.

---

## ¿Qué es una base de datos?

Una base de datos es, en esencia, un conjunto de tablas. Si sabes usar Excel, ya entiendes las bases de datos al 80%.

Piensa en cada tabla como una hoja de Excel:
- Las **columnas** son los campos (nombre, estado, fecha, etc.).
- Las **filas** son los registros (cada proyecto, cada persona).
- Cada fila tiene un **ID** único que la identifica (como un número de fila que nunca cambia).

Para el Pipeline Tool, necesitas al menos estas tablas:

**Tabla `projects` (proyectos):**

| id | name | status | pm_id | weight | due_date |
|---|---|---|---|---|---|
| 1 | Market Study Q3 | Active | 3 | 4 | 2025-09-15 |
| 2 | Brand Tracker ACME | Pipeline | 1 | 2 | 2025-10-01 |
| 3 | Consumer Survey XYZ | On Hold | 3 | 5 | 2025-08-30 |

**Tabla `team_members` (miembros del equipo):**

| id | name | role | capacity |
|---|---|---|---|
| 1 | Alex | PM | 10 |
| 2 | María | Analyst | 10 |
| 3 | Carlos | PM | 10 |

**Tabla `assignments` (asignaciones):**

| id | project_id | member_id | points |
|---|---|---|---|
| 1 | 1 | 2 | 3 |
| 2 | 1 | 1 | 2 |
| 3 | 2 | 1 | 2 |

¿Ves cómo la tabla `assignments` conecta proyectos con personas? El `project_id` y el `member_id` son **referencias** a las otras tablas. Esto es lo que en bases de datos se llama una **relación**. El proyecto 1 ("Market Study Q3") tiene asignados a María (member_id 2, con 3 puntos) y a Alex (member_id 1, con 2 puntos).

No necesitas memorizar la terminología técnica. Cuando trabajes con Claude Code, puedes decirle algo como: "Necesito una tabla para los proyectos con estas columnas: nombre, estado, PM asignado, peso y fecha de entrega." El agente se encarga de traducir eso a la estructura correcta de la base de datos.

---

## CRUD — Las 4 operaciones de toda app

Todo lo que tu aplicación hace con los datos se reduce a cuatro operaciones. Se llaman **CRUD** por sus siglas en inglés:

**C — Create (Crear).** Agregar un registro nuevo. Ejemplo: crear un proyecto nuevo en el Pipeline Tool.

**R — Read (Leer).** Consultar datos que ya existen. Ejemplo: cargar la lista de proyectos al abrir la app.

**U — Update (Actualizar).** Modificar un registro existente. Ejemplo: cambiar el estado de un proyecto de "Pipeline" a "Active".

**D — Delete (Borrar).** Eliminar un registro. Ejemplo: borrar un proyecto que se canceló.

Cada funcionalidad de tu app es una combinación de estas cuatro operaciones. La vista de proyectos es un Read. El formulario de crear proyecto es un Create. El botón de cambiar estado es un Update. El botón de eliminar es un Delete.

Cuando le pidas a Claude Code que implemente funcionalidades, puedes usar estos términos directamente. "Implementa el CRUD de proyectos" es una instrucción que el agente entiende perfectamente y que le dice exactamente qué necesitas: crear, leer, actualizar y borrar proyectos.

---

## ¿Cómo se comunican el frontend y el backend?

Esta es la última pieza del rompecabezas conceptual antes de pasar a la práctica.

Cuando tu frontend necesita datos del backend (o necesita guardar datos), hace una **petición** (request). Es como una llamada telefónica entre dos sistemas:

1. El frontend llama al backend: "Dame todos los proyectos con estado Active."
2. El backend recibe la llamada, busca en la base de datos y responde: "Aquí tienes, son estos 15 proyectos."
3. El frontend recibe la respuesta y muestra los datos en pantalla.

Con Supabase, esta comunicación es especialmente sencilla porque te da una librería (un paquete de código) que se encarga de hacer las llamadas por ti. En vez de configurar llamadas complicadas, tu código dice algo como:

```javascript
// Obtener todos los proyectos activos
const { data } = await supabase
  .from('projects')
  .select('*')
  .eq('status', 'Active');
```

No necesitas escribir esto — Claude Code lo va a generar. Pero quiero que entiendas la lógica: estás leyendo de la tabla `projects`, seleccionando todos los campos (`*`), y filtrando por los que tienen status igual a `Active`. Es casi como leer una frase en inglés.

Para crear un proyecto:

```javascript
// Crear un proyecto nuevo
const { data } = await supabase
  .from('projects')
  .insert({ name: 'New Study', status: 'Pipeline', weight: 3 });
```

Para actualizar:

```javascript
// Cambiar estado de un proyecto
const { data } = await supabase
  .from('projects')
  .update({ status: 'Active' })
  .eq('id', 5);
```

Para borrar:

```javascript
// Eliminar un proyecto
const { data } = await supabase
  .from('projects')
  .delete()
  .eq('id', 5);
```

¿Ves el patrón? Siempre es `supabase.from('tabla').operación()`. Create, Read, Update, Delete — CRUD. Todo lo que tu app hace con datos se resume en estas cuatro formas.

---

## Supabase — Lo que necesitas saber antes de la clase

### Tu proyecto en Supabase

Cuando creaste tu cuenta en supabase.com (si seguiste las instrucciones de la sesión 3), te encontraste con un dashboard. Ahí es donde vive tu proyecto de backend. Las secciones que nos importan son:

- **Table Editor** — Donde ves y editas tus tablas. Es como un Excel en la nube. Puedes crear tablas, agregar columnas, insertar datos de prueba, todo desde la interfaz visual.
- **SQL Editor** — Donde puedes escribir consultas SQL directamente. No lo vas a usar mucho — Claude Code genera el SQL por ti. Pero está ahí si lo necesitas.
- **Authentication** — Donde configuras cómo los usuarios inician sesión. Lo vamos a usar en la sesión 6 cuando implementemos permisos.
- **Settings > API** — Donde encuentras tus credenciales: la URL de tu proyecto y la API key. Son las que van en tu archivo `.env`.

### Las credenciales de Supabase

Cuando abras Settings > API, vas a ver dos cosas importantes:

1. **Project URL** — La dirección de tu backend. Algo como `https://abcdefgh.supabase.co`. Esta es tu `SUPABASE_URL`.
2. **API Keys** — Hay dos: la `anon` key (pública, para el frontend) y la `service_role` key (privada, solo para el servidor). Por ahora usamos la `anon` key. La `service_role` nunca debe estar en el frontend.

Estas dos credenciales van en tu archivo `.env`:

```
SUPABASE_URL=https://abcdefgh.supabase.co
SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

Y en tu `.env.example` (sin los valores):

```
SUPABASE_URL=
SUPABASE_ANON_KEY=
```

### Row Level Security (RLS) — Lo mencionamos, no lo implementamos hoy

Supabase tiene un sistema de seguridad llamado **Row Level Security** que controla quién puede ver o modificar cada fila de una tabla. Es muy potente, pero también es fácil equivocarse con él. Por hoy, lo vamos a configurar de forma básica para que la app funcione. En la sesión 6, cuando implementemos permisos de verdad (admin vs. viewer), vamos a configurar RLS correctamente.

Si Supabase te muestra un aviso de que RLS está desactivado, no te preocupes — lo vamos a resolver.

---

## El plan de hoy: de datos falsos a datos reales

Ayer en la sesión 4 montamos el Pipeline Tool con datos hardcodeados — números y nombres escritos directamente en el código para probar la interfaz. Hoy vamos a hacer la transición a datos reales:

1. **Crear las tablas en Supabase** — Definir la estructura de proyectos, equipo y asignaciones.
2. **Conectar Supabase al proyecto** — Instalar la librería, configurar las credenciales.
3. **Reemplazar datos hardcodeados con datos reales** — Que la lista de proyectos lea de la base de datos en vez de un array en el código.
4. **Implementar Create** — Formulario para agregar proyectos nuevos que se guarden en la base de datos.
5. **Implementar Update** — Poder cambiar el estado de un proyecto.
6. **Implementar Delete** — Poder eliminar un proyecto.
7. **Verificar persistencia** — Refrescar la página y confirmar que los datos siguen ahí.

El paso 7 es el momento de la verdad. Cuando refresques la página y los datos sigan ahí, habrás resuelto el bug original que te trajo a este curso.

---

## Cómo pedirle esto a Claude Code

No le vas a pedir todo de un golpe. Vas a seguir el flujo que aprendimos ayer: un paso a la vez, commit después de cada paso, verificar antes de avanzar.

Algunas instrucciones de ejemplo para la sesión:

**Para crear las tablas:**
> "Necesito configurar la base de datos en Supabase para el Pipeline Tool. Crea un archivo de migración SQL con tres tablas: projects (con columnas para nombre, estado, pm asignado, peso y fecha de entrega), team_members (nombre, rol, capacidad) y assignments (que conecte proyectos con personas e incluya los puntos asignados). Incluye datos de ejemplo."

**Para conectar Supabase:**
> "Instala la librería de Supabase y crea el archivo de conexión en src/lib/. Usa las variables de entorno SUPABASE_URL y SUPABASE_ANON_KEY del .env."

**Para reemplazar los datos hardcodeados:**
> "Modifica el componente ProjectList para que lea los proyectos de Supabase en vez de usar el array hardcodeado. Muestra un loading mientras carga y un mensaje si no hay proyectos."

**Para implementar el formulario de crear proyecto:**
> "Agrega un botón 'Nuevo Proyecto' en la vista de proyectos que abra un modal con un formulario. Campos: nombre (texto), estado (dropdown con las opciones Pipeline/Active/On Hold/Completed), peso (número del 1 al 5) y fecha de entrega (date picker). Al guardar, que inserte el proyecto en Supabase y actualice la lista."

Nota cómo cada instrucción es específica pero no le dice al agente cómo programarlo. Le dices el qué y el agente decide el cómo — dentro de las reglas de tu CLAUDE.md.

---

## Debugging de conexión — Qué puede salir mal

La primera vez que conectas un frontend con un backend, es normal que algo falle. Estos son los problemas más comunes y cómo describirlos para que Claude Code los resuelva:

**"La página se queda en loading infinito."** Probablemente la conexión con Supabase está fallando. Puede ser un error en las credenciales del `.env`, que la URL esté mal escrita, o que la tabla no exista todavía en Supabase.

**"Me sale un error de CORS en la consola."** CORS es un mecanismo de seguridad del navegador. Si lo ves, generalmente significa que la URL de Supabase está mal configurada. Dile al agente: "Me sale un error de CORS al intentar conectar con Supabase. ¿Puedes revisar la configuración?"

**"Los datos se guardan pero no aparecen en la lista."** Probablemente la app no está recargando la lista después de insertar un dato nuevo. Dile al agente: "Al crear un proyecto nuevo, se guarda en Supabase pero la lista no se actualiza. Necesito que la lista se refresque automáticamente después de cada operación."

**"Me sale un error 401 o 403."** Eso es un problema de permisos. Probablemente RLS está bloqueando las operaciones. Dile al agente: "Me da error 401/403 al intentar leer/escribir datos en Supabase. ¿Puedes revisar la configuración de Row Level Security?"

En todos estos casos, la clave es **describir el síntoma** (qué ves o qué pasa) y **qué esperabas** que pasara. El agente se encarga de diagnosticar y corregir.

---

## Lo que haremos en clase

1. Abrir el dashboard de Supabase y crear las tablas (proyectos, equipo, asignaciones).
2. Copiar las credenciales de Supabase al `.env`.
3. Pedirle a Claude Code que instale y configure la librería de Supabase.
4. Conectar la lista de proyectos a la base de datos — verificar que carga datos reales.
5. Implementar crear proyecto — verificar que se guarda en Supabase.
6. Implementar actualizar estado — verificar que el cambio persiste.
7. Implementar eliminar proyecto — verificar que desaparece de la base de datos.
8. **El test final:** refrescar la página y confirmar que todo sigue ahí.
9. Commit y push a GitHub.

Llega con tu cuenta de Supabase lista y un proyecto creado (no hace falta que tengas tablas — las creamos juntos). Si te perdiste en algún paso de la sesión anterior, avísame antes de la clase para que no perdamos tiempo.

---

## Glosario rápido

Términos nuevos de esta sesión.

| Término | Qué significa |
|---|---|
| **Backend** | La parte de una aplicación que maneja los datos y la lógica de negocio. Corre en un servidor, no en el navegador del usuario. |
| **Frontend** | La parte visual de una aplicación — lo que el usuario ve y toca. Corre en el navegador. |
| **Base de datos** | Un sistema para guardar datos de forma permanente y estructurada. Piénsalo como un Excel que vive en un servidor y que nunca pierde los datos. |
| **Tabla** | Una estructura de datos con filas y columnas, como una hoja de Excel. Cada tabla guarda un tipo de dato (proyectos, personas, asignaciones). |
| **Registro (fila)** | Un dato individual dentro de una tabla. Por ejemplo, un proyecto específico con su nombre, estado y fecha. |
| **ID** | Un identificador único para cada registro. Nunca se repite. Permite referenciar un dato específico sin ambigüedad. |
| **Relación** | Una conexión entre dos tablas usando IDs. La tabla de asignaciones "relaciona" proyectos con personas usando `project_id` y `member_id`. |
| **Supabase** | Un servicio que te da backend completo (base de datos, API, autenticación) sin que configures servidores. Es lo que vamos a usar. |
| **CRUD** | Las cuatro operaciones básicas con datos: Create (crear), Read (leer), Update (actualizar), Delete (borrar). Toda app es una combinación de estas cuatro. |
| **API (en este contexto)** | La interfaz que permite que tu frontend hable con tu backend. Supabase genera una API automática para cada tabla. |
| **Petición (request)** | Una llamada del frontend al backend pidiendo o enviando datos. "Dame los proyectos" es una petición de lectura. "Guarda este proyecto" es una petición de escritura. |
| **Migración** | Un archivo SQL que define o modifica la estructura de la base de datos. Es el "plano" de tus tablas. Se guarda en Git para tener historial de los cambios en la base de datos. |
| **RLS (Row Level Security)** | Sistema de seguridad de Supabase que controla quién puede ver o modificar cada fila. Lo vamos a configurar bien en la sesión 6. |
| **CORS** | Mecanismo de seguridad del navegador que a veces bloquea las conexiones entre tu frontend y tu backend. Si ves un error de CORS, generalmente es un problema de configuración. |
| **Loading state** | El estado de "cargando" que se muestra mientras la app espera datos del backend. Es importante mostrarlo para que el usuario sepa que algo está pasando. |

---

> **Nos vemos en clase.** Crea un proyecto en [supabase.com](https://supabase.com) si no lo has hecho. No necesitas crear tablas — lo hacemos juntos. Trae también las credenciales (URL y anon key) a la mano, las vamos a necesitar al inicio.
