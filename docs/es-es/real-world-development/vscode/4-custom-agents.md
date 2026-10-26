---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes con archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, buenas prácticas, patrones de programación y conocimiento específico para que Copilot realice ese trabajo con más eficacia. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno de accesibilidad que aplique [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos `name` y `description`, seguido de instrucciones Markdown que describen su comportamiento, experiencia y directrices.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente en Markdown y explican a una IA cómo realizar tareas. La diferencia más sencilla: el **agente personalizado** es quien trabaja y las **habilidades** son herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; podría usar habilidades de flujo de trabajo de pull requests o para ejecutar y gestionar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Prueba e itera para descubrir qué funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todo el mundo, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que el tema oscuro es difícil de leer por el contraste insuficiente entre el texto y el fondo. El equipo de diseño ha pedido implementar un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar la funcionalidad lo antes posible.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad;
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya existe un agente de accesibilidad. Revisa su contenido para comprender cómo guiará a Copilot.

1. Vuelve a Codespace y crea una rama nueva desde `main` para mantener separada la PR del ejercicio anterior:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

4. Revisa las secciones siguientes, que describen:
   - las responsabilidades al generar código para sitios web accesibles;
   - buenas prácticas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector de la vista Copilot Chat. Puedes seleccionar uno para limitar la conversación a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve a Codespaces, abre Copilot Chat y selecciona **New Chat**. El modo y el modelo varían según el ejercicio; cada paso indicará los detalles necesarios.

1. Comprueba que **Agent** está seleccionado en el selector de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes de la parte inferior del chat. De forma predeterminada, muestra **default**.
3. Selecciona **Accessibility agent**.
4. Envía este mensaje:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot explorará el código, propondrá cambios y editará los archivos. Cada edición aparecerá en el chat con la ruta y las diferencias para que la revises.
6. El indicador **Files changed** mostrará los archivos modificados.

> [!NOTE]
> Este proceso puede tardar unos minutos. Copilot hará cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás el trabajo en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados para tareas y ámbitos concretos. Con ellos puedes convertir la experiencia y los estándares del equipo en directrices reutilizables para Copilot.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
