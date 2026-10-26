---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos en una aplicación suelen afectar a varios archivos y requieren ejecutar pruebas. Un desarrollador localiza los archivos, hace cambios, ejecuta pruebas, depura, busca lo que falta y vuelve a modificar el código. El modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes, coordinarlos, ejecutar pruebas y corregir los problemas que detecta. En este ejercicio usarás el modo agente para añadir una funcionalidad.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y cloud agent ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de feedback rápidos. Cloud agent trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, localiza los archivos relevantes y coordina los cambios;
- el modo agente de GitHub Copilot implementa funciones en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generadas antes de combinar el código.

## Escenario

Como la lista de juegos crece, quieres que los usuarios puedan filtrarla por categoría y editorial. Ya has añadido un helper de editoriales en el ejercicio anterior; ahora terminarás el trabajo de datos, interfaz y pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con un tema para desarrolladores. Permite mostrar juegos y sus detalles. Es una aplicación Astro única que genera HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend separada ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables viven en `src/lib/`, y la interactividad usa scripts Astro pequeños y acotados con API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo Astro.

1. Vuelve a Codespace. Deberías estar en `main` tras el ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, verás un banner con la URL:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un diálogo para abrir `http://localhost:4321` en el navegador, ciérralo con **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, el enlace localhost se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

### Explora el sitio web

El sitio es una plataforma de financiación colectiva de juegos de mesa con un tema para desarrolladores. La página principal muestra juegos; al seleccionar uno, aparece la página de detalles con información como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve a Codespaces, abre Copilot Chat y selecciona **New Chat**. El modo y el modelo varían según el ejercicio; cada paso indicará los detalles necesarios.

Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te lo muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal y usa herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo por encima de otro. Usamos Claude Sonnet 4.5 al crear el taller para que las sugerencias fueran relativamente coherentes. Como los modelos de lenguaje son probabilísticos, las sugerencias pueden diferir de las indicadas; es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt para consultar las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le proporciona contexto. Usarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo añadir funcionalidad.

1. En Codespaces, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar las directrices, dividir conjuntos grandes en otros más pequeños y ofrecer ejemplos.

## Implementa el filtrado

Para completar el filtrado, hay que:

- añadir o mejorar la lógica de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar pruebas;
- actualizar la página de listado de juegos con la interfaz de filtrado.

Además, hay que ejecutar las pruebas antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de datos, interfaz y pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar unos minutos. Es posible que aparezcan los controles **Keep** y **Undo** mientras trabaja; no hace falta seleccionarlos hasta que termine.

7. Cuando Copilot lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunos fallos durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta terminar y no detectar errores.

   ![Sesión de Copilot Agent Mode terminada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y prueba la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot implementa funciones en la interfaz y la capa de datos;
- el modo agente explora el proyecto, localiza los archivos pertinentes y coordina los cambios;
- revisar los cambios y las pruebas generadas antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio adicional: Implementa la paginación

Al crecer la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que la implemente. Ten en cuenta:

- sigue las buenas prácticas y usa los archivos de instrucciones existentes;
- decide si el usuario podrá elegir el tamaño de página o si será fijo;
- explica a Copilot cómo quieres que funcione;
- itera y proporciona más contexto cuando sea necesario: es lo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
