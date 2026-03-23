# Semiótica de la Imagen · Semana 11
## Signos en la vida universitaria

**Herramienta educativa interactiva** para el análisis semiótico de espacios, objetos y códigos visuales del entorno académico cotidiano.

---

### Información del curso

| Campo | Detalle |
|-------|---------|
| **Docente** | Mg. Mario Rafael Quiroz Martínez |
| **Curso** | Semiótica de la Imagen |
| **Institución** | Universidad Tecnológica del Perú (UTP) |
| **Ciclo** | 2026-I |
| **Semana** | 11 |
| **Correo docente** | c12139@utp.edu.pe |
| **Marca** | d3magindesign |

---

### Descripción general

Formulario HTML interactivo autocontenido (single-file) que permite a estudiantes universitarios analizar fotografías del entorno académico mediante hotspots etiquetados, capas de significante/significado, un cuestionario mixto y envío automático de respuestas por correo electrónico con generación de PDF.

El contenido se organiza en **3 ejemplos resueltos** y **2 ejercicios de desarrollo**, articulados con los conceptos de Ferdinand de Saussure (signo, significante, significado, reciprocidad).

---

### Estructura de contenidos

#### Ejemplos resueltos (3)

| # | Título | Imagen | Significantes clave | Significado |
|---|--------|--------|---------------------|-------------|
| 1 | La tesis como signo | `Redactando.png` | Laptop con artículos, cuaderno, café, palabra "TESIS" | Investigación extensa, proyecto clave para graduarse |
| 2 | Señales y códigos en el campus | `Biblioteca.png` | Icono de libro, palabra "BIBLIOTECA", pictograma de silencio | Espacio de estudio en silencio |
| 3 | La cafetería como espacio semiótico | `Estudiantes.png` (recorte superior) | Vasos de café, laptops, audífonos, sonrisas | Networking, trabajo colaborativo, tercer espacio |

#### Ejercicios para desarrollo (2)

| # | Título | Imagen | Tarea del estudiante |
|---|--------|--------|----------------------|
| 4 | El cartel universitario como texto multimodal | `Estudiantes.png` (recorte inferior derecho) | Identificar significantes (logo, tipografía, foto, colores) y sus significados (prestigio, entretenimiento, formación) |
| 5 | Espacios universitarios como signos | `Estudiantes.png` (recorte inferior izquierdo) | Emparejar elementos del pasillo (mochilas, postura, arquitectura) con nociones (clase, encuentro social, exposición) |

#### Sección de conceptos (Saussure)

Síntesis del video explicativo sobre el signo lingüístico:
- Signo lingüístico como unidad mínima
- Significante: traducción fónica / imagen acústica
- Significado: correlato mental / concepto
- Ejemplo canónico: "manzana"
- Representación gráfica con elipse y flechas de reciprocidad

#### Cuestionario (6 preguntas)

- **P1** — Cerrada (radio): componentes del signo según Saussure
- **P2** — Cerrada (checkbox múltiple): significantes en las imágenes
- **P3** — Cerrada (radio): redundancia semiótica en la señal
- **P4** — Abierta (textarea): objetos-signo en la cafetería
- **P5** — Cerrada (radio): flechas en el esquema de Saussure
- **P6** — Abierta (textarea): análisis libre de un espacio universitario

---

### Características técnicas

#### Arquitectura

- **Tipo**: HTML autocontenido (single-file, sin dependencias locales)
- **Imágenes**: 5 fotografías embebidas en base64 (recortadas y optimizadas a JPEG ~80%)
- **Peso total**: ~401 KB
- **CDN externo**: jsPDF 2.5.1 (generación de PDF)
- **Google Fonts**: Playfair Display, Source Sans 3, Caveat

#### Sistema de hotspots

Los hotspots (`.hs`) están implementados como elementos `position:absolute` dentro de contenedores `.hs-container` superpuestos sobre las imágenes. Características:

- **Capas toggleables**: botones "Significante" / "Significado" controlan la visibilidad mediante la clase `.layer-visible`
- **Atributos data**: cada hotspot almacena `data-tag`, `data-title`, `data-desc`, `data-color` (sin HTML interno)
- **Animación**: pulso CSS (`@keyframes hsPulse`) con `box-shadow` animado
- **Accesibilidad táctil**: evento `touchstart` con cierre automático a 3.5s

#### Tooltip global (`#g-tip`)

Implementado como elemento único en el `<body>` con `position:fixed`:

```
Creación: una sola vez al cargar el DOM
Posicionamiento: getBoundingClientRect() + cálculo dinámico
Preferencia: encima del hotspot (clase .above)
Fallback: debajo si no hay espacio (clase .below)
Clamping: Math.max/min contra bordes del viewport (8px de margen)
z-index: 99999
```

Este enfoque garantiza que el tooltip **nunca sea cortado** por contenedores padre con `overflow:hidden`.

#### Sistema de capas

```javascript
toggleLayer(btn, stageId)
  → Lee botones activos del .layer-bar padre
  → Filtra hotspots del contenedor #hs-{stageId}
  → Aplica/remueve clase .layer-visible
```

Permite activar ambas capas simultáneamente o una sola.

#### Barra de progreso

Calcula porcentaje sobre 10 campos obligatorios:
- 2 datos del estudiante (nombre, correo)
- 4 preguntas cerradas (q1, q2, q3, q5r)
- 2 preguntas abiertas (q4, q6)
- 2 ejercicios de desarrollo (ex4, ex5)

Actualización en tiempo real vía listeners `input` y `change`.

#### Validación

- Nombre: no vacío
- Correo: no vacío + contiene `@`
- Preguntas cerradas: al menos una opción seleccionada
- Preguntas abiertas y ejercicios: texto no vacío
- Feedback: toast flotante con mensaje de error

#### Envío por correo

Utiliza protocolo `mailto:` con:
- **Destinatario**: `c12139@utp.edu.pe`
- **CC**: correo del estudiante
- **Subject**: `Semiótica S11 - {nombre}`
- **Body**: texto formateado con todas las respuestas

Se abre el cliente de correo del sistema operativo.

#### Generación de PDF

Usa **jsPDF** para generar un documento A4 con:
- Encabezado con colores UTP (navy + franja roja)
- Datos del estudiante y fecha
- Respuestas del cuestionario
- Respuestas de los ejercicios de desarrollo
- Pie de página con créditos

Nombre del archivo: `Semiotica_S11_{nombre}.pdf`

---

### Paleta de colores UTP

| Variable | Color | Uso |
|----------|-------|-----|
| `--utp-red` | `#C8102E` | Primario, hotspots de significante, acentos |
| `--utp-red-light` | `#E8384F` | Hover, hotspots secundarios |
| `--utp-navy` | `#1B2A4A` | Encabezados, hotspots de significado, footer |
| `--utp-gold` | `#D4A843` | Acentos dorados, barra de progreso |
| `--solved-border` | `#4CAF50` | Borde de ejemplos resueltos |
| `--exercise-border` | `#FF9800` | Borde de ejercicios para desarrollo |

---

### Imágenes fuente

| Archivo | Dimensiones originales | Uso | Recorte |
|---------|----------------------|-----|---------|
| `Redactando.png` | 2000×1090 | Escena 1 | Completa, redimensionada a 800px ancho |
| `Biblioteca.png` | 2000×1090 | Escena 2 | Completa, redimensionada a 800px ancho |
| `Estudiantes.png` | 2000×1090 | Escenas 3, 4, 5 | Recorte superior (cafetería), inferior derecho (cartel), inferior izquierdo (pasillo) |
| `Servicios.png` | 1024×559 | No utilizada | Imagen negra (4KB), posible error de exportación |
| `Proyectos.png` | 1024×559 | No utilizada | Imagen negra (4KB), posible error de exportación |

---

### Accesibilidad y responsive

- **WCAG**: contraste verificado en combinaciones texto/fondo UTP
- **Reduced motion**: `@media (prefers-reduced-motion: reduce)` desactiva animaciones
- **Responsive**: grid de 2 columnas colapsa a 1 en `≤768px`; controles adaptan padding en `≤600px`
- **Táctil**: hotspots responden a `touchstart` con cierre automático
- **Impresión**: `@media print` oculta botones y hotspots, mantiene layout de contenido

---

### Modelo pedagógico

La herramienta sigue una progresión **ejemplo → ejercicio**:

1. **Observación guiada** (3 ejemplos resueltos): el estudiante explora hotspots y lee análisis completos
2. **Práctica con andamiaje** (2 ejercicios): se proporcionan pistas y hotspots parciales; el estudiante completa el análisis
3. **Reflexión teórica** (sección Saussure): conceptos formales como marco de referencia
4. **Evaluación formativa** (cuestionario): preguntas cerradas verifican comprensión; abiertas evalúan capacidad analítica
5. **Registro** (envío + PDF): evidencia de participación para seguimiento docente

Marco teórico base: **Ferdinand de Saussure** — signo lingüístico, significante, significado, arbitrariedad del signo, reciprocidad.

---

### Archivos del proyecto

```
semiotica-s11-v2.html    Herramienta interactiva (archivo único, 401 KB)
README.md                Este documento
Redactando.png           Imagen fuente — Escena 1
Biblioteca.png           Imagen fuente — Escena 2
Estudiantes.png          Imagen fuente — Escenas 3, 4, 5
Servicios.png            Imagen fuente — No utilizada (negra)
Proyectos.png            Imagen fuente — No utilizada (negra)
```

---

### Requisitos para el estudiante

- Navegador moderno (Chrome, Firefox, Edge, Safari)
- Conexión a internet (carga de fuentes Google y jsPDF CDN)
- Cliente de correo configurado (para envío vía `mailto:`)

---

### Licencia y créditos

Desarrollado por **d3magindesign** para uso educativo en la Universidad Tecnológica del Perú.

Docente: Mg. Mario Rafael Quiroz Martínez  
Curso: Semiótica de la Imagen · Ciclo 2026-I · Semana 11
