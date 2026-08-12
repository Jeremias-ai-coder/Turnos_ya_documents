# 📚 Hub de Documentación Global - Turnos_ya

Bienvenido al centro principal de documentación y especificaciones técnicas del proyecto **Turnos_ya**. Este repositorio agrupa el índice de arquitectura del ecosistema completo, las especificaciones de API basadas en **OpenAPI 3.0 (DDD)**, el sistema de diseño visual y las guías de cada subsistema.

---

## 🧭 Índice General del Proyecto

Para la exposición y consulta del proyecto, se dispone de las siguientes guías por módulo:

| Módulo / Componente | Descripción | Documentación |
| :--- | :--- | :--- |
| 🖥️ **Backend RESTful API** | Servidor Node.js + Express + Prisma + MySQL (JWT, RBAC, Hold tokens) | [backend-api.md](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_documents-main/backend-api.md) \| [README en Backend](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_backend-api-main/README.md) |
| 🌐 **Frontend Web SPA** | Aplicación Web cliente en React 19 + Vite + TypeScript | [frontend-web.md](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_documents-main/frontend-web.md) \| [README en Web](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_frontend_web-main/README.md) |
| 📱 **Frontend Mobile** | Aplicación Móvil multiplataforma (iOS & Android) | [frontend-mobile.md](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_documents-main/frontend-mobile.md) \| [README en Mobile](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_frontend_mobile-main/README.md) |
| 🎨 **Design System** | Guía de diseño visual, paleta de colores y componentes UI | [design-system.md](file:///c:/xampp/htdocs/Turnos_ya/Turnos_ya_documents-main/design-system.md) |

---

## 📑 Especificación OpenAPI 3.0 (Modular / DDD)

Este repositorio contiene el contrato oficial de la API RESTful para `Turnos_ya`, diseñado bajo una arquitectura orientada a dominios (Domain-Driven Design).

### Estructura de la Especificación
- `openapi.yaml`: El punto de entrada principal (*entrypoint*).
- `components/`: Contiene los esquemas transversales (`errors.yaml` según norma RFC 7807, `pagination.yaml`, `security.yaml`).
- `modules/`: Agrupación lógica de dominios (`auth`, `users`, `businesses`, `services`, `schedules`, `appointments`, `staff`, `reviews`). Cada uno con sus schemas y endpoints independientes.
- `postman_collection.json`: Colección exportada lista para importar en Postman y ejecutar pruebas de integración (incluye scripts de Login para almacenar automáticamente el `authToken`).

---

## 🛠️ Herramientas y Scripts para la Especificación

Dado que los archivos YAML están particionados usando referencias relativas `$ref`, puedes visualizarlos o validarlos con las siguientes herramientas definidas en `package.json`:

1. **Instalar dependencias**:
   ```bash
   npm install
   ```

2. **Previsualizar Documentación en el Navegador (Redocly)**:
   Compila al vuelo los `$ref` y despliega una interfaz interactiva.
   ```bash
   npm run docs:preview
   ```

3. **Servidor Mock Simulado (Prism)**:
   Levanta un servidor simulado en `http://localhost:4040` que responde con datos de prueba válidos según la especificación OpenAPI.
   ```bash
   npm run mock
   ```

4. **Validación de Estilo y Reglas (Spectral Linter)**:
   Valida el cumplimiento estricto del diseño OpenAPI 3.0.
   ```bash
   npm run lint
   ```

5. **Generación Automática de Tipos TypeScript**:
   Genera definiciones de tipos estáticos para TypeScript a partir del contrato YAML.
   ```bash
   npm run generate:types
   ```

---

## 🧪 Pruebas con Postman

1. Importa `postman_collection.json` en Postman.
2. Ejecuta la petición **Login** dentro de la carpeta `Auth`. Esto guardará automáticamente el JWT en la variable `{{authToken}}`.
3. Utiliza dicho token para autenticar las peticiones en los endpoints protegidos (ej. **Hold Appointment**, **Create Business**, etc.).