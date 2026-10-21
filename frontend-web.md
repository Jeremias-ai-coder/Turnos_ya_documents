# Especificación y Arquitectura del Frontend Web - Turnos_ya

El módulo **Frontend Web** de **Turnos_ya** es una aplicación de página única (Single Page Application - SPA) construida con **React 19**, **Vite 8** y **TypeScript**. Ofrece una experiencia de usuario moderna, fluida e intuitiva tanto para clientes finales como para prestadores de servicios y administradores globales.

---

## 🛠️ Stack Tecnológico

| Tecnología | Descripción / Rol |
| :--- | :--- |
| **React 19 (v19.2)** | Biblioteca base para desarrollo de la interfaz basada en componentes funcionales |
| **Vite 8 (v8.2)** | Entorno de desarrollo ultrarrápido y empaquetador de módulos ESM |
| **TypeScript (v6.0)** | Sistema de tipado estático para asegurar contratos y prevenir errores de interfaz |
| **React Router DOM 7 (v7.18)** | Manejo de rutas del lado del cliente, navegación declarativa y protección de vistas |
| **Axios (v1.19)** | Cliente HTTP con interceptores automáticos para inyección de JWT Bearer |
| **Google Identity Services** | Integración nativa con Google OAuth 2.0 (botón oficial y One-Tap) |
| **Lucide React (v1.31)** | Librería de íconos vectoriales modernos |
| **Date-fns (v4.4)** | Manejo y formateo de fechas con localización en español |
| **Vitest (v5.0)** | Framework de pruebas unitarias de alto rendimiento |
| **Oxlint** | Linter de código ultraveloz |
| **CSS System Variables** | Sistema de diseño propio mediante variables CSS (`variables.css`) y clases globales (`global.css`) |

---

## 💻 Módulos de la Aplicación (Pantallas y Flujos)

### 1. Zona Pública y de Clientes
- **Portada (`Home.tsx`)**:
  - Banner interactivo de bienvenida y buscador en tiempo real.
  - Filtro dinámico por categorías de negocio (Barbería, Estética, Salud, Deportes, etc.).
  - Detección de geolocalización del navegador y cálculo de distancia en kilómetros hacia cada comercio mediante la fórmula de Haversine.
- **Ficha del Comercio (`BusinessDetail.tsx`)**:
  - Información detallada del negocio, dirección física, mapa y horarios de atención.
  - Catálogo de servicios con precios, duraciones y botón de reserva directa.
  - Valoraciones y reseñas de clientes.
- **Asistente de Reserva en 3 Pasos (`Booking.tsx`)**:
  - Paso 1: Selección del servicio y confirmación de duración/precio.
  - Paso 2: Elección de fecha en calendario y franja horaria disponible (*wall-clock slots*).
  - Paso 3: Generación del **Hold Token** (bloqueo transaccional de 10 min para prevenir colisiones) y confirmación final de la cita.
- **Mis Turnos (`MyAppointments.tsx`)**:
  - Pestañas organizadas en turnos **Próximos** y turnos **Pasados / Historial**.
  - Indicadores semánticos de estado pastel (`CONFIRMED`, `PENDING`, `CANCELLED`, `COMPLETED`).
  - Modal para cancelación de turnos con registro de motivo.
- **Perfil de Usuario (`Profile.tsx`)**:
  - Visualización y edición de datos de contacto (nombre, teléfono).
  - **Panel de Verificación de Email**: Envío y validación interactiva de código numérico de 6 dígitos con modo QA para desarrollo.
  - **Cambio de Contraseña Seguro (2FA)**: Solicita la clave actual y un código de seguridad de 6 dígitos despachado al correo antes de aplicar la modificación.
  - Configuración de canales de alerta (switches para notificaciones por Email y WhatsApp).
- **Recuperación de Contraseña (`RecoverPassword.tsx` & `ResetPassword.tsx`)**:
  - Formulario de solicitud de enlace de recuperación enviado por correo.
  - Pantalla para ingresar la nueva contraseña validando el token temporal de un solo uso.
- **Verificación de Email en 1 Clic (`VerifyEmail.tsx`)**:
  - Ruta directa `/verify-email?token=...` que valida la cuenta automáticamente al hacer clic en el enlace del correo.

### 2. Zona de Prestadores / Dueños (`Dashboard.tsx`)
- **Gestión de Agenda**: Visualización diaria y semanal de turnos recibidos, con actualización rápida de estado (`CONFIRMED`, `COMPLETED`, `CANCELLED`).
- **Catálogo de Servicios**: Alta, edición y eliminación de servicios con tarifa, duración y tiempo de colchón (*buffer time*).
- **Horarios de Atención**: Configuración flexible de los días de la semana y rangos horarios de apertura y cierre.

### 3. Zona de Administración Global (`SystemAdmin.tsx`)
- **Tablero de Supervisión**: Vista general de métricas del sistema (número total de usuarios, turnos agendados y comercios registrados).
- **Control de Usuarios**: Listado completo de usuarios con capacidad de modificar su rol (`client`, `owner`, `administrator`) o suspender la cuenta.

---

## 🔔 Centro de Notificaciones en Tiempo Real

El componente `NotificationBell.tsx` en el Header:
- Consulta periódicamente las alertas del usuario autenticado.
- Muestra una insignia con el conteo de notificaciones no leídas.
- Ofrece un menú desplegable con detalles de recordatorios de turnos, confirmaciones y cancelaciones, junto con un botón para marcar todas como leídas.

---

## 🔑 Gestión de Estado y Autenticación (`AuthContext.tsx`)

Mantiene de forma centralizada:
- Estado del usuario autenticado (`user`, `token`, `role`, `isAuthenticated`, `isEmailVerified`).
- Persistencia automática del Token JWT en `localStorage`.
- Métodos `login()`, `register()`, `googleAuth()`, `logout()`, `updateProfile()` y actualización de estado de verificación.

---

## 🎨 Sistema de Diseño y Estilos

El diseño visual adopta una estética limpia y de alto contraste ("Estilo Mercado Libre") sin requerir frameworks CSS pesados:
- **`src/styles/variables.css`**: Define la paleta de colores (`#009ee3` primario, fondos `#f5f8fa`, texto `#1e293b`), sombras suaves, radios de borde (`8px` / `12px`) y tipografía `Plus Jakarta Sans`.
- **`src/styles/global.css`**: Clases utilitarias, cards con elevación (`.ml-card`), píldoras horarias (`.btn-slot-pill`), badges semánticos pastel y animaciones fluidas.

---

## 📁 Estructura del Código

```text
Turnos_ya_frontend_web-main/
├── public/
├── src/
│   ├── components/
│   │   ├── common/
│   │   ├── GoogleSignInButton.tsx
│   │   ├── Layout.tsx
│   │   └── NotificationBell.tsx
│   ├── context/
│   │   └── AuthContext.tsx
│   ├── pages/
│   │   ├── Booking.tsx
│   │   ├── BusinessDetail.tsx
│   │   ├── Dashboard.tsx
│   │   ├── Home.tsx
│   │   ├── Login.tsx
│   │   ├── MyAppointments.tsx
│   │   ├── Profile.tsx
│   │   ├── RecoverPassword.tsx
│   │   ├── Register.tsx
│   │   ├── ResetPassword.tsx
│   │   ├── SystemAdmin.tsx
│   │   └── VerifyEmail.tsx
│   ├── services/
│   │   ├── api.ts
│   │   └── api.test.ts
│   ├── styles/
│   │   ├── global.css
│   │   └── variables.css
│   ├── types/
│   ├── utils/
│   │   ├── businessFilterHelper.ts & .test.ts
│   │   ├── locationHelper.ts & .test.ts
│   │   └── timeHelper.ts & .test.ts
│   ├── App.tsx
│   └── main.tsx
├── package.json
└── vite.config.ts
```

---

## 🧪 Pruebas Unitarias (Vitest)

El frontend web cuenta con pruebas automatizadas con **Vitest**:
- `locationHelper.test.ts`: Fórmulas de cálculo de distancia en kilómetros y ordenamiento por proximidad.
- `businessFilterHelper.test.ts`: Búsqueda de comercios por palabras clave y categorías.
- `timeHelper.test.ts`: Generación de slots horarios y validaciones temporales.
- `api.test.ts`: Configuración de Axios e interceptores de autenticación JWT.
