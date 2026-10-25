---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Ha llegado el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para simplificar el taller, todo el flujo de filtrado y calidad se ha mantenido en una sesión y una rama, y se incluirá en esta PR.

En esta lección:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Revisarás toda la PR de la función y las pruebas de verificación.
- Autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener el control de la autorización de la combinación en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios mediante una sesión de la aplicación GitHub Copilot. Cuando lo habilitas, la sesión lee la PR y resuelve los impedimentos: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y aplica un `rebase` cuando hace falta. Después, combina la solicitud en cuanto GitHub lo permite. El trabajo continúa en segundo plano incluso si reinicias la aplicación, y Agent Merge se desactiva cuando la solicitud se combina.

Hasta ahora, tú mismo seleccionabas **Merge pull request**. Agent Merge puede encargarse de esa tarea, pero la autorización para editar código y combinar sigue dependiendo de ti. Revisa las acciones permitidas y el trabajo antes de otorgar permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Con todo el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí solo no lo inicia.

Agent Merge se pondrá en marcha y:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- Reorganizará la base o resolverá posibles conflictos con la rama de destino.
- Supervisará CI para comprobar que se superen todas las verificaciones.
- Supervisará los comentarios de otros desarrolladores y de la revisión de código de Copilot, y realizará cambios para resolverlos.
- Opcionalmente, combinará automáticamente la PR cuando todo se haya superado.

Deja que Agent Merge combine la PR cuando todo esté listo.

5. Revisa la diferencia de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que esté marcada la opción **Merge pull request**.
8. Pide explícitamente al agente que la combine:

    ```plaintext
    Combina la solicitud cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
    ```

9. Sigue el progreso y resuelve los impedimentos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**; tener activado el permiso de combinación o un agente en ejecución no significa que la operación haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos impedimentos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora también gestionar la solicitud de incorporación de cambios. En esta lección:

- Has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Has revisado toda la PR de la función y las pruebas de verificación.
- Has autorizado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
