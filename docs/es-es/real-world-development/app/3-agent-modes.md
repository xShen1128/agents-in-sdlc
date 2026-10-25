---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una función pequeña al proyecto, pero los cambios mayores requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y ayudar a crear lo adecuado de la manera correcta. En estas lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para generar una función, comprobarás que el código es válido y que la función se comporta como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la función. Normalmente se usarían sesiones o PR distintas para diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- Iniciarás una sesión de agente desde una incidencia de GitHub.
- Definirás los requisitos en modo **Plan**.
- Implementarás la función con **Autopilot**.
- Revisarás el código.
- Validarás manualmente la función en un lienzo de navegador.

Mientras avanzas con esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación MCP, crearás un agente QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys crece y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero hay que acordar detalles, como combinar categorías, antes de empezar a programar. Utilizarás Plan para resolver estas decisiones y después autorizarás una implementación con un alcance definido mediante Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los principios básicos; de hecho, los hace más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Abrir una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se va a desarrollar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validarlo.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de revisarlo y de que se supere correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles dependen del equipo y la organización, pero la mayoría de los procesos se parece a este.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina el nivel de autonomía del agente. Puedes configurarlo en el menú desplegable debajo del campo del mensaje y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente colaboráis. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que ejecute los pasos.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de valoraciones por estrellas y que `main` está actualizado localmente.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Esta incidencia ya existe en la plantilla; no hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal pero no en la sesión, comprueba `PATH` o la configuración del gestor de versiones de la aplicación y, después, ciérrala por completo y vuelve a abrirla. Repite la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Implementa esta función.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando no hay juegos coincidentes.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba cuál es el modo activo en lugar de dar por hecho que la aprobación lo ha cambiado. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la función. Verás cómo sigue el plan, genera código e incluso ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén únicamente los servidores que hayas iniciado.

## Revisa y verifica la implementación

El código generado debe revisarse antes de combinarlo, igual que cualquier otro cambio. Revisa el código y ejecuta el sitio para confirmar que todo funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y sus pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que se respetan las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Envía este mensaje para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Inicia la aplicación y ábrela en el lienzo del navegador.
    ```

2. Al cabo de unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Quita el filtro y selecciona una editorial; confirma que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial, y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Quita o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión:

    ```plaintext
    Detén el servidor de desarrollo y cierra el lienzo del navegador.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, este árbol de trabajo y esta rama para las lecciones 4 a 8. En la siguiente aplicarás instrucciones personalizadas al código de filtrado; después añadirás las mejoras de la habilidad y del agente QA antes de crear y combinar la PR conjunta en la lección 8. Después de detener el servidor de desarrollo, puedes dejar la sesión inactiva y volver a ella en la próxima lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para crear y revisar una función. En esta lección:

- Has iniciado una sesión de agente desde una incidencia de GitHub.
- Has definido los requisitos en modo **Plan**.
- Has implementado la función con **Autopilot**.
- Has revisado el código.
- Has validado manualmente la función en un lienzo de navegador.

A continuación, profundizarás en la generación de código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
