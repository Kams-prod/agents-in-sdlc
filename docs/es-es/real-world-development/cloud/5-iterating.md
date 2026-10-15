---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Pediste a Copilot que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios de código y, si es necesario, envía comentarios a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para crear documentación, añadir la funcionalidad de juegos relacionados y mejorar la accesibilidad antes de habilitar las revisiones del resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabajará.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por explorar la primera solicitud de incorporación de cambios (PR) generada por el agente de GitHub Copilot en la nube: la que añade documentación al código. Para ello, usarás la interfaz habitual de PR en GitHub.com.

> [!NOTE]
> Al abrir la PR, es posible que veas un aviso de que el firewall ha bloqueado a GitHub Copilot. **Es lo esperado**: Copilot tiene acceso limitado de forma predeterminada a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la PR titulada algo parecido a **Add missing documentation** o con un título más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar un momento o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado: incluye los nuevos comentarios de documentación TSDoc y otros cambios de documentación. Los detalles exactos variarán.

    Al revisar los cambios, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones del proyecto para la aplicación Astro. Esto puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es unidireccional. Puedes mencionar a Copilot en los comentarios de la PR, igual que mencionarías a otra persona del equipo, o añadir comentarios en línea al código. Copilot verá los comentarios e iniciará otra sesión para abordarlos. Como los resultados son no deterministas, no podemos proporcionar un texto exacto que debas usar. Algunas ideas de cambios que puedes solicitar:

- Añadir encabezados de comentario al principio de cada archivo de código para describir brevemente su función.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia relacionada con la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Fíjate en que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te satisfaga. Cuando estés conforme, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o algo más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones del proyecto para la aplicación Astro. Esto puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente de Copilot en la nube para solicitar cambios. Por ejemplo, podrías pedirle que ajuste la cantidad de juegos relacionados que se muestran o que añada encabezados de comentario y comentarios TSDoc (recuerda: esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Como en la PR anterior, puedes solicitar estos cambios mediante un nuevo comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local del navegador para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - cualquier código JavaScript/TypeScript que gestione el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones del proyecto para la aplicación Astro. Esto puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama que creó Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan como se espera y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a orientar a Copilot para que implementara estas funciones siguiendo las buenas prácticas de accesibilidad. Si detectas problemas u oportunidades de mejora, puedes mencionar a **@copilot** en un comentario para solicitar cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta especialmente útil para delegarle tareas reales del backlog mientras tú te ocupas de otros asuntos. Para acostumbrarte a usarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario de interés para mecenas en la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados incluidos en el repositorio** para observar más adelante su efecto en el resultado del agente en la nube.
- **Has asignado incidencias al agente de Copilot en la nube** y observado cómo configuraba el entorno, planificaba y ejecutaba tareas de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad, con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los demás recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
