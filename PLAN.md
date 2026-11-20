# Plan de proyecto: prototipo del panel de administración AgentHub

## Objetivo

Construir un prototipo frontend navegable del panel de administración de AgentHub, tomando `SPEC.md` como fuente funcional y `Graphic references/` como fuente de identidad visual y diseño. El resultado debe centrarse en `index.html`, ser responsive y permitir recorrer las seis secciones principales sin backend.

## Alcance y decisiones

- Implementar exclusivamente un prototipo frontend con HTML semántico, Tailwind CSS por CDN y JavaScript vanilla.
- No incorporar frameworks, backend, herramientas de compilación, jQuery, CSS personalizado ni atributos `style`.
- Usar datos de ejemplo hardcodeados y simular las acciones que requieren persistencia.
- **Estructura recomendada:** un único `index.html` con una sección `<section>` por área y navegación cliente sin recargar la página. Esta opción se ajusta mejor a la navegación descrita en `SPEC.md` y evita duplicar el layout común.
- **Alternativa permitida:** un `index.html` y un HTML enlazado por sección si se prioriza que cada área sea una página independiente. En ese caso, mantener navegación, cabecera, tema y estilos coherentes entre archivos; aceptar que la navegación implique cargar otra página.
- Considerar la versión de un solo archivo como objetivo principal y decidir antes de implementar si se adopta la alternativa multipágina.

## Fuentes de requisitos y diseño

1. **`SPEC.md`:** requisitos de layout, secciones, datos mínimos, interacciones y criterios CA-01 a CA-30.
2. **`Graphic references/html_references/`:** ejemplos de dashboard, gestión de usuarios y agentes, skills, contrataciones y log de errores, con variantes claras y oscuras cuando están disponibles. Sus documentos pueden usarse como fuente para los datos hardcodeados de demostración (por ejemplo, filas, estados, descripciones y detalles), respetando los campos y cantidades mínimos definidos en `SPEC.md`.
3. **`Graphic references/Logos Agent Hub/`:** logotipos claros/oscuros y recursos de dashboard y catálogo de skills.
4. **`Graphic references/color palette`:** valores de color oficiales. La paleta de referencia incluye índigo AgentHub (`#6366F1`, hover `#4F46E5`), canvas claro/oscuro (`#F8FAFC` / `#0B0F19`), tarjetas (`#FFFFFF` / `#131A2B`), superficies elevadas (`#F1F5F9` / `#1E293B`), bordes (`#E2E8F0` / `#293548`) y texto primario/secundario (`#0F172A` / `#F8FAFC`, `#64748B` / `#94A3B8`).

Las referencias guían el lenguaje visual (sidebar persistente, topbar, tarjetas y tablas, badges, modales, iconografía y uso de color), pero los requisitos de `SPEC.md` prevalecen cuando haya diferencias. Reutilizar los recursos gráficos locales apropiados en lugar de depender de imágenes externas.

## Estructura funcional

### Layout compartido

- Sidebar con el logotipo y exactamente seis destinos: Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes y Log de errores.
- Topbar con título que refleje la sección activa y control de modo claro/oscuro.
- Área principal con una sección por destino; identificar claramente cuál está activa.
- En móvil, sustituir la sidebar persistente por un botón hamburguesa que permita mostrarla y ocultarla.

### Secciones y contenido mínimo

1. **Dashboard:** cuatro tarjetas KPI (ingresos, pérdidas por descuentos, agentes activos y agentes fallando) y marcador de posición del gráfico semanal.
2. **Gestión de usuarios:** al menos cinco registros, plan, email, estado y acciones; detalle en modal y eliminación simulada con confirmación.
3. **Gestión de agentes:** al menos cuatro registros, propietario, estado, skills expandibles y acciones para configurar el prompt o simular eliminación.
4. **Skills:** bloque explicativo y catálogo de las cuatro skills especificadas, con descripción, contador de agentes y modal de detalle.
5. **Contrataciones:** al menos cuatro contratos con cliente, agente, skills, fechas e importe; modal con detalle y desglose de precios.
6. **Log de errores:** al menos seis entradas con timestamp, agente, tipo y descripción; detalle con traza y acción visual para marcar resuelto.

## Componentes e interacciones compartidas

- Reutilizar patrones visuales y de comportamiento para badges, tablas responsive, botones de acciones y modales.
- Permitir como máximo un dropdown de acciones abierto al mismo tiempo; cerrarlo al hacer clic fuera o al elegir una acción.
- Cerrar los modales mediante su botón de cierre o el backdrop, y evitar que se abra un segundo modal mientras haya uno activo.
- Mostrar los datos del registro seleccionado en cada modal.
- Hacer que la expansión de skills sea independiente por agente y que los errores resueltos se distingan visualmente.
- Alternar el modo de color aplicando o quitando `dark` en `<html>`. Mantener el estado durante la sesión, como mínimo.
- Simular las acciones de eliminación o configuración sin presentar cambios como persistidos en un servidor.

## Plan de implementación por fases

### Fase 1 — Alinear requisitos y preparar la base

- Repasar los criterios CA-01 a CA-30 y mapear cada criterio a una sección o interacción.
- Comparar las referencias claras y oscuras para fijar una presentación consistente con la identidad visual de AgentHub.
- Elegir explícitamente el formato de un solo `index.html` o la alternativa multipágina; tomar el archivo único como opción inicial.
- Definir el inventario de datos ficticios necesarios para cumplir los mínimos de cada tabla.

**Salida:** decisiones de estructura, inventario de contenido y trazabilidad de requisitos.

### Fase 2 — Crear el esqueleto y el sistema visual

- Preparar el documento HTML, metadatos, configuración de Tailwind CDN y tipografía/iconos usados por las referencias.
- Construir sidebar, topbar, contenedor de contenido y navegación entre secciones.
- Aplicar la paleta, los logotipos locales y los patrones comunes de superficies, bordes, estados y tipografía.
- Incorporar el layout responsive y el menú móvil.

**Salida:** shell navegable con las seis secciones identificables y con tema claro/oscuro.

### Fase 3 — Completar el contenido de cada sección

- Añadir primero el Dashboard y después usuarios, agentes, skills, contrataciones y errores.
- Completar todas las tablas y tarjetas con la cantidad mínima de datos requerida por `SPEC.md`.
- Añadir marcadores, badges, controles de expansión y contenido de detalle necesario para cada área.

**Salida:** seis secciones completas con datos de demostración coherentes.

### Fase 4 — Implementar las interacciones

- Conectar navegación, estado activo, título de topbar y menú móvil.
- Implementar el toggle global de tema.
- Implementar dropdowns de fila, cierre al hacer clic fuera y exclusividad de dropdown abierto.
- Implementar los modales requeridos, su selección de registro y sus mecanismos de cierre.
- Añadir expansión/colapso de skills, confirmaciones simuladas y marcado visual de errores resueltos.

**Salida:** prototipo funcional sin backend ni dependencias de frameworks.

### Fase 5 — Verificar y ajustar

- Revisar estructura y semántica HTML, enlaces, controles e identificadores.
- Probar en anchos móvil (<768 px), tablet (768–1024 px) y escritorio (>1024 px).
- Recorrer cada flujo en modo claro y oscuro.
- Corregir desbordamientos, contraste, estados de foco y cierres de overlays antes de considerar terminado el prototipo.

**Salida:** `index.html` listo para abrir en navegador y validado frente a los criterios de aceptación.

## Matriz de verificación

- [ ] Documento HTML semántico y sin frameworks, backend, CSS personalizado ni atributos `style`.
- [ ] Estructura de archivo acorde con la decisión documentada; navegación presenta las seis áreas.
- [ ] Sidebar móvil desplegable y estado activo/título correctos.
- [ ] Toggle claro/oscuro afecta a todos los componentes visibles.
- [ ] Dashboard presenta las cuatro métricas y el marcador del gráfico.
- [ ] Usuarios: cinco o más filas, estados y modal de detalle.
- [ ] Agentes: cuatro o más filas, skills expandibles y modal de configuración.
- [ ] Skills: explicación, cuatro entradas y modal de detalle.
- [ ] Contrataciones: cuatro o más filas y modal con desglose.
- [ ] Errores: seis o más entradas, modal con traza y estado resuelto visible.
- [ ] Dropdowns se cierran adecuadamente y solo uno permanece abierto.
- [ ] Modales se cierran con × y backdrop; no se apilan.
- [ ] Tablas y layout se pueden usar en móvil, tablet y escritorio.
- [ ] Recursos locales y paleta de `Graphic references/` se aplican coherentemente.

## Fuera de alcance

- Autenticación, autorización, API, almacenamiento persistente y datos reales.
- Eliminaciones o cambios persistentes fuera de la sesión del prototipo.
- Gráficos funcionales, procesamiento real de agentes o ejecución de skills.
- Despliegue, pipeline de build y pruebas automatizadas que requieran agregar tooling.
