---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios individuales y orientar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivo propuestas.
- orientarás al agente durante la sesión para añadir un modo claro junto con el modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado otra mejora: un modo claro para quienes consideren demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a una herramienta. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición de archivo, la diferencia aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para deshacerlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados por el agente durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando del terminal—, el agente se detendrá y mostrará el aviso **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios realizados.

## Pedir al agente que añada el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector del modo claro. Al continuar en la misma sesión de chat, el agente conserva el contexto de su trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local que configuró.

1. En el cuadro de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará a partir de su trabajo anterior, explorará los cambios y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada nueva edición a medida que se aplique, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta refactorizar de una forma que no te gusta la interfaz de alternancia—, detenlo con el botón **Stop** junto al cuadro de entrada y envía un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de ajustes existente») o ampliar el alcance («añade también atajos de teclado para ambos modos»).

## Revisar los archivos modificados

Antes de confirmar el trabajo, revisa brevemente todos los archivos que haya tocado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición al aplicarse, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- cómo supervisar el trabajo de un agente en tiempo real.
- cómo aceptar o rechazar ediciones individuales de archivos.
- cómo orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que el agente personalizado local ha generado durante este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
