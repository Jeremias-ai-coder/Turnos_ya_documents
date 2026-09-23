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
* **Bloqueo Temporal de Seguridad (*Hold Tokens*):** Mecanismo transaccional que reserva temporalmente un turno mientras el usuario confirma la operación, previniendo colisiones de reservas simultáneas.
* **Panel de Control Integral para Comercios:** Gestión inteligente de agenda diaria/semanal, configuración de horarios de atención, colchón de tiempo entre citas (*buffer time*), políticas de cancelación y seguimiento de turnos.
* **Control de Acceso Basado en Roles (RBAC):** Seguridad estricta que distingue privilegios entre Clientes (`client`), Dueños/Prestadores (`owner`) y Administradores de Sistema (`administrator`).
* **Arquitectura Escalable y Desacoplada:** Backend con Arquitectura Limpia de 3 Capas, persistencia tipada con Prisma ORM sobre MySQL y contrato oficial estandarizado en OpenAPI 3.0.

### Público Objetivo y Rubros
* **Fase 1 (MVP de Alta Frecuencia):** Peluquerías, barberías, salones de manicura y centros de estética integral.
* **Fase 2 (Escalabilidad de Rubros):** Consultorios médicos y odontológicos, profesionales de la salud, veterinarias, canchas deportivas, talleres mecánicos y cualquier rubro con atención por turnos.

---

## 👥 2. Equipo de Trabajo y Definición de Roles

Para garantizar una ejecución eficiente y organizada, el proyecto distribuye sus responsabilidades técnicas y operativas de la siguiente manera:

| Integrante | Rol Principal | Especialidad / Enfoque | Responsabilidades Clave |
| :--- | :--- | :--- | :--- |
| **Jeremías Collaud** | **Tech Lead & Backend Architect** | Arquitectura de Software, Backend RESTful & Base de Datos | • Diseño e implementación de la Arquitectura de 3 Capas en Node.js, Express y TypeScript.<br>• Modelado relacional y migraciones con Prisma ORM en base de datos MySQL.<br>• Implementación de seguridad: Autenticación JWT, RBAC, hashing bcrypt y manejo uniforme de errores RFC 7807.<br>• Lógica de negocio crítica de reservas concurrentes y gestión de *Hold Tokens*.<br>• Co-diseño del contrato de integración OpenAPI 3.0 y coordinación general del repositorio. |
| **Emmanuel Román** | **Frontend Web Lead & UI/UX Designer** | Desarrollo Web SPA & Sistema de Diseño Visual | • Arquitectura y desarrollo de la aplicación cliente SPA en React 19 + TypeScript + Vite.<br>• Diseño e implementación del *Design System* CSS (paleta cromática, variables CSS globales, tokens de superficie y componentes reutilizables).<br>• Flujos de usuario: Buscador interactivo de comercios, Wizard de reserva paso a paso (*Booking Flow*), gestión de "Mis Turnos" y Perfil.<br>• Desarrollo del panel de gestión comercial (*Dashboard*) para prestadores (agenda, catálogo de servicios y horarios de atención). |
| **Miqueas Müller** | **Mobile Lead, QA & Integración de Sistemas** | Desarrollo Móvil & Aseguramiento de Calidad (QA) | • Planificación y desarrollo del cliente móvil multiplataforma para smartphones (Android / iOS).<br>• Aseguramiento de calidad de software (QA): Diseño y ejecución de suites de pruebas en Postman para validación de la API REST.<br>• Auditoría de conformidad OpenAPI mediante Spectral Linter y configuración de servidores simulados con Prism.<br>• Planificación de servicios periféricos: Geolocalización en mapa interactivo y sistema de alertas/notificaciones push contra el ausentismo (*no-show*). |

---

## 🗓️ 3. Agenda de Trabajo y Cronograma

El proyecto sigue un ciclo iterativo estructurado en fases para validar cada componente antes de su integración final:

```text
  [ Fase 1: Arquitectura Base ] ──▶ [ Fase 2: Backend & Contrato API ] ──▶ [ Fase 3: Frontend Web SPA ]
                                                                                   │
                                  [ Fase 5: Testing Integral & Cierre ] ◀── [ Fase 4: Mobile & Integraciones ]
```

| Fase / Hito | Alcance y Entregables | Responsables | Estado |
| :--- | :--- | :--- | :---: |
| **Fase 1: Análisis y Arquitectura Base** | • Relevamiento de necesidades y redacción del *Project Brief*.<br>• Diseño del Modelo Entidad-Relación y definición del stack tecnológico.<br>• Lineamientos visuales iniciales y tokens del sistema de diseño. | Todo el equipo | **Completado** ✅ |
| **Fase 2: Contrato OpenAPI & Backend API** | • Especificación OpenAPI 3.0 estructurada por dominios (DDD).<br>• Servidor Node.js + Express con arquitectura de 3 capas (Inversión de dependencias).<br>• Prisma ORM + MySQL, endpoints de Auth, Business, Services, Schedules, Appointments y Hold Tokens.<br>• Colección automatizada de Postman y servidores Mock (Prism). | Jeremías Collaud<br>Miqueas Müller | **Completado** ✅ |
| **Fase 3: Frontend Web SPA & Paneles** | • Aplicación Web React 19 + TypeScript + Vite operativa.<br>• Implementación del Sistema de Diseño CSS propio (sin dependencias pesadas).<br>• Vistas de cliente (Home, Detalle, Reserva interactiva, Perfil, Mis Turnos).<br>• Panel de control comercial para prestadores (Dashboard de agenda y servicios). | Emmanuel Román<br>Jeremías Collaud | **En Ajustes Finales** 🚀 |
| **Fase 4: Frontend Mobile & Servicios Periféricos** | • Estructuración del proyecto móvil multiplataforma (Android / iOS).<br>• Integración de consumo de API según contrato OpenAPI.<br>• Geolocalización de comercios en mapa interactivo.<br>• Notificaciones push / recordatorios de turnos programados. | Miqueas Müller<br>Emmanuel Román | **En Planificación / Desarrollo** 📱 |
| **Fase 5: Pruebas Integrales, Despliegue & Cierre** | • Pruebas End-to-End (E2E) y de concurrencia.<br>• Despliegue en entorno de pruebas / producción.<br>• Consolidación de manuales técnicos, documentación académica y defensa de proyecto. | Todo el equipo | **Próximo Hito** 🎯 |

---

## 🧭 4. Índice General del Ecosistema

Para consultar en detalle cada aspecto técnico del sistema, dispones de las siguientes guías específicas:

| Módulo / Documento | Descripción Técnica | Enlace Directo |
| :--- | :--- | :--- |
| 🖥️ **Backend RESTful API** | Servidor Node.js + Express + Prisma + MySQL. Arquitectura de 3 capas, Inversión de Dependencias (DIP), Patrón Repositorio, JWT, RBAC y sistema de *Hold Tokens*. | [backend-api.md](./backend-api.md) \| [Repositorio Backend](../Turnos_ya_backend-api-main/README.md) |
| 🌐 **Frontend Web SPA** | Aplicación Web cliente construida en React 19 + Vite + TypeScript. Gestión de estado con Context API, consumo de endpoints y vistas adaptativas. | [frontend-web.md](./frontend-web.md) \| [Repositorio Web](../Turnos_ya_frontend_web-main/README.md) |
| 📱 **Frontend Mobile** | Aplicación móvil multiplataforma orientada a Android e iOS con enfoque en reserva rápida, mapas y alertas push. | [frontend-mobile.md](./frontend-mobile.md) \| [Repositorio Mobile](../Turnos_ya_frontend_mobile-main/README.md) |
| 🎨 **Design System** | Sistema de diseño visual propietario: variables CSS, paleta de colores, tipografía, escalas de espaciado y componentes modulares. | [design-system.md](./design-system.md) |
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
* **`modules/`**: Agrupación lógica de dominios de negocio, cada uno con sus endpoints y esquemas de datos:
  * `auth`: Registro, autenticación y obtención de perfil del usuario autenticado.
  * `users`: Administración y consulta de usuarios del sistema.
  * `businesses`: Alta, modificación, geolocalización y consulta pública de comercios.
  * `services`: Catálogo de servicios por comercio (duración, precio, buffer time, políticas).
  * `schedules`: Configuración de matrices de atención horaria semanal.
  * `appointments`: Ciclo de vida de turnos, verificación de disponibilidad, bloqueo temporal (*hold*) y confirmaciones.
  * `staff`: Gestión de profesionales y personal asignado a comercios.
  * `reviews`: Calificaciones, valoraciones y reseñas post-servicio.

---

## 🛠️ 6. Herramientas y Scripts para la Especificación

Los contratos YAML modularizados se validan, compilan y ejecutan localmente mediante herramientas de última generación configuradas en `package.json`:

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
   Inicia un servidor local en `http://localhost:4040` que emula el comportamiento de la API y responde datos basados en el contrato:
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
│   ├── auth/                      # Endpoints de autenticación y registro
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