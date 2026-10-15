---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Haz un pequeño cambio en las tarjetas de juegos con Copilot CLI, revísalo en un navegador reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación; ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero todavía no se muestra en las tarjetas de la página de inicio. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica de Copilot en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmar el cambio en un navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git activos en el terminal, en lugar de crear un worktree dedicado. Las conversaciones guardadas te permiten volver al mismo diálogo más adelante, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres cosas: tus prompts y las respuestas del agente; la actividad de sus herramientas mientras explora y modifica archivos; y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y pedir el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve al Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para pedir el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisar la diferencia

Todos los cambios generados por IA deben revisarse antes de combinarlos, incluso los pequeños. Inspecciona aquí mismo los cambios en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico, no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para poder examinar la interfaz de usuario (UI) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora crearás una solicitud de incorporación de cambios (PR) para combinar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Al crear la PR, Copilot CLI enviará el commit existente y mostrará la URL de la PR.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación específica de Copilot en una rama de funcionalidad.
- has indicado al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación para confirmar la valoración por estrellas en un navegador reenviado.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
