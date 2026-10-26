---
title: "Lección 4: Guiar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma determinada, conviene que Copilot tenga a mano esas directrices. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas;
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene una serie de directrices y requisitos para sus prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar los contratos de `Props`, y los comentarios deben actualizarse cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos del proyecto. Puedes especificar convenciones, bibliotecas e incluso los tipos de comentarios que prefieres. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las interacciones.
- `.github/instructions/*.instructions.md`, archivos con directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> Los demás formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad de las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

El proyecto de ejemplo ya incluye un conjunto de archivos de instrucciones. Explóralos antes de modificar nada para observar su efecto.

1. Vuelve a Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Lee la descripción breve del proyecto y las directrices. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de la carpeta `.github/instructions`. Hay instrucciones para Astro, la capa de datos de Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir.

## Añade un nuevo estándar a las instrucciones

Los archivos que incluye el proyecto son un buen comienzo, pero aún falta un estándar. Modificarás `copilot-instructions.md` para pedir comentarios TSDoc en los archivos TypeScript nuevos.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar aproximadamente en la línea 35.
2. Añade la siguiente viñeta al final:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las nuevas directrices

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar los cambios, cierra y reanuda la conversación sobre el filtrado para que Copilot cargue las instrucciones nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te pregunta, selecciona la conversación de filtrado y confirma que tienes abierta la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI selecciona las instrucciones del proyecto y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas;
- has actualizado el archivo de instrucciones para que se sigan los estándares de programación;
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la skill reutilizable `quality-checks`][next-lesson] para aplicar de forma coherente las pruebas y el lint.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Referencia de compatibilidad de las instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
