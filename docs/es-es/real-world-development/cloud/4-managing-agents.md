---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que se ocupe de tres tareas independientes para mejorar la experiencia de usuario y añadir funcionalidad. Aunque cloud agent trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas a cloud agent, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas e incluso orientar una sesión en curso.

En este ejercicio, vas a:

- explorar la página de agentes para supervisar las tareas de cloud agent.
- orientar una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo se dio cuenta de que también era un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio e implementando controles de cambio de modo, parecía razonable incluir esta función. Orientarás el trabajo del agente para que añada el modo claro junto al de alto contraste.

## Revisar las tareas de Copilot cloud agent

Veamos el estado actual de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código base.
   - Generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot. Pueden incluir:
   - Obtener el código del repositorio.
   - Preparar el entorno de Copilot.
   - Configurar servidores de Model Context Protocol (MCP).
   - Realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir supervisando el trabajo.

## Orientar cloud agent

Ya has visto las tareas activas. Ahora pide a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe el mensaje siguiente:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea de cloud agent en la página de agentes con el cuadro para orientar una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot comenzará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando en el momento en que enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En este ejercicio has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas de Copilot cloud agent. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar el alcance o proporcionar indicaciones adicionales.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas de cloud agent.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
