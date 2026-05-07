# Temario Personalizado — Alex Lorenzo

## Perfil del alumno

- **Rol:** Project Manager en empresa americana de market research (~70–80 personas)
- **Ubicación:** España (horario laboral US)
- **Experiencia técnica:** Excel avanzado, análisis de datos básico. Ha experimentado con Claude Design → Claude Code → Netlify. Contacto mínimo con Git.
- **Herramientas actuales:** ChatGPT Pro, Claude Pro. No usa Gemini.
- **Ecosistema laboral:** Google Workspace, HubSpot, email corporativo.
- **Motivación:** Supervivencia laboral + capacidad freelance para ofrecer servicios a negocios pequeños.

## Proyectos reales

1. **Project Pipeline Tool** — Herramienta de gestión de proyectos y carga de trabajo del equipo (reemplazo de su Excel actual). Ya tiene prototipo visual en Claude Design/Netlify con bugs de persistencia.
2. **Opportunity Radar** — Plataforma que detecta oportunidades de negocio en tiempo real combinando señales externas (noticias vía APIs) con datos internos (emails, HubSpot, lista de clientes) para generar alertas y recomendaciones de outreach. Proyecto asignado por la empresa con presión de entrega.

## Estructura

- **15 sesiones de 1 hora**, de lunes a viernes
- **Formato:** Concepto breve → Demo en vivo → Práctica guiada sobre sus proyectos
- **Entregables:** Ambos proyectos funcionales con tests y desplegados
- **Notas de clase:** Disponibles en repositorio compartido antes de cada sesión

---

## BLOQUE 1 — FUNDAMENTOS (Sesiones 1–3)

> *Objetivo del bloque: Que Alex entienda el framework de vibecoding, tenga su entorno listo y sepa usar Git como red de seguridad.*

### Sesión 1: Vibecoding — El framework, no la herramienta

**Objetivo:** Entender qué es vibecoding como metodología profesional y romper mitos.

- Qué es vibecoding: de Andrej Karpathy al flujo de producción
- El loop fundamental: Intención → Especificación → Generación → Revisión → Iteración → Entrega
- Mapa del ecosistema: coding agents (Claude Code, Gemini CLI) vs. doing agents (Anton) vs. app builders (Claude Design, Lovable, Bolt)
- Cuándo vibecoding funciona y cuándo no — expectativas realistas
- El problema que tuvo Alex: por qué el ciclo Claude Design → Code → Netlify → rezar no escala
- **Práctica:** Instalar UV, configurar terminal, ejecutar un "hola mundo" con Claude Code

### Sesión 2: Git — Tu red de seguridad

**Objetivo:** Dominar Git como herramienta de supervivencia, no como herramienta de programador.

- Qué es Git y por qué es obligatorio si tu proyecto va a evolucionar
- Modelo mental: snapshots, no archivos — cada commit es un punto al que puedes regresar
- Flujo básico: `init` → `add` → `commit` → `push` — el ciclo de vida de un cambio
- GitHub: crear repo, clonar, subir cambios
- Branches: trabajar en paralelo sin romper lo que ya funciona
- **Práctica:** Crear un repositorio para el Project Pipeline Tool, hacer 3 commits con cambios distintos, revertir uno

### Sesión 3: Context Engineering — La meta-habilidad

**Objetivo:** Aprender a comunicarle al agente exactamente qué quieres y cómo lo quieres.

- Qué es un prompt vs. qué es context engineering — por qué el prompt no es suficiente
- Archivos de contexto: CLAUDE.md como "manual de instrucciones" para tu agente
- Anatomía de un buen CLAUDE.md: reglas, convenciones, restricciones, stack tecnológico
- El problema del "drift": cuando el agente se olvida de lo que le dijiste
- Seguridad básica: por qué nunca pones credenciales en el código (el ejemplo que mencionó su colega)
- Variables de entorno y `.env` — cómo manejar secretos correctamente
- **Práctica:** Escribir el CLAUDE.md del Project Pipeline Tool con reglas de stack, estilo y restricciones de seguridad

---

## BLOQUE 2 — CONSTRUCCIÓN (Sesiones 4–8)

> *Objetivo del bloque: Construir el Project Pipeline Tool completo usando Claude Code, y empezar el Opportunity Radar.*

### Sesión 4: Claude Code — El coding agent de terminal

**Objetivo:** Dominar Claude Code como herramienta principal de construcción.

- Instalación y configuración avanzada
- Flujo profesional: describir → generar → revisar diff → aprobar/rechazar → iterar
- Navegación de codebase: pedirle al agente que explique, busque patrones, refactorice
- Integración con Git: commits asistidos por IA, branches desde el agente
- CLAUDE.md en acción: cómo el archivo de contexto cambia la calidad del output
- **Práctica:** Rehacer el Project Pipeline Tool desde cero con Claude Code — estructura de proyecto, componentes base, layout principal

### Sesión 5: Project Pipeline Tool — Backend y persistencia

**Objetivo:** Resolver el problema principal del prototipo de Alex: que los datos no se pierdan al refrescar.

- Qué es un backend y por qué Alex lo necesita (su bug de persistencia explicado)
- Opciones para un PM: Supabase, Firebase, SQLite — cuál elegir y por qué
- Pedirle al agente que configure la base de datos y la conecte al frontend
- CRUD: las 4 operaciones básicas (crear, leer, actualizar, borrar) sin escribir código
- **Práctica:** Conectar el Pipeline Tool a una base de datos — que los proyectos, asignaciones y estados persistan

### Sesión 6: Project Pipeline Tool — Funcionalidades y permisos

**Objetivo:** Completar las funcionalidades que Alex necesita para su equipo.

- Roles y permisos: admin vs. viewer — cómo pedirle al agente un sistema de autenticación básico
- Carga de trabajo del equipo: visualización por semana, por persona
- Peso de proyectos: lógica de priorización
- PTO y holidays: integración con el calendario del equipo
- Iteración efectiva: cómo describir bugs y features al agente para que los resuelva bien
- **Práctica:** Implementar permisos, vista de carga de trabajo y priorización de proyectos

### Sesión 7: Despliegue y entrega del Pipeline Tool

**Objetivo:** Publicar el Pipeline Tool de forma profesional y segura.

- Despliegue correcto: Netlify/Vercel vs. Railway/Render — frontend vs. full-stack
- Variables de entorno en producción: cómo manejar secretos fuera del código
- Dominio personalizado (opcional): cómo conectar un dominio si la empresa lo pide
- Checklist de seguridad antes de publicar — lo que su colega le advirtió
- **Práctica:** Desplegar el Pipeline Tool completo, verificar que funciona, compartir con el equipo

### Sesión 8: Opportunity Radar — Diseño y especificación

**Objetivo:** Planificar el proyecto grande antes de escribir una sola línea con el agente.

- Especificación en lenguaje natural: cómo un PM escribe un brief que un agente de IA pueda ejecutar
- Arquitectura del Opportunity Radar:
  - Fuentes de datos externas: APIs de noticias (NewsAPI, Google News API, etc.)
  - Fuentes internas: HubSpot API, emails (si es viable), lista de clientes
  - Motor de matching: cómo conectar una noticia con un cliente relevante
  - Canal de salida: email, Slack, o dashboard
- MVP vs. versión final: qué entra y qué no en la primera versión
- Escribir el CLAUDE.md del Opportunity Radar
- **Práctica:** Redactar la especificación completa del MVP y el archivo de contexto

---

## BLOQUE 3 — HERRAMIENTAS COMPLEMENTARIAS (Sesiones 9–10)

> *Objetivo del bloque: Expandir el toolkit de Alex con Gemini CLI y automatización.*

### Sesión 9: Gemini CLI — La alternativa económica

**Objetivo:** Dominar Gemini CLI para cuando Claude Code se quede sin tokens.

- Instalación y setup con GEMINI.md
- Diferencias prácticas vs. Claude Code: ventana de contexto, tokens, velocidad
- Cuándo usar Gemini vs. Claude: estrategia de costos y capacidades
- Agent Skills de Gemini: crear y reutilizar
- **Práctica:** Resolver una tarea del Opportunity Radar con Gemini CLI y comparar resultado con Claude Code

### Sesión 10: Automatización sin código — APIs y agentes

**Objetivo:** Conectar fuentes de datos externas y configurar automatizaciones.

- Qué es una API y cómo hablarle sin programar (el agente lo hace por ti)
- APIs de noticias: NewsAPI, Google News, RSS feeds — setup y pruebas
- HubSpot API: extraer lista de clientes, industria, historial
- Introducción a Anton: el "doing agent" para análisis y automatización rápida
- Cron jobs en lenguaje natural: tareas programadas que corren solas
- **Práctica:** Conectar una API de noticias al Opportunity Radar y obtener las primeras alertas de prueba

---

## BLOQUE 4 — OPPORTUNITY RADAR (Sesiones 11–13)

> *Objetivo del bloque: Construir, testear y desplegar el Opportunity Radar como producto entregable a la empresa.*

### Sesión 11: Opportunity Radar — Motor de matching

**Objetivo:** Construir el núcleo del sistema que conecta noticias con clientes.

- Motor de keywords/patrones: cómo matchear noticias con industrias y clientes
- Scoring de relevancia: no todas las alertas son iguales
- Generación de recomendaciones: template de outreach personalizado por cliente
- Almacenamiento: base de datos de alertas y acciones tomadas
- **Práctica:** Implementar el motor de matching con datos reales de prueba

### Sesión 12: Opportunity Radar — Dashboard y notificaciones

**Objetivo:** Darle cara al sistema y configurar los canales de salida.

- Dashboard: vista de alertas activas, filtros por industria/cliente/fecha
- Notificaciones: configurar envío por email o Slack cuando se detecta una oportunidad
- Historial: registro de alertas enviadas y acciones tomadas
- UX para no-técnicos: que el equipo de ventas pueda usar el radar sin ayuda
- **Práctica:** Montar el dashboard y configurar al menos un canal de notificación funcional

### Sesión 13: Opportunity Radar — Despliegue y presentación

**Objetivo:** Entregar el MVP listo para mostrar a los jefes.

- Testing del flujo completo: de la noticia a la alerta al outreach recomendado
- Despliegue en producción con variables de entorno correctas
- Documentación automática: pedirle al agente que genere un README profesional
- Preparar la demo: qué mostrar, en qué orden, qué preguntas anticipar
- **Práctica:** Despliegue final + ensayo de demo de 10 minutos para presentar a la empresa

---

## BLOQUE 5 — CALIDAD Y CIERRE (Sesiones 14–15)

> *Objetivo del bloque: Asegurar calidad, consolidar aprendizajes y definir ruta de crecimiento.*

### Sesión 14: Testing y calidad — Lo que nadie enseña

**Objetivo:** Validar que lo que genera la IA realmente funciona y es seguro.

- Por qué el testing es no negociable (el 96% del código IA no es correcto de entrada)
- Pruebas unitarias vs. pruebas de integración: cuándo importa cada una
- Cómo pedirle tests al agente: prompts que generan tests útiles
- Cobertura de código: qué medir y qué ignorar
- Seguridad: los errores más comunes del código generado por IA y cómo detectarlos
- **Práctica:** Generar suite de tests para ambos proyectos, medir cobertura, corregir fallos

### Sesión 15: Cierre — Flujo completo y próximos pasos

**Objetivo:** Consolidar el framework y definir cómo seguir creciendo solo.

- Repaso del flujo completo: Especificación → Context Engineering → Construcción → Testing → Despliegue
- Debugging asistido: qué hacer cuando el agente se equivoca
- Mantenimiento: cómo actualizar y evolucionar proyectos vibecodeados
- Ecosistema emergente: Lovable, Bolt, Cursor, Windsurf, Replit — panorama rápido
- Ruta freelance: cómo ofrecer estos servicios a negocios pequeños
- Recursos para seguir aprendiendo: comunidades, docs, newsletters
- **Entregables finales:**
  - Project Pipeline Tool funcional y desplegado
  - Opportunity Radar MVP funcional y desplegado
  - Documentación generada por IA para ambos proyectos

---

## Material de apoyo (por sesión)

- Notas de clase en el repositorio compartido (leer antes de la sesión)
- Cheatsheet de comandos y prompts clave
- Templates de archivos de contexto (CLAUDE.md, GEMINI.md)
- Lista de prompts probados para cada herramienta
- Links a documentación oficial

---
