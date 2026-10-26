---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar el enfoque, Autopilot para desarrollar el filtrado desde una incidencia e Interactive para revisar y validar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una funcionalidad pequeña al proyecto. Los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la forma correcta. En esta primera de varias lecciones seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que se comporta como se esperaba y, por último, la integrarás en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma sesión durante el flujo de la funcionalidad. Normalmente usarías sesiones o PR distintas para diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la funcionalidad con **Autopilot**;
- revisarás el código;
- validarás manualmente la funcionalidad en un lienzo de navegador.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill existente `quality-checks`, añadirás validación mediante MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y sus visitantes necesitan acotar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero hay que acordar detalles —como la combinación de categorías— antes de programar. Utilizarás Plan para resolverlos y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. Muchos desarrolladores siguen un proceso similar:

1. Abrir una incidencia con los detalles del trabajo necesario.
2. Crear un plan de lo que hay que desarrollar.
3. Implementar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Cuando se haya revisado el código y completado la integración continua, combinar los cambios.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen una variante de este enfoque.

Al seguir este proceso estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo control que el código escrito a mano.

## Modos de sesión

El **modo de sesión** controla cuánta autonomía tiene el agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y lo apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Confirma que la PR de las valoraciones por estrellas está combinada y que tu `main` local está actualizado antes de continuar.

1. En la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, en la navegación superior del repositorio, selecciona **Issues**. Abre **Allow users to filter games by category and publisher**. Esta incidencia ya existe en el repositorio; no crees otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planifica el filtrado

La planificación permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluya el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después del flujo de calidad. Pide cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para usarlas al validar el resultado.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Confirma el modo activo en lugar de dar por hecho que la aprobación lo cambia. El indicador debe mostrar **Autopilot**.

Copilot empezará a implementar la funcionalidad. Iterará siguiendo el plan, generará código y ejecutará pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y valida la implementación

Una vez generado el código, revísalo antes de combinarlo, igual que cualquier otro código. También iniciaremos el sitio para comprobar su funcionamiento.

1. Abre **Changes** y revisa la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios sigan las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y repite las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este worktree, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación y abre el sitio en un lienzo de navegador.

1. Envía este mensaje para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana de navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados corresponden a esa editorial.
4. Combina los filtros de categoría y editorial, y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Al terminar, pide a Copilot que detenga el servidor que inició para esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva la sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el worktree y la rama para las Lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código, añadirás mejoras a la skill y al agente de QA, y después crearás y combinarás la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva tras detener su servidor y volver a ella en la siguiente lección.

## Resumen y siguientes pasos

Has usado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub;
- has definido los requisitos en modo **Plan**;
- has implementado la funcionalidad con **Autopilot**;
- has revisado el código;
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
