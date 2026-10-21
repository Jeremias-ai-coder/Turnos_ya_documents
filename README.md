# 📚 Hub de Documentación & Especificaciones Técnicas — Turnos_ya

Bienvenido al centro principal de documentación, especificaciones de arquitectura y planificación del proyecto **Turnos_ya**. Este repositorio centraliza el **Project Brief**, la **Agenda de Trabajo**, la **Matriz de Roles del Equipo**, los contratos de interfaz basados en **OpenAPI 3.0 (Domain-Driven Design)**, el **Sistema de Diseño Visual** y las guías de arquitectura de cada subsistema del ecosistema.

---

## 📌 Tabla de Contenidos

1. [📋 Project Brief](#-1-project-brief)
   - [Visión y Misión](#visión-y-misión)
   - [Problema Detectado](#problema-detectado)
   - [Propuesta de Solución y Valor](#propuesta-de-solución-y-valor)
   - [Público Objetivo y Rubros](#público-objetivo-y-rubros)
2. [👥 Equipo de Trabajo y Roles](#-2-equipo-de-trabajo-y-definición-de-roles)
3. [🗓️ Agenda de Trabajo y Cronograma](#-3-agenda-de-trabajo-y-cronograma)
4. [🧭 Índice General del Ecosistema](#-4-índice-general-del-ecosistema)
5. [📑 Especificación OpenAPI 3.0 (Modular / DDD)](#-5-especificación-openapi-30-modular--ddd)
6. [🛠️ Herramientas y Scripts del Contrato](#-6-herramientas-y-scripts-para-la-especificación)
7. [🧪 Pruebas de Integración con Postman](#-7-pruebas-con-postman)
8. [📁 Estructura del Repositorio de Documentación](#-8-estructura-del-repositorio-de-documentación)

---

## 📋 1. Project Brief

### Visión y Misión
* **Visión:** Convertirse en la plataforma referente y de máxima agilidad para la reserva y gestión de turnos en tiempo real, inspirada en la simplicidad e inmediatez de los modelos *on-demand* (como el concepto de "Pedidos Ya", aplicado a los turnos y servicios profesionales).
* **Misión:** Transformar y digitalizar la experiencia de reserva tanto para los prestadores de servicios como para los clientes, eliminando barreras de comunicación, esperas innecesarias y desorganización de agendas mediante una plataforma accesible desde la web y dispositivos móviles.

### Problema Detectado
Actualmente, una gran cantidad de comercios y profesionales independientes gestionan su agenda mediante métodos analógicos (cuadernos, planillas estáticas) o canales de mensajería sincrónicos (WhatsApp, llamadas, mensajes directos de Instagram). Esto genera:
1. **Fricción y Demora en la Atención:** El cliente debe esperar a que el negocio responda para conocer los horarios disponibles, perdiendo reservas potenciales fuera del horario comercial.
2. **Superposición de Turnos y Errores Humanos (*Overbooking*):** Falta de sincronización en tiempo real al coordinar citas entre múltiples canales.
3. **Alto Ausentismo (*No-Show*):** Pérdida de ingresos por citas olvidadas debido a la ausencia de recordatorios y confirmaciones automáticas.
4. **Carga Administrativa Excesiva:** Horas dedicadas a responder consultas repetitivas de disponibilidad y precios en lugar de enfocarse en brindar el servicio.

### Propuesta de Solución y Valor
**Turnos_ya** ofrece una solución integral multiplataforma compuesta por una API centralizada, un portal Web SPA interactivo y una aplicación Móvil:
* **Autonomía 24/7 para el Cliente:** Visualización transparente de comercios, catálogo de servicios, precios, duración y franjas horarias disponibles al instante.
* **Bloqueo Temporal de Seguridad (*Hold Tokens*):** Mecanismo transaccional que reserva temporalmente un turno (10 min) mientras el usuario confirma la operación, previniendo colisiones de reservas simultáneas.
* **Geolocalización Inmediata:** Detección de proximidad y cálculo de distancia en kilómetros tanto en la Web como en la App Móvil.
* **Panel de Control Integral para Comercios:** Gestión inteligente de agenda diaria/semanal, configuración de horarios de atención, colchón de tiempo entre citas (*buffer time*), políticas de cancelación y seguimiento de turnos en Web y Móvil.
* **Autenticación Híbrida y Seguridad:** Soporte para registro tradicional, Google OAuth 2.0, verificación de correo progresiva (código de 6 dígitos y token en 1 clic), recuperación de contraseñas y 2FA por correo para cambios de clave.
* **Centro de Notificaciones y Worker de Recordatorios:** Campanita interactiva con contador no leído y worker en segundo plano que despacha correos automáticos 1 hora antes del turno a través de Nodemailer SMTP.
* **Control de Acceso Basado en Roles (RBAC):** Distinción estricta entre Clientes (`client`), Dueños/Prestadores (`owner`) y Administradores de Sistema (`administrator`).

### Público Objetivo y Rubros
* **Fase 1 (MVP de Alta Frecuencia):** Peluquerías, barberías, salones de manicura y centros de estética integral.
* **Fase 2 (Escalabilidad de Rubros):** Consultorios médicos y odontológicos, profesionales de la salud, veterinarias, canchas deportivas, talleres mecánicos y cualquier rubro con atención por turnos.

---

## 👥 2. Equipo de Trabajo y Definición de Roles

| Integrante | Rol Principal | Especialidad / Enfoque | Responsabilidades Clave |
| :--- | :--- | :--- | :--- |
| **Jeremías Collaud** | **Tech Lead & Backend Architect** | Arquitectura de Software, Backend RESTful & Base de Datos | • Diseño e implementación de la Arquitectura Limpia de 3 Capas en Node.js, Express y TypeScript.<br>• Modelado relacional y migraciones con Prisma ORM en MySQL.<br>• Implementación de seguridad: JWT, Google OAuth 2.0, RBAC, hashing bcrypt, verificación de email y 2FA.<br>• Worker asíncrono de recordatorios de turnos (Nodemailer SMTP) y Hold Tokens transaccionales.<br>• Co-diseño del contrato OpenAPI 3.0, suites de pruebas Jest y coordinación general del repositorio. |
| **Emmanuel Román** | **Frontend Web Lead & UI/UX Designer** | Desarrollo Web SPA & Sistema de Diseño Visual | • Arquitectura y desarrollo de la SPA en React 19 + TypeScript + Vite 8.<br>• Implementación del Sistema de Diseño CSS nativo (sin dependencias pesadas).<br>• Flujos de usuario: Buscador interactivo con geolocalización, Asistente de reserva en 3 pasos (*Booking Flow*), gestión de "Mis Turnos" y Perfil.<br>• Centro de notificaciones interactivo (campanita) y autenticación con Google Identity Services.<br>• Dashboard para prestadores y panel global de administración del sistema (`SystemAdmin`). |
| **Miqueas Müller** | **Mobile Lead, QA & Integración de Sistemas** | Desarrollo Móvil & Aseguramiento de Calidad (QA) | • Desarrollo del cliente móvil multiplataforma en React Native + Expo SDK 57 (Android / iOS).<br>• Flujos móviles: Exploración por geolocalización (`expo-location`), reserva táctil con Hold Tokens y panel de Dueño (`OwnerAgendaScreen`, `OwnerServicesScreen`, etc.).<br>• Aseguramiento de calidad (QA): Suites de pruebas automatizadas con Vitest y validación de contrato con Spectral Linter.<br>• Colecciones de pruebas en Postman para validación de endpoints y testing de integración. |

---

## 🗓️ 3. Agenda de Trabajo y Cronograma

```text
  [ Fase 1: Arquitectura Base ] ──▶ [ Fase 2: Backend & Contrato API ] ──▶ [ Fase 3: Frontend Web SPA ]
                                                                                    │
                                  [ Fase 5: Pruebas Integrales & Cierre ] ◀── [ Fase 4: Frontend Mobile ]
```

| Fase / Hito | Alcance y Entregables | Responsables | Estado |
| :--- | :--- | :--- | :---: |
| **Fase 1: Análisis y Arquitectura Base** | • Relevamiento de requerimientos y redacción del *Project Brief*.<br>• Diseño del Modelo Entidad-Relación y definición del stack tecnológico.<br>• Lineamientos visuales iniciales y tokens del sistema de diseño. | Todo el equipo | **Completado** ✅ |
| **Fase 2: Contrato OpenAPI & Backend API** | • Especificación OpenAPI 3.0 estructurada por dominios (DDD).<br>• Servidor Node.js + Express con Clean Architecture (Inversión de dependencias).<br>• Prisma ORM + MySQL, endpoints de Auth, Businesses, Services, Schedules, Appointments y Hold Tokens.<br>• Worker de recordatorios de turnos (Nodemailer SMTP), endpoints de verificación y suites Jest. | Jeremías Collaud<br>Miqueas Müller | **Completado** ✅ |
| **Fase 3: Frontend Web SPA & Paneles** | • Aplicación Web React 19 + TypeScript + Vite 8 operativa.<br>• Sistema de diseño en CSS Vanilla nativo.<br>• Vistas de cliente (Home con geolocalización, Detalle, Reserva en 3 pasos, Perfil, Mis Turnos, Verificación de Email y Recuperación de Clave).<br>• Dashboard para prestadores y panel global de administración (`SystemAdmin`).<br>• Botón de Google OAuth y campanita de notificaciones en tiempo real.<br>• Suite de pruebas unitarias con Vitest. | Emmanuel Román<br>Jeremías Collaud | **Completado** ✅ |
| **Fase 4: Frontend Mobile & Experiencia Nativa** | • Aplicación móvil multiplataforma en React Native + Expo SDK 57.<br>• Consumo de API REST con configuración dinámica de IP para testing en celular físico.<br>• Geolocalización de comercios (`expo-location`) y cálculo de distancia.<br>• Asistente de reserva táctil con Hold Tokens y gestión de turnos.<br>• Panel de administración para dueños integrado en la app móvil (Agenda, Local, Horarios, Servicios).<br>• Suite de pruebas unitarias con Vitest. | Miqueas Müller<br>Emmanuel Román | **Completado** ✅ |
| **Fase 5: Pruebas Integrales, Despliegue & Cierre** | • Pruebas End-to-End (E2E) y de concurrencia de reservas.<br>• Despliegue en entorno de pruebas / producción (Render / Vercel).<br>• Consolidación de manuales técnicos, documentación académica y defensa de proyecto. | Todo el equipo | **En Ejecución** 🎯 |

---

## 🧭 4. Índice General del Ecosistema

| Módulo / Documento | Descripción Técnica | Enlace Directo |
| :--- | :--- | :--- |
| 🖥️ **Backend RESTful API** | Servidor Node.js + Express + Prisma + MySQL. Arquitectura Limpia de 3 capas, Inversión de Dependencias (DIP), Patrón Repositorio, JWT, Google OAuth, Nodemailer SMTP, worker de recordatorios y suites Jest. | [backend-api.md](./backend-api.md) \| [Repositorio Backend](../Turnos_ya_backend-api-main/README.md) |
| 🌐 **Frontend Web SPA** | Aplicación Web cliente construida en React 19 + Vite 8 + TypeScript. Gestión de estado con Context API, Google OAuth, campanita de notificaciones, panel de dueño y administrador, y tests con Vitest. | [frontend-web.md](./frontend-web.md) \| [Repositorio Web](../Turnos_ya_frontend_web-main/README.md) |
| 📱 **Frontend Mobile** | Aplicación móvil multiplataforma en React Native + Expo SDK 57 + TypeScript. Reserva táctil, geolocalización (`expo-location`), panel completo de dueño y tests con Vitest. | [frontend-mobile.md](./frontend-mobile.md) \| [Repositorio Mobile](../Turnos_ya_frontend_mobile-main/README.md) |
| 🎨 **Design System** | Sistema de diseño visual propietario: variables CSS, paleta "Mercado Libre", tipografía Plus Jakarta Sans, escalas de espaciado y componentes modulares. | [design-system.md](./design-system.md) |
| 📑 **Contrato OpenAPI 3.0** | Especificación formal del contrato de la API RESTful particionado en dominios DDD. | [openapi.yaml](./openapi.yaml) |
| 🧪 **Colección Postman** | Colección exportada de endpoints con scripts automáticos para guardar el token JWT en entornos de testing. | [postman_collection.json](./postman_collection.json) |

---

## 📑 5. Especificación OpenAPI 3.0 (Modular / DDD)

Este repositorio contiene la definición formal y canónica de la API RESTful de **Turnos_ya**, diseñada bajo principios de **Domain-Driven Design (DDD)** para garantizar modularidad y mantenibilidad a largo plazo.

### Estructura de la Especificación
* **`openapi.yaml`**: Punto de entrada principal (*entrypoint*) que consolida los componentes y módulos mediante referencias `$ref`.
* **`components/`**: Schemas y objetos transversales reutilizables:
  * `errors.yaml`: Respuestas estandarizadas de error bajo la norma internacional **RFC 7807** (*Problem Details for HTTP APIs*).
  * `pagination.yaml`: Estructura común para paginación de listados (`page`, `limit`, `total`, `totalPages`).
  * `security.yaml`: Esquema de autenticación Bearer JWT (`bearerAuth`).
* **`modules/`**: Agrupación lógica de dominios de negocio:
  * `auth`: Registro, autenticación, perfil, recuperación de contraseñas y verificación.
  * `users`: Administración y consulta de usuarios del sistema.
  * `businesses`: Alta, modificación, geolocalización y consulta pública de comercios.
  * `services`: Catálogo de servicios por comercio (duración, precio, buffer time, políticas).
  * `schedules`: Configuración de matrices de atención horaria semanal.
  * `appointments`: Ciclo de vida de turnos, verificación de disponibilidad, bloqueo temporal (*hold*) y confirmaciones.
  * `staff`: Gestión de profesionales y personal asignado a comercios.
  * `reviews`: Calificaciones, valoraciones y reseñas post-servicio.

---

## 🛠️ 6. Herramientas y Scripts para la Especificación

Los contratos YAML modularizados se validan, compilan y ejecutan localmente mediante herramientas configuradas en `package.json`:

1. **Instalación de Dependencias**:
   ```bash
   npm install
   ```

2. **Previsualización Interactiva de la Documentación (Redocly)**:
   Compila las referencias relativas `$ref` y levanta una interfaz web interactiva para explorar todos los endpoints:
   ```bash
   npm run docs:preview
   ```

3. **Servidor Mock Simulado (Prism)**:
   Inicia un servidor local en `http://localhost:4040` que emula el comportamiento de la API:
   ```bash
   npm run mock
   ```

4. **Validación de Estilo y Reglas (Spectral Linter)**:
   Audita la sintaxis OpenAPI contra reglas estrictas de diseño y calidad de APIs:
   ```bash
   npm run lint
   ```

5. **Generación Automática de Tipos TypeScript**:
   Genera interfaces tipadas estáticas para utilizarlas en el frontend o backend a partir del contrato YAML:
   ```bash
   npm run generate:types
   ```

---

## 🧪 7. Pruebas con Postman

Para verificar el comportamiento de la API y facilitar la integración con los clientes:

1. Abre **Postman** y selecciona la opción **Import**.
2. Selecciona el archivo [`postman_collection.json`](./postman_collection.json) ubicado en la raíz de este repositorio.
3. Ejecuta la petición **Login** dentro de la carpeta `Auth`. El script posterior a la petición guardará automáticamente el JWT resultante en la variable de entorno `{{authToken}}`.
4. Utiliza dicho token para ejecutar las peticiones protegidas (como creación de comercios, generación de bloqueos de turno `Hold Appointment`, etc.).

---

## 📁 8. Estructura del Repositorio de Documentación

```text
Turnos_ya_documents-main/
├── components/                    # Esquemas transversales reutilizables
│   ├── errors.yaml                # Formato estándar de errores (RFC 7807)
│   ├── pagination.yaml            # Metadatos de paginación
│   └── security.yaml              # Esquema de autenticación Bearer JWT
├── modules/                       # Dominios de negocio (DDD)
│   ├── appointments/              # Endpoints y esquemas de turnos y hold tokens
│   ├── auth/                      # Endpoints de autenticación, recuperación y verificación
│   ├── businesses/                # Endpoints de gestión y consulta de comercios
│   ├── reviews/                   # Endpoints de valoraciones y comentarios
│   ├── schedules/                 # Endpoints de configuración de horarios
│   ├── services/                  # Endpoints de catálogo de servicios
│   ├── staff/                     # Endpoints de asignación de personal
│   └── users/                     # Endpoints de administración de usuarios
├── backend-api.md                 # Arquitectura detallada del Backend API
├── design-system.md               # Especificación del Sistema de Diseño CSS
├── frontend-mobile.md             # Especificación técnica del cliente Móvil
├── frontend-web.md                # Especificación técnica del cliente Web SPA
├── openapi.yaml                   # Entrypoint principal del contrato OpenAPI 3.0
├── package.json                   # Scripts de Redocly, Prism y Spectral Linter
├── postman_collection.json        # Colección de integración para Postman
└── README.md                      # Hub de documentación global (este archivo)
```

---

> 💡 **Nota para la Cátedra:** Este repositorio actúa como la **única fuente de verdad (Single Source of Truth - SSOT)** de la arquitectura, diseño y contratos entre subsistemas del proyecto **Turnos_ya**. Para consultar el código fuente en ejecución, remitirse a las carpetas homónimas del ecosistema.