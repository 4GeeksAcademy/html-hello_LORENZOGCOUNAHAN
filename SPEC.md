# SPEC — Panel de Administración AgentHub

> **Versión:** 1.0  
> **Propósito:** Prototipo frontend (HTML + Tailwind CSS + JavaScript vanilla) para el panel de administración de AgentHub, una plataforma SaaS donde las empresas alquilan agentes de IA preconfigurados equipados con skills (habilidades).  
> **Stack:** HTML semántico, Tailwind CSS vía CDN (con utilidades `dark:`), JavaScript vanilla. Sin frameworks, sin jQuery, sin herramientas de build, sin backend, sin CSS personalizado, sin atributos `style` en línea.

---

## Índice

1. [Estructura general del layout](#1-estructura-general-del-layout)
2. [Secciones del panel](#2-secciones-del-panel)
   - [2.1 Dashboard](#21-dashboard)
   - [2.2 Gestión de usuarios](#22-gestión-de-usuarios)
   - [2.3 Gestión de agentes](#23-gestión-de-agentes)
   - [2.4 Skills](#24-skills)
   - [2.5 Contrataciones de agentes](#25-contrataciones-de-agentes)
   - [2.6 Log de errores](#26-log-de-errores)
3. [Inventario de componentes reutilizables](#3-inventario-de-componentes-reutilizables)
4. [Criterios de aceptación](#4-criterios-de-aceptación)

---

## 1. Estructura general del layout

### 1.1 Navegación lateral persistente (sidebar)

| Campo | Descripción |
|---|---|
| **Nombre de componente** | `Sidebar` |
| **Descripción de contenido** | Barra lateral fija a la izquierda (ocultable en móviles) que contiene el logotipo de AgentHub y la lista de enlaces a las seis secciones: Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes y Log de errores. Cada enlace muestra un icono descriptivo y el nombre de la sección. |
| **Descripción de comportamiento** | La sidebar permanece visible en escritorio y tablet. En móvil (< 768 px) se oculta detrás de un botón hamburguesa en la barra superior. La sección activa se resalta con un color de acento y un indicador visual (ej. borde izquierdo o fondo diferenciado). Al hacer clic en un enlace, el contenido principal cambia a la sección correspondiente sin recargar la página. |

### 1.2 Barra superior (topbar)

| Campo | Descripción |
|---|---|
| **Nombre de componente** | `Topbar` |
| **Descripción de contenido** | Barra horizontal superior que contiene el título de la sección actualmente visible a la izquierda y un toggle de modo oscuro/claro a la derecha. |
| **Descripción de comportamiento** | El título se actualiza dinámicamente al cambiar de sección. El toggle de modo oscuro (interruptor o icono sol/luna) alterna la clase `dark` en el elemento `<html>` usando las utilidades `dark:` de Tailwind, cambiando toda la interfaz entre modo claro y modo oscuro al instante. El estado persiste durante la sesión mediante una variable en JavaScript. |

### 1.3 Área de contenido principal

| Campo | Descripción |
|---|---|
| **Nombre de componente** | `MainContent` |
| **Descripción de contenido** | Área variable a la derecha de la sidebar que muestra el contenido de la sección activa. Cada sección es un `<section>` independiente con `id` único; solo la sección activa tiene `display: block`, el resto permanece oculto. |
| **Descripción de comportamiento** | Al navegar desde la sidebar, el contenido de la sección anterior se oculta y el de la nueva sección se muestra, produciendo un cambio instantáneo. No hay animación de transición entre secciones. |

---

## 2. Secciones del panel

### 2.1 Dashboard

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `MetricCard` (×4) |
|   | **Descripción de contenido** | Cuatro tarjetas de métrica en una cuadrícula responsive 2×2 en escritorio, 2×2 en tablet y 1×4 en móvil. Cada tarjeta contiene un icono representativo, una etiqueta textual y un valor numérico hardcodeado. Las cuatro métricas son: **Ingresos Totales** (valor positivo, icono de moneda/crecimiento), **Pérdidas por descuentos** (valor negativo o de resta, icono de descuento/rebaja), **Agentes activos** (conteo, icono de robot/persona), **Agentes fallando** (conteo de alerta, icono de advertencia/error). |
|   | **Descripción de comportamiento** | Cada tarjeta usa un color de acento distinto (verde para ingresos, rojo/anaranjado para pérdidas, azul para activos, rojo para fallando) y una sombra sutil (`shadow-md`). Los valores están hardcodeados directamente en el HTML. No hay interacción al hacer clic en ellas. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `ChartPlaceholder` |
|   | **Descripción de contenido** | Un `<div>` de ancho completo situado debajo de las cuatro tarjetas. Muestra un borde discontinuo (`dashed`) y una etiqueta centrada que dice "Gráfico de actividad semanal — próximo a implementar". |
|   | **Descripción de comportamiento** | Elemento puramente decorativo/de marcador de posición. Sin interacción. Ocupa todo el ancho disponible del contenido principal. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `DashboardGrid` |
|   | **Descripción de contenido** | Contenedor grid que organiza las cuatro tarjetas en filas y columnas según el viewport. |
|   | **Descripción de comportamiento** | En escritorio usa `grid-cols-2` (2 columnas, 2 filas). En tablet `grid-cols-2`. En móvil `grid-cols-1`. Las tarjetas se distribuyen automáticamente con `gap` uniforme. |

---

### 2.2 Gestión de usuarios

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `UserTable` |
|   | **Descripción de contenido** | Tabla HTML semántica (`<table>`) con `<thead>` y `<tbody>` que lista al menos 5 usuarios hardcodeados. Las columnas son: **Nombre**, **Email**, **Plan** (Básico / Profesional / Enterprise) y **Estado** (Activo / Inactivo / Suspendido). El estado se muestra como un badge de color (verde para Activo, gris para Inactivo, rojo para Suspendido). Cada fila incluye además una columna de acciones con un botón que muestra un icono de tres puntos verticales (⁝). |
|   | **Descripción de comportamiento** | La tabla es responsive: en escritorio se ven todas las columnas; en móvil la tabla se vuelve scrollable horizontalmente con `overflow-x: auto`. Los badges de estado siguen el código de color definido. Cada fila es independiente. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `ActionDropdown` (usuarios) |
|   | **Descripción de contenido** | Menú contextual que aparece al hacer clic en el botón ⁝ de cada fila. Contiene dos opciones: **"Ver detalle"** (icono ojo/info) y **"Eliminar"** (icono papelera). |
|   | **Descripción de comportamiento** | Al hacer clic en ⁝ se abre el menú justo debajo/del lado del botón. El menú se cierra al hacer clic fuera de él (backdrop implícito), al seleccionar una opción, o al hacer clic de nuevo en ⁝. Solo un menú puede estar abierto a la vez. "Eliminar" muestra una confirmación en consola o un `alert()` temporal. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `UserDetailModal` |
|   | **Descripción de contenido** | Modal overlay que se abre al seleccionar "Ver detalle". Muestra el registro completo del usuario: nombre completo, email, plan contratado, estado actual, fecha de registro, último inicio de sesión, método de pago asociado y número de agentes activos a su cargo. Incluye un botón de cierre (×) en la esquina superior derecha. |
|   | **Descripción de comportamiento** | El modal aparece centrado con un fondo semitransparente (backdrop) detrás. Se cierra al hacer clic en el botón × o al hacer clic en el backdrop. El contenido del modal se rellena dinámicamente con los datos del usuario seleccionado. El modal es de tamaño fijo en escritorio y de ancho completo en móvil. |

---

### 2.3 Gestión de agentes

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `AgentList` |
|   | **Descripción de contenido** | Listado en formato tabla (`<table>`) con al menos 4 agentes hardcodeados. Columnas: **Nombre del agente**, **Propietario** (cliente al que pertenece), **Estado** (Activo / Inactivo / Fallando — con badge de color: verde, gris, rojo respectivamente), **Skills** (lista colapsada — las skills se eligen del catálogo: Gestión de Calendario, Lectura y Resumen de Documentos, Redactor y Clasificador de Correos, Transcriptor de Reuniones), y **Acciones** (botón ⁝). |
|   | **Descripción de comportamiento** | La tabla es responsive (scroll horizontal en móvil). Cada fila contiene un control expandible (triángulo ▶/▼ o icono +/−) en la columna de skills. Las skills aparecen ocultas por defecto y se revelan con una transición suave (`transition` CSS) al hacer clic. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `CollapsibleSkillList` |
|   | **Descripción de contenido** | Dentro de cada fila de agente, una lista de nombres de skills que el agente tiene habilitadas, elegidas del catálogo de 4 skills: **Gestión de Calendario**, **Lectura y Resumen de Documentos**, **Redactor y Clasificador de Correos**, **Transcriptor de Reuniones**. Inicialmente visible solo como un texto truncado o un botón que indica el número de skills (ej. "3 skills"). |
|   | **Descripción de comportamiento** | Al hacer clic en el control expandible, la lista de skills se despliega con una transición suave de altura (`max-height` + `overflow: hidden` + `transition`). Al colapsar, las skills vuelven a ocultarse suavemente. Cada agente puede expandirse/colapsarse independientemente. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `AgentActionDropdown` |
|   | **Descripción de contenido** | Dropdown de acciones activado por el botón ⁝. Opciones: **"Configurar"** (abre modal con el prompt del sistema del agente) y **"Eliminar"** (eliminación simulada). |
|   | **Descripción de comportamiento** | Mismo comportamiento que el `ActionDropdown` de usuarios. "Configurar" abre el `AgentConfigModal`. "Eliminar" muestra confirmación. |

| # | Campo | Descripción |
|---|---|---|
| 4 | **Nombre de componente** | `AgentConfigModal` |
|   | **Descripción de contenido** | Modal que muestra el "system prompt" del agente — las instrucciones base que definen su personalidad y comportamiento, así como la lista de skills que tiene habilitadas (elegidas del catálogo de 4 skills). Incluye etiqueta "Prompt del sistema" y el texto del prompt en un bloque estilizado como código o cita, más un listado de las skills activas del agente. Tiene botón de cierre (×) y fondo con backdrop. |
|   | **Descripción de comportamiento** | Se abre al hacer clic en "Configurar". Se cierra con × o clic en backdrop. El contenido del prompt se carga dinámicamente según el agente seleccionado. |

---

### 2.4 Skills

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `SkillCatalogExplanation` |
|   | **Descripción de contenido** | Un bloque informativo al inicio de la sección que explica qué es una "skill" en el contexto de AgentHub. Texto sugerido: *"Las skills son capacidades modulares que pueden asignarse a los agentes de IA. Cada skill otorga una habilidad específica: gestión de calendario, lectura y resumen de documentos, redacción y clasificación de correos, transcripción de reuniones, entre otras. Los administradores pueden habilitar o deshabilitar skills en cada agente según las necesidades del cliente. El catálogo actual de AgentHub incluye 4 skills: **Gestión de Calendario**, **Lectura y Resumen de Documentos**, **Redactor y Clasificador de Correos** y **Transcriptor de Reuniones**."* |
|   | **Descripción de comportamiento** | Elemento estático. Sin interacción. Se muestra siempre al cargar la sección. Usa un fondo ligeramente diferenciado (ej. `bg-gray-100` / `dark:bg-gray-700`) para destacar del resto del contenido. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `SkillCatalogTable` |
|   | **Descripción de contenido** | Tabla con las siguientes 4 skills hardcodeadas, cada una con su nombre, descripción breve y contador de agentes habilitados (con badge azul). Las skills del catálogo son: |

|   | | |
|---|---|---|
|   | **Skill** | **Descripción corta** | **Agentes habilitados** |
|   | `Gestión de Calendario` | *Organiza tu agenda automáticamente. Consulta disponibilidad, crea reuniones, envía invitaciones y reprograma eventos sin conflictos de horario.* | 18 agentes |
|   | `Lectura y Resumen de Documentos` | *Extrae lo más importante de tus archivos en segundos. Analiza contratos, informes o manuales en PDF y genera resúmenes claros y accionables.* | 24 agentes |
|   | `Redactor y Clasificador de Correos` | *Mantén tu bandeja de entrada bajo control. Etiqueta mensajes según su urgencia y redacta borradores de respuesta listos para enviar con un clic.* | 15 agentes |
|   | `Transcriptor de Reuniones` | *Convierte tus videollamadas en actas organizadas. Transcribe el audio a texto y extrae automáticamente los acuerdos y tareas pendientes.* | 11 agentes |

|   | **Columnas de la tabla:** **Nombre de la skill**, **Descripción breve** (texto de la tarjeta de catálogo), **Agentes habilitados** (número con badge azul), y **Acciones** (botón ⁝). |
|   | **Descripción de comportamiento** | Tabla responsive con scroll horizontal en móvil. La columna "Agentes habilitados" muestra un badge de color azul con el número de agentes. Cada fila incluye su propio dropdown de acciones. Los nombres de skill usan el formato de código o énfasis para distinguirse visualmente. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `SkillActionDropdown` |
|   | **Descripción de contenido** | Dropdown con opciones **"Ver detalle"** (abre modal con información completa de la skill) y **"Eliminar"** (acción simulada). |
|   | **Descripción de comportamiento** | Mismo patrón que los dropdowns anteriores. "Ver detalle" abre el `SkillDetailModal` con los datos de la skill seleccionada. El modal se cierra con × o clic en backdrop. |

| # | Campo | Descripción |
|---|---|---|
| 4 | **Nombre de componente** | `SkillDetailModal` |
|   | **Descripción de contenido** | Modal que muestra la ficha completa de la skill seleccionada. Incluye: **Nombre de la skill**, **Descripción detallada** (texto de la vista de producto), **Capacidades clave** (lista de capacidades separadas por •), **Precio por agente** (valor hardcodeado en USD), **Versión** de la skill, **Fecha de creación**, y **Agentes que la tienen habilitada** (lista de nombres de agentes). El contenido específico para cada skill es: |

|   | | |
|---|---|---|
|   | **Skill** | **Descripción detallada** | **Capacidades clave** |
|   | `Gestión de Calendario` | *Olvídate de las cadenas interminables de correos para cuadrar una hora. Este agente se conecta con tu calendario corporativo para detectar huecos libres, enviar invitaciones con enlace de videollamada y ajustar horarios automáticamente ante imprevistos o cambios de zona horaria.* | Detección de conflictos • Envío automático de invitaciones • Ajuste de zonas horarias |
|   | `Lectura y Resumen de Documentos` | *Ahorra horas de lectura revisando documentación extensa. Sube cualquier informe, contrato o presentación y el agente identificará las ideas principales, fechas clave y puntos críticos, entregándote un resumen estructurado listo para compartir con tu equipo.* | Lectura de PDF y Word • Extracción de puntos clave • Resúmenes ejecutivos al instante |
|   | `Redactor y Clasificador de Correos` | *Prioriza lo urgente y responde más rápido. Este agente analiza cada correo entrante, lo clasifica por categoría o prioridad y prepara borradores de respuesta profesionales adaptados al contexto de la conversación, dejándolos listos para tu revisión final.* | Etiquetado inteligente por prioridad • Borradores contextuales • Filtrado de consultas frecuentes |
|   | `Transcriptor de Reuniones` | *Céntrate en la conversación mientras el agente toma notas por ti. Transcribe reuniones grabadas o en directo identificando a cada participante y genera al instante una minuta con las decisiones tomadas y los próximos pasos asignados a cada responsable.* | Transcripción de audio a texto • Identificación de hablantes • Lista automática de tareas y acuerdos |

|   | **Descripción de comportamiento** | Se abre al seleccionar "Ver detalle" en el dropdown de la skill correspondiente. El modal se cierra con el botón × o haciendo clic en el backdrop. El contenido se carga dinámicamente según la skill seleccionada, mostrando la descripción detallada y las capacidades clave específicas de esa skill. |

---

### 2.5 Contrataciones de agentes

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `ContractTable` |
|   | **Descripción de contenido** | Tabla con al menos 4 contratos de alquiler hardcodeados. Columnas: **Cliente** (nombre de la empresa), **Agente** (nombre del agente contratado), **Skills contratadas** (lista de nombres separados por coma, elegidas del catálogo: Gestión de Calendario, Lectura y Resumen de Documentos, Redactor y Clasificador de Correos, Transcriptor de Reuniones), **Fecha inicio**, **Fecha fin** e **Importe pagado** (formato moneda). |
|   | **Descripción de comportamiento** | Tabla responsive con scroll horizontal en móvil. Las fechas se muestran en formato ISO corto (ej. 2025-01-15). Los importes incluyen el símbolo de moneda (USD/EUR). Cada fila incluye columna de acciones con ⁝. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `ContractDetailModal` |
|   | **Descripción de contenido** | Modal que muestra el desglose completo del contrato: datos del cliente y agente, fechas del contrato, importe total pagado, y una tabla desglosada de las skills contratadas con sus precios individuales. Las skills del catálogo y sus precios unitarios son: **Gestión de Calendario** ($120/mes), **Lectura y Resumen de Documentos** ($95/mes), **Redactor y Clasificador de Correos** ($110/mes), **Transcriptor de Reuniones** ($130/mes). Cada skill contratada aparece con su nombre, precio unitario y subtotal (precio × cantidad si aplica), más un total visible al final. |
|   | **Descripción de comportamiento** | Se abre al seleccionar "Ver detalle" en el dropdown de la fila. Se cierra con × o clic en backdrop. Los datos se cargan dinámicamente según la fila seleccionada. El desglose de precios permite ver cuánto aporta cada skill al costo total. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `ContractActionDropdown` |
|   | **Descripción de contenido** | Dropdown con opciones **"Ver detalle"** (abre `ContractDetailModal`) y **"Eliminar"** (eliminación simulada del contrato). |
|   | **Descripción de comportamiento** | Mismo patrón de dropdown. "Ver detalle" carga el modal con datos específicos del contrato. "Eliminar" muestra confirmación. |

---

### 2.6 Log de errores

| # | Campo | Descripción |
|---|---|---|
| 1 | **Nombre de componente** | `ErrorLogTable` |
|   | **Descripción de contenido** | Tabla con al menos 6 entradas de error hardcodeadas. Columnas: **Timestamp** (fecha y hora del error), **Agente** (nombre del agente que falló), **Tipo de error** (badge con código de color según la categoría: `CRITICAL` rojo, `WARNING` amarillo/naranja, `INFO` azul, `TIMEOUT` púrpura, `AUTH_ERROR` rojo oscuro), **Descripción breve**, y **Acciones** (botón ⁝). |
|   | **Descripción de comportamiento** | Tabla responsive. Los badges de tipo de error usan colores distintivos para identificación visual rápida. Las filas pueden tener un color de fondo ligeramente alternado (zebra striping) para facilitar la lectura. |

| # | Campo | Descripción |
|---|---|---|
| 2 | **Nombre de componente** | `ErrorDetailModal` |
|   | **Descripción de contenido** | Modal que muestra la traza completa del error: timestamp completo, nombre del agente, tipo de error con badge, descripción breve, y una sección de "Traza completa" con un bloque de texto en formato código (`<pre><code>`) que simula el stack trace del error. Incluye botón de cierre (×). |
|   | **Descripción de comportamiento** | Se abre al seleccionar "Ver detalle". Se cierra con × o clic en backdrop. La traza completa se renderiza en un bloque de texto monoespaciado con scroll interno si es muy largo. |

| # | Campo | Descripción |
|---|---|---|
| 3 | **Nombre de componente** | `ErrorActionDropdown` |
|   | **Descripción de contenido** | Dropdown con opciones **"Ver detalle"** (abre `ErrorDetailModal`) y **"Marcar como resuelto"** (cambia el estado visual del error). |
|   | **Descripción de comportamiento** | Al seleccionar "Marcar como resuelto", el badge de tipo de error de esa fila cambia visualmente (ej. se añade una etiqueta "RESUELTO" superpuesta, la fila se atenúa, o el badge se vuelve verde) para indicar que el error ha sido resuelto. Esta acción es irreversible dentro de la sesión (solo visual). |

---

## 3. Inventario de componentes reutilizables

| # | Componente | Descripción | Secciones donde aparece |
|---|---|---|---|
| 1 | **Sidebar** | Barra de navegación lateral con enlaces a las seis secciones, indicador de sección activa y colapso responsive. | Global |
| 2 | **Topbar** | Barra superior con título dinámico y toggle de modo oscuro. | Global |
| 3 | **MetricCard** | Tarjeta con icono, etiqueta y valor numérico; color de acento diferenciado; sombra sutil. | Dashboard |
| 4 | **ActionDropdown (⁝)** | Menú contextual con opciones variables; se cierra al hacer clic fuera o al seleccionar una opción; solo uno abierto a la vez. | Gestión de usuarios, Gestión de agentes, Skills, Contrataciones, Log de errores |
| 5 | **Modal** | Ventana overlay centrada con backdrop semitransparente; botón de cierre (×); se cierra con × o clic en backdrop; contenido dinámico. | Gestión de usuarios, Gestión de agentes, Skills, Contrataciones, Log de errores |
| 6 | **Badge** | Etiqueta de estado con color codificado (verde/rojo/amarillo/azul/gris) para indicar estado o categoría. | Gestión de usuarios, Gestión de agentes, Log de errores |
| 7 | **CollapsibleSkillList** | Lista expandible/colapsable de skills con transición suave; cada agente se expande independientemente. | Gestión de agentes |
| 8 | **DarkModeToggle** | Interruptor o icono que alterna la clase `dark` en `<html>` usando utilidades `dark:` de Tailwind; afecta a toda la interfaz. | Global (Topbar) |
| 9 | **ResponsiveTable** | Tabla semántica con `<thead>`/`<tbody>`, soporte de scroll horizontal en móvil, zebra striping opcional. | Gestión de usuarios, Gestión de agentes, Skills, Contrataciones, Log de errores |
| 10 | **ChartPlaceholder** | Div de marcador de posición con borde discontinuo y etiqueta centrada para futuros gráficos. | Dashboard |

---

## 4. Criterios de aceptación

### 4.1 Condiciones generales

- [ ] **CA-01:** El archivo es un único documento HTML válido que no depende de ningún otro archivo (ni CSS externo, ni JS externo, excepto Tailwind CDN).
- [ ] **CA-02:** No se utiliza ningún framework (React, Vue, etc.), ni jQuery, ni herramientas de build, ni backend.
- [ ] **CA-03:** No hay atributos `style` en línea ni archivos CSS personalizados. Todo el estilo proviene de las clases de Tailwind (vía CDN), incluyendo las utilidades `dark:`.
- [ ] **CA-04:** Se usan etiquetas HTML semánticas (`<header>`, `<nav>`, `<main>`, `<section>`, `<table>`, `<th>`, `<tr>`, `<td>`, `<footer>` etc.) de forma correcta y significativa.
- [ ] **CA-05:** El diseño es responsive en móvil (< 768 px), tablet (768–1024 px) y escritorio (> 1024 px). Se verifica que no hay roturas de layout en ningún breakpoint.

### 4.2 Sidebar y navegación

- [ ] **CA-06:** La sidebar contiene exactamente seis enlaces, uno por sección, y cada enlace tiene un icono y el nombre de la sección.
- [ ] **CA-07:** Al hacer clic en un enlace de la sidebar, el contenido principal cambia a la sección correspondiente y el enlace se marca visualmente como activo.
- [ ] **CA-08:** En móvil, la sidebar se oculta detrás de un botón hamburguesa; al hacer clic en el hamburguesa, la sidebar se muestra/oculta.

### 4.3 Toggle de modo oscuro

- [ ] **CA-09:** El toggle en la barra superior cambia toda la interfaz entre modo claro y modo oscuro usando las utilidades `dark:` de Tailwind (ej. `dark:bg-gray-900`, `dark:text-white`).
- [ ] **CA-10:** El cambio es inmediato y afecta a sidebar, topbar, tarjetas, tablas, modales, badges y todos los elementos visibles.

### 4.4 Dashboard

- [ ] **CA-11:** Se muestran exactamente cuatro tarjetas de métrica en una cuadrícula responsive (2×2 en escritorio/tablet, 1×4 en móvil).
- [ ] **CA-12:** Cada tarjeta tiene un icono, una etiqueta y un valor hardcodeado, con un color de acento distinto y sombra sutil.
- [ ] **CA-13:** Debajo de las tarjetas hay un div de ancho completo con borde discontinuo y la etiqueta "Gráfico de actividad semanal — próximo a implementar".

### 4.5 Gestión de usuarios

- [ ] **CA-14:** La tabla contiene al menos 5 filas de usuarios con nombre, email, plan y badge de estado (Activo/Inactivo/Suspendido).
- [ ] **CA-15:** Cada fila tiene un botón ⁝ que abre un dropdown con las opciones "Ver detalle" y "Eliminar".
- [ ] **CA-16:** Al hacer clic en "Ver detalle" se abre un modal overlay con el registro completo del usuario; el modal se cierra al hacer clic en × o en el backdrop.

### 4.6 Gestión de agentes

- [ ] **CA-17:** La tabla contiene al menos 4 agentes con nombre, propietario, estado (con badge) y lista colapsable de skills.
- [ ] **CA-18:** Las skills están ocultas por defecto; al hacer clic en el control expandible se revelan con una transición suave.
- [ ] **CA-19:** El dropdown de acciones contiene "Configurar" (abre modal con el prompt del sistema) y "Eliminar".

### 4.7 Skills

- [ ] **CA-20:** Se muestran las 4 skills del catálogo (Gestión de Calendario, Lectura y Resumen de Documentos, Redactor y Clasificador de Correos, Transcriptor de Reuniones) con nombre, descripción breve y contador de agentes habilitados.
- [ ] **CA-21:** Hay un bloque informativo al inicio explicando qué es una skill en AgentHub.
- [ ] **CA-22:** Cada skill tiene un dropdown con "Ver detalle" y "Eliminar".

### 4.8 Contrataciones de agentes

- [ ] **CA-23:** La tabla contiene al menos 4 contratos con cliente, agente, skills contratadas, fechas de inicio/fin e importe pagado.
- [ ] **CA-24:** Cada fila tiene un dropdown con "Ver detalle"; al seleccionarlo se abre un modal con el desglose completo del contrato, incluyendo skills desglosadas y sus precios individuales.

### 4.9 Log de errores

- [ ] **CA-25:** La tabla contiene al menos 6 entradas de error con timestamp, nombre del agente, badge de tipo de error con código de color, y descripción breve.
- [ ] **CA-26:** Cada entrada tiene un dropdown con "Ver detalle" (abre modal con traza completa del error) y "Marcar como resuelto".
- [ ] **CA-27:** Al seleccionar "Marcar como resuelto", la fila cambia visualmente para indicar que el error ha sido resuelto.

### 4.10 Comportamiento interactivo transversal

- [ ] **CA-28:** Los dropdowns de acciones (⁝) se abren al hacer clic en el botón, se cierran al hacer clic fuera o al seleccionar una opción, y solo un dropdown puede estar abierto a la vez en toda la página.
- [ ] **CA-29:** Todos los modales se abren centrados con backdrop semitransparente, se cierran con el botón × o haciendo clic en el backdrop, y no se permite abrir un segundo modal mientras uno está activo.
- [ ] **CA-30:** Las listas colapsables de skills en la sección de agentes se expanden y colapsan con una transición suave, de forma independiente por cada agente.

---

*Fin del documento SPEC.md — AgentHub Admin Panel v1.0*