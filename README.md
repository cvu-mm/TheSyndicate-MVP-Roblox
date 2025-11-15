# TheSyndicate-MVP-Roblox
MVP de un RPG de Facciones en Roblox (Lua) diseñado para demostrar los principios de Modularidad, Persistencia de Datos, Lógica Condicional y Comunicación Cliente-Servidor. Este proyecto es una experiencia educativa y autoexplicativa sobre la arquitectura técnica de un videojuego.
#  THE SYNDICATE: RISE TO POWER - DEMOSTRACIÓN DE ARQUITECTURA TÉCNICA

##  Objetivo del Proyecto

Este proyecto fue desarrollado en Roblox Studio (Lua) con el fin de crear una experiencia inmersiva y autoexplicativa que demuestre la aplicación práctica de los conceptos fundamentales de programación vistos en clase: **Modularidad**, **Persistencia de Datos**, **Lógica Condicional (If/Then)** y **Comunicación Cliente-Servidor**.

El mundo virtual simula el Producto Mínimo Viable (MVP) de un RPG de facciones criminales, donde el progreso del jugador está directamente ligado al cumplimiento de estos principios técnicos.

##  Acceso al Mundo Virtual

* **Plataforma:** Roblox Studio (Lenguaje de Programación: Lua)
* **Enlace de Juego: https://www.roblox.com/es/games/73959544913988/cvu-12vals-Place-11152025-3

##  Guía de Pruebas y Conceptos

Para verificar la funcionalidad de todos los conceptos técnicos, el usuario debe seguir el siguiente ciclo de jugabilidad:

1.  **Misión (RemoteEvent):** Interactuar con el **Jefe de Misiones**. Esta acción inicia la comunicación unidireccional (FireServer) entre el **Cliente** y el **Servidor**.
2.  **Progresión (Lógica Condicional):** Repetir la misión hasta que la **Reputación** alcance el umbral de 100. Esto valida la sentencia **IF/THEN** en el servidor, que solo permite el ascenso al rango de **"Matón"** si la condición se cumple.
3.  **Compra y Persistencia:** Visitar al **Vendedor NPC** y adquirir la "Pistola Inicial". Esta es una transacción bidireccional segura (**RemoteFunction**) que prueba el control del **Inventario persistente**.
4.  **Verificación Final:** Salir y volver a entrar al juego. El **Rango**, el **Cash** restante y el ítem comprado deben cargarse sin pérdida, demostrando el éxito de la **Persistencia de Datos**.

---

## a) Representación Visual (Creatividad y Diseño)

La fase creativa se enfocó en traducir conceptos abstractos en elementos tangibles dentro del entorno de juego.

**Metáfora Central:** El **Escondite** es la representación física de nuestro **Servidor (`ServerScriptService`)**. Es una zona segura y controlada donde reside la lógica, mientras que la periferia (el mapa exterior) es la zona del cliente.

**Cómo se Representó Cada Concepto:**

* **Persistencia de Datos:** Se visualiza en el **HUD (Heads-Up Display)**. El **Cash** y el **Rango** son los datos almacenados en la "Caja Fuerte" del juego (`DataStoreManager`). El hecho de que persistan demuestra la función del `DataStoreService`.
* **Lógica Condicional (IF/THEN):** La **Jerarquía de Rangos** (Recluta $\rightarrow$ Matón) representa el flujo de control. El código solo avanza al siguiente nivel si la **Reputación** es **`>=`** al umbral de 100. Los rótulos conceptuales en el entorno refuerzan esta regla.
* **Modularidad:** Se representó mediante la **separación funcional** de los NPCs. El Jefe de Misiones (responsable de `MissionSystem`) y el Vendedor (responsable de `PlayerService` y `Inventario`) están en puntos distintos, reflejando cómo cada módulo tiene una responsabilidad única.

**Criterios y Justificación de Diseño:**

1.  **Estética y Paleta de Colores:** Se eligió un diseño de **"Ciudad Oscura (Estilo Urbano/Mafia)"**. La paleta de negros, grises y luces de neón rojas cumple un propósito funcional: dirigir el foco del jugador. La oscuridad contrasta con las **fuentes de luz** que destacan los **NPCs** y los **rótulos conceptuales**, haciendo la experiencia **autoexplicativa** y eliminando distracciones.
2.  **Disposición Espacial:** La lógica de negocio está confinada a la **Zona Segura (El Escondite)**. Esta decisión refuerza el concepto de que la **lógica del Servidor** está aislada y protegida, mientras que el jugador interactúa con la periferia del sistema.

---

## b) Proceso de Desarrollo

El mundo se construyó en **Roblox Studio** empleando el lenguaje **Lua** bajo una arquitectura de **Servicios Modulares**.

**Arquitectura Implementada:** Toda la lógica del servidor reside en una carpeta **`SyndicateServices`** dentro de `ServerScriptService`. Esta elección fue clave para la **Modularidad** y la **Seguridad**. La comunicación entre Cliente y Servidor se canaliza a través de la carpeta **`Remote`** en `ReplicatedStorage`.

**Desafíos Encontrados y Soluciones Técnicas (Debugging):**

1.  **Desafío: Falla de Acceso a Módulos (`require`):** Al inicio, los scripts de servicio fallaban al cargarse mutuamente, generando errores de ruta.
    * **Solución:** Se corrigió la ruta de acceso a **`require(script.Parent.NombreDelModulo)`**. Esto demostró la comprensión de la jerarquía de directorios en Lua, donde los archivos hermanos deben ser referenciados a través de su padre.

2.  **Desafío: Violación de Seguridad (`FireServer`):** Los scripts de interacción de los NPCs generaban el error **`FireServer/InvokeServer can only be called from the client`**.
    * **Causa:** Los scripts estaban configurados incorrectamente como **Scripts de Servidor**, lo cual es una violación de las reglas de comunicación de red.
    * **Solución:** Los scripts fueron eliminados y reemplazados por **LocalScripts**. Esto permitió que la solicitud fuera iniciada por el lado del jugador (Cliente), cumpliendo con el protocolo de seguridad.

3.  **Desafío: Fallo de Interacción Física (`ProximityPrompt`):** Después de las correcciones de código, la interacción con los NPCs seguía sin aparecer.
    * **Solución:** El problema se diagnosticó como un fallo físico. Se forzó la estabilidad de las Partes de los NPCs al marcar las propiedades **`Anchored` (Anclado)** y **`CanCollide` (Puede Colisionar)**. Esto resolvió el fallo del motor de física al asegurar que los objetos de interacción fueran estables y detectables.

**Timeline del Proyecto:** El proyecto siguió una línea de tiempo estructurada: primero, la creación de la arquitectura modular (Días 1-2); luego, la implementación de la lógica y los Remotes (Días 3-5); y finalmente, la depuración intensiva de los fallos de comunicación y física (Días 6-7).

---

## c) Investigación del Reto ASCII

El desafío fue analizar la viabilidad técnica de dibujar arte ASCII en la Consola (CMD) desde el mundo virtual.

**Análisis de Viabilidad Técnica:**

* **Lógicamente Posible:** **Sí.** El arte ASCII es simplemente una *string* de texto estructurada con caracteres especiales. Lua tiene plena capacidad para manejar y almacenar estas *strings*.
* **Viabilidad de Acceso a CMD Externo:** **No es viable.**
    * **Limitación Crítica:** **Seguridad del Sandbox de Roblox.** El código Lua de Roblox no tiene permisos de acceso al sistema operativo subyacente del usuario (Terminal, CMD). Esto es una medida de seguridad fundamental.
    * **Conclusión:** El reto se debe cumplir imprimiendo en el **Output Log Interno de Roblox**.

**Métodos y Herramientas Propuestas para la Implementación Segura:**

1.  **Método de Impresión Directa (Output Log):**
    * **Herramienta:** Función nativa **`print()`** de Lua.
    * **Funcionamiento:** Almacenar el arte ASCII (ej., el logo de la facción) como una variable *string* multilínea. Al llamar a **`print(arte_ascii)`**, el arte se dibuja en la ventana de Salida de Roblox Studio, demostrando el control sobre *strings* complejas.

2.  **Método de Simulación en UI:**
    * **Herramienta:** Elemento de UI (`TextBox` o `TextLabel`) y fuente **monoespaciada**.
    * **Funcionamiento:** Simular una consola dentro del juego. Es crucial usar una fuente **monoespaciada** (`SourceSansMono`) para que la alineación del arte ASCII se mantenga correcta.

**Conclusiones:** El reto fue resuelto a nivel conceptual y de implementación segura. Aunque se respetó la limitación de no acceder al CMD externo, la capacidad de manejar y mostrar datos complejos de tipo *string* en la consola interna fue demostrada con éxito.

---

## d) Control de Versiones

El repositorio se gestionó profesionalmente utilizando Git, asegurando un historial claro que refleja la evolución del proyecto.

**Estructura del Repositorio:** La estructura de carpetas (`SyndicateServices`, `Remote`) está organizada para reflejar la modularidad implementada.

**Convención de Commits Utilizada:**

Se utilizó la convención de *commits* semánticos para facilitar la auditoría de código y el seguimiento de las diferentes fases de desarrollo y depuración.

* **`feat:` (Feature):** Implementación de una nueva funcionalidad.
    * *Ejemplo:* `feat: Implementar arquitectura de DataStore y Persistencia.`
* **`fix:` (Fix):** Corrección de un error o *bug* importante.
    * *Ejemplo:* `fix: Corregir error FireServer cambiando script de NPC a LocalScript.`
* **`refactor:` (Refactor):** Mejoras en la estructura o código.
    * *Ejemplo:* `refactor: Optimizar lógica de ascensos en PlayerService.`
* **`docs:` (Documentation):** Cambios o adiciones a la documentación.

**Historial de Commits:** El historial demuestra claramente las fases de la arquitectura (implementación de módulos), la resolución de los fallos de comunicación (`FireServer`), y la solución final de los problemas de interacción física (`Anchored`), cumpliendo con la transparencia y el profesionalismo requeridos.
