---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos últimos ejercicios, pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar sus tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo una función de alternancia, parecía lógico incluir esta mejora. Orientarás el trabajo del agente para asegurarte de que añada el modo claro, además del alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación de la base de código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot. Pueden incluir:
    - obtener el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar los distintos pasos necesarios para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Permite ver la PR y los archivos modificados para supervisar el trabajo.

## Orientar al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada una opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto variará según el nombre que haya usado Copilot y el estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso concreto que estaba ejecutando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde este centro de control puedes seguir todas las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir tareas en curso para ajustar el alcance o aportar indicaciones.

Has explorado estos conceptos:

- has explorado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
