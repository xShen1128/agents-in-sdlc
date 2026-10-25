---
title: "Ejercicio 3 - Prueba la función con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador real para probar la función. Después publicarás la rama y abrirás la solicitud.

En este ejercicio:

- Aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- Añadirás el servidor MCP de Playwright al espacio de trabajo.
- Pedirás a Copilot que lo utilice para probar manualmente la función de filtrado en un navegador.
- Publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a herramientas externas al editor. Model Context Protocol (MCP) es el estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama de los componentes del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Los agentes acceden a las herramientas y los recursos a través de servidores MCP, que actúan como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación con las herramientas externas, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP proporciona al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: ofrece acceso a API para gestionar repositorios de GitHub y permite realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar navegadores mediante Playwright, por ejemplo, navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Utiliza solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías encontrar ya configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`; por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalarlo localmente: VS Code se conecta por HTTP y la primera vez que Copilot usa una herramienta del servidor te autenticas con GitHub.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright ejecute el navegador sin una ventana visible. Esto es necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para sus pruebas de extremo a extremo, por lo que el navegador necesario suele estar instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y marca como de confianza el servidor

VS Code inicia los servidores MCP **cuando hacen falta**: la primera vez que Copilot necesita sus herramientas, VS Code los inicia y te pide que confirmes que confías en ellos.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

El inicio manual es opcional. Si no lo haces, VS Code iniciará el servidor y te pedirá que confirmes si confías en él la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Prueba la función de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de navegar por la aplicación para comprobar el trabajo, el agente puede abrir una página, desplazarse, aplicar filtros y leerte el resultado; después resumirá lo que ha observado. Es una forma rápida de comprobar el comportamiento esperado sin salir de la conversación.

El servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. Así, el agente razona a partir de elementos estructurados y etiquetados (botones, enlaces, elementos de lista), como hace la tecnología de asistencia. Por tanto, una comprobación funcional rápida también permite realizar una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la función de filtrado:

```text
Utiliza el servidor MCP de Playwright para abrir un navegador con la aplicación en ejecución en http://localhost:4321 y verificar la nueva función de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Quita el filtro y aplica uno de editorial; confirma que la lista se actualiza para mostrar los juegos de esa editorial.
4. Combina un filtro de categoría y uno de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica si algo no funciona según lo previsto.
```

Copilot abrirá el navegador mediante el servidor MCP de Playwright, seguirá los pasos y comunicará los resultados. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas o pide que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el mensaje. La primera vez que Copilot utiliza el servidor MCP de Playwright quizá tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y que vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Después de confirmar que la función funciona, puedes abrir una solicitud de incorporación de cambios para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, desde github.com o mediante GitHub CLI. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede encontrar la incidencia correspondiente y asociarla a la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. En el menú desplegable de agentes, confirma que **Agent** esté seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree la solicitud:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y asígnala a la incidencia correcta.
   ```

4. Si hace falta, selecciona **Continue** para permitir que Copilot recabe información y realice las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, quizá tengas que autenticarte; sigue las instrucciones para autorizarlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correspondiente y crea la solicitud.
6. Selecciona el enlace que genere Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- Has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- Has añadido el servidor MCP de Playwright al espacio de trabajo.
- Lo has utilizado para probar manualmente la función de filtrado en un navegador antes de publicarla.
- Has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una función, pero MCP es solo una manera de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
