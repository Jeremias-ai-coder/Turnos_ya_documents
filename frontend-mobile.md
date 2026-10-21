# Especificación y Arquitectura del Frontend Mobile - Turnos_ya

El módulo **Frontend Mobile** representa la solución nativa/multiplataforma orientada a smartphones para la plataforma **Turnos_ya**. Desarrollada en **React Native + Expo SDK 57 (TypeScript)**, proporciona una experiencia de reserva ágil e intuitiva en dispositivos móviles Android e iOS, además de un panel móvil completo para prestadores y dueños de negocio.

---

## 🛠️ Stack Tecnológico

| Componente | Tecnología Seleccionada | Justificación |
| :--- | :--- | :--- |
| **Framework Multiplataforma** | **Expo SDK 57 / React Native 0.86** | Desarrollo ágil con acceso directo a APIs nativas y previsualización con Expo Go |
| **Lenguaje** | **TypeScript (v6.0)** | Coherencia de tipos de dominio compartidos con el backend y frontend web |
| **Navegación** | **React Navigation 7** | Navegación nativa fluida con pilas (*Native Stack*) y pestañas inferiores (*Bottom Tabs*) |
| **Cliente HTTP** | **Axios (v1.20)** | Interceptores automáticos de JWT y selector de IP de servidor en tiempo de ejecución |
| **Almacenamiento Local** | **AsyncStorage** | Persistencia segura del token de sesión y configuración de conectividad |
| **Geolocalización** | **`expo-location`** | Detección de coordenadas GPS para calcular la distancia a los negocios cercanos |
| **Iconografía** | **`@expo/vector-icons` (Ionicons)** | Iconos nativos limpios y de alto contraste |
| **Manejo de Fechas** | **`date-fns`** | Localización en español para meses, días de la semana y horas de reloj |
| **Pruebas Automatizadas** | **Vitest** | Validación de utilidades de cálculo de distancia y lógica de turnos |

---

## 🎨 Sistema de Diseño Móvil ("Estilo Mercado Libre")

La aplicación móvil implementa fielmente los tokens del [Sistema de Diseño](./design-system.md):
- **Color Primario:** `#009ee3` (Celeste Mercado Libre) para botones de acción principal, pestañas activas e indicadores.
- **Fondos Limpios:** `#f5f8fa` como color de fondo general de pantalla y `#ffffff` para superficies de tarjetas.
- **Componente `.ml-card`:** Tarjetas con esquinas redondeadas (12px) y sombra difusa para jerarquía visual.
- **Componente `.btn-slot-pill`:** Píldoras táctiles redondeadas para la selección de turnos en el calendario.
- **Badges Semánticos Pastel:** Etiquetas de estado con fondo suave y texto de contraste:
  - Confirmado: `#e6f7ed` (fondo) / `#00a650` (texto).
  - Pendiente: `#e6f5fc` (fondo) / `#009ee3` (texto).
  - Cancelado: `#fbebee` (fondo) / `#d93838` (texto).

---

## 📱 Módulos de la Aplicación y Flujos de Usuario

### 1. Experiencia del Cliente Final
- **Exploración y Cercanía (`HomeScreen.tsx`)**:
  - Buscador dinámico por nombre de comercio o categoría.
  - Selector horizontal con chips de categorías (Barbería, Estética, Salud, Deportes, etc.).
  - Detección de posición geográfica mediante `expo-location` y cálculo de distancia en kilómetros hacia cada comercio mediante la fórmula de Haversine.
- **Detalle del Comercio (`BusinessDetailScreen.tsx`)**:
  - Presentación del local, dirección física, horarios y listado de servicios con tarifas y duración.
  - Valoraciones de clientes anteriores.
- **Asistente Táctil de Reserva en 3 Pasos (`BookingWizardScreen.tsx`)**:
  - **Paso 1 (Servicio):** Selección del servicio a contratar.
  - **Paso 2 (Fecha y Hora):** Calendario con visualización de franjas horarias disponibles organizadas en píldoras táctiles.
  - **Paso 3 (Hold & Confirmación):** Bloqueo transaccional temporal (*Hold Token* de 10 min) para garantizar el turno mientras se confirma la cita.
- **Mis Turnos (`MyAppointmentsScreen.tsx`)**:
  - Pestañas organizadas en turnos **Próximos** e **Historial**.
  - Cancelación rápida del turno con confirmación modal.
- **Perfil y Conectividad (`ProfileScreen.tsx`)**:
  - Datos de usuario, switches de notificación y panel para configurar la IP del servidor API en caliente.

### 2. Panel Móvil para Dueños de Comercios (`screens/owner/`)
- **Agenda del Negocio (`OwnerAgendaScreen.tsx`)**:
  - Navegación diaria entre fechas para visualizar citas programadas.
  - Cambio inmediato de estado de cada turno (`CONFIRMED`, `COMPLETED`, `CANCELLED`).
- **Configuración del Local (`OwnerBusinessScreen.tsx`)**:
  - Edición del nombre, categoría, dirección física, coordenadas de latitud/longitud y URL de webhook para notificaciones externas.
- **Matriz de Horarios Semanales (`OwnerSchedulesScreen.tsx`)**:
  - Alta y eliminación de franjas de apertura y cierre para cada día de la semana.
- **Catálogo de Servicios (`OwnerServicesScreen.tsx`)**:
  - Creación y actualización de servicios con precio, duración en minutos y tiempo de colchón.

---

## 📁 Estructura del Proyecto

```text
Turnos_ya_frontend_mobile-main/
├── src/
│   ├── components/
│   │   ├── business/
│   │   │   └── BusinessCard.tsx       # Tarjeta de comercio con cálculo de distancia
│   │   └── common/
│   │       ├── Button.tsx             # Botones primario, outline, danger
│   │       ├── Card.tsx               # Tarjeta con elevación (.ml-card)
│   │       ├── SlotPill.tsx           # Píldora horaria táctil (.btn-slot-pill)
│   │       └── StatusBadge.tsx        # Badges pastel de estado
│   ├── constants/
│   │   └── theme.ts                   # Tokens de diseño visual
│   ├── context/
│   │   └── AuthContext.tsx            # Autenticación global y sincronización con el backend
│   ├── navigation/
│   │   └── RootNavigator.tsx          # Bottom Tabs + Stack Navigator
│   ├── screens/
│   │   ├── appointments/
│   │   │   └── MyAppointmentsScreen.tsx
│   │   ├── auth/
│   │   │   ├── LoginScreen.tsx
│   │   │   └── RegisterScreen.tsx
│   │   ├── booking/
│   │   │   └── BookingWizardScreen.tsx
│   │   ├── business/
│   │   │   └── BusinessDetailScreen.tsx
│   │   ├── home/
│   │   │   └── HomeScreen.tsx
│   │   ├── owner/
│   │   │   ├── OwnerAgendaScreen.tsx
│   │   │   ├── OwnerBusinessScreen.tsx
│   │   │   ├── OwnerSchedulesScreen.tsx
│   │   │   └── OwnerServicesScreen.tsx
│   │   └── profile/
│   │       └── ProfileScreen.tsx
│   ├── services/
│   │   └── api.ts                     # Cliente Axios con URL persistida en AsyncStorage
│   ├── types/
│   │   └── index.ts                   # Tipos TypeScript
│   └── utils/
│       ├── locationHelper.ts & .test.ts # Cálculo de distancias en km (Haversine)
│       └── timeHelper.ts & .test.ts     # Manejo de franjas horarias y fechas
├── App.tsx
├── app.json
├── package.json
└── tsconfig.json
```

---

## 🌐 Conexión con el Servidor API

El cliente móvil permite definir la URL de la API de forma flexible:
1. **Emulador Android**: Conecta a `http://10.0.2.2:3000/api/v1`.
2. **Simulador iOS o Web**: Conecta a `http://localhost:3000/api/v1`.
3. **Celular Físico vía Expo Go**: Permite ingresar desde la pantalla de **Perfil** la IP local de la computadora (ej. `http://192.168.1.50:3000/api/v1`) y se almacena en `AsyncStorage`.

---

## 🧪 Pruebas Unitarias (Vitest)

El módulo móvil cuenta con tests automatizados mediante **Vitest**:
- `locationHelper.test.ts`: Pruebas de la fórmula de Haversine y cálculo de distancias en kilómetros.
- `timeHelper.test.ts`: Validación de franjas horarias, generación de slots y formateo de horas.
