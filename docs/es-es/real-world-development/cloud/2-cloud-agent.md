---
title: "Ejercicio 2: GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código antiguo que hay que actualizar o solicitudes de funcionalidad que llevan tiempo en el backlog por falta de recursos. GitHub Copilot cloud agent puede actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, crea una solicitud de cambios (PR) en borrador para que un desarrollador la revise. Así puedes delegar tareas tediosas y dedicarte a otras prioridades.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. La documentación del proyecto está incompleta y el primer paso es añadir comentarios de documentación TSDoc a todas las funciones exportadas. Además, el equipo de diseño quiere facilitar el descubrimiento de juegos mostrando títulos relacionados —de la misma categoría— en la página de detalles. No hace falta una implementación definitiva; el equipo quiere probar la experiencia de usuario. Como hay otras prioridades, estas tareas son buenas candidatas para Copilot cloud agent, que puede trabajar en segundo plano y dejarte revisar los resultados más tarde.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma similar a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al recibirla, Copilot crea una PR en borrador para hacer seguimiento, prepara un entorno e inicia el trabajo. Puedes consultar la sesión mientras trabaja o después. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden tener detalles complejos, así que [define con cuidado el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio práctico de instrucciones personalizadas, conoces `.github/copilot-instructions.md`; si no, puedes consultarlo ahora. Antes de asignar una tarea, revisa los archivos de instrucciones del repositorio para detectar después cómo influyen en el resultado.

- `.github/copilot-instructions.md`: revisa **Code standards**, en particular las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa cómo el frontmatter `applyTo` limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario de la PR. Proceden de estos archivos de instrucciones. Los volverás a revisar en un ejercicio posterior.

## Configura el entorno de desarrollo de Copilot cloud agent

La creación de código suele requerir un entorno específico y scripts de configuración. Copilot también necesita un entorno preparado para trabajar de forma similar a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow], en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar. El flujo ya está configurado para este laboratorio e instala Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas end-to-end y la base de datos SQLite migrada y preparada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *estos pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (toda la aplicación usa Astro + Drizzle/libSQL)
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

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de GitHub, pero hay algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions**. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

La documentación suele estar desactualizada o faltar por completo, lo que dificulta el mantenimiento y la incorporación de nuevos desarrolladores. Copilot puede ayudar a resolver esa deuda técnica en segundo plano; más adelante revisarás su trabajo.

1. En una pestaña nueva, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que se acaba de generar; su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera unos minutos y actualiza el navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot prepara el entorno ejecutando el flujo mencionado, analiza el proyecto y decide cómo abordar la tarea.

13. Revisa las tareas que tiene previsto realizar.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de una sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo: Copilot explora el código, analiza su estado, decide un plan y crea código.

La tarea puede tardar varios minutos. Mientras Copilot trabaja de forma asíncrona, puedes ocuparte de otras tareas. Asignarás una segunda tarea y después seguirás explorando el desarrollo de la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja de Copilot cloud agent es que permite repartir tareas: tú puedes ocuparte de una mientras él trabaja en otra. Asigna al agente la tarea de añadir juegos relacionados para el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Show related games on the game details page`.
5. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. Selecciona **Create**. En el lado derecho, selecciona **Assign to Copilot** y, después, **Assign**.

    ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

En breve verás varios ojos 👀 en el primer comentario de la incidencia: significa que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot está trabajando en la segunda tarea. No hace falta que la supervises continuamente: podrás revisar el resultado cuando termine. Mientras tanto, aprenderás a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona y usarlo para resolver deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has aprendido a:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras cloud agent trabaja en segundo plano, aprenderás a crear y usar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que exploraste en los módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

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
