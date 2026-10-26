---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma asíncrona y autónoma, también es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar tareas asignadas a cloud agent, incluida la [página de agentes][agents-page] en GitHub.com. Desde este centro puedes ver las tareas de los agentes con solicitudes de cambios abiertas, consultar sus operaciones y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar tareas de cloud agent;
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo ha decidido añadir también un modo claro. Como ya está actualizando los estilos del sitio y añadiendo controles, pedirás al agente que incorpore el modo claro además del alto contraste.

## Revisa las tareas de Copilot cloud agent

Consulta el estado actual de las tareas que has asignado a Copilot cloud agent.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas de los paneles principal y lateral. Deberían aparecer las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa los pasos realizados por Copilot, que pueden incluir:
   - obtener el código del repositorio;
   - configurar el entorno;
   - configurar servidores MCP;
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios del lado derecho, donde puedes consultar la PR y los archivos modificados.

## Dirige cloud agent

Ahora pedirás a Copilot que incorpore el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre elegido por Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que termine la configuración y empiece el trabajo. Verás mensajes que indican que ha comenzado.
3. En el diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la página de agentes con el diálogo para orientar una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma el mensaje y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo después de terminar el paso que estaba realizando. El proceso tardará unos minutos; puedes aprovechar el tiempo para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas de Copilot cloud agent. Desde allí puedes consultar las tareas activas y completadas, revisar el trabajo y modificar el alcance de una tarea en curso.

Has aprendido a:

- supervisar las tareas de cloud agent desde mission control y la página de agentes;
- orientar una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot termina las funciones de accesibilidad, aprenderás a [iterar sobre las solicitudes de cambios][next-lesson] que ha creado.

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
