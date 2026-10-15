---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda buena herramienta CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explora ahora otros que pueden resultar útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` permite exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas, puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario; también puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Es posible que apenas haya cambiado si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` para decidir cuándo hacerlo. Usa `/clear` o `/new` si vas a cambiar a una tarea no relacionada y quieres empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite ver la lista y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar en equipo y compartir lo aprendido ayuda a todo el mundo a sacar más partido de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir esta sesión, elige el destino que mejor se adapte a tus necesidades, como `/share file` para exportarla localmente a Markdown.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de conversaciones que el equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Has completado el taller principal. Copilot CLI también incluye comandos para tareas de mayor alcance:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea aparte.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear worktrees adicionales o iniciar trabajo remoto. Empieza por una tarea nueva y bien delimitada, y revisa el resultado con el flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller sobre el agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver qué ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` permite exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Copilot CLI incluye más comandos de barra y aún queda mucho por explorar. Para cerrar el recorrido, [repasarás lo aprendido][next-lesson] y verás los siguientes pasos para continuar.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
