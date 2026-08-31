# Especificación y Arquitectura del Backend API - Turnos_ya

El módulo **Backend API** es el motor principal del sistema **Turnos_ya**. Proporciona una interfaz RESTful robusta y escalable que sirve de backend unificado tanto para la aplicación Web como para la aplicación Móvil.

---

## 🛠️ Stack Tecnológico

| Tecnología | Descripción / Rol |
| :--- | :--- |
| **Node.js** | Entorno de ejecución en servidor (v20+) |
| **TypeScript** | Lenguaje tipado para consistencia, contratos de interfaces y prevención de errores |
| **Express.js** | Framework web para el enrutamiento HTTP y middlewares |
| **Prisma ORM** | Mapper objeto-relacional para interacción fuertemente tipada con MySQL |
| **MySQL** | Sistema de gestión de base de datos relacional |
| **JWT (jsonwebtoken)** | Mecanismo de autenticación mediante JSON Web Tokens |
| **Bcrypt** | Hashing seguro de contraseñas con salt rounds |
| **Zod** | Librería de validación de esquemas y sanitización estricta de entrada |
| **Helmet & CORS** | Seguridad de encabezados HTTP y control de acceso de origen cruzado |

---

## 🏗️ Arquitectura de 3 Capas (Clean / Layered Architecture)

El backend implementa una **Arquitectura de 3 Capas estricta** con **Inversión de Dependencias (Dependency Inversion Principle - DIP)** y el **Patrón Repositorio (Repository Pattern)**:

```text
               ┌────────────────────────────────────────────────────────┐
               │                     Cliente HTTP                       │
               └───────────────────────────┬────────────────────────────┘
                                           │
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 1. CAPA DE PRESENTACIÓN / CONTROLADORES (Presentation Layer)                            │
│    • src/routes/       : Definición de endpoints y asociación de middlewares.           │
│    • src/middlewares/  : Autenticación JWT (authGuard) y manejo global RFC 7807.       │
│    • src/validators/   : Validación y tipado de payloads de entrada con Zod.            │
│    • src/controllers/  : Procesamiento de req/res, mapeo de DTOs y status HTTP.        │
└──────────────────────────────────────────┬──────────────────────────────────────────────┘
                                           │  Invoca (Inyección de dependencias)
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 2. CAPA DE LÓGICA DE NEGOCIO (Domain & Business Logic Layer)                            │
│    • src/services/     : Reglas de negocio (hash bcrypt, generación JWT, control de     │
│                          bloqueos de turnos "Hold Tokens", validación de roles/dueños). │
│    • src/utils/        : Helpers transversales (paginación normalizada).                │
└──────────────────────────────────────────┬──────────────────────────────────────────────┘
                                           │  Consume Interfaces (IUserRepository, etc.)
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 3. CAPA DE ACCESO A DATOS (Data Access Layer / Persistence)                             │
│    • src/interfaces/   : Contratos e interfaces abstractas (desacoplan el ORM).         │
│    • src/repositories/ : Implementaciones concretas de queries usando Prisma Client.     │
│    • src/config/       : Instancia Singleton de PrismaClient (única en toda la app).    │
└──────────────────────────────────────────┬──────────────────────────────────────────────┘
                                           │
                                           ▼
                                 [ Base de Datos MySQL ]

┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ CAPA DE INFRAESTRUCTURA (Infrastructure / External Services)                            │
│    • src/infrastructure/ : Despacho asíncrono de Webhooks y notificaciones externas.    │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

### Principios Clave de Diseño

1. **Singleton de Prisma**: `src/config/prisma.ts` inicializa una única instancia de `PrismaClient`, evitando la sobrecarga de conexiones y fugas de memoria.
2. **Desacoplamiento con Interfaces**: Los servicios no conocen la implementación de la base de datos; interactúan exclusivamente a través de interfaces (`IUserRepository`, `IBusinessRepository`, `IAppointmentRepository`, etc.).
3. **Contenedor de Inyección de Dependencias**: `src/container.ts` ensambla e inyecta los repositorios en los servicios y los servicios en los controladores.
4. **Controladores Libres de Lógica de Negocio**: Los controladores solo gestionan la capa HTTP (parseo, validación con Zod y envío de respuestas con código de estado adecuado).
5. **Manejo Estandarizado de Errores**: Middleware centralizado (`errorHandler.ts`) alineado con el estándar **RFC 7807 Problem Details for HTTP APIs**.

---

## 📊 Modelo de Datos (Prisma / MySQL)

- **`User`**: Usuarios registrados con roles (`client`, `owner`, `administrator`), credenciales hasheadas y preferencias de notificación.
- **`Business`**: Comercios o locales registrados por los dueños (`ownerId`). Incluye coordenadas geográficas y configuración de `webhookUrl`.
- **`Service`**: Servicios prestados por un comercio (duración en minutos, precio, estado de aprobación `pending`/`approved`/`rejected`).
- **`Schedule`**: Matriz de horarios semanales por día de la semana y rango horario (`dayOfWeek`, `startTime`, `endTime`).
- **`Appointment`**: Registros de reservas con campos de estado (`PENDING`, `CONFIRMED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED`, `NO_SHOW`), campos de *Hold* temporal (`holdToken`, `holdExpiresAt`) y auditoría de cancelaciones.

---

## 📁 Estructura del Código

```text
Turnos_ya_backend-api-main/
├── prisma/
│   └── schema.prisma                 # Definición del esquema relacional
├── src/
│   ├── config/
│   │   └── prisma.ts                 # Instancia Singleton de PrismaClient
│   ├── interfaces/                   # Contratos de repositorios y DTOs (DIP)
│   │   ├── appointment.interface.ts
│   │   ├── business.interface.ts
│   │   ├── schedule.interface.ts
│   │   ├── service.interface.ts
│   │   └── user.interface.ts
│   ├── repositories/                 # Capa de Acceso a Datos (Queries Prisma)
│   │   ├── prisma-appointment.repository.ts
│   │   ├── prisma-business.repository.ts
│   │   ├── prisma-schedule.repository.ts
│   │   ├── prisma-service.repository.ts
│   │   └── prisma-user.repository.ts
│   ├── services/                     # Capa de Lógica de Negocio
│   │   ├── admin.service.ts
│   │   ├── appointment.service.ts
│   │   ├── auth.service.ts
│   │   ├── business.service.ts
│   │   ├── schedule.service.ts
│   │   └── service.service.ts
│   ├── controllers/                  # Capa HTTP (Req / Res)
│   │   ├── admin.controller.ts
│   │   ├── appointment.controller.ts
│   │   ├── auth.controller.ts
│   │   ├── business.controller.ts
│   │   ├── schedule.controller.ts
│   │   └── service.controller.ts
│   ├── routes/                       # Enrutamiento Express
│   │   ├── admin.routes.ts
│   │   ├── appointment.routes.ts
│   │   ├── auth.routes.ts
│   │   ├── business.routes.ts
│   │   └── health.routes.ts
│   ├── infrastructure/               # Servicios e integraciones externas
│   │   └── webhook.service.ts
│   ├── middlewares/                  # AuthGuard y ErrorHandler RFC 7807
│   │   ├── authGuard.ts
│   │   └── errorHandler.ts
│   ├── validators/                   # Esquemas de validación Zod
│   │   ├── appointment.validator.ts
│   │   ├── auth.validator.ts
│   │   ├── business.validator.ts
│   │   ├── schedule.validator.ts
│   │   └── service.validator.ts
│   ├── utils/                        # Utilidades compartidas
│   │   └── pagination.ts
│   ├── container.ts                  # Inversión de Control / Wire-up de DI
│   └── server.ts                     # Punto de entrada de la aplicación
├── package.json
└── tsconfig.json
```

---

## 🔗 Principales Endpoints de la API

### Autenticación (`/api/v1/auth`)
- `POST /api/v1/auth/register` - Registro de nuevos usuarios con contraseña hasheada.
- `POST /api/v1/auth/login` - Inicio de sesión y generación de token JWT.
- `GET /api/v1/auth/me` - Obtención del perfil del usuario autenticado.
- `PATCH /api/v1/auth/profile` - Actualización de perfil y preferencias.

### Comercios y Servicios (`/api/v1/businesses`)
- `GET /api/v1/businesses` - Listado paginado de comercios.
- `POST /api/v1/businesses` - Creación de un nuevo comercio (eleva rol a `owner`).
- `GET /api/v1/businesses/my` - Listado de comercios pertenecientes al usuario actual.
- `GET /api/v1/businesses/:id` - Detalle de un comercio por ID.
- `PATCH /api/v1/businesses/:id` - Edición de datos del comercio (solo dueño o admin).
- `GET /api/v1/businesses/:id/services` - Listado de servicios del comercio.
- `POST /api/v1/businesses/:id/services` - Creación de un servicio para el comercio.
- `DELETE /api/v1/businesses/:id/services/:serviceId` - Eliminación de un servicio.
- `GET /api/v1/businesses/:id/schedules` - Consulta de horarios de atención del comercio.
- `POST /api/v1/businesses/:id/schedules` - Creación de horarios de atención.
- `DELETE /api/v1/businesses/:id/schedules/:scheduleId` - Eliminación de horario.

### Turnos y Reservas (`/api/v1/appointments`)
- `GET /api/v1/appointments` - Listado paginado de turnos del usuario autenticado.
- `POST /api/v1/appointments` - Confirmación directa o por token de reserva.
- `POST /api/v1/appointments/hold` - Solicitud de reserva temporal (*Hold Token* de 10 min).
- `PATCH /api/v1/appointments/:id/cancel` - Cancelación de un turno con motivo y auditoría.

### Administración (`/api/v1/admin`)
- `GET /api/v1/admin/users` - Listado global de usuarios registrados.
- `PATCH /api/v1/admin/users/:id/role` - Modificación del rol de un usuario.
- `DELETE /api/v1/admin/users/:id` - Eliminación de un usuario del sistema.
- `GET /api/v1/admin/stats` - Métricas y estadísticas globales del sistema.
- `GET /api/v1/admin/appointments` - Visualización global de turnos del sistema.

### Monitoreo y Salud (`/api/v1/health` o `/health`)
- `GET /health` - Verificación de disponibilidad de la API y conectividad con la base de datos MySQL.
