# SPECS.md — AgentHub Admin Panel Prototype

> **Estándar de calidad:** Un agente de código IA debe poder leer este documento y construir el panel completo sin hacer preguntas.

---

## 1. Product Description

**AgentHub** es una plataforma SaaS donde empresas contratan (alquilan) agentes de IA preconfigurados. Cada agente puede equiparse con distintas *skills* (habilidades como navegación web, lectura de documentos, gestión de calendarios, etc.) y desplegarse para tareas de negocio específicas.

**Usuario administrador:** Es el usuario interno de AgentHub que gestiona la plataforma: supervisa ingresos, administra usuarios y agentes, gestiona el catálogo de skills disponibles, revisa contratos de alquiler activos, y monitorea errores de ejecución de agentes.

---

## 2. Tech Stack & Constraints

| Elemento | Especificación |
|----------|---------------|
| **Lenguaje** | HTML5 semántico |
| **CSS** | Tailwind CSS v3 (vía CDN: `https://cdn.tailwindcss.com`) + utilidad `dark:` para modo oscuro |
| **JavaScript** | Vanilla JS (sin frameworks, sin librerías de UI) |
| **Backend** | Ninguno — todos los datos hardcodeados en el HTML/JS |
| **Fuente de iconos** | Font Awesome v6 (vía CDN) |
| **Tipografía** | Inter (vía Google Fonts CDN) |
| **Responsive** | Sí — el panel debe funcionar en escritorio (primario) y tablets |
| **Persistencia** | Ninguna — no hay base de datos, localStorage ni API calls |

---

## 3. Per-Section Specifications

### 3.1 Dashboard (*Visión general del negocio*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Tarjetas de métrica (Metric Cards)** | Cuatro tarjetas en un grid responsive 2×2 (2 columnas en desktop, 1 columna en móvil). Cada tarjeta contiene un icono (Font Awesome) a la izquierda, una etiqueta textual (ej. "Ingresos Totales") y un valor numérico hardcodeado con formato legible (ej. "$12,430"). Detalle de cada tarjeta: |
| | — *Ingresos Totales* | Icono: `fa-chart-line` (o similar monetario). Acento: azul. Valor: `$12,430`. Sombra sutil. |
| | — *Pérdida por Descuentos* | Icono: `fa-tag` (o similar descuento). Acento: ámbar (naranja/amarillo). Valor: `$940`. Sombra sutil. |
| | — *Agentes Activos* | Icono: `fa-robot`. Acento: verde. Valor: `24`. Sombra sutil. |
| | — *Agentes Fallando* | Icono: `fa-exclamation-triangle`. Acento: rojo. Valor: `3`. Sombra sutil. |
| **b)** | **Grid responsive** | Las tarjetas se distribuyen en 2 columnas en desktop (`md:grid-cols-2`) y 1 columna en móvil. Cada tarjeta ocupa una celda del grid. |
| **c)** | **Placeholder de gráfico** | Debajo de las tarjetas, un `div` de ancho completo con borde discontinuo (`dashed`), altura fija (~250px), y una etiqueta centrada que dice "📊 Gráfico de actividad semanal (próximamente)". Fondo ligeramente grisáceo en modo claro, oscuro en modo oscuro. |

### 3.2 Gestión de Usuarios (*Tabla de usuarios registrados*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Tabla de usuarios** | Tabla HTML con cabecera fija y filas con datos hardcodeados. Columnas: Nombre, Email, Plan (Free / Pro / Enterprise), Estado (Activo / Inactivo / Suspendido). Mínimo 6 filas de ejemplo. |
| **b)** | **Dropdown de acciones (⋮)** | Cada fila tiene un botón `⋮` (tres puntos verticales) que al hacer clic despliega un menú contextual con opciones. El menú se posiciona relativo al botón. Debe cerrarse al hacer clic fuera del menú, al hacer clic en una opción, o al presionar Escape. Opciones del menú: "👁 Ver detalle" y "🗑 Eliminar". |
| **c)** | **Modal «Ver detalle»** | Al hacer clic en "Ver detalle" se abre un modal overlay centrado. El modal muestra: nombre, email, plan, estado, fecha de registro, última sesión, y empresa (todos hardcodeados). Tiene un botón de cierre (✕) en la esquina superior derecha. El modal se cierra al: (1) hacer clic en el botón ✕, (2) hacer clic en el backdrop (fondo oscuro semitransparente), (3) presionar Escape. El backdrop usa `bg-black/50` con `backdrop-blur-sm`. |
| **d)** | **Acción «Eliminar»** | Al seleccionar "Eliminar" del dropdown, se muestra un `alert()` de confirmación. Tras confirmar, la fila se elimina visualmente de la tabla (DOM) con una transición de opacidad. |

### 3.3 Gestión de Agentes (*Agentes registrados en la plataforma*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Lista/tabla de agentes** | Tabla con columnas: Nombre del Agente, Propietario (email del cliente), Estado (badge coloreado: Activo/Inactivo/Fallando — verde, gris, rojo respectivamente), Skills (colapsable). Mínimo 5 agentes hardcodeados. |
| **b)** | **Lista de skills colapsable** | En la columna "Skills", un texto tipo "3 skills »" que al hacer clic revela las skills con una transición suave (`max-height` + `opacity` con `transition` CSS). Al expandirse, se muestra una lista de nombres de skills como badges pequeños (ej. "Web", "Docs", "Calendar"). El icono cambia entre "»" y "▼". |
| **c)** | **Dropdown de acciones (⋮)** | Similar al de usuarios. Opciones: "⚙ Configurar" (abre modal con el system prompt del agente) y "🗑 Eliminar". Comportamiento de cierre idéntico. |
| **d)** | **Modal «Configurar»** | Modal overlay (mismo sistema que usuarios) que muestra: nombre del agente, su system prompt en un `<textarea readonly>` (con fondo oscuro y tipografía monoespaciada), y skills asignadas. Botón de cierre ✕ + backdrop. |

### 3.4 Skills (*Catálogo de habilidades disponibles*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Explicación contextual** | Un párrafo informativo en la parte superior de la sección que explica: *"Las Skills son capacidades modulares que puedes adjuntar a cualquier agente. Cada skill añade una habilidad específica — como navegar por la web, leer documentos, o gestionar calendarios. Un agente puede tener una o varias skills activas."* |
| **b)** | **Lista/tabla de skills** | Tabla con columnas: Nombre de la Skill, Descripción breve, Agentes que la usan (número + indicador visual tipo badge). Mínimo 6 skills: Web Navigation, Document Reader, Calendar, Email, Data Analyzer, Code Interpreter. |
| **c)** | **Dropdown de acciones (⋮)** | Opciones: "👁 Ver detalle" y "🗑 Eliminar". Al hacer clic en "Ver detalle", se abre un modal mostrando: nombre, descripción completa, casos de uso (lista), precio base, y nivel de habilitación (Beta / Stable / Deprecated con badge). |

### 3.5 Contrataciones de Agentes (*Rental contracts*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Tabla de contratos** | Tabla con columnas: Cliente (nombre), Agente, Skills contratadas (lista resumida), Fecha Inicio, Fecha Fin, Importe Total. Mínimo 6 contratos. |
| **b)** | **Dropdown de acciones (⋮)** | Opción única: "📄 Ver detalle". Comportamiento de cierre idéntico al estándar. |
| **c)** | **Modal «Detalle del Contrato»** | Modal que muestra: nombre del cliente, agente alquilado, duración (fechas), importe total, y una tabla desglosada de skills contratadas con sus precios individuales (skill, precio, subtotal). Formato tipo factura. Cierre: ✕ + backdrop + Escape. |

### 3.6 Log de Errores (*Ejecución de agentes*)

| # | Componente | Descripción | Comportamiento |
|---|-----------|-------------|----------------|
| **a)** | **Tabla de errores** | Tabla con columnas: Timestamp (fecha y hora), Agente (nombre), Tipo de Error (badge coloreado), Descripción breve. Mínimo 6 errores con distintos tipos y gravedades. |
| **b)** | **Badges por tipo/gravedad** | Cada tipo de error tiene un badge con un color distintivo: `Critical` (rojo), `Warning` (ámbar), `Timeout` (naranja), `DataError` (púrpura), `AuthError` (azul). El badge es un `<span>` con padding, border-radius, y fuente seminegrita. |
| **c)** | **Dropdown de acciones (⋮)** | Opciones: "🔍 Ver detalle" (abre modal con traza completa del error) y "✅ Marcar como resuelto". |
| **d)** | **Modal «Detalle del Error»** | Modal que muestra: timestamp completo, agente, tipo (badge), descripción larga, y una traza técnica simulada en un `<pre>` con fondo oscuro y tipografía monoespaciada. |
| **e)** | **Acción «Marcar como resuelto»** | Al seleccionar "Marcar como resuelto", la fila del error se sombrea en verde tenue (clase `bg-green-100` en modo claro, o equivalente en modo oscuro) y el badge cambia a "Resuelto" con color verde. |

---

## 4. Component Inventory

### 4.1 Componentes de UI Reutilizables

| # | Componente | Descripción | Uso en secciones |
|---|-----------|-------------|------------------|
| **1.** | **Sidebar de navegación** | Barra lateral izquierda fija (o colapsable en móvil). Fondo oscuro. Contiene: logo/ nombre de AgentHub, enlaces a las 6 secciones con iconos, link de logout. El enlace activo se marca visualmente (fondo más claro o borde izquierdo). | Global (todas las vistas) |
| **2.** | **Top bar** | Barra horizontal superior con: título de la sección actual a la izquierda, toggle de modo oscuro a la derecha (interruptor visual), y espacio para el breadcrumb o información contextual. | Global (todas las vistas) |
| **3.** | **Metric Card (tarjeta de métrica)** | Componente rectangular con icono (FA), etiqueta, valor numérico, color de acento, sombra sutil (`shadow-md`), bordes redondeados (`rounded-lg`). | Dashboard |
| **4.** | **Dropdown de acciones (⋮)** | Botón `⋮` que despliega un menú posicionado absolutamente. Se cierra al hacer clic fuera, al seleccionar una opción, o con Escape. Misma implementación en todas las secciones. | Usuarios, Agentes, Skills, Contratos, Log |
| **5.** | **Modal overlay** | Overlay de pantalla completa con backdrop semitransparente (`bg-black/50`, `backdrop-blur-sm`). Ventana centrada con padding, esquinas redondeadas. Cierre: ✕, backdrop, Escape. | Usuarios (detalle), Agentes (config), Skills (detalle), Contratos (detalle), Log (detalle) |
| **6.** | **Badge coloreado** | Pequeño `<span>` con `px-2 py-1`, `rounded-full` (o `rounded-md`), fuente seminegrita. Colores por estado o tipo. | Agentes (estado), Skills (estado/habilitación), Errores (tipo) |
| **7.** | **Lista de skills colapsable** | Texto expandible con transición CSS (`max-height` + `opacity`). Estado inicial colapsado, alterna con clic. | Agentes |
| **8.** | **Toggle de modo oscuro** | Interruptor deslizante (checkbox estilizado) en la top bar. Alterna entre modo claro y modo oscuro usando la utilidad `dark:` de Tailwind. El cambio es inmediato y afecta a toda la interfaz. | Global (todas las vistas) |
| **9.** | **Tabla estándar** | Tabla HTML con `<thead>` fijo, `<tbody>` con filas, y diseño limpio. Alternancia sutil de color en filas (zebra striping opcional). | Usuarios, Agentes, Skills, Contratos, Log |

### 4.2 Árbol de componentes (anidamiento)

```
Page
├── Sidebar (componente 1)
│   ├── Logo
│   ├── Nav links (6)
│   └── Logout
├── Top bar (componente 2)
│   ├── Section title
│   └── Dark mode toggle (componente 8)
└── Main content (varía por sección)
    ├── Dashboard
    │   ├── Metric Cards (componente 3) × 4
    │   └── Chart placeholder
    ├── User Management
    │   └── Table (componente 9) + Dropdown (componente 4) + Modal (componente 5)
    ├── Agent Management
    │   ├── Table (componente 9) + Collapsible Skills (componente 7) + Dropdown (componente 4)
    │   └── Modal (componente 5)
    ├── Skills
    │   ├── Explanatory text
    │   ├── Table (componente 9) + Badge (componente 6) + Dropdown (componente 4)
    │   └── Modal (componente 5)
    ├── Rentals
    │   ├── Table (componente 9) + Dropdown (componente 4)
    │   └── Modal (componente 5) con tabla de desglose
    └── Error Log
        ├── Table (componente 9) + Badge (componente 6) + Dropdown (componente 4)
        └── Modal (componente 5) + <pre> block
```

---

## 5. Acceptance Criteria

> Lista numerada de condiciones verificables. El prototipo se considera **completo** cuando TODAS las condiciones se cumplen.

### Navegación y Layout
1. [ ] El sidebar lateral izquierdo contiene enlaces a las 6 secciones y un link de logout.
2. [ ] Al hacer clic en un enlace del sidebar, la vista principal cambia al contenido de esa sección (sin recargar la página).
3. [ ] La sección activa en el sidebar está visualmente resaltada.
4. [ ] La top bar muestra el título de la sección actual y el toggle de modo oscuro.

### Dashboard
5. [ ] Cuatro tarjetas de métrica visibles en grid 2×2, cada una con icono, etiqueta y valor hardcodeado.
6. [ ] Cada tarjeta tiene un color de acento distinto (azul, ámbar, verde, rojo).
7. [ ] El placeholder del gráfico semanal tiene borde discontinuo, altura fija y etiqueta centrada.

### Dropdown de acciones
8. [ ] Cada botón `⋮` despliega un menú contextual al hacer clic.
9. [ ] El menú se cierra al hacer clic fuera de él, al seleccionar una opción, o al presionar Escape.
10. [ ] Cada opción del menú ejecuta la acción correspondiente (abrir modal o mostrar alert).

### Modales
11. [ ] Cada modal se abre con un overlay de fondo semitransparente.
12. [ ] El modal se cierra al hacer clic en ✕, al hacer clic en el backdrop, o al presionar Escape.
13. [ ] El modal «Detalle de Usuario» muestra al menos: nombre, email, plan, estado, empresa.
14. [ ] El modal «Configurar Agente» contiene un `<textarea readonly>` con el system prompt.
15. [ ] El modal «Detalle de Contrato» incluye una tabla desglosada de skills con precios.
16. [ ] El modal «Detalle de Error» contiene un bloque `<pre>` con la traza simulada.
17. [ ] El modal «Detalle de Skill» muestra nombre, descripción, casos de uso, precio y nivel de habilitación.

### Skills colapsables
18. [ ] Las skills de cada agente están ocultas por defecto en la tabla de agentes.
19. [ ] Al hacer clic en el control expandible, las skills se revelan con una transición suave CSS.
20. [ ] Al hacer clic de nuevo, las skills se ocultan con la misma transición.

### Modo oscuro
21. [ ] El toggle de modo oscuro en la top bar cambia toda la interfaz a modo oscuro usando `dark:` (Tailwind).
22. [ ] El toggle de modo oscuro cambia toda la interfaz de vuelta a modo claro al desactivarse.
23. [ ] El modal, sidebar, tablas, badges y todos los componentes respetan el modo oscuro.

### Badges por tipo de error
24. [ ] Cada tipo de error en el log tiene un badge con color distintivo (Critical=rojo, Warning=ámbar, Timeout=naranja, DataError=púrpura, AuthError=azul).

### Acción «Marcar como resuelto»
25. [ ] Al seleccionar "Marcar como resuelto" en un error, la fila cambia a fondo verde y el badge pasa a "Resuelto".

### Acción «Eliminar»
26. [ ] Al seleccionar "Eliminar", se muestra un `alert()` de confirmación.
27. [ ] Tras confirmar la eliminación, la fila se elimina del DOM con transición de opacidad.

### Responsive
28. [ ] En viewports menores a 768px, las tarjetas del dashboard se apilan en 1 columna.
29. [ ] En viewports menores a 768px, el sidebar se colapsa u oculta con un botón de menú (hamburger).

---

> **Fin de la especificación.** Este documento es el único insumo necesario para construir el prototipo HTML del panel de administración de AgentHub.