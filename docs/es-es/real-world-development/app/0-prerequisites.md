---
title: "Lección 0 - Requisitos previos"
description: "Prepara las lecciones de la aplicación GitHub Copilot: instala Node.js para el proyecto Tailspin Toys y crea tu copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que sirve como centro de operaciones tanto para Copilot como para GitHub. Proporciona acceso rápido a incidencias y solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. Durante este taller trabajarás localmente con la aplicación Tailspin Toys, creada con Astro, y con la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js esté instalado localmente y, después, instala la aplicación Copilot.

En esta lección:

- Instalarás Node.js para ejecutar las pruebas del proyecto en tu equipo.
- Crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla.

## Instala Node.js

En varias lecciones se pide al agente que cree funciones y ejecute localmente las pruebas de Tailspin Toys. Para ello se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en cualquier plataforma es el instalador oficial:

1. Abre un terminal en el sistema operativo, como Windows Terminal, Terminal de macOS o la aplicación que utilices habitualmente.
2. Comprueba las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos se ejecutan correctamente y Node.js cumple los requisitos del README y de `package.json` del proyecto, puedes pasar a la sección siguiente.

> [!TIP]
> Solo tienes que seguir estos pasos si no tienes Node instalado o necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, deja seleccionada la opción **Add to PATH**.
7. Después de la instalación, abre un terminal nuevo.
8. Confirma la instalación en el terminal nuevo:

    ```bash
    node --version
    npm --version
    ```

9. Deberías ver las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles en la aplicación. Cierra la aplicación por completo y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si uno de los comandos sigue fallando en una sesión, sigue los pasos para [solucionar problemas del entorno de la sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada árbol de trabajo también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio Tailspin Toys al preparar un árbol de trabajo y revisa cualquier solicitud de instalación antes de aprobarla.

## Configura el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora desde el [repositorio de plantilla][template-repository]. El nuevo repositorio incluye todos los archivos necesarios para el taller y lo conectarás al instalar la aplicación.

1. Abre una ventana nueva del navegador y ve al repositorio de GitHub del taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea tu propia copia seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template con Create a new repository seleccionado en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el nuevo repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio rellenado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**), ya que la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante todo el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla. Incluye instrucciones del repositorio, código de la aplicación, pruebas, la habilidad `quality-checks` y una extensión de lienzo existente. Durante el taller personalizarás la habilidad y crearás un agente QA. Si utilizas una copia anterior, confirma con la persona que imparte el taller que tiene los archivos necesarios.

## Resumen y siguientes pasos

Ya tienes todo preparado. En esta lección:

- Has instalado Node.js para compilar y probar el proyecto en tu equipo.
- Has creado tu propia copia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Información sobre la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicia-una-sesión-desde-la-incidencia
