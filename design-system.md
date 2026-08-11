# Design System: Turnos Ya

Este documento define la identidad visual y los tokens de diseño extraídos de la interfaz legacy (`vistas/`). Esta especificación asegura que los futuros Frontends (Web App y Mobile App) mantengan la estética "Mercado Libre" coherente, limpia y profesional de la aplicación.

## 1. Paleta de Colores (Color Tokens)

La aplicación utiliza un esquema de colores muy limpio y de alto contraste inspirado en e-commerces modernos.

### Colores Base
- **Primary Color:** `#009ee3` (Celeste Mercado Libre - usado en botones principales, navbar, estados activos)
- **Primary Hover:** `#0081bb` (Usado para interacciones y hover en botones)
- **Background App:** `#f5f8fa` (Grisáceo muy claro y limpio)
- **White (Cards/Inputs):** `#ffffff`

### Escala de Grises (Texto y Bordes)
- **Text Main:** `#1e293b` (Color principal de texto en el body)
- **Text Title:** `#0f172a` (Títulos de cards y headings)
- **Text Secondary:** `#64748b` (Subtítulos, descripciones secundarias)
- **Text Disabled/Empty:** `#94a3b8`
- **Border Default:** `#e2e8f0` (Bordes de cards, líneas divisorias)
- **Border Input:** `#cbd5e1` (Bordes de inputs y controles inactivos)

### Colores de Estado (Tokens Semánticos)
Usados para los badges de estado de los Turnos (Appointments) y alertas:
- **Pending / Info (Primary):**
  - Background: `#e6f5fc`
  - Text: `#009ee3`
  - Border: `#cce9f8`
- **Confirmed / Success:**
  - Background: `#e6f7ed`
  - Text: `#00a650`
  - Border: `#ccefdc`
- **Cancelled / Danger:**
  - Background: `#fbebee`
  - Text: `#d93838`
  - Border: `#f6ccd2`
- **Completed / Secondary:**
  - Background: `#f1f5f9`
  - Text: `#475569`
  - Border: `#e2e8f0`
- **Reviews / Rating:** `#ffb800` (Dorado premium)

---

## 2. Tipografía

El sistema utiliza tipografía moderna, geométrica y altamente legible, apoyada enteramente en Google Fonts.

- **Fuente Principal:** `'Plus Jakarta Sans', sans-serif`
- **Pesos Utilizados:**
  - `400` (Regular): Textos de párrafos y descripciones.
  - `500` (Medium): Textos secundarios de UI.
  - `600` (SemiBold): Botones, Badges, Labels de inputs.
  - `700` (Bold): Títulos de Cards y Headings principales.
  - `800` (ExtraBold): Avatares, Logos y números destacados.
- **Tamaños Comunes (Rem):**
  - Textos descriptivos: `0.88rem` a `0.95rem`
  - Textos base: `1rem`
  - Títulos de tarjeta: `1.05rem`

---

## 3. Componentes UI (Snippets de Referencia)

Se basan en las clases estructurales de Bootstrap 5, pero fuertemente sobreescritos con CSS personalizado para dar el aspecto "Premium SaaS".

### Botones Principales (`.btn-primary`)
Bordes redondeados a 8px, con un efecto sutil de elevación en hover.
```css
.btn-primary {
    background-color: #009ee3;
    border: none;
    font-weight: 600;
    border-radius: 8px;
    transition: all 0.18s ease;
}
.btn-primary:hover {
    background-color: #0081bb;
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(0, 158, 227, 0.15);
}
```

### Controles de Formulario (`.form-control`, `.form-select`)
```css
.form-control, .form-select {
    border-radius: 8px;
    border: 1px solid #cbd5e1;
    padding: 10px 14px;
    font-size: 0.95rem;
    transition: all 0.18s ease;
}
.form-control:focus, .form-select:focus {
    border-color: #009ee3;
    box-shadow: 0 0 0 3px rgba(0, 158, 227, 0.12);
    outline: none;
}
```

### Tarjetas de Negocios (`.ml-card`)
Tarjetas interactivas con elevación pronunciada en hover (Mercado Libre style).
```css
.ml-card {
    background: #ffffff;
    border-radius: 14px;
    border: 1px solid #e2e8f0;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.02);
    transition: all 0.22s cubic-bezier(0.4, 0, 0.2, 1);
    cursor: pointer;
}
.ml-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 10px 24px rgba(0, 158, 227, 0.08);
    border-color: #009ee3;
}
```

### Badges de Estado (Pastel style)
Forma de píldora redondeada (20px), combinando fondo pastel, borde ligero y texto intenso.
```css
.badge-status {
    font-weight: 600;
    padding: 6px 12px;
    border-radius: 20px;
}
/* Variante Success */
.badge-success { background-color: #e6f7ed; color: #00a650; border: 1px solid #ccefdc; }
/* Variante Danger */
.badge-danger { background-color: #fbebee; color: #d93838; border: 1px solid #f6ccd2; }
```

### Time Slots (`.btn-slot-pill`)
Botones seleccionables para elegir horarios en la reserva.
```css
.btn-slot-pill {
    background-color: #ffffff;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    color: #334155;
    font-weight: 600;
    padding: 8px 12px;
    transition: all 0.18s ease;
}
.btn-slot-pill:hover {
    border-color: #009ee3;
    color: #009ee3;
    background-color: #f6fbfd;
}
.btn-slot-pill.active {
    background-color: #009ee3;
    color: #ffffff;
    box-shadow: 0 4px 10px rgba(0, 158, 227, 0.2);
}
```

---

## 4. Disposición / Layout
- **Estructura Base:** El layout confía enteramente en la rejilla flexible de Bootstrap 5 (`.container`, `.row`, `.col-*`).
- **Navbar:** Sticky-top con `box-shadow: 0 4px 12px rgba(0, 158, 227, 0.08);`.
- **Modales/Dialogs:** No sobre-estilizados, utilizan el layout por defecto de Bootstrap con bordes redondeados adaptados a 14px (`.custom-card` behavior).
- **Sombras:** El sistema de sombras se mantiene muy sutil para dar la impresión de un diseño "plano pero profundo" (Soft UI). Sombras oscuras difuminadas para hover, y sombras apenas perceptibles (0.02 de opacidad) para estados de reposo.
