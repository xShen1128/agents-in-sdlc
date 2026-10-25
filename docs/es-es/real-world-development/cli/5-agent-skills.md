---
title: "Lección 5 - Personaliza y utiliza la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para asegurarte de que respete los estándares. Pero ¿qué pasa con las pruebas, lint y los demás aspectos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección:

- Explorarás la habilidad `quality-checks` existente.
- Personalizarás el formato de sus resultados.
- Volverás a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sea más fácil de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad y un archivo Markdown `SKILL.md`. El Markdown contiene frontmatter con un nombre y una descripción, un resumen de la función y pautas sobre cuándo utilizarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir archivos o carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden residir en `.github/skills` para compartirlas con el equipo como parte del repositorio o en la carpeta personal `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que el equipo de Tailspin Toys creó para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo orientan a Copilot durante las pruebas y la ejecución de lint.
4. Observa que la habilidad aún no incluye una sección **Results output formatting**.

## Ejecuta la habilidad antes de modificarla

Puedes invocar habilidades directamente en Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejoremos el informe para que indique qué se ejecutó, si se superó y qué comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Al terminar, informa de cada comando ejecutado e indica si se superó, falló o quedó bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas únicamente cuando las comunique la herramienta. Indica el siguiente paso para cualquier error o bloqueo y nunca describas como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- Has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- Has personalizado el formato de los resultados.
- Has vuelto a cargar y ejecutar la habilidad.

Este cambio acompañará al filtrado en la solicitud de la función. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
