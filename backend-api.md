# Especificación y Arquitectura del Backend API - Turnos_ya

El módulo **Backend API** es el motor central del sistema **Turnos_ya**. Proporciona una interfaz RESTful robusta y escalable que sirve de backend unificado tanto para la aplicación Web como para la aplicación Móvil.

---

## 🛠️ Stack Tecnológico

| Tecnología | Rol y Justificación |
| :--- | :--- |
| **Node.js (v20+ LTS)** | Entorno de ejecución en servidor de alto rendimiento y bajo consumo |
| **TypeScript (v5.1+)** | Prevención de errores en tiempo de compilación y tipado estricto de extremo a extremo |
| **Express.js (v4.18)** | Framework web para el enrutamiento HTTP y middlewares |
| **Prisma ORM (v5.22)** | Mapper objeto-relacional con tipado fuertemente acoplado al esquema de MySQL |
| **MySQL 8.0** | Sistema de gestión de base de datos relacional ACID |
| **JWT (jsonwebtoken)** | Mecanismo de autenticación sin estado mediante JSON Web Tokens |
| **Google OAuth 2.0** | Autenticación federada para inicio de sesión en un solo clic |
| **Bcrypt** | Algoritmo de hashing seguro para almacenamiento de credenciales con salt rounds |
| **Nodemailer (v10.0+)** | Envío de correos electrónicos transaccionales mediante SMTP (Gmail) |
| **Zod (v3.21)** | Validación y sanitización estricta de esquemas de entrada en payloads HTTP |
| **Helmet & CORS** | Seguridad en cabeceras HTTP y control de acceso cruzado entre orígenes |
| **Jest (v30+) & ts-jest** | Framework de pruebas unitarias automatizadas para la lógica de negocio |

---

## 🏗️ Arquitectura de 3 Capas (Clean / Hexagonal Architecture)

El backend implementa una **Arquitectura Limpia estricta** con **Inversión de Dependencias (Dependency Inversion Principle - DIP)** y el **Patrón Repositorio (Repository Pattern)**:

```text
               ┌────────────────────────────────────────────────────────┐
               │                     Cliente HTTP                       │
               └───────────────────────────┬────────────────────────────┘
                                           │
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 1. CAPA DE PRESENTACIÓN / CONTROLADORES (Presentation Layer)                            │
│    • src/routes/        : Declaración de rutas Express (v1 y alias directos).           │
│    • src/middlewares/   : Autenticación JWT (authGuard), logging y RFC 7807 handler.   │
│    • src/validators/    : Esquemas Zod para validación y tipado de entrada.             │
│    • src/controllers/   : Orquestación de peticiones, status HTTP y respuestas JSON.    │
└──────────────────────────────────────────┬──────────────────────────────────────────────┘
                                           │  Invoca (Inyección de dependencias)
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 2. CAPA DE LÓGICA DE NEGOCIO (Domain & Business Logic Layer)                            │
│    • src/services/      : Reglas de negocio (hash bcrypt, generación de JWT, lógica     │
│                           de Hold Tokens, verificación de email, recordatorios).       │
│    • src/container.ts   : Contenedor IoC donde se instancian e inyectan servicios.       │
│    • src/utils/         : Helpers transversales (keepAlive, paginación normalizada).    │
└──────────────────────────────────────────┬──────────────────────────────────────────────┘
                                           │  Consume Interfaces (IUserRepository, etc.)
                                           ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│ 3. CAPA DE PERSISTENCIA Y EXTERNOS (Data Access & Infrastructure)                       │
│    • src/interfaces/    : Contratos e interfaces abstractas (desacoplan el ORM).        │
│    • src/repositories/  : Implementaciones de acceso a datos usando Prisma Client.      │
│    • src/config/        : Singleton de PrismaClient y ensureDatabaseSchema().           │
│    • src/infrastructure/: Despacho de Webhooks HTTP salientes.                          │
│    • src/services/email : Despacho de emails transaccionales HTML con Nodemailer.       │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

### Principios Clave de Diseño

1. **Singleton de Prisma**: `src/config/prisma.ts` gestiona una única instancia de `PrismaClient` compartida, previniendo la proliferación de conexiones abiertas hacia MySQL.
2. **Desacoplamiento con Interfaces**: Los servicios de negocio no dependen directamente de Prisma, sino de interfaces abstractas (`IUserRepository`, `IBusinessRepository`, `IAppointmentRepository`, etc.).
3. **Contenedor de Inyección de Dependencias (IoC)**: `src/container.ts` ensambla las piezas del sistema, inyectando los repositorios en los servicios y los servicios en los controladores.
4. **Controladores Libres de Lógica de Negocio**: Los controladores se limitan a recibir parámetros, invocar validaciones de Zod, delegar al servicio correspondiente y responder con códigos HTTP semánticos.
5. **Manejo Centralizado de Errores RFC 7807**: Middleware `errorHandler.ts` que normaliza excepciones conocidas (`AppError`), violaciones de unicidad de Prisma y errores de validación de Zod bajo la estructura internacional de *Problem Details for HTTP APIs*.

---

## 📊 Modelo de Datos (Prisma / MySQL)

- **`User`**: Usuarios registrados en la plataforma. Poseen campos de rol (`client`, `owner`, `administrator`), hash de contraseña, preferencias de notificación (`emailNotifications`, `whatsappNotifications`), estado de verificación de correo (`isEmailVerified`, `verificationToken`, `verificationCode`) y tokens de reseteo de clave.
- **`Business`**: Comercios registrados por prestadores (`ownerId`). Contiene información de nombre, slug, categoría, dirección física, coordenadas geográficas (`latitude`, `longitude`) y configuración de `webhookUrl`.
- **`Service`**: Prestaciones ofrecidas por cada comercio, con duración en minutos, precio, margen de colchón (*buffer time*) y políticas de cancelación.
- **`Schedule`**: Matriz semanal de atención con día de la semana (`dayOfWeek` 0-6), hora de inicio (`startTime`) y hora de fin (`endTime`).
- **`Appointment`**: Registro de turnos. Incluye estados (`PENDING`, `CONFIRMED`, `IN_PROGRESS`, `COMPLETED`, `CANCELLED`, `NO_SHOW`), campos para el bloqueo temporal transaccional (`holdToken`, `holdExpiresAt`), motivo de cancelación y auditoría de usuario cancelador.
- **`Review`**: Calificaciones numéricas (1 a 5 estrellas) y comentarios asociados a un turno completado y al comercio.
- **`Notification`**: Alertas in-app dirigidas al usuario, con título, mensaje, tipo y estado de lectura (`isRead`).

---

## 📁 Estructura del Código

```text
Turnos_ya_backend-api-main/
├── prisma/
│   ├── schema.prisma                 # Modelo de datos Prisma
│   └── seeds/index.ts                # Semillero inicial de usuarios, comercios y servicios
├── scripts/
│   ├── check-db.ts                   # Comprobación de estado MySQL
│   ├── grant-admin.ts                # Asignación de rol de administrador por correo
│   └── sync-businesses-to-prod.ts    # Sincronización de comercios semilla a producción
├── src/
│   ├── config/
│   │   └── prisma.ts                 # Singleton PrismaClient y ensureDatabaseSchema()
│   ├── container.ts                  # Inversión de dependencias (IoC Container)
│   ├── controllers/                  # Controladores HTTP
│   │   ├── admin.controller.ts
│   │   ├── appointment.controller.ts
│   │   ├── auth.controller.ts
│   │   ├── business.controller.ts
│   │   ├── category.controller.ts
│   │   ├── notification.controller.ts
│   │   ├── review.controller.ts
│   │   ├── schedule.controller.ts
│   │   └── service.controller.ts
│   ├── infrastructure/
│   │   └── webhook.service.ts
│   ├── interfaces/                   # Contratos de repositorios (DIP)
│   │   ├── appointment.interface.ts
│   │   ├── business.interface.ts
│   │   ├── schedule.interface.ts
│   │   ├── service.interface.ts
│   │   └── user.interface.ts
│   ├── middlewares/
│   │   ├── authGuard.ts              # Middleware JWT y RBAC
│   │   ├── errorHandler.ts           # Captura centralizada RFC 7807
│   │   └── requestLogger.ts          # Logging en consola
│   ├── repositories/                 # Implementaciones Prisma
│   │   ├── prisma-appointment.repository.ts
│   │   ├── prisma-business.repository.ts
│   │   ├── prisma-schedule.repository.ts
│   │   ├── prisma-service.repository.ts
│   │   └── prisma-user.repository.ts
│   ├── routes/                       # Enrutadores Express
│   │   ├── admin.routes.ts
│   │   ├── appointment.routes.ts
│   │   ├── auth.routes.ts
│   │   ├── business.routes.ts
│   │   ├── category.routes.ts
│   │   ├── health.routes.ts
│   │   └── notification.routes.ts
│   ├── services/                     # Lógica de negocio y tests unitarios
│   │   ├── admin.service.ts
│   │   ├── appointment.service.ts & appointment.service.test.ts
│   │   ├── auth.service.ts & auth.service.test.ts
│   │   ├── business.service.ts & business.service.test.ts
│   │   ├── category.service.ts
│   │   ├── email.service.ts          # Servicio de correos Nodemailer SMTP
│   │   ├── notification.service.ts   # Notificaciones y worker de recordatorios
│   │   ├── review.service.ts
│   │   ├── schedule.service.ts
│   │   └── service.service.ts
│   ├── utils/
│   │   ├── keepAlive.ts              # Auto-ping para Render
│   │   └── pagination.ts             # Metadatos de paginación
│   ├── validators/                   # Schemas Zod
│   │   ├── appointment.validator.ts
│   │   ├── auth.validator.ts
│   │   ├── business.validator.ts
│   │   ├── schedule.validator.ts
│   │   └── service.validator.ts
│   └── server.ts                     # Punto de entrada de la aplicación
├── docker-compose.yml
├── jest.config.js
├── package.json
└── tsconfig.json
```

---

## 🔗 Catálogo de Endpoints RESTful

### Autenticación y Cuentas (`/api/v1/auth`)
- `POST /login` — Inicio de sesión con credenciales, retorno de JWT.
- `POST /register` — Registro de usuarios y despacho de email de bienvenida.
- `POST /google` — Autenticación federada mediante token de Google OAuth 2.0.
- `POST /forgot-password` — Solicitud de enlace de recuperación de clave por email.
- `POST /reset-password` — Cambio de contraseña con token temporal de reseteo.
- `GET /me` — Datos del usuario autenticado actual.
- `PATCH /profile` — Actualización de nombre, teléfono y switches de notificación.
- `POST /send-verification-email` — Envío de código de 6 dígitos para validar email.
- `GET /verify-email` & `POST /verify-email` — Validación de email en 1 clic o por código.
- `POST /request-password-change-code` — Despacho de código de 6 dígitos para cambio de contraseña desde el perfil.
- `POST /confirm-password-change` — Confirmación del cambio de contraseña con validación del código.

### Comercios y Catálogo (`/api/v1/businesses`)
- `GET /` — Listado público paginado con filtros.
- `POST /` — Creación de comercio (promoción a rol `owner`).
- `GET /my` — Listado de comercios del usuario autenticado.
- `GET /:id` — Ficha completa del comercio.
- `PATCH /:id` — Edición de datos del comercio.
- `GET /:id/services` — Listado de servicios ofrecidos.
- `POST /:id/services` & `PUT ...` & `DELETE ...` — Gestión de servicios.
- `GET /:id/schedules` & `POST ...` & `DELETE ...` — Gestión de matriz horaria semanal.
- `GET /:id/busy-slots?date=YYYY-MM-DD` — Franjas horarias ocupadas en una fecha.
- `GET /:id/reviews` — Reseñas y valoraciones del comercio.

### Turnos y Reservas (`/api/v1/appointments`)
- `GET /` & `GET /my` — Listado de turnos del usuario (o del comercio si es dueño).
- `GET /:id` — Detalle del turno por identificador.
- `POST /` — Creación / confirmación definitiva del turno.
- `POST /hold` — Solicitud de bloqueo temporal (*Hold Token* de 10 min) para evitar colisiones.
- `PATCH /:id/cancel` — Cancelación de turno con motivo registrado.
- `PATCH /:id/status` — Actualización de estado del turno.
- `POST /:id/reviews` — Calificación y reseña tras completar el servicio.

### Notificaciones (`/api/v1/notifications`)
- `GET /` — Listado de notificaciones in-app del usuario logueado.
- `PATCH /read-all` — Marcar todas las notificaciones como leídas.
- `PATCH /:id/read` — Marcar una notificación como leída.

### Categorías (`/api/v1/categories`)
- `GET /` — Catálogo general de categorías disponibles.
- `GET /:slug` — Comercios asociados a una categoría específica.

### Administración Global (`/api/v1/admin`)
- `GET /users` — Listado de todos los usuarios registrados.
- `PATCH /users/:id/role` — Modificación de rol (`client`, `owner`, `administrator`).
- `DELETE /users/:id` — Eliminación de usuario.
- `GET /stats` — Métricas globales del sistema.
- `GET /appointments` — Visualización global de turnos.

### Salud del Servicio (`/health`)
- `GET /health` & `GET /api/v1/health` — Diagnóstico en vivo de conectividad con MySQL y uptime.

---

## ⏰ Procesos en Segundo Plano (Workers)

1. **`reminderJob` (Recordatorios de Turnos)**:
   - Se ejecuta cada 60 segundos en el proceso del servidor.
   - Analiza reservas en estado `CONFIRMED` en los próximos 65 minutos.
   - Crea una notificación in-app y envía un correo HTML mediante Nodemailer si el usuario tiene `emailNotifications = true`.
2. **`startKeepAlive`**:
   - Envía un ping HTTP interno cada 10 minutos para evitar que el servicio sea suspendido por inactividad en entornos gratuitos como Render.

---

## 🧪 Pruebas Automatizadas (Jest)

El backend cuenta con pruebas unitarias que cubren los flujos más sensibles de la lógica de negocio:
- `auth.service.test.ts`: Validación de registro, hashing de claves, login, generación de JWT y recuperación de contraseñas.
- `appointment.service.test.ts`: Validación de cálculo de disponibilidad, generación y caducidad de Hold Tokens, y prevención de reservas duplicadas.
- `business.service.test.ts`: Control de permisos y asignación correcta de dueños de comercio.
