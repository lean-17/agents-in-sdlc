---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o hay información de fondo que Copilot debería conocer, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] son una forma de proporcionar directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las convenciones de código y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos específicos para determinadas rutas;
- generarás el primer bloque de datos del filtrado (un helper de editoriales) con las instrucciones actuales;
- añadirás un nuevo estándar general a `.github/copilot-instructions.md`;
- volverás a ejecutar el mismo prompt y observarás cómo el código sigue el nuevo estándar;
- confirmarás los cambios en `main` para que Copilot pueda usar las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede desviarse de algunos estándares. Copilot no es determinista. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para el desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para que Copilot trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot comprenda mejor el estilo de programación y los requisitos del proyecto. Puedes definir convenciones, bibliotecas y el tipo de comentarios que prefieres. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Contiene información general del proyecto, relevante para la mayoría de las interacciones: la pila tecnológica, una descripción de lo que se está creando, buenas prácticas y directrices globales.
- `.github/instructions/*.instructions.md`, archivos para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en el proyecto: qué se está creando, su estructura y los estándares globales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Usa lenguaje natural, directrices claras y ejemplos del código esperado y del que se debe evitar.

No existe una única forma de crear instrucciones. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto hay directrices para distintos tipos de tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar instrucciones. Cada interfaz ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo indicará cuando sea relevante.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], una colección de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones que incluye el repositorio: `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora su descripción y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y busca las instrucciones de Astro, Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define el patrón de archivos al que se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y archivos del proyecto. Esto permite dividir instrucciones extensas en archivos reutilizables y enlazar ejemplos. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> **Code formatting requirements** en `copilot-instructions.md` documenta los estándares del proyecto, pero aún no exige documentación en el código. En los próximos pasos añadirás comentarios TSDoc y encabezados de comentario para cada archivo.

## Crea una rama para los cambios

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cambia a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve a Codespaces, abre el panel de Copilot Chat y selecciona **New Chat**. El modo y el modelo varían según el ejercicio; cada paso indicará los detalles necesarios.

Para observar el efecto de las instrucciones, empezarás enviando un prompt con el archivo actual. Más tarde, lo actualizarás y enviarás el mismo prompt.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot reciba solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para mostrarle dónde debe estar el helper.
3. En la vista Chat, selecciona **Agent** en el selector de agentes para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, posiblemente al helper y a sus pruebas.
6. Observa que el helper es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Sigue las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Copilot es probabilístico, así que quizá añada comentarios de documentación aunque no se lo pidas. Si ocurre, no pasa nada: la idea es observar la mejora de la *coherencia* después de actualizar las instrucciones.

## Añade un estándar nuevo al repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, que debería estar cerca de la línea 27. La sección define las convenciones del proyecto, pero aún no incluye una regla de documentación en el código.
3. Debajo de los estándares existentes, añade estas líneas:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, `.github/copilot-instructions.md` es el lugar adecuado para estándares generales, mientras que `*.instructions.md` sirve para lenguajes, tipos de archivo o tareas concretos.

## Vuelve a ejecutar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el búfer.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real que usarás en el próximo ejercicio.

## Confirma las instrucciones y envía la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que están presentes los cambios de instrucciones y del helper:

   ```bash
   git status
   ```

3. Prepara los cambios de instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de cambios

Con la rama enviada, crea una solicitud de cambios asociada a la incidencia correspondiente del backlog. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Esta es la primera lección en la que Copilot actúa en GitHub. La plantilla del proyecto ya incluye las herramientas de GitHub, aunque quizá VS Code te pida iniciar sesión la primera vez. Más adelante aprenderás cómo se conecta esta integración con Model Context Protocol.

1. Abre Copilot Chat en Codespaces.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y comprueba que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia y cree una PR desde la rama actual, explicando la actualización de instrucciones y el helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
5. Una vez creada la PR, pídele que la combine y vuelva a `main` local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot pida permiso para acciones de GitHub y comandos del terminal, revisa el comando y selecciona **Allow** según corresponda.

¡Has creado y combinado una solicitud de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general a `.github/copilot-instructions.md`;
- volver a enviar el prompt y observar cómo se aplica el nuevo estándar;
- confirmar las instrucciones y la base del helper de editoriales en `main` para que el siguiente ejercicio pueda usarlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] para añadir una funcionalidad al proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
