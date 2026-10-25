---
title: "Lección 7 - Crea y utiliza un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad definido.

En esta lección:

- Explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- Crearás y revisarás un perfil reutilizable de control de calidad.
- Seleccionarás el agente de control de calidad y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios, Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida mediante un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller, definirás un perfil de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones utilizadas cumplen distintas funciones: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de control de calidad indica a Copilot cómo utilizar estas capacidades para evaluar los requisitos y presentar los resultados. No sustituye las demás herramientas ni requiere iniciar otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la solicitud de incorporación de cambios de la función, pide a Copilot que cree un perfil reutilizable. Definirá las comprobaciones y los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comparar las funciones con sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, utilizar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe informar del estado de cada requisito como aprobado, fallido o bloqueado, y aportar pruebas que lo respalden. Debe pedir permiso antes de modificar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Revisa el perfil

Antes de utilizar el agente, comprueba que Copilot haya incluido el proceso y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio modifique la función cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero facilita identificar el agente.
3. Lee las instrucciones del perfil y confirma que comienza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que presenta pruebas, pide permiso antes de modificar código de implementación y no confirma cambios ni abre solicitudes.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que utilice el contexto existente de la incidencia y la planificación.

1. Activa el agente:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Pide al agente que revise la función:

   ```plaintext
   Revisa la función de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una solicitud de incorporación de cambios?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un perfil especializado reutilizable y revisado su trabajo. En esta lección:

- Has explorado cómo se relaciona un agente personalizado con instrucciones, habilidades y herramientas MCP.
- Has creado y revisado un perfil reutilizable de control de calidad.
- Has seleccionado el agente y revisado sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la habilidad actualizada, el perfil QA, las pruebas y el informe de verificación. A continuación, [reunirás todo en una solicitud de incorporación de cambios y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
