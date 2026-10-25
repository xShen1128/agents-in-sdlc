---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en el codespace, familiarízate con sus funciones y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Puedes delegar tareas, pedir cambios y mantener la concentración. El primer paso es instalar la herramienta, algo que puedes hacer con herramientas conocidas.

En esta lección:

- Instalarás GitHub Copilot CLI con npm.
- Te autenticarás con tu cuenta de GitHub.
- Confiarás en el repositorio del taller y probarás una conversación.
- Encontrarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a utilizar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores ya trabajan. En esta lección instalarás y autenticarás la herramienta para utilizarla durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, utilizarás npm.

1. Vuelve al codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Es posible que ya estés autenticado mediante tu sesión de GitHub en Codespaces. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarízate con la herramienta

Los comandos que escribas en el terminal normal se ejecutan directamente en el codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos slash controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsarlo para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Utiliza GitHub MCP para encontrar en este repositorio la incidencia titulada «Allow users to filter games by category and publisher». Indícame su URL y un breve resumen. No realices cambios.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de realizar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama activa del terminal; no crea automáticamente un árbol de trabajo. Crearás una rama antes de cada cambio.

## Utiliza el atajo del taller

Normalmente Copilot CLI pide permiso antes de utilizar herramientas que no estén incluidas en los permisos establecidos. En este taller, volverás a iniciarlo con `--yolo`, un atajo aprobado por el usuario que elimina las solicitudes de permiso dentro del codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un codespace o una máquina virtual, y no lo conviertas en el alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de GitHub de lectura y escritura que se utilizarán más adelante con incidencias y solicitudes de incorporación de cambios. Aunque el codespace limita el acceso a tu equipo local, los recursos de GitHub a los que te autentiques son reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para comprobar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, utiliza `copilot --resume` para volver a la misma conversación de la función y a la misma rama.

## Resumen y siguientes pasos

¡Enhorabuena! En esta lección:

- Has instalado GitHub Copilot CLI con npm.
- Te has autenticado con tu cuenta de GitHub.
- Has confiado en el repositorio del taller y probado una conversación.
- Has localizado la incidencia de filtrado con el servidor MCP integrado de GitHub.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar la valoración de estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
