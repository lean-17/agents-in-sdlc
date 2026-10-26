---
title: "Ejercicio 3: Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de cambios, confirma que funciona en el navegador. En lugar de probar la aplicación a mano, conectarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas de Copilot;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar el filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más potente cuando puede acceder a herramientas externas. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con esas herramientas y servicios.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a las herramientas y los recursos a través de un servidor MCP, que hace de puente entre el agente y las herramientas externas, como API y utilidades locales. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright, para navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en Codespaces.
2. Verás que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por eso Copilot pudo consultar el backlog de incidencias en el ejercicio anterior. Usa el servidor GitHub MCP remoto; VS Code se conecta mediante HTTP y te autenticas la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` hace que el navegador se ejecute sin una ventana visible, lo que es necesario en Codespaces, donde no hay un escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para sus pruebas end-to-end, así que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y confirma la confianza en el servidor

VS Code inicia los servidores MCP **a petición**. La primera vez que Copilot necesita sus herramientas, VS Code inicia el servidor y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start** encima de la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y acepta para iniciarlo.

Iniciarlo manualmente es opcional. Si lo omites, VS Code lo iniciará y te pedirá que confirmes la confianza la primera vez que Copilot use sus herramientas.

## Prueba el filtrado

El servidor MCP de Playwright permite que Copilot controle un navegador real: puede abrir una página, navegar, aplicar filtros y leer el resultado. Así podrás comprobar rápidamente la funcionalidad sin salir de la conversación.

Internamente, Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente razona sobre elementos etiquetados y estructurados (botones, enlaces y elementos de lista), como la tecnología de asistencia; por eso una comprobación funcional también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador con Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, pregunta o pide que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor, inícialo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP quizá tenga que descargar un navegador; si falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de cambios

Una vez comprobada la funcionalidad, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** en Codespaces y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de cambios

Hay varias formas de crear una solicitud de cambios (PR), por ejemplo, github.com o GitHub CLI. En este caso, pedirás a Copilot que la cree y la asocie a la incidencia correspondiente.

1. Abre el panel Copilot Chat y selecciona **New Chat**.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pídele que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot obtenga información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte.
5. Observa cómo busca la incidencia correcta y crea la PR.
6. Abre el enlace que genera Copilot para revisarla, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo sus servidores amplían las herramientas de Copilot;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has usado para probar manualmente la funcionalidad en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas especializadas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
