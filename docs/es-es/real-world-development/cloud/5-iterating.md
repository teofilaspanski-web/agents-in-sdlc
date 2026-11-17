---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Supervisar y administrar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Has pedido a Copilot que documente el código, que cree una función de juegos relacionados para que el equipo de diseño pueda iterar y que implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si es necesario, dar indicaciones a Copilot para mejorarlos.

### Escenario

Como hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de permitir que el resto del equipo las revise.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent trabaja de forma asíncrona y sin supervisión, se han establecido algunas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para el código.
- Cloud agent se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisar la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que generó GitHub Copilot cloud agent: añadir documentación al código. Revisarás los cambios en la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas un aviso de que el firewall bloquea a GitHub Copilot. Es **esperable** porque Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar un momento o repasar lo aprendido hasta ahora.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar e iniciar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.

> [!TIP]
> ¿Quieres otra opinión sobre los cambios de accesibilidad? Menciona a `@copilot` en un comentario de la PR y pídele, por ejemplo, que «revise esta PR para detectar otros problemas relacionados con las pautas de accesibilidad para el contenido web (WCAG, por sus siglas en inglés)» o que «sugiera mejoras para la navegación con teclado». Copilot iniciará una nueva sesión para atender el comentario.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR, igual que harías con otras personas del equipo, o en comentarios en línea del código. Copilot verá estos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos indicar un mensaje exacto. Puedes pedirle, por ejemplo:

- Añadir a cada archivo de código una cabecera breve que describa su propósito.
- Añadir comentarios TSDoc a archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como lo harías con cualquier usuario. Usa una de las ideas anteriores o sugiere otro cambio de documentación que quieras ver en el código base.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer un nuevo commit en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y fusionarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar e iniciar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes pedir cambios a Copilot cloud agent de forma iterativa. Por ejemplo, puedes solicitar que ajuste el número de juegos relacionados o que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Como en la PR anterior, puedes pedir cambios añadiendo un comentario en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de la interfaz para cambiar entre modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de hojas de estilo en cascada (CSS) o de estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas de las aplicaciones de Internet enriquecidas accesibles (ARIA) y la navegación mediante teclado.
   - El código JavaScript/TypeScript que controla el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar e iniciar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver los flujos de trabajo en la cola de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de modo de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, igual que hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

Cloud agent resulta especialmente útil cuando puedes asignarle incidencias reales del trabajo pendiente y ocuparte de otra cosa. Para practicar, crea más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para mostrar interés de los patrocinadores en la página de detalles del juego.
- Implementar la paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el entorno de cloud agent. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizadas del repositorio** para observar más adelante su efecto en el trabajo de cloud agent.
- **Asignaste incidencias a Copilot cloud agent** y observaste cómo preparaba el entorno, planificaba y ejecutaba las tareas de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios de cloud agent**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno de cloud agent. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados mediante los modos Plan y Autopilot, las habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate`.

En tu propio repositorio, prueba estas ideas:

- Asignar a cloud agent una incidencia de refactorización o cobertura de pruebas.
- Crear un agente personalizado para otro ámbito.
- Configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Ejercicio anterior: Supervisar y administrar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
