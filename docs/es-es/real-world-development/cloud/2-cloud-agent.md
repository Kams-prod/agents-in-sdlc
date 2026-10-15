---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya muy pocas organizaciones que no tengan problemas de deuda técnica. Puede tratarse de incidencias de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una PR en borrador para que la revise una persona desarrolladora. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que el equipo se centre en asuntos de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere abordar parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: observarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere que la página de detalles de cada juego muestre otros juegos relacionados —títulos de la misma categoría— para que las personas puedan seguir explorando. Todavía no necesitan una implementación pulida; buscan algo que puedan usar para probar la experiencia de usuario (UX). Este trabajo está bloqueado por ahora, ya que hay otras incidencias de mayor prioridad.

Ambos son ejemplos de tareas que pueden quedar relegadas, y que conviene asignar al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras la persona desarrolladora se ocupa de otras tareas y, después, vuelve para revisar el trabajo y confirmar que cumple las expectativas.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a una persona desarrolladora. Puedes asignarle trabajo de varias maneras, por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios (PR) en borrador para registrar su progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución propuesta esté lista para revisión, te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el razonamiento. Incluso las operaciones que parecen sencillas pueden ser complejas si se examinan a fondo, así que [delimita cuidadosamente las tareas que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a una persona que programa contigo: trabaja por etapas, aprende, experimenta y adapta el proceso. Los principios fundamentales del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas y su función para orientar a Copilot. Si ya has completado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, examina en modo de solo lectura los archivos de instrucciones incluidos en este repositorio. Así podrás reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace si ya tienes uno iniciado:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, observa si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estos elementos se deben a los archivos de instrucciones. Volveremos a mencionarlo al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente de Copilot en la nube

La generación de código suele requerir un entorno concreto y la ejecución de scripts de configuración para garantizar que todo esté preparado, independientemente de quién realice el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a una persona ingeniera de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias para el desarrollo. Este archivo se ha configurado antes del laboratorio para facilitar el recorrido y ofrecer esta experiencia. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos de ejemplo para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar estos pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración necesitan clonar el repositorio, por ejemplo, para instalar dependencias, se requiere el permiso `contents: read`. Si no se clona el repositorio en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación ahora es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos de ejemplo en la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Resulta útil para probar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añadir documentación

Todo el mundo reconoce la importancia de la documentación, pero la mayoría de los proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar sin resolver, lo que ralentiza el trabajo y dificulta el mantenimiento de la base de código o la incorporación de nuevos miembros al equipo. Copilot destaca en la generación de documentación, por lo que esta es una incidencia ideal para asignársela al agente de Copilot en la nube. Trabajará en segundo plano para crear la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está creando el entorno —ejecutando el flujo de trabajo descrito antes—, analizando el proyecto y determinando el mejor enfoque para resolver la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene para analizar y decidir el plan adecuado, y genera código.

Es probable que tarde varios minutos. Uno de los principales objetivos del agente de Copilot en la nube es permitir que realice tareas de forma asíncrona para que podamos centrarnos en otros trabajos. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos ocuparemos de añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite repartir el trabajo: tú puedes centrarte en unas tareas mientras el agente aborda otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, podrías aprovechar ese tiempo para otras tareas. Asígnasela al agente de Copilot en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver varios 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente de Copilot en la nube funciona de forma parecida a una persona ingeniera de software, no hace falta supervisarlo activamente; basta con revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias a Copilot para que las resuelva de forma asíncrona. Puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente de Copilot en la nube también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
