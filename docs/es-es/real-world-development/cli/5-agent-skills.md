---
title: "Lección 5: Personalizar y usar una skill de comprobaciones de calidad"
description: "Explora la skill quality-checks existente, personaliza el formato de sus informes y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has validado manualmente que funciona y has usado archivos de instrucciones para comprobar que sigue los estándares. ¿Y las pruebas, el lint y los demás componentes de la integración continua (CI)?

Para este tipo de tareas, las **skills de agente** son una buena opción. Ayudan a Copilot a ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la skill `quality-checks` existente;
- personalizarás el formato de los resultados;
- volverás a cargar la skill y la ejecutarás.

## Escenario

Tailspin Toys usa la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. Una skill es una carpeta que contiene un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción, una explicación de lo que hace la skill e indicaciones sobre cuándo usarla. También puede incluir subcarpetas con scripts y recursos de apoyo.

> [!NOTE]
> No es obligatorio incluir carpetas ni archivos adicionales. La skill `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las skills pueden estar en `.github/skills` para compartirlas con el equipo desde el repositorio, o en la carpeta de skills del usuario, `~/.copilot/skills`.

## Explora la skill

Explorarás la skill `quality-checks` creada por el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a Codespace. En el editor, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo usar la skill.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Observa que aún no hay una sección **Results output formatting**.

## Ejecuta la skill antes de modificarla

Puedes llamar a las skills directamente desde Copilot CLI o pedirlas en lenguaje natural. Pídele a Copilot que ejecute sus tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejoraremos el informe para que indique qué se ejecutó, si se superó y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la skill actualizada

Copilot CLI puede volver a cargar las skills editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el nuevo informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una skill de agente existente. En esta lección:

- has explorado la skill `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de los resultados;
- has vuelto a cargar y ejecutado la skill.

Ese cambio se incluirá en la PR de la funcionalidad de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad sirven como referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
