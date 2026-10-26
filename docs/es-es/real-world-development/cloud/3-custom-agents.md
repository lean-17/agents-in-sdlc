---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o ámbitos del flujo de desarrollo. Al definir agentes con archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, buenas prácticas, patrones de programación y conocimientos específicos para que Copilot realice ese trabajo con más eficacia. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes se definen en archivos Markdown de `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, `name` y `description`, seguido de instrucciones Markdown que describen el comportamiento, la experiencia y las directrices del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente en Markdown y explican a la IA cómo realizar tareas. La diferencia más sencilla: el **agente personalizado** es quien trabaja y las **habilidades** son herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar skills e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; como parte de ese trabajo, podría usar una skill de flujo de pull requests o una para ejecutar y gestionar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para descubrir qué funciona en tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás los siguientes temas:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todo el mundo, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que el tema oscuro es difícil de leer por el contraste insuficiente entre el texto y el fondo. El equipo de diseño ha pedido implementar un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar esta funcionalidad lo antes posible.

## Revisa el agente personalizado de accesibilidad

Ya existe un agente personalizado de accesibilidad. Revisa su contenido para comprender cómo guiará a Copilot.

1. Vuelve a Codespace y abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades para generar código de sitios accesibles;
   - buenas prácticas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - errores habituales.

## Crea y asigna una incidencia

Mission control es el lugar central para trabajar con los agentes del entorno. Puedes asignar tareas a Copilot cloud agent, supervisarlas y proporcionar indicaciones adicionales. Asignarás una tarea para crear el modo de alto contraste.

1. Abre el repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. En **Title**, escribe `Add high contrast mode to website`.
5. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. Selecciona **Create**.
7. En el lado derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Diálogo de asignación de cloud agent con el agente personalizado de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados para tareas y ámbitos concretos. Con ellos puedes convertir la experiencia y los estándares del equipo en directrices reutilizables para Copilot.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, aprenderás a [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: GitHub Copilot cloud agent][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
