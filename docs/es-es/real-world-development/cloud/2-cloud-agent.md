---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan tiempo en el backlog porque nunca ha habido tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así se pueden delegar tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en objetivos de mayor alcance.

Con el agente de Copilot en la nube aprenderás a:

- Personalizar el entorno para generar código.
- Asegurarte de que las operaciones se realicen de forma segura.
- Comprender la importancia de definir incidencias con un alcance claro.
- Asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente; de hecho, comprobarás que prácticamente no existe. El primer paso será añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que las personas puedan seguir explorando. Por ahora no necesita una implementación refinada, sino algo que pueda utilizar para probar la experiencia de usuario. Este trabajo es actualmente un impedimento, pero hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son buenas candidatas para asignarlas al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisa el trabajo para confirmar que todo esté correcto.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Al igual que con una persona, puedes colaborar con él de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea un borrador de solicitud de incorporación de cambios para hacer seguimiento del progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud.

## La importancia de definir bien el alcance

GitHub Copilot no es magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas al analizarlas en detalle, así que [define con cuidado el alcance de las tareas del agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los principios básicos del desarrollo de software no cambian con la llegada de la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores se han presentado las instrucciones personalizadas y su función para orientar a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echar un vistazo. Antes de asignar trabajo al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones incluidos en este repositorio para que puedas observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace, si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario de la solicitud generada. Estos elementos proceden de los archivos de instrucciones y volverán a aparecer al revisar solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Para crear código suele hacer falta un entorno específico y ejecutar algunos scripts de configuración para que todo esté listo. Lo mismo sucede al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo especial de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecutará antes de que empiece a trabajar. Así tendrá acceso a las herramientas y dependencias de desarrollo necesarias. El flujo ya se ha configurado para este taller: instala Node.js y las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y configura la base de datos SQLite migrada y con datos de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para realizar sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que se ejecuta correctamente sin tener que esperar a que Copilot lo inicie.

## Añade documentación

Aunque todos conocen la importancia de la documentación, la mayoría de los proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar sin resolver, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores al equipo. Copilot es especialmente eficaz para crear documentación, por lo que esta incidencia es adecuada para asignársela al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria y volverás a revisar el resultado en un ejercicio posterior.

1. Abre una pestaña nueva del navegador y ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización requiere que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando resulten útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación que aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un momento y actualiza la página del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la solicitud. Copilot está preparando su entorno (ejecuta el flujo de trabajo descrito anteriormente), analiza el proyecto y determina el mejor enfoque para resolver el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud: debería aparecer una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo para ver cómo resuelve Copilot el problema: explora el código y analiza su estado, se detiene a pensar y decidir el plan adecuado y, a continuación, crea el código.

Es probable que el proceso tarde varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es realizar tareas de forma asíncrona para que podamos centrarnos en otras cosas. Aprovecharemos esa función asignándole otra tarea y, después, dedicaremos nuestra atención a añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las principales ventajas del agente de Copilot en la nube es que permite repartir el trabajo: tú puedes ocuparte de unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. Asignemos esta tarea al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando otros juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

    - Añadir un ayudante de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, excluido ese juego.
    - Mostrar en la página de detalles una sección «Juegos relacionados» que utilice el ayudante.
    - Gestionar el caso en que un juego no tenga juegos relacionados.
    - Crear pruebas unitarias para el nuevo ayudante.
    - Asegurarse de que todas las pruebas se superen antes de crear la solicitud de incorporación de cambios.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, debería aparecer un emoji de ojos 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de Copilot en la nube trabaja de forma parecida a un ingeniero de software, así que no hace falta supervisarlo continuamente: basta con revisar el resultado cuando haya terminado. Ahora, crea y utiliza agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ocuparse de la deuda técnica, crear funcionalidades o ayudar a migrar código de un marco de trabajo a otro.

Has aprendido a:

- Personalizar el entorno para generar código.
- Asegurarte de que las operaciones se realicen de forma segura.
- Definir incidencias con un alcance claro.
- Asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes empezar a crear y utilizar agentes personalizados. [El agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que se han presentado en módulos anteriores.

## Recursos

- [Información sobre el agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
