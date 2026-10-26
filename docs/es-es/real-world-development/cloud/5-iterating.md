---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones: documentar el código, crear una sección de juegos relacionados y añadir funciones de accesibilidad con modos de alto contraste y claro. Revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent trabaja de forma asíncrona y sin supervisión constante, hay restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- Cloud agent se ejecuta en GitHub Actions, en un entorno temporal y separado.
- Una persona debe aprobar los flujos de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezarás por la primera solicitud de cambios (PR) que haya creado Copilot cloud agent: añadir documentación al código. Lo revisarás con la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Es posible que veas una advertencia de que el firewall bloquea a GitHub Copilot. Es lo esperado, ya que Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP. Si lo deseas, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye comentarios TSDoc y otros comentarios de documentación. Los cambios concretos pueden variar. Comprueba que haya comentarios TSDoc, convenciones de TypeScript y encabezados de comentario como los indicados por las instrucciones personalizadas del Ejercicio 2.
6. Cuando termines la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay una indicación de que algún flujo espera aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto, una única aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot en una PR no es unidireccional. Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código. Copilot leerá el comentario e iniciará otra sesión para atenderlo. Como los resultados no son deterministas, no hay un texto de ejemplo único. Puedes pedir, por ejemplo:

- que añada encabezados al principio de cada archivo de código con una breve descripción;
- que añada comentarios TSDoc a los archivos TypeScript y Astro;
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario sobre la documentación y menciona a **@copilot** como harías con otro usuario. Puedes usar una de las sugerencias anteriores o pedir otro cambio.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Marcar una PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR de Copilot para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos que esperan aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Confirma que las comprobaciones del proyecto se hayan superado. Puede tardar unos minutos.
9. **Opcional:** cambia a la rama en Codespaces para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes pedirle cambios con nuevos comentarios en la pestaña **Conversation**, por ejemplo, ajustar el número de juegos o añadir comentarios de archivo y TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad implementadas por el agente de accesibilidad. Esta PR debería incluir el modo de alto contraste del Ejercicio 3 y el modo claro que pediste en mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Selecciona **Files changed** para revisar el código generado.
4. Revisa la implementación, prestando atención a:
   - los controles de interfaz para cambiar de modo;
   - el uso de almacenamiento local para guardar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.);
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
5. Cuando termines la revisión, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos que esperan aprobación y, si es así, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. Confirma que las comprobaciones del proyecto se hayan superado. Puede tardar unos minutos.
8. **Opcional:** cambia a la rama en Codespaces y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se mantienen al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo buenas prácticas. Si encuentras problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle cambios, como en las PR anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent funciona bien cuando puedes asignarle tareas reales del backlog y ocuparte de otras. Crea varias incidencias y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que los usuarios indiquen su interés en los juegos;
- implementar la paginación de la lista de juegos;
- añadir validación de entrada y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido de cloud agent. En estas lecciones:

- has **inspeccionado los archivos de instrucciones personalizadas** incluidos en el repositorio para observar su efecto en los resultados de cloud agent;
- has **asignado incidencias a Copilot cloud agent** y has visto cómo configura su entorno, planifica y trabaja de forma asíncrona;
- has **creado y usado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro;
- has **usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad;
- has **revisado e iterado sobre las solicitudes de cambios** de cloud agent, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido de cloud agent. Si quieres seguir explorando, los demás recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` a cloud agent.

En tu repositorio, prueba también estas ideas:

- asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de Copilot agents][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
