# Sesión 1: Vibecoding — El framework, no la herramienta

> **Lectura previa.** Lee esto antes de la clase. En la sesión vamos directo a la práctica.

---

## ¿Por qué estás aquí?

Tú ya intentaste construir algo con IA. Abriste Claude Design, generaste un HTML, lo pasaste a Claude Code, lo subiste a Netlify, lo probaste, encontraste un error, volviste a Claude Design, ajustaste, volviste a subir… y así en loop. El resultado se veía bien, pero los datos desaparecían al refrescar la página y no tenías claro por qué.

Ese ciclo no está mal como primer intento. De hecho, el que hayas llegado a tener algo visual y desplegado ya te pone por delante de mucha gente. Pero ese flujo no escala. No escala porque:

- No hay una fuente de verdad — cada vez que pasas de una herramienta a otra, pierdes contexto.
- No hay red de seguridad — si algo se rompe, no puedes volver a una versión que funcionaba.
- No hay especificación — le pides cosas al agente sobre la marcha, y el resultado depende de tu memoria, no de un plan.

Lo que vamos a aprender en este curso es un **framework profesional** para hacer exactamente lo que tú ya estabas haciendo, pero de forma que el resultado sea confiable, mantenible y seguro. Ese framework se llama **vibecoding**.

---

## ¿Qué es vibecoding?

El término lo acuñó **Andrej Karpathy** (cofundador de OpenAI, ex-director de IA en Tesla) a principios de 2025. La idea original era simple: en vez de escribir código, le describes a una IA lo que quieres y ella lo genera por ti. Tú solo "vibeas" — te dejas llevar.

Esa definición inicial era medio informal. Lo que ha pasado desde entonces es que la industria ha tomado esa idea y la ha convertido en una **metodología de trabajo seria**. Hoy hay empresas enteras que operan así: nadie escribe código a mano, todos trabajan a través de agentes de IA.

**Lo que vibecoding NO es:**

- No es "preguntarle a ChatGPT cómo hacer algo y copiar-pegar el resultado."
- No es usar Claude Design para generar pantallitas bonitas sin backend.
- No es un atajo para saltarse la calidad o la seguridad.

**Lo que vibecoding SÍ es:**

- Una forma de **comunicarte con agentes de IA** para que ellos escriban, modifiquen y mantengan código por ti.
- Un **flujo de trabajo completo** que incluye especificación, generación, revisión, pruebas y despliegue.
- Una **meta-habilidad**: no aprendes a programar, aprendes a dirigir al que programa.

Piénsalo desde tu rol de PM: tú no necesitas saber construir una casa para gestionar un proyecto de construcción. Pero sí necesitas saber qué pedirle al arquitecto, cómo revisar los planos, cómo verificar que la estructura es sólida y cuándo dar luz verde para avanzar. Vibecoding es exactamente eso, pero con software.

---

## El loop fundamental

Todo proyecto de vibecoding sigue el mismo ciclo. Memorízalo porque lo vas a usar en cada sesión:

```
Intención → Especificación → Generación → Revisión → Iteración → Entrega
```

Vamos punto por punto:

**1. Intención** — ¿Qué quieres lograr? No "quiero una app", sino "quiero una herramienta donde mi equipo pueda ver los proyectos activos, quién está asignado a cada uno y cuál es la carga de trabajo por semana." Cuanto más claro seas aquí, mejor será todo lo demás.

**2. Especificación** — Escribir un documento (en lenguaje natural, no en código) que le diga al agente exactamente qué construir. Esto incluye: qué funcionalidades tiene, qué restricciones hay, qué tecnologías usar, qué NO debe hacer. Piensa en esto como el brief de un proyecto.

**3. Generación** — El agente escribe el código. Tú no tocas una sola línea. Solo le das instrucciones.

**4. Revisión** — Verificas que lo que generó funciona. No necesitas leer el código línea por línea, pero sí necesitas probarlo: ¿hace lo que pediste? ¿Se rompe en algún caso? ¿Los datos persisten?

**5. Iteración** — Si algo no está bien, se lo dices al agente y él lo corrige. No empiezas de cero cada vez — iteras sobre lo que ya existe.

**6. Entrega** — Despliegas el resultado en un lugar donde otros puedan usarlo. Con pruebas, con seguridad, con documentación.

Cada vuelta de este ciclo puede tomar minutos. En una hora de clase podemos dar varias vueltas. La clave es que **nunca te saltas pasos** — especialmente la revisión. El código generado por IA se equivoca mucho (algunos estudios dicen que el 96% tiene errores de entrada). La diferencia entre un amateur y un profesional de vibecoding es que el profesional siempre revisa.

---

## Mapa del ecosistema

Hay muchas herramientas ahí afuera y puede ser confuso. Para simplificar, hay **tres categorías**:

### Coding agents (los constructores)

Son agentes que viven en tu terminal y escriben código directamente en tu proyecto. Tú les hablas, ellos modifican archivos, crean carpetas, instalan dependencias, hacen commits. Son los que usamos para construir cosas serias.

- **Claude Code** — El agente de Anthropic (los creadores de Claude). Es el que tú ya tienes con Claude Pro. Muy bueno para proyectos complejos, pero tiene un límite diario de uso que a veces se agota rápido.
- **Gemini CLI** — El agente de Google. Funciona muy parecido a Claude Code, pero el plan de Google te da muchos más tokens (o sea, puedes usarlo más tiempo sin que se agote). Lo veremos más adelante como alternativa.

### Doing agents (los asistentes de tareas)

No escriben código. Hacen cosas por ti: analizan datos, hacen scraping, generan reportes, llaman APIs. Piensa en ellos como un analista junior al que le das instrucciones.

- **Anton** — Ideal para análisis de datos, automatización y tareas repetitivas. Lo veremos cuando trabajemos con el Opportunity Radar.

### App builders (los generadores de pantallas)

Generan interfaces visuales rápido, pero no siempre con la estructura necesaria para proyectos que van a crecer. Son buenos para prototipos rápidos.

- **Claude Design** — Lo que tú usaste. Genera HTML/CSS bonito, pero no tiene backend, no tiene persistencia, no tiene manejo de permisos. Sirve para validar una idea visual, no para entregar un producto.
- **Lovable, Bolt, Replit** — Otras herramientas similares. Las veremos de pasada al final del curso.

**¿Cuál usamos nosotros?** Principalmente **Claude Code** (y Gemini CLI como respaldo). Para tareas de datos y automatización, **Anton**. Los app builders los dejamos para prototipado visual rápido cuando necesitemos validar una idea antes de construirla bien.

---

## Cuándo vibecoding funciona y cuándo no

Esto es importante. Vibecoding no es magia y no resuelve todo.

**Funciona muy bien para:**

- MVPs y prototipos funcionales
- Herramientas internas (como tu Pipeline Tool)
- Dashboards y reportes
- Automatizaciones y bots
- Integraciones entre servicios (conectar APIs, enviar emails, monitorear noticias)
- Páginas web y plataformas sencillas

**Funciona con cuidado para:**

- Plataformas con muchos usuarios simultáneos (necesitas pensar en escalabilidad)
- Sistemas con datos sensibles (necesitas seguridad seria — lo cubriremos)
- Proyectos que cambian de requerimientos cada semana (necesitas disciplina de PM para delimitar el alcance)

**No funciona (aún) para:**

- Sistemas críticos donde un error tiene consecuencias graves (bancos, hospitales, aviones)
- Proyectos que requieren optimización de rendimiento extrema
- Machine learning avanzado con modelos propios (esto ya es data science, no vibecoding)

Tus dos proyectos — el Pipeline Tool y el Opportunity Radar — caen perfectamente en la zona donde vibecoding funciona muy bien. Son herramientas internas, con pocos usuarios, con requerimientos claros y con un PM (tú) que sabe delimitar alcance.

---

## Lo que vamos a instalar hoy

En la clase de hoy vamos a dejar tu máquina lista para trabajar. Necesitamos dos cosas: **UV** (el gestor de entorno de Python) y **Claude Code** (el coding agent).

### ¿Qué es UV?

Cuando un agente de IA genera código en Python, ese código necesita librerías (paquetes de código que otras personas ya escribieron). UV es la herramienta que instala y organiza esas librerías de forma limpia. Piénsalo como el gestor de dependencias de tu proyecto — se asegura de que todo esté en su lugar y que nada entre en conflicto.

No necesitas entender los detalles técnicos de UV. Solo necesitas saber que **cada vez que empecemos un proyecto, lo primero que hacemos es un `uv init`** y que **cada vez que el agente necesite una librería, usamos `uv add`** en vez de instalarla a lo loco.

### ¿Qué es Claude Code?

Es un programa que vive en tu terminal (esa pantalla negra con texto que parece de los 90, pero que es la herramienta más poderosa que tienes en tu computadora). Una vez instalado, tú abres la terminal, escribes `claude`, y empieza una conversación. Pero a diferencia de la conversación que tienes en claude.ai, esta conversación tiene acceso directo a tu proyecto: puede leer tus archivos, modificarlos, crear nuevos, correr pruebas, hacer commits en Git… todo sin que tú toques código.

### Requisitos previos

Antes de la clase, asegúrate de tener lo siguiente:

1. **Una computadora con macOS, Linux o Windows (con WSL).** Si estás en Windows, necesitamos instalar WSL (Windows Subsystem for Linux) — es un paso extra pero sencillo. Si no sabes si lo tienes, no te preocupes, lo vemos al inicio de la clase.

2. **Node.js instalado.** Claude Code necesita Node.js para funcionar. Para verificar si lo tienes, abre tu terminal y escribe:
   ```
   node --version
   ```
   Si te sale un número (como `v20.11.0` o similar), ya lo tienes. Si te da error, lo instalamos juntos en clase.

3. **Tu cuenta de Claude Pro activa.** Que es la que ya tienes.

4. **Un editor de texto.** No para escribir código, sino para ver archivos cuando necesitemos revisar algo. Visual Studio Code (VS Code) es gratis y es el estándar. Si no lo tienes, descárgalo de [code.visualstudio.com](https://code.visualstudio.com).

---

## Lo que haremos en clase

1. Verificar que tu entorno esté listo (terminal, Node.js, sistema operativo).
2. Instalar UV.
3. Instalar Claude Code.
4. Crear un proyecto vacío con `uv init`.
5. Abrir Claude Code dentro de ese proyecto.
6. Tu primer "hola mundo": pedirle al agente que genere un script simple, ejecutarlo y verificar que funciona.
7. Recapitular: qué acabamos de hacer, por qué lo hicimos así y qué viene en las siguientes sesiones.

No te preocupes si algún paso no te queda claro de la lectura. La clase es para hacerlo juntos. El objetivo de estas notas es que cuando lleguemos a un concepto en la sesión, tú ya lo hayas leído y podamos ir más rápido.

---

## Glosario rápido

Términos que vamos a usar desde hoy. No necesitas memorizarlos — solo tenlos a la mano.

| Término | Qué significa |
|---|---|
| **Agente** | Un programa de IA que no solo responde preguntas, sino que ejecuta acciones (crear archivos, modificar código, instalar cosas). |
| **Terminal** | La interfaz de texto de tu computadora. También se le dice "consola" o "línea de comandos". Es donde vive Claude Code. |
| **Repositorio (repo)** | Una carpeta de proyecto controlada por Git. Guarda todo el historial de cambios. Lo veremos a fondo en la sesión 2. |
| **Backend** | La parte de una aplicación que maneja los datos y la lógica del negocio. Es lo que le faltaba a tu prototipo del Pipeline Tool — por eso los datos desaparecían al refrescar. |
| **Frontend** | La parte visual de una aplicación — lo que el usuario ve y toca. Tu prototipo de Claude Design era puro frontend. |
| **Dependencia** | Una librería o paquete que tu proyecto necesita para funcionar. UV las gestiona. |
| **Deploy (desplegar)** | Publicar tu aplicación para que otros la puedan usar. Lo que tú hacías con Netlify. |
| **MVP** | Minimum Viable Product. La versión más pequeña de tu producto que ya resuelve el problema principal. Es lo que vamos a construir, no la versión final con todos los features. |
| **Token** | La unidad que mide cuánto puedes usar un agente de IA antes de que se agote tu cuota. Cuando Claude Code "se acaba", es porque gastaste tus tokens del día. |
| **Prompt** | Lo que le dices al agente. Puede ser una instrucción, una pregunta, una descripción de lo que quieres. La calidad de tu prompt determina la calidad del resultado. |
| **Context engineering** | El arte de darle al agente toda la información que necesita para hacer bien su trabajo. Va más allá de un prompt: incluye archivos de configuración, reglas y restricciones. Lo veremos en la sesión 3. |

---

> **Nos vemos en clase.** Si tienes dudas sobre los requisitos previos, mándame mensaje antes para que no perdamos tiempo en la sesión.
