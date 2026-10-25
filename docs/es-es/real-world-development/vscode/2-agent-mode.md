---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla de una aplicación suele requerir cambios en varios archivos y ejecutar operaciones como las pruebas. Normalmente, un desarrollador localiza todos los archivos necesarios, realiza los cambios, ejecuta las pruebas, depura los errores, averigua qué archivo falta y vuelve a modificar el código. El proceso puede alargarse.

Para eso sirve el modo de agente de Copilot.

El modo de agente trabaja de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de comentarios rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- Explorar el proyecto con el modo de agente de Copilot, identificar archivos relevantes y coordinar cambios.
- Implementar funciones en la interfaz de usuario y la capa de datos.
- Revisar los cambios y las pruebas generados antes de incorporarlos al código base.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior ya añadiste un ayudante de editoriales; ahora completarás con el modo de agente el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web Tailspin Toys

Antes de realizar cambios, explora el sitio para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite enumerar juegos y mostrar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz del lado del cliente. Los ayudantes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade mediante pequeños elementos `<script>` de Astro, con las API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te ofrece abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace local en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

La primera versión del sitio funciona, pero queremos ampliarla. Empieza por revisar el backlog, que se creó automáticamente al configurar el proyecto desde la plantilla. Pide a GitHub Copilot que muestre las incidencias.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo específico. Se utilizó Claude Sonnet 4.5 al preparar las instrucciones para que las sugerencias fueran relativamente coherentes y la experiencia resultara adecuada. Sin embargo, los modelos de lenguaje son probabilísticos, por lo que las sugerencias pueden diferir de las del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot podría utilizar un comando MCP distinto, pero debería completar la tarea.

3. Envía este mensaje a Copilot para preguntarle por el backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionarás a Copilot como contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funciones:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a archivos de instrucciones específicos de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar pautas, dividir conjuntos complejos en archivos más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, hay que realizar al menos tres cambios independientes:

- Añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento de filtrado.
- Actualizar la página con la lista de juegos para incorporar la interfaz de filtrado.

También hay que ejecutar las pruebas y confirmar que se superen antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Puedes continuar en la conversación actual o seleccionar **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** siga seleccionado en el menú desplegable de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad conforme a la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría según los requisitos de la incidencia de GitHub relacionada del backlog. Ya existe un ayudante de editoriales del ejercicio anterior: consérvalo y perféccionalo si hace falta, y completa el trabajo restante en la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en marcha, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: los ayudantes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Durante el trabajo, quizá aparezcan los controles **Keep** y **Undo** en el editor. Al terminar, habrá uno de estos controles para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión de Copilot Chat con los cambios propuestos por el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está en marcha el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has aprendido a:

- Implementar funciones en la interfaz de usuario y la capa de datos con el modo de agente.
- Explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- Revisar los cambios y las pruebas generados antes de incorporarlos al código base.

A continuación, [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que aumente la lista de juegos, será necesario añadir paginación. Utiliza lo aprendido para pedir a Copilot que actualice el sitio. Al diseñar la implementación:

- Sigue las prácticas recomendadas existentes y utiliza los archivos de instrucciones.
- Decide cómo se implementará la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- Incluye en el mensaje la orientación necesaria para lograr la implementación que quieres.
- Puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: para qué sirven y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
