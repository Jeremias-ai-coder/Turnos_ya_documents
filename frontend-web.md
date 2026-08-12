# Especificación y Arquitectura del Frontend Web - Turnos_ya

El módulo **Frontend Web** de **Turnos_ya** es una aplicación de página única (Single Page Application - SPA) construida con React 19 y Vite. Ofrece una experiencia de usuario moderna y dinámica tanto para clientes finales como para administradores de comercios.

---

## 🛠️ Stack Tecnológico

| Tecnología | Descripción / Rol |
| :--- | :--- |
| **React 19** | Biblioteca base para desarrollo de la interfaz basada en componentes |
| **Vite 8** | Entorno de desarrollo ultrarrápido y empaquetador de módulos ESM |
| **TypeScript** | Sistema de tipos estático |
| **React Router DOM 7** | Manejo de rutas del lado del cliente y protección de navegación |
| **Axios** | Cliente HTTP con interceptores JWT automáticos |
| **Lucide React** | Librería de íconos vectoriales modernos |
| **CSS System Variables** | Sistema de diseño propio mediante variables CSS (`variables.css`) y clases globales (`global.css`) |

---

## 💻 Módulos de la Aplicación (Pantallas)

### 1. Zona Pública / Clientes
- **Home (`Home.tsx`)**: Pantalla de bienvenida con banner héroe, motor de búsqueda, filtro por categorías de negocio (Barbería, Estética, Salud, etc.) y grid interactivo de comercios destacados.
- **Detalle del Comercio (`BusinessDetail.tsx`)**: Presentación del negocio con ubicación, descripción, fotos, horarios de atención y catálogo de servicios disponibles.
- **Asistente de Reserva (`Booking.tsx`)**: Flujo por pasos para seleccionar servicio, elegir fecha/hora disponible, aplicar *hold token* y confirmar la reserva.
- **Mis Turnos (`MyAppointments.tsx`)**: Panel donde el cliente visualiza sus reservas futuras y pasadas con opción de cancelación.
- **Perfil (`Profile.tsx`)**: Gestión de datos de contacto y configuración de canales de notificación (Email y WhatsApp).

### 2. Zona de Prestadores / Dueños (`Dashboard.tsx`)
- **Gestión de Agenda**: Vista diaria y semanal de turnos solicitados por clientes.
- **Catálogo de Servicios**: Creación y edición de servicios offered (duración, precio, margen entre turnos).
- **Horarios de Atención**: Configuración flexible de turnos de trabajo por día de la semana.

### 3. Zona de Administración (`SystemAdmin.tsx`)
- **Panel Global**: Supervisión de usuarios registrados, estado de comercios y aprobación de nuevos servicios.

---

## 🔑 Gestión de Estado y Autenticación

A través del componente `AuthContext.tsx`, la aplicación mantiene globalmente:
- El estado de la sesión del usuario (`isAuthenticated`, `user`, `role`).
- La persistencia del Token JWT en `localStorage`.
- Los métodos `login()` y `logout()` que notifican a los componentes suscritos.

---

## 🎨 Sistema de Diseño y Estilos

El diseño visual no utiliza dependencias pesadas de terceros como Tailwind, sino un sistema CSS puro altamente optimizado:
- **`src/styles/variables.css`**: Define la paleta de colores (tonos primarios, acentos, superficies oscuras/claras), radios de borde, sombreados y fuentes tipográficas.
- **`src/styles/global.css`**: Proveedor de clases de utilidad, layouts flexbox/grid, animaciones de transición y componentes UI universales (cards, modales, badges de estado).

---

## 📁 Estructura del Código

```text
Turnos_ya_frontend_web-main/
├── src/
│   ├── components/Layout.tsx
│   ├── context/AuthContext.tsx
│   ├── pages/
│   │   ├── Booking.tsx
│   │   ├── BusinessDetail.tsx
│   │   ├── Dashboard.tsx
│   │   ├── Home.tsx
│   │   ├── Login.tsx
│   │   ├── MyAppointments.tsx
│   │   ├── Profile.tsx
│   │   ├── Register.tsx
│   │   └── SystemAdmin.tsx
│   ├── services/api.ts
│   └── styles/
│       ├── global.css
│       └── variables.css
├── index.html
└── vite.config.ts
```
