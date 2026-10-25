---
title: "Lección 9 - Comandos slash de GitHub Copilot CLI"
description: "Explora los comandos slash para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI, GitHub Copilot CLI incluye muchos comandos slash. Ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos algunos más.

En esta lección:

- Utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Utilizarás `/model` para explorar los modelos disponibles.
- Aprenderás cómo `/share` puede exportar o compartir una sesión.
- Explorarás comandos opcionales para trabajar en paralelo, utilizar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo principal de CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar tareas al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas, puedes acercarte al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta y ofrece comandos slash para inspeccionarla o compactarla manualmente.

1. Vuelve al codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Puede que no haya cambiado mucho si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Utiliza `/compact` para decidir cuándo hacerlo. Usa `/clear` o `/new` si cambias a una tarea no relacionada y quieres iniciar una conversación nueva.

## Elige un modelo

Los distintos modelos tienen capacidades diferentes, y cada desarrollador tiene sus preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y las experiencias ayuda a que todo el equipo mejore el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` de tu versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir mensajes, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio ni de una conversación que el equipo no quiera compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también incluye comandos útiles para tareas mayores:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabajará de forma asíncrona y quizá abra una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa el resultado mediante el flujo habitual. Para aprender más sobre el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos slash permiten configurar Copilot CLI, compartir sesiones y descubrir lo que ocurre internamente. En esta lección:

- Has utilizado `/context` y `/compact` para explorar la gestión del contexto.
- Has utilizado `/model` para explorar los modelos disponibles.
- Has aprendido cómo `/share` puede exportar o compartir una sesión.
- Has explorado los comandos opcionales para trabajo en paralelo, árboles de trabajo y delegación al agente en la nube.

Copilot CLI tiene más comandos slash y funciones que explorar. Termina el recorrido [repasando lo aprendido][next-lesson] y los siguientes pasos para continuar.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
