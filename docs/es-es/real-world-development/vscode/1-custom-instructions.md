---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta o hay información de fondo que Copilot debería conocer, conviene asegurarse de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] son una forma importante de proporcionar a Copilot orientación para generar código.

En este ejercicio:

- Explorarás cómo llega a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas.
- Generarás la primera parte de los datos para el filtrado (un ayudante de editoriales) con las instrucciones actuales.
- Añadirás un estándar nuevo para todo el repositorio a `.github/copilot-instructions.md`.
- Repetirás el mismo mensaje y observarás cómo el código generado adopta el nuevo estándar.
- Confirmarás los cambios de instrucciones y de filtrado en `main` para que Copilot pueda utilizar las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no respetar algunos estándares definidos. Copilot no es determinista. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado, carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo seria, Tailspin Toys cuenta con pautas y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación mediante comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que pueda entender mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de programación, bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está desarrollando, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos. Puedes utilizarlos para pautas de lenguajes específicos (como TypeScript o Astro) o tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, Copilot Chat utiliza los archivos de instrucciones para generar código, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones con otros estándares, incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Describir por completo cómo crear archivos de instrucciones queda fuera del ámbito de este taller, pero los ejemplos del proyecto muestran un enfoque representativo. En general:

- Limita el contenido de `copilot-instructions.md` a pautas generales del proyecto: qué se está desarrollando, cómo se organiza el proyecto y qué estándares globales de programación se siguen.
- Usa archivos `*.instructions.md` para pautas específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Escribe las pautas en lenguaje natural y de forma clara. Incluye ejemplos de cómo debe y cómo no debe ser el código.

No hay una única manera correcta de crear archivos de instrucciones ni de utilizar IA. La experimentación te ayudará a encontrar el enfoque que mejor se adapte al proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto hay archivos para distintos tipos de tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generarlos. Cada entorno ofrece esta opción de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección de cada entorno lo indicará cuando corresponda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], que contiene archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Fíjate en la descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa también las pautas anidadas de **GitHub Actions Workflows**. Se aplican a todas tus interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`: establece un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican estas instrucciones. En este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Fíjate en los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes. Esto permite dividir instrucciones extensas en archivos más pequeños y reutilizables, y enlazar ejemplos. Las rutas se interpretan con respecto al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero aún no exige documentación en el código. En los siguientes pasos añadirás las reglas para los comentarios TSDoc y los encabezados de comentario de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

Para ver el efecto de las instrucciones personalizadas, primero envía un mensaje con el archivo de instrucciones actual. Más adelante lo actualizarás y repetirás el mismo mensaje para comparar el resultado.

1. Cierra las pestañas del editor de ejercicios anteriores para que Copilot solo reciba el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse el ayudante.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios en los archivos.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, a menudo tanto al archivo del ayudante como a sus pruebas.
6. Observa que el ayudante propuesto es una función tipada que acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, podría añadir comentarios de documentación incluso sin que se le pidan. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar nuevo para todo el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares del proyecto, pero todavía no incluye una regla de documentación en el código.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en la raíz del repositorio (`.github/copilot-instructions.md`) para pautas globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios que acabas de añadir.

## Repite el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia.
2. Vuelve a `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo mensaje**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo propuesto comienza con un bloque parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base para el siguiente ejercicio.

## Confirma los cambios de instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que cualquier otro archivo del repositorio. Confirma y publica la rama.

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que los cambios de instrucciones y del ayudante estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma el cambio de instrucciones y todos los archivos que Copilot haya modificado para crear la base del ayudante:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios complementarios (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Después de publicar la rama, crea una solicitud de incorporación de cambios y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede hacerlo mediante las herramientas de GitHub conectadas al proyecto: pídele que encuentre la incidencia, cree la solicitud para cerrarla y la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no hay que configurarlas. La primera vez que Copilot utilice una, VS Code quizá te pida iniciar sesión en GitHub. Sigue los mensajes para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una solicitud desde la rama actual. Indica que describa tanto las instrucciones actualizadas como la nueva base del ayudante de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la solicitud, pídele que la combine y que vuelva a `main` localmente con este mensaje:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot solicita permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- Enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- Añadir un estándar nuevo para todo el repositorio a `.github/copilot-instructions.md`.
- Repetir el mismo mensaje y observar cómo el código propuesto adopta el nuevo estándar.
- Confirmar los cambios de instrucciones y la base del ayudante en `main` para que el siguiente ejercicio pueda utilizarla.

A continuación, utilizarás estas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una función nueva al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
