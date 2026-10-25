---
title: "Lección 2 - Añade valoraciones con estrellas: una mejora rápida"
description: "Utiliza Copilot CLI para hacer un cambio pequeño en las tarjetas de juegos, revísalo en el navegador reenviado y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación; ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal aún no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- Iniciarás una conversación centrada en una rama de funcionalidad.
- Pedirás al agente que haga un pequeño cambio en el proyecto.
- Revisarás el cambio con `/diff`.
- Ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado.
- Abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git en la que se encuentra el terminal; no crea un árbol de trabajo independiente. Las conversaciones guardadas permiten volver al mismo diálogo, mientras los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tus mensajes y las respuestas del agente, su actividad con herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este mensaje:

   ```plaintext
   Muestra en las tarjetas de la lista de juegos la valoración starRating sobre 5. Si la valoración es null, muestra "No rating yet". Conserva el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una función con Copilot CLI.

## Revisa la diferencia

Hay que revisar todos los cambios generados por IA antes de combinarlos, incluso los pequeños.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico, no determinista. El código exacto puede variar. Revisa el comportamiento en vez de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y suponer que funciona. Pide a Copilot que inicie el sitio para inspeccionar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de las valoraciones con estrellas. Indícame la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora abre una solicitud de incorporación de cambios para incorporar el código al proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones con estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la solicitud y mostrará su URL.
3. Abre la solicitud manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando todo esté listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- Has iniciado una conversación centrada en una rama de funcionalidad.
- Has pedido al agente un pequeño cambio en las tarjetas de juegos.
- Has revisado el cambio con `/diff`.
- Has ejecutado la aplicación y comprobado la valoración en un navegador reenviado.
- Has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
