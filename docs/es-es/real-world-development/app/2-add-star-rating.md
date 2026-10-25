---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de juegos y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has explorado el espacio de trabajo y utilizado un chat rápido. Ahora iniciarás una **sesión de agente** y realizarás el primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- Iniciarás una sesión de agente y aprenderás cómo se estructura.
- Pedirás al agente un cambio pequeño y específico.
- Revisarás el cambio en la vista de diferencias del espacio de trabajo.
- Ejecutarás la aplicación localmente para comprobar el cambio en el navegador.
- Abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión dispone de un **árbol de trabajo y una rama de Git propios**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una función y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen agrupadas por repositorio en la barra lateral; selecciona una para cambiar a ella.

En una sesión verás la **conversación** con el agente, su **actividad con herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Inicia una sesión y solicita el cambio

Inicia una sesión nueva para explorar el proyecto e implementar la función. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Crearás una sesión para ese repositorio y solicitarás el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Debajo del cuadro de mensaje, elige un **new working tree** y el modo **Interactive**. Envía este mensaje:

    ```plaintext
    Muestra en las tarjetas de la lista de juegos la valoración starRating sobre 5. Si la valoración es null, muestra "No rating yet". Conserva el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el mensaje a Copilot.

La aplicación Copilot empezará por crear un árbol de trabajo nuevo, es decir, una copia aislada del proyecto. Después explorará el proyecto, localizará los archivos que debe actualizar y creará el código necesario. Ya has añadido una función con la aplicación Copilot.

## Revisa la diferencia

Hay que revisar todos los cambios generados por IA antes de combinarlos, incluso los pequeños. Revisa los cambios en la aplicación Copilot:

1. En la esquina superior derecha, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, situado a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal para mostrar los detalles de los juegos. Debería parecerse a este bloque, que muestra la valoración cuando existe y el texto `No rating yet` cuando `starRating` es `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como Copilot, al igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. Debería ser relativamente parecido.

## Comprueba los cambios

Revisa los resultados de las comprobaciones automatizadas del agente antes de abrir el navegador. Confirma que las pruebas cubren una valoración numérica `starRating` y el caso alternativo `null`. Un requisito previo ausente o una comprobación omitida no cuentan como superados; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio web y examina la interfaz actualizada en un lienzo de navegador.

> [!TIP]
> Un lienzo es un widget interactivo integrado en la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero ahora utilizarás el lienzo de navegador integrado.

1. Envía este mensaje para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Inicia la aplicación y ábrela en el lienzo del navegador.
    ```

2. Al cabo de unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de los juegos valorados muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión:

    ```plaintext
    Detén el servidor de desarrollo y cierra el lienzo del navegador.
    ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora abre una solicitud de incorporación de cambios (PR) para incorporar el código al proyecto.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las indicaciones para autenticarte.
3. Si Copilot indica que la directiva del repositorio exige una incidencia relacionada antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona el globo **PR** situado justo encima del chat para abrirla en el panel de revisión. Puedes revisarla allí.
6. Cuando esté lista, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para combinar la solicitud.

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- Has iniciado una sesión de agente y aprendido cómo se estructuran las sesiones.
- Has pedido al agente un cambio pequeño y específico en las tarjetas de juegos.
- Has revisado el cambio en la vista de diferencias del espacio de trabajo.
- Has ejecutado la aplicación localmente para comprobar las valoraciones en el navegador.
- Has abierto la PR 1, revisado sus comprobaciones y autorizado explícitamente la combinación.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instala-y-configura-la-aplicación-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
