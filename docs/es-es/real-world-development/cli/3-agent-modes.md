---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una funcionalidad pequeña al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la forma correcta. En esta primera de varias lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para generar una funcionalidad, comprobarás que el código es válido y que se comporta como se esperaba y, por último, la integrarás en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma conversación y rama durante el flujo de la funcionalidad. Normalmente usarías ramas o PR distintas para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad en el modo Autopilot;
- revisarás el código;
- comprobarás manualmente la funcionalidad en un navegador reenviado.

Al avanzar, actualizarás las instrucciones del repositorio, personalizarás la skill existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys no deja de crecer y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero antes de programarla hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para resolverlos y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA en un flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. Muchos desarrolladores siguen un proceso similar a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Planificar lo que hay que crear.
3. Implementar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Cuando se haya revisado el código y se haya completado la integración continua, combinar los cambios.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen una variante de este enfoque.

Al seguir este proceso estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo control que el código escrito a mano.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta para continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, a continuación, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de trabajar en el filtrado, vuelve a Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje empieza por `#7` (o un número similar). El carácter `#` permite añadir al contexto una incidencia o solicitud de cambios de GitHub.

## Planifica el filtrado

Planificar te permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. Para cualquier tarea compleja, merece la pena dedicar tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y decidirá cuál es el mejor enfoque.

3. Durante el proceso, puede preguntarte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez escrito y revisado el plan, toca implementarlo. Usa Autopilot para que Copilot se encargue del trabajo.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador situado debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o un conflicto de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y comprueba la implementación

Debes revisar el código generado antes de combinarlo, igual que cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones que acordaste durante la planificación.
4. Confirma que el reinicio y los resultados vacíos se comportan según lo acordado y lo indicado en la incidencia.
5. Vuelve a Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad en el modo Autopilot;
- has revisado el código;
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en la generación de código y en cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
