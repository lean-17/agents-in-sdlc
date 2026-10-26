---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como ocurre en tu equipo y en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las modificaciones. Puedes revisar las diferencias, aceptar o rechazar cambios individuales y orientar la conversación con mensajes de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos;
- pedirás al agente que añada un control del modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control de modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista de Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición incluye diferencias en el chat y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver sus llamadas a herramientas, lecturas, ediciones y comandos del terminal.
3. Para cada edición propuesta:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y pedir otra propuesta.
4. También puedes abrir **Files changed**, en la parte superior del chat, para ver el conjunto de archivos modificados.

> [!NOTE]
> Si una llamada a herramienta pide permiso (por ejemplo, para ejecutar un comando), el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que aceptes.

5. Espera a que el agente termine de implementar el modo de alto contraste. Verás un mensaje final que resume los cambios.

## Pide al agente que añada el modo claro

Ahora ampliarás la misma conversación para añadir un control del modo claro. El agente conserva el contexto anterior: los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará los cambios y propondrá cómo añadir el modo claro.
3. Revisa cada nueva edición con **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles—, detenlo con **Stop**, junto al campo de chat, y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes, corregir el enfoque o ampliar el alcance.

## Revisa los archivos modificados

Antes de confirmar los cambios, revisa todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia en almacenamiento local.
4. Cuando hayas terminado, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la siguiente lección.

## Resumen y siguientes pasos

Has usado Copilot Chat en modo agente para supervisar y orientar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado instrucciones adicionales sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real;
- has aceptado y rechazado cambios individuales;
- has orientado la sesión con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una PR y revisarás los cambios del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
