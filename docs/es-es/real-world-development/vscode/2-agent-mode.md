---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a actualizarlo. Y así sucesivamente.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores detectados. Veamos cómo puedes usar el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y cloud agent ofrecen experiencias distintas. El modo de agente realiza las tareas en el IDE, lo que permite recibir comentarios e interactuar rápidamente. Cloud agent trabaja de forma asíncrona como un compañero del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- usar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de fusionarlos en el código base.

## Escenario

A medida que aumenta la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para los editores; ahora completarás con el modo de agente de Copilot el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas del lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en el frontmatter mediante Drizzle, un mapeador objeto-relacional (ORM). No hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Las funciones reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro que usa las interfaces de programación de aplicaciones (API) del modelo de objetos del documento (DOM) estándar.

### Iniciar el sitio web

Se ha proporcionado la tarea `npm run dev` para iniciar el único servidor de desarrollo de Astro. Para ejecutarla en tu GitHub Codespace:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en tu Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un aviso con una URL parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Al seleccionar en el terminal de Codespaces un enlace a una URL de localhost, Codespaces te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar sus funciones. Entre ellas:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el trabajo pendiente con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección del modo y el modelo varía en cada ejercicio; los pasos correspondientes lo indicarán cuando sea necesario.

La implementación inicial del sitio web funciona, pero queremos añadir nuevas funciones. Empecemos por revisar el trabajo pendiente. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios de forma autónoma en distintos archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo concreto. Al preparar el taller, usamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para que las sugerencias de código sean relativamente uniformes. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias que recibas pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias pendientes enviándole el siguiente mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según cuáles considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, conviene revisar el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivos, como:
     - Astro
     - Tailwind CSS (hojas de estilo en cascada)
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos grandes de instrucciones en secciones más manejables y señalar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar los filtros, habrá que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de fusionar los cambios. El modo de agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del trabajo pendiente con el mensaje siguiente:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: tanto las funciones auxiliares de la capa de datos y la interfaz de usuario como las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás una opción **Keep** o **Undo** para cada cambio; no es necesario seleccionarlas mientras el trabajo está en curso.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que el proceso se detenga un momento y que algunas pruebas fallen. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completada con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has aprendido a:

- usar el modo de agente de GitHub Copilot para implementar funciones en la interfaz de usuario y la capa de datos.
- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de fusionarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si este será fijo.
- al redactar el mensaje, proporciona a Copilot las indicaciones necesarias para que implemente lo que necesitas.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Ese es el flujo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
