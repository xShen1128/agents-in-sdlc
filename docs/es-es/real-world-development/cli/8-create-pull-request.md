---
title: "Lección 8 - Crea y combina la solicitud de incorporación de cambios de la función"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una solicitud y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad personalizada, el perfil de control de calidad y las pruebas están guardados en una rama. Ha llegado el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones con estrellas; esta vez, dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En circunstancias normales, separaríamos la función, las instrucciones, la actualización de la habilidad y el agente QA en varias solicitudes. Para simplificar el taller, todo el flujo de calidad y el filtrado se han mantenido en la misma conversación y rama.

En esta lección:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Revisarás todos los cambios de la función y las pruebas de verificación.
- Crearás la solicitud de filtrado.
- Habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo restante de la solicitud, sin quitar al desarrollador el control de autorización de la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud. Cuando lo habilitas, Copilot resuelve los impedimentos de la solicitud —por ejemplo, corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y aplica un `rebase` cuando hace falta— y, después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, tú mismo seleccionabas **Merge pull request**. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Utiliza Agent Merge para gestionar la solicitud

Revisa el código completo, crea la solicitud y deja que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud y la combinará cuando todo se supere.
7. Confirma que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y, cuando el repositorio lo permita, realiza manualmente la combinación revisada.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección:

- Has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Has revisado todos los cambios de la función y las pruebas de verificación.
- Has creado la solicitud de filtrado.
- Has habilitado Agent Merge solo después de revisar los cambios y has confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
