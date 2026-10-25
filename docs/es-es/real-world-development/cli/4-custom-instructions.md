---
title: "Lección 4 - Orienta a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- Explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- Actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- Observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys cuenta con pautas y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben actualizarse cuando cambie el código relacionado.
- Se deben mantener las pautas existentes de formato y lint.

Los archivos de instrucciones proporcionan a Copilot el contexto necesario para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que pueda entender mejor el estilo de programación y los requisitos del equipo. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones, bibliotecas y comentarios preferidos, tanto para todo el repositorio como para tipos de archivo concretos.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio y contiene información general del proyecto.
- `.github/instructions/*.instructions.md` contiene pautas para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de modificarlos para poder observar el efecto de los cambios.

1. Vuelve al codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora la descripción del proyecto y las pautas de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y revisa los archivos, que incluyen instrucciones para Astro, la capa de datos Drizzle, pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob para determinar a qué archivos se aplican estas instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos del proyecto. Permiten dividir instrucciones extensas en archivos reutilizables y enlazar ejemplos.

## Actualiza las instrucciones para reflejar las pautas del equipo

Los archivos existentes son un buen comienzo, pero aún falta documentar el uso de comentarios TSDoc en TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Utiliza las pautas actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot cargue las nuevas pautas sin perder el contexto de la función.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación:

   ```bash
   copilot --resume
   ```

   Si se te pregunta, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las pautas de código. ¿Puedes actualizar el código generado para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc describen correctamente el código.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la función. En concreto:

- Has explorado cómo llegan al agente las instrucciones del repositorio y los archivos aplicables a rutas concretas.
- Has actualizado el archivo de instrucciones para aplicar los estándares de programación.
- Has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar de forma coherente el lint y las pruebas.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
