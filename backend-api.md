# Especificación y Arquitectura del Backend API - Turnos_ya

El módulo **Backend API** es el motor principal del sistema **Turnos_ya**. Proporciona una interfaz RESTful robusta y escalable que sirve de backend unificado tanto para la aplicación Web como para la futura aplicación Móvil.

---

## 🛠️ Stack Tecnológico

| Tecnología | Descripción / Rol |
| :--- | :--- |
| **Node.js** | Entorno de ejecución en servidor (v20+) |
| **TypeScript** | Lenguaje tipado para mantener consistencia y prevenir errores de tipo |
| **Express.js** | Framework web minimalist y flexible para el enrutamiento HTTP |
| **Prisma ORM** | Mapper objeto-relacional para interacción fuertemente tipada con MySQL |
| **MySQL** | Sistema de gestión de base de datos relacional |
| **JWT (jsonwebtoken)** | Mecanismo de autenticación mediante JSON Web Tokens |
| **Zod** | Librería de validación de esquemas y sanitización de entrada |
| **Helmet & CORS** | Seguridad de encabezados HTTP y control de acceso de origen cruzado |

---

## 🏗️ Arquitectura de Capas

El backend adopta una arquitectura en capas limpia (*Clean Layered Architecture*):

1. **Capa de Enrutamiento (`src/routes/`)**: Captura las solicitudes HTTP y aplica middlewares de seguridad/validación.
2. **Capa de Controladores (`src/controllers/`)**: Procesa los parámetros de entrada, invoca las reglas de negocio y construye la respuesta HTTP.
3. **Capa de Validadores (`src/validators/`)**: Garantiza la integridad de los datos recibidos mediante esquemas Zod antes de ser procesados.
4. **Capa de Servicios (`src/services/`)**: Aloja lógica transversal como la notificación por webhooks a sistemas externos.
5. **Capa de Persistencia (`prisma/schema.prisma`)**: Define la estructura relacional de los modelos `User`, `Business`, `Service`, `Schedule` y `Appointment`.

---

## 📊 Modelo de Datos (Prisma / MySQL)

- **`User`**: Usuarios registrados con roles (`client`, `owner`, `administrator`).
- **`Business`**: Comercios o locales registrados por los dueños (`ownerId`). Incluye coordenadas geográficas y configuración de webhooks.
- **`Service`**: Servicios prestados por un comercio (duración en minutos, precio, estado de aprobación `pending`/`approved`/`rejected`).
- **`Schedule`**: Matriz de horarios semanales por día y rango horario (`dayOfWeek`, `startTime`, `endTime`).
- **`Appointment`**: Registros de reservas con campos de estado (`PENDING`, `CONFIRMED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED`, `NO_SHOW`), campos de *Hold* temporal (`holdToken`, `holdExpiresAt`) y datos de cancelación.

---

## 🔗 Principales Endpoints de la API

### Autenticación (`/api/auth`)
- `POST /api/auth/register` - Registro de nuevos usuarios.
- `POST /api/auth/login` - Inicio de sesión y devolución de JWT.
- `GET /api/auth/profile` - Obtención del perfil del usuario autenticado.

### Comercios y Servicios (`/api/businesses`)
- `GET /api/businesses` - Búsqueda y listado de comercios filtrados por categoría.
- `POST /api/businesses` - Creación de un nuevo comercio (Rol: `owner`).
- `GET /api/businesses/:id/services` - Listado de servicios de un comercio.
- `POST /api/businesses/:id/services` - Alta de un nuevo servicio.
- `GET /api/businesses/:id/schedules` - Consulta de horarios de atención.

### Turnos y Reservas (`/api/appointments`)
- `POST /api/appointments/hold` - Solicitud de reserva temporal (*Hold Token*).
- `POST /api/appointments` - Confirmación definitiva de la reserva.
- `GET /api/appointments` - Listado de turnos (filtrados por cliente o dueño de comercio).
- `PATCH /api/appointments/:id/cancel` - Cancelación de un turno.

---

## 📁 Estructura del Código

```text
Turnos_ya_backend-api-main/
├── prisma/
│   └── schema.prisma
├── src/
│   ├── controllers/
│   ├── middlewares/
│   ├── routes/
│   ├── services/
│   ├── validators/
│   └── server.ts
├── _legacy_php/
├── seed.ts
└── grant-admin.ts
```
