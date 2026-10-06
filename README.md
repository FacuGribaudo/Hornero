# 🏗️ Hornero

> Proyecto para la automatización del desarrollo de software de calidad. Utiliza IA bajo principios de ingeniería sólidos, asegurando buenas prácticas y control humano desde el requerimiento hasta la validación.

## 💡 Nuestra Filosofía

Hoy en día, escribir líneas de código se está convirtiendo en un *commodity* gracias a los LLMs, pero diseñar el sistema correcto no. El verdadero valor de la ingeniería en sistemas reside en la arquitectura, el modelado, la captura rigurosa de requerimientos y la garantía de que la solución técnica es robusta. 

**Hornero** delega la implementación mecánica a los agentes de IA, reservando el juicio humano para donde realmente aporta valor. En este ecosistema, el desarrollador evoluciona hacia un rol de Arquitecto, manteniendo siempre el control de las decisiones críticas mediante el principio de **Human-in-the-loop (HITL)**.

## ✨ Principios de Diseño

- **Contratos Estrictos:** Los agentes se comunican mediante artefactos estructurados y versionados (JSON/Markdown). Cero dependencia de "memoria" implícita.
- **Responsabilidad Única:** El Planificador diseña, el Desarrollador ejecuta, el Tester verifica.
- **Trazabilidad Total:** Cada línea de código y caso de prueba está mapeado a la historia de usuario original en Jira.
- **Control Humano (HITL):** Nada avanza tras un punto de decisión crítico sin aprobación explícita.
- **Aislamiento Seguro:** Ejecución en ramas dedicadas, sin acceso a credenciales en texto plano ni merges automáticos a la rama principal.

## ⚙️ El Pipeline de Agentes

El flujo de trabajo se divide en 5 etapas secuenciales, cada una operada por un agente especializado (basado en Claude):

1. **🕵️ Revisor (Reviewer):** Analiza la historia de Jira, detecta ambigüedades y propone mejoras técnicas y de requerimientos.
2. **🧠 Planificador (Planner):** Toma la historia aprobada y genera el diseño técnico, módulos a crear y estrategia de pruebas.
3. **💻 Desarrollador (Coder):** Implementa el código ciñéndose estrictamente al plan aprobado. No rediseña ni improvisa.
4. **🧪 Tester:** Escribe y ejecuta pruebas de integración y funcionales. Gestiona los bucles de corrección con el Desarrollador si hay fallos.
5. **⚖️ QA:** Valida la funcionalidad contra los criterios de aceptación originales y evalúa la deuda técnica introducida.

## 🚦 Máquina de Estados (Flujo de la Historia)

Cada historia extraída de Jira se convierte en una entidad de estado finito. El motor se detiene en los estados marcados con 🔒 esperando la intervención humana.

- `BACKLOG` → Historia importada.
- `ANALYSIS_IN_PROGRESS` → Revisor analizando.
- `REQUIRES_REQUIREMENT_APPROVAL` 🔒 → Esperando aprobación de mejoras propuestas.
- `PLANNING_IN_PROGRESS` → Planificador diseñando.
- `REQUIRES_PLAN_APPROVAL` 🔒 → Esperando aprobación del plan de acción.
- `CODING_IN_PROGRESS` → Desarrollador escribiendo código en rama aislada.
- `TESTING_IN_PROGRESS` → Tester ejecutando pruebas.
- `QA_EVALUATION` → QA validando criterios de aceptación.
- `REJECTED_BY_QA` 🔄 → Retrocede a `CODING_IN_PROGRESS` (con límite de iteraciones).
- `REQUIRES_FINAL_APPROVAL` 🔒 → Esperando validación final para dar por terminada la funcionalidad.
- `DONE` → Lista para integrar.

## 🏗️ Arquitectura y Stack Tecnológico

El sistema mantiene una arquitectura cliente-servidor estrictamente separada para garantizar la mantenibilidad:

- **Frontend (Panel Local):** Interfaz desarrollada en **Angular** que permite visualizar el estado de la máquina, logs en tiempo real y presentar comparativas limpias para los puntos de aprobación humana.
- **Backend (Orquestador API):** API robusta desarrollada en **Java Spring Boot**. Actúa como el motor de la máquina de estados, gestiona los despachos de tareas a los agentes y maneja la comunicación externa.
- **Base de Datos & Estado:** Servicios administrados con **Supabase** para persistir de manera segura la máquina de estados, el registro de auditoría y los artefactos generados entre agentes.
- **Integraciones:** Conexión vía API REST con Jira, interactuación directa con el sistema de archivos/Git para ramas, y consumo de la API de Anthropic (Claude) para la inferencia de los agentes.

---
*Desarrollado para devolver el foco de la ingeniería al diseño y la calidad.*
