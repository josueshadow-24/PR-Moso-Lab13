# Laboratorio N° 13: Prototipado interactivo y testing con usuarios


**Repositorio GitHub:** [https://github.com/josueshadow-24/PR-Moso-Lab13.git](https://github.com/josueshadow-24/PR-Moso-Lab13.git)


**Producto:** Portal de atención ciudadana y trámites en línea – Municipalidad Distrital de El Porvenir

**Archivo Principal PR-Moso-Lab13:** [...](...)

**Archivo Testing PR-Moso-Lab13-Testing:** [...](...)

---

## Paso 1: Preparación del archivo y definición de flujos

| N° | Flujo (objetivo del usuario) | Pantalla inicial | Pantallas involucradas | Criterio de éxito |
| :--- | :--- | :--- | :--- | :--- |
| **F1** | Consultar estado de un expediente municipal | `01-Home` | `01-Home`, `02-Busqueda`, `03-Resultados`, `04-Detalle` | Llega al detalle del expediente consultado y visualiza su estado |
| **F2** | Solicitar una Licencia de Funcionamiento | `04-Detalle` | `04-Detalle`, `05-Formulario`, `05b-Error`, `Modal-Confirmar`, `05c-Procesando`, `06-Exito` | Completa la solicitud y visualiza la constancia de registro exitoso |
| **F3** | Anular una solicitud de trámite en curso | `01-Home` | `01-Home`, `07-MisTramites`, `Modal-Cancelar`, `07b-Cancelado` | El trámite deja de figurar activo en Mis Trámites |

### Diagrama de flujo de usuario
![Diagrama de Flujo de Usuario](assets/paso1-user-flow.png)

### Inventario de pantallas y modales (`Prototipo v1`)

Se diseñaron y estandarizaron **12 frames** en la página `Prototipo v1` del archivo `PR-Moso-Lab13` utilizando instancias vinculadas al sistema de diseño (`Button`, `Input`, `Card`), prefijos numéricos sin espacios y nomenclatura homologada en capas compartidas para soportar transiciones con **Smart Animate**:

- **Flujo F1 (Consulta):** `01-Home`, `02-Busqueda`, `03-Resultados`, `04-Detalle`
- **Flujo F2 (Licencia):** `05-Formulario`, `05b-Error`, `Modal-Confirmar`, `05c-Procesando`, `06-Exito`
- **Flujo F3 (Anulación):** `07-MisTramites`, `Modal-Cancelar`, `07b-Cancelado`

![Inventario de Pantallas Prototipo v1](assets/paso1-pantallas-v1.png)

---

## Paso 2: Componentes interactivos (`Sistema`)

### 2.1 Estados hover en `Button` (`Mouse enter` / `Mouse leave`)
Se configuraron transiciones interactivas bidireccionales en las variantes `Primary`, `Secondary` y `Ghost` del component set `Button` utilizando `Mouse enter` y `Mouse leave` (evitando *While hovering* para conservar compatibilidad con eventos `On click` de navegación):

![Estados interactivos en Button](assets/paso2-button-hover.png)

### 2.2 Indicador de foco en `Input` (WCAG 2.4.7)
Se configuró la transición `On click → Change to → State=Focus` en el component set `Input` para evidenciar el anillo de foco al interactuar con los campos de entrada, cumpliendo con el criterio de accesibilidad WCAG 2.4.7:

![Estado focus en input](assets/paso2-input-focus.png)

### 2.3 Componente interactivo `Checkbox` (`State=Off` / `State=On`)
Se creó el component set `Checkbox` mediante Auto Layout y tokens de color/tipografía del sistema, conectando ambos estados de forma bidireccional con `On click → Change to` (`Smart Animate`, `Ease out`, `200 ms`) e integrándolo en `05-Formulario` para la aceptación de términos:

![Componente interactivo Checkbox](assets/paso2-checkbox.png)

### 2.4 Validación en modo presentación (`Present`)
Se verificó en un frame de prueba (`Ctrl + Alt + Enter`) que las interacciones definidas en los componentes maestros (`Button`, `Input` y `Checkbox`) se heredan automáticamente en todas las instancias sin necesidad de reconfigurarlas pantalla por pantalla:

![Validacion en Present](assets/paso2-present-prueba.png)

---

## Paso 3: Navegación, overlays y flujos (`Prototipo v1`)

### 3.1 Navegación base, dispositivo y scroll vertical
Se configuró el entorno de prototipado en `Desktop`, activando `Overflow → Vertical` y encabezado `Position: Fixed` en pantallas extensas, junto con las transiciones `On click → Navigate to` y acciones `Back` (`Smart Animate`, `300 ms`):

![Navegacion base en Prototipo v1](assets/paso3-navegacion-base.png)

### 3.2 Superposiciones y modales (`Open overlay`)
Se configuraron `Modal-Confirmar` y `Modal-Cancelar` como frames independientes invocados mediante `Open overlay` (`Center`, `Close when clicking outside`, fondo `#000000` al `40 %`, animación `Move in ↑`, `Ease out`, `250 ms`), conectando la acción de confirmación hacia `05c-Procesando` / `06-Exito`:

![Configuracion de overlays en Prototipo v1](assets/paso3-overlays.png)

### 3.3 Simulación de estado de error (`05b-Error`)
Se implementó el flujo de validación de campos obligatorios mediante el frame `05b-Error`, empleando la variante `Input (State: Error)` y mensajes de ayuda visibles (WCAG 3.3.1) antes de habilitar la apertura de `Modal-Confirmar`:

![Simulacion de estado de error en 05b-Error](assets/paso3-estado-error.png)

### 3.4 Puntos de inicio de flujo (`Flow Starting Points`)
Se definieron tres puntos de inicio independientes en `Prototipo v1` para evaluar cada recorrido de usuario por separado:
- **F1 – Buscar trámite (`01-Home`):** Búsqueda, filtrado y consulta de requisitos de trámites municipales.
- **F2 – Solicitar trámite (`04-Detalle`):** Llenado de solicitud, validación de error (`05b-Error`), confirmación modal (`Modal-Confirmar`) y constancia (`06-Exito`).
- **F3 – Cancelar trámite (`07-MisTramites`):** Gestión y cancelación de expedientes activos mediante `Modal-Cancelar`.

![Puntos de inicio de flujo en Prototipo v1](assets/paso3-flujos.png)

### 3.5 Matriz de interacciones principales (`Prototipo v1`)

| Elemento (frame) | Trigger | Acción | Destino | Animación |
| :--- | :--- | :--- | :--- | :--- |
| Botón `Buscar Expediente` (`01-Home`) | On click | Navigate to | `02-Busqueda` | Smart Animate · Ease out · 300 ms |
| Botón `Ver Requisitos` (`01-Home`) | On click | Navigate to | `03-Resultados` | Smart Animate · Ease out · 300 ms |
| Botón `Consultar ahora` (`02-Busqueda`) | On click | Navigate to | `03-Resultados` | Smart Animate · Ease out · 300 ms |
| Card `Licencia de Funcionamiento` (`03-Resultados`) | On click | Navigate to | `04-Detalle` | Smart Animate · Ease out · 300 ms |
| Enlace `← Catálogo de Trámites` (`04-Detalle`) | On click | Back | Frame anterior | Smart Animate · Ease out · 300 ms |
| Botón `Iniciar trámite` (`04-Detalle`) | On click | Navigate to | `05-Formulario` | Smart Animate · Ease out · 300 ms |
| Botón `Enviar Solicitud` vacío (`05-Formulario`) | On click | Navigate to | `05b-Error` | Smart Animate · Ease out · 200 ms |
| Botón `Enviar Solicitud` corregido (`05b-Error`) | On click | Open overlay | `Modal-Confirmar` | Move in ↑ · Ease out · 250 ms |
| Botón `Confirmar envío` (`Modal-Confirmar`) | On click | Navigate to | `05c-Procesando` | Dissolve · Ease out · 200 ms |
| Frame completo (`05c-Procesando`) | After delay (1200 ms) | Navigate to | `06-Exito` | Dissolve · Ease out · 300 ms |
| Enlace `Mesa de Partes Virtual` (`06-Exito` / Header) | On click | Navigate to | `07-MisTramites` | Dissolve · Ease out · 250 ms |
| Botón `Cancelar solicitud` (`07-MisTramites` / `05-Formulario`) | On click | Open overlay | `Modal-Cancelar` | Move in ↑ · Ease out · 250 ms |

---

Se estandarizaron las curvas y tiempos de animación (`Ease out` en navegación `250–350 ms` y `Ease in and out` en componentes `100–200 ms`). Se emparejaron las capas `expediente/title`, `expediente/badge` y `expediente/progress` entre `03-Resultados` y `04-Detalle` para generar continuidad espacial mediante `Smart Animate` (`300 ms`):

![Configuracion Smart Animate](assets/paso4-smart-animate.png)

### 4.2 Microinteracción de carga, retroalimentación de exito y accesibilidad de movimiento
- **Carga (`05c-Procesando`):** Disparador `After delay (1200 ms) → Navigate to → 06-Exito` (`Dissolve`, `300 ms`), cumpliendo la Heurística 1 de Nielsen (*visibilidad del estado del sistema*).
- **Éxito (`06-Exito`):** Animación de aparición del ícono de confirmación (`Success Badge`, escala `0.8` a `1.0` con `Smart Animate` / `Spring`, `300 ms`) y acción siguiente `Ir a mis trámites` hacia `07-MisTramites`.
- **Accesibilidad (WCAG 2.3.1 / 2.4.7):** Sin destellos (>3 Hz), transiciones `≤ 300 ms` y foco visible conservado.

![Retroalimentacion de exito en 06-Exito](assets/paso4-feedback-exito.png)

---

## Paso 5: Revisión del prototipo y prueba piloto

### 5.1 Auditoría de Hotspots, Dead Ends y dispositivo móvil
Se ejecutaron los tres flujos (`F1`, `F2`, `F3`) en modo `Present` inspeccionando *hotspots* para eliminar pantallas sin salida (*dead ends*) y se validó en dispositivo móvil real la altura mínima táctil de `44 px`, legibilidad tipográfica y funcionamiento del `Header` fijo:

![Auditoria de hotspots y revision movil](assets/paso5-hotspots-desktop.png)

![Auditoria de hotspots y revision movil](assets/paso5-prueba-movil.jpeg)

### 5.2 Enlace limpio sin sesgos y versión base (`v1-prueba`)
Se configuró el enlace de presentación con permisos `Anyone with the link – can view`, desactivando *Show hotspot hints on click* y comentarios visibles para garantizar pruebas sin sesgos. Asimismo, se registró la línea base **`v1-prueba`** en el historial de versiones de Figma:

![Version v1-prueba y enlace sin sesgos](assets/paso5-version-v1-prueba.png)

### 5.3 Prueba piloto y calibración de escenarios
Se ejecutó una sesión piloto de **22 minutos** (objetivo: 20–25 min) con un compañero utilizando `v1-prueba`. Los datos cuantitativos del piloto fueron excluidos de los resultados finales y permitieron refinar la redacción de los escenarios de búsqueda (`F1`) y validación de error (`F2`) antes de iniciar las sesiones formales del Paso 6.

---
## Paso 6: Plan de la prueba de usabilidad (`Testing`)

Se estructuró el plan de evaluación en la página `Testing` de Figma bajo protocolo moderado con *Think Aloud* para 5 participantes anonimizados (`P1`–`P5`, Ley N.° 29733), definiendo roles (Moderador, Observador–anotador, Cronometrista), escenarios sin términos de interfaz (`T1 ≤ 90s`, `T2 ≤ 120s`, `T3 ≤ 60s`), consentimiento informado y guion de 5 fases con cuestionario SUS:

![Plan de prueba de usabilidad en pagina Testing](assets/paso6-plan-testing-1.png)

![Plan de prueba de usabilidad en pagina Testing](assets/paso6-plan-testing-2.png)

---

## Paso 7: Ejecución de la Prueba y Registro de Métricas

### 7.1 Resumen de Desempeño por Tarea (`n = 5`)

| Tarea | Tasa de éxito (%) | Tiempo promedio (s) | Errores promedio | Observaciones clave |
| :--- | :---: | :---: | :---: | :--- |
| **T1** | **90 %** (4.5 / 5) | **58.0 s** | **0.4** | Ruta fluida desde `01-Home`; 1 de 5 confundió inicialmente la pestaña *Trámites* con *Consulta Expediente*. |
| **T2** | **70 %** (3.5 / 5) | **108.0 s** | **1.4** | 3 de 5 no identificaron de inmediato el campo obligatorio omitido al saltar `05b-Error`. |
| **T3** | **90 %** (4.5 / 5) | **45.0 s** | **0.2** | Flujo claro mediante `Modal-Cancelar`; 1 de 5 tardó en ubicar el acceso a `07-MisTramites`. |

### 7.2 Evaluación de Satisfacción SUS (*System Usability Scale*)
- **Puntajes individuales:** `P1 = 77.5`, `P2 = 92.5`, `P3 = 67.5`, `P4 = 100.0`, `P5 = 77.5`.
- **Puntaje SUS Promedio:** **83.0 / 100** (superior al benchmark de 68 y al umbral de excelencia de 80).

![Registro de metricas y calculo SUS](assets/paso7-metricas-sus-3.png)

---
## Paso 8: Análisis de hallazgos, priorización e iteración (`v2-iteracion`)

### 8.1 Affinity Mapping y Matriz Impacto / Esfuerzo ($2 \times 2$)
- **Fallo sistémico identificado ($\ge 3 / 5$ participantes):** Dificultad para localizar el campo obligatorio vacío (**Nombre Comercial del Establecimiento**) en `05b-Error` y ambigüedad previa sobre el formato esperado antes de enviar el formulario (`P1`, `P3`, `P5`).
- **Quick Wins priorizados (Alto Impacto / Bajo Esfuerzo):**
  1. Propagación de fondo tintado `#FEF2F2` y borde `2 px #DC2626` en el componente maestro `Input / Error` (`WCAG 3.3.1`).
  2. Incorporación de *helper text* preventivo en `05-Formulario` y enlace directo al campo con error en `05b-Error`.
  3. Diferenciación semántica en el componente maestro `Header` (`Catálogo de Trámites` vs. `Consultar Expediente`) y jerarquía de botón primario en `06-Exito` (`Ir a mis trámites`).

![Affinity Mapping y Matriz Impacto Esfuerzo](assets/paso8-affinity-matriz-1.png)

![Affinity Mapping y Matriz Impacto Esfuerzo](assets/paso8-affinity-matriz-2.png)

### 8.2 Tabla consolidada de hallazgos y severidad de Nielsen

| N° | Problema observado | Tarea | Usuarios ($x / 5$) | Heurística relacionada | Severidad (0–4) | Mejora propuesta |
| :-: | :--- | :-: | :-: | :--- | :-: | :--- |
| **1** | No identifican de inmediato que el campo obligatorio **Nombre Comercial del Establecimiento** tiene error en `05b-Error`; el fondo sigue siendo blanco. | **T2** | **3 / 5** *(Fallo sistémico)* | **H9:** Ayudar a reconocer, diagnosticar y recuperarse de errores (`WCAG 3.3.1`) | **3** *(Mayor – Alta)* | Aplicar fondo tintado `#FEF2F2`, borde `2 px #DC2626` y mensaje explicativo bajo el campo Nombre Comercial en `Input / Error`. |
| **2** | Falta de guía preventiva en `05-Formulario` sobre el formato requerido en Nombre Comercial antes de enviar la solicitud. | **T2** | **3 / 5** *(Fallo sistémico)* | **H5:** Prevención de errores | **3** *(Mayor – Alta)* | Incorporar *helper text* permanente (`"Obligatorio: escribe el nombre público de tu negocio (ej. Bodega El Triunfo)."`). |
| **3** | Confusión inicial en la barra superior entre `"Trámites"` y `"Consulta Expediente"` al iniciar búsqueda. | **T1** | **2 / 5** | **H2:** Relación con el mundo real / **H4:** Consistencia | **2** *(Menor – Media)* | Renombrar en el componente `Header` a `"Catálogo de Trámites"` y `"Consultar Expediente"` con ícono de lupa. |
| **4** | En `06-Exito`, `"Descargar Cargo Digital"` compite visualmente con `"Ir a mis trámites"`, retrasando la gestión posterior. | **T3** | **1 / 5** | **H8:** Diseño estético y minimalista (jerarquía visual) | **2** *(Menor – Media)* | Convertir `"Ir a mis trámites"` en botón primario relleno (`#1E3A5F`) y `"Descargar Cargo Digital"` en secundario con contorno. |
| **5** | En `03-Resultados`, al mostrarse dos expedientes similares sin badge de categoría destacado, el usuario duda brevemente cuál abrir. | **T1** | **1 / 5** | **H6:** Reconocer antes que recordar | **1** *(Cosmético – Baja)* | Añadir badge de categoría (`Licencia Comercial` / `Catastro`) en la esquina superior de cada tarjeta. |

### 8.3 Implementación en `Prototipo v2` y Línea Base `v2-iteracion`

![Comparativa Antes y Despues en Prototipo v2](assets/paso8-antes-despues-v2.png)

![Historial de versiones v2-iteracion en Figma](assets/paso8-version-v2-iteracion.png)

### 8.4 Validación Comparativa de `v2-iteracion` ($n = 2$ Nuevos Participantes)

| Versión evaluada | Participantes ($n$) | Tarea evaluada | Tasa de éxito (%) | Tiempo promedio (s) | Errores promedio |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`v1-prueba` (Línea base)** | $n = 5$ (`P1–P5`) | **T2** (Solicitud y corrección de error) | **70 %** (3.5 / 5) | **108.0 s** | **1.4** |
| **`v2-iteracion` (Iterado)** | $n = 2$ (`P6–P7`) | **T2** (Solicitud y corrección de error) | **100 %** (2.0 / 2) | **74.5 s** *(-31 %)* | **0.0** *(-100 %)* |

![Validacion comparativa de Prototipo v2](assets/paso8-validacion-v2.png)
