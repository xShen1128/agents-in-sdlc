---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y utiliza el navegador para explorar la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya se ha explicado, programar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos y proporcionar a Copilot automatizaciones adicionales. Para eso sirven los servidores MCP: amplían las herramientas y los servicios disponibles en Copilot CLI.

En esta lección:

- Aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- Añadirás el servidor MCP de Playwright si aún no está disponible.
- Pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot utilice el sitio como lo haría un usuario para automatizar mejor la validación y aumentar la confianza en que los cambios funcionan según lo previsto.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada y realicen acciones en tu nombre.

Los agentes acceden a las herramientas y los recursos a través de servidores MCP, que actúan como puente con las herramientas y servicios externos. Cada servidor proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios de GitHub, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si Copilot CLI todavía no tiene disponible el servidor MCP de Playwright, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor sea **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. En el cuadro de diálogo **Command**, pega:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la función con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora pide a Copilot que repita la comprobación con el servidor MCP de Playwright.

1. Envía este mensaje para pedirle que valide la función con Playwright MCP:

   ```plaintext
   Inicia la aplicación y utiliza Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Indícame qué funciona y qué no, sin realizar cambios. Cuando termines, detén el servidor que hayas iniciado.
   ```

> [!NOTE]
> No hace falta indicar expresamente que utilice MCP: Copilot suele detectarlo por sí solo. Aun así, especificarlo puede ayudarle a elegir la herramienta adecuada y obtener resultados más coherentes.

2. Observa los pasos que realiza Copilot en el navegador para comprobar que la función trabaja correctamente.
3. Lee el informe y comprueba que todo se comporta según lo previsto.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has utilizado el servidor MCP de Playwright para explorar la función en un navegador real desde Copilot CLI. En esta lección:

- Has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- Has añadido el servidor MCP de Playwright si aún no estaba disponible.
- Has pedido al agente que controle un navegador y explore el filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combine la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
