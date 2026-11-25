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

## Paso 3: Navegación, overlays y flujos (`Prototipo v1`)

### 3.1 Navegación base, dispositivo y scroll vertical
Se configuró el entorno de prototipado en `Desktop`, activando `Overflow → Vertical` y encabezado `Position: Fixed` en pantallas extensas, junto con las transiciones `On click → Navigate to` y acciones `Back` (`Smart Animate`, `300 ms`):

![Navegacion base en Prototipo v1](assets/paso3-navegacion-base.png)

### 3.2 Superposiciones y modales (`Open overlay`)
Se configuraron `Modal-Confirmar` y `Modal-Cancelar` como frames independientes invocados mediante `Open overlay` (`Center`, `Close when clicking outside`, fondo `#000000` al `40 %`, animación `Move in ↑`, `Ease out`, `250 ms`), conectando la acción de confirmación hacia `05c-Procesando` / `06-Exito`:

![Configuracion de overlays en Prototipo v1](assets/paso3-overlays.png)