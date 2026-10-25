---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, conoce el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo por tu cuenta, y automatizas tareas repetitivas. Con Node.js instalado y la copia del proyecto lista, instala la aplicación y conecta el repositorio.

En esta lección:

- Instalarás la aplicación GitHub Copilot e iniciarás sesión.
- Añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- Te familiarizarás con el espacio de trabajo y con el backlog que creó la plantilla.
- Probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

La aplicación está disponible para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio Tailspin Toys.

1. En el navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de esa página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce tu nombre de host `*.ghe.com` cuando se solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración se te ofrece seleccionar un repositorio, elige el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir ese paso y añadirlo después.
6. Cuando se te solicite, elige un tema y termina la configuración inicial.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no hace falta volver a añadirlo.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión nueva y, después, selecciona **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En esta disposición, las carpetas de repositorio, como `tailspin-toys`, se agrupan bajo **Sessions**, con las sesiones individuales debajo.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza sus secciones en la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: muestra las solicitudes mediante la integración nativa con GitHub. Puedes examinarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias del mismo modo. Puedes examinarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto u otras tareas repetitivas. El resumen del taller las presenta como un paso siguiente, no como otro ejercicio.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, complementos, habilidades y otros componentes. Lo utilizarás para configurar Playwright MCP.
- **Chats**: contiene conversaciones ligeras para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo. Esta sección aparece tras iniciar tu primer chat. Si no la ves, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa carpetas de repositorio y sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista o una sesión debajo para volver a esa conversación. Cada sesión tiene un espacio de trabajo aislado, por lo que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás tu primera sesión cuando añadas las valoraciones por estrellas.

Explorarás el espacio de trabajo a lo largo del taller.

> [!TIP]
> Si tienes dudas sobre cómo hacer algo o sobre si una función está disponible, pregunta a Copilot. Te ayudará a orientarte.

### Localiza el backlog inicial

Tailspin Toys también tiene un backlog. Explora las incidencias creadas cuando generaste el repositorio a partir de la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por título, sin dar por hecho sus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que hayas añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es preguntarle por su propio funcionamiento. Un **chat rápido** permite hacer preguntas o plantear ideas sin crear una rama ni un árbol de trabajo: es ideal para consultas breves y desechables, sin necesidad de iniciar una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si aún no aparece **Chats**, selecciona **+** junto a **Sessions** para iniciar tu primer chat; **Chats** aparecerá después.
2. Pregunta a la aplicación cómo funcionan sus propias sesiones:

   ```plaintext
   ¿Cómo utiliza árboles de trabajo la aplicación GitHub Copilot?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra en cualquier momento.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- Instalar la aplicación e iniciar sesión en GitHub.
- Añadir un proyecto desde su repositorio de GitHub.
- Familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- Utilizar un chat rápido para hacer una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[download-app]: https://gh.io/app
