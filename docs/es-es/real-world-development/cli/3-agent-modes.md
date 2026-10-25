---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una función pequeña al proyecto, pero los cambios mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de la organización y ayudar a crear lo adecuado de la manera correcta. En esta serie de lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia, generarás una función, comprobarás el código, validarás el comportamiento y, finalmente, lo combinarás en el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama durante todo el flujo de trabajo de esta función. Normalmente se utilizarían ramas o solicitudes distintas para diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- Iniciarás una conversación de Copilot desde una incidencia de GitHub.
- Definirás los requisitos en el modo Plan.
- Implementarás la función con el modo Autopilot.
- Revisarás el código.
- Validarás manualmente la función en un navegador reenviado.

Mientras continúas con esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás la validación MCP, crearás un agente de control de calidad y abrirás la solicitud de incorporación de cambios.

## Escenario

El catálogo de Tailspin Toys crece y las personas que lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero hay que acordar detalles, como la combinación de categorías, antes de programarla. Utilizarás el modo Plan para resolver estas decisiones y después autorizarás una implementación de alcance definido con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los principios básicos; de hecho, los hace más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Partir de una incidencia registrada que describe el trabajo.
2. Planificar lo que se va a crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validarlo.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios.
7. Combinar el código cuando se haya revisado y se haya completado correctamente la integración continua.

> [!NOTE]
> Los detalles dependen del equipo y la organización, pero la mayoría de los procesos se parece a este.

Seguir este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente colaboráis. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, a continuación, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de iniciar el trabajo de filtrado, vuelve al codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la solicitud de incorporación de cambios de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para seleccionar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje comienza ahora con `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planifica el filtrado

La planificación permite acordar cómo se implementará una función antes de pedir a Copilot que trabaje en ella. Es recomendable dedicar tiempo a planificar las tareas complejas.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Crea un plan para implementar esta función.
   ```

Copilot explorará el proyecto y determinará el enfoque adecuado.

3. Puede que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique las partes que no reflejen tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, deja que Copilot lo implemente con Autopilot. El modo permite que itere sobre el problema hasta considerar que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador de modo muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede comenzar inmediatamente; revísalo antes. Si Copilot detecta dependencias ausentes o un conflicto de puerto, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado, el código debe revisarse antes de combinarlo, como cualquier otro cambio. Revisa el código y ejecuta el sitio para confirmar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones acordadas durante la planificación.
4. Cuando termines de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explora la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la función de filtrado en el navegador. Indícame la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría y editorial y las combinaciones acordadas en el plan.
4. Confirma que el reinicio de los filtros y los resultados vacíos coincidan con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección:

- Has iniciado una conversación de Copilot desde una incidencia de GitHub.
- Has definido los requisitos en el modo Plan.
- Has implementado la función con el modo Autopilot.
- Has revisado el código.
- Has validado manualmente la función en un navegador reenviado.

A continuación, [profundizarás en la generación de código y te asegurarás de que siga las prácticas documentadas mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
