---
title: "Ejercicio 1: Instrucciones personalizadas (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Copilot cloud agent →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o hay información de fondo que Copilot debería conocer, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para determinadas rutas;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás un prompt anterior y posterior. Copilot cloud agent trabaja de forma asíncrona en incidencias de GitHub, así que resulta más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones al revisar las PR que genere el agente en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para el desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para que Copilot disponga de la información adecuada y trabaje de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas aportan contexto y preferencias para que Copilot comprenda mejor el estilo de programación y los requisitos del proyecto. Puedes definir convenciones, bibliotecas y el tipo de comentarios que prefieres. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, el único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se está creando, buenas prácticas y otras directrices globales.
- `.github/instructions/*.instructions.md`, archivos para tipos de archivo o tareas concretos. Puedes usarlos para lenguajes como TypeScript o Astro, o para crear componentes de interfaz y pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto: su descripción, estructura y estándares generales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Escribe con lenguaje natural, mantén las directrices claras y añade ejemplos del código esperado y del que se debe evitar.

No existe una única forma de crear instrucciones. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto hay archivos para varios tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz ofrece una opción distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo indicará cuando sea relevante.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Este repositorio incluye un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y busca las instrucciones de Astro, Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define el patrón de archivos al que se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, como `**/*.test.ts`.
5. Revisa las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes separar conjuntos de instrucciones y enlazar ejemplos que Copilot pueda seguir. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentación en el código. En los siguientes pasos añadirás comentarios TSDoc y encabezados de comentario para los archivos.

## Añade un estándar nuevo al repositorio

Añadirás una regla general para incluir comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará estas instrucciones en las incidencias que le asignes en ejercicios posteriores.

1. En el terminal de Codespaces, crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27.
4. Debajo de los estándares existentes, añade estas líneas:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones del repositorio (`.github/copilot-instructions.md`) son adecuadas para estándares generales. Usa archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas.

## Confirma, envía y combina la actualización de instrucciones

Copilot cloud agent lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, partirá de `main`, por lo que debes integrar las instrucciones en `main` para que pueda usarlas.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` a `main` y combínala. El agente podrá leer las instrucciones al trabajar en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar los cambios en esa rama. La rama se usa para que el taller siga un flujo realista.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás la influencia de esa regla en las PR que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia a Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
