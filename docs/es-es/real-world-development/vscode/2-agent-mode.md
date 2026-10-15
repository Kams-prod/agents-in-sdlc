---
title: "Ejercicio 2 - Añadir funcionalidad nueva con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Normalmente, un desarrollador tiene que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización… y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía dentro del IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura existente del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo usar el modo agente para añadir una nueva funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente trabaja en el IDE y permite ciclos rápidos de respuesta e interacción. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo puede el modo agente de Copilot explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- cómo puede GitHub Copilot Agent Mode implementar funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas generadas por el modo agente de Copilot antes de combinarlos en la base de código.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. Ya has añadido un helper de editoriales en el ejercicio anterior; ahora completarás con el modo agente de Copilot el trabajo restante de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de realizar cambios, explora el sitio web de Tailspin Toys para entender su funcionamiento actual.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con un pequeño `<script>` de Astro acotado que usa las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, se ha proporcionado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces así:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost desde el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, donde se ejecuta el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso los especificará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir funciones nuevas. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente puede planificar e implementar cambios de forma autónoma en varios archivos, ejecutar comandos del terminal y usar herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo. Al crear el taller usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias sean relativamente coherentes y la experiencia resulte fluida. Sin embargo, como los LLM son probabilísticos, las sugerencias pueden diferir de los ejemplos del taller. Es totalmente normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot podría usar un comando MCP distinto y aun así completar la tarea.

3. Pregunta a Copilot por el backlog enviando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado las tareas según las que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, facilidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar las instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios en la aplicación:

- añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en la base de código. El modo agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar una nueva seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de la interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla en la que Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y de la base de código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer todos los cambios; no tienes que seleccionar estos botones mientras el trabajo está en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un diálogo en el panel Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido:

- cómo implementar nuevas funcionalidades en la interfaz y la capa de datos con el modo agente de GitHub Copilot.
- cómo puede el modo agente explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- cómo revisar los cambios y las pruebas generadas por el modo agente antes de combinarlos en la base de código.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que implemente la paginación. Al preparar el prompt, ten en cuenta:

- seguir las buenas prácticas existentes, incluidos los archivos de instrucciones.
- decidir cómo funcionará la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluir en el prompt las directrices necesarias para obtener la implementación que quieres.
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
