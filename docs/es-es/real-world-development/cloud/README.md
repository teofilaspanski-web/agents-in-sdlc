---
slug: es-es/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas el trabajo en GitHub y cloud agent se ocupa de él en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios (PR), mientras tú puedes dedicarte a otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá cloud agent y, después, le asignarás una incidencia de GitHub para que implemente el trabajo. Revisarás y usarás agentes personalizados para orientar su enfoque, supervisarás y dirigirás las sesiones desde el panel de agentes y terminarás revisando las PR e iterando sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá cloud agent |
| [2. Cloud agent][ex2] | Agente asíncrono | Asigna incidencias a Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Administrar agentes][ex4] | Supervisión | Supervisa y orienta las sesiones del agente |
| [5. Iterar][ex5] | Revisión | Revisa las solicitudes de incorporación de cambios, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Copilot cloud agent habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se usan en este taller, así que puedes completar todos los entornos con ese plan.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**; el único plan excluido es Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> En este entorno no se trata MCP (Model Context Protocol). Para aprender a usar servidores MCP con Copilot, consulta el [entorno de CLI](../cli/) o el [entorno de VS Code](../vscode/).

## Empezar

**[Empieza por el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
