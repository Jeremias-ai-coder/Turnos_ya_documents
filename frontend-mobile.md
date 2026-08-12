# Especificación y Arquitectura del Frontend Mobile - Turnos_ya

El módulo **Frontend Mobile** representa la solución nativa/multiplataforma orientada a smartphones para la plataforma **Turnos_ya**. Su objetivo principal es ofrecer una experiencia de reserva instantánea en dispositivos móviles Android e iOS.

---

## 🛠️ Stack Tecnológico Planificado

| Componente | Tecnología |
| :--- | :--- |
| **Framework Multiplataforma** | Flutter / React Native |
| **Lenguaje** | Dart / TypeScript |
| **Integración HTTP** | Dio / Axios (Consumiendo contrato `Turnos_ya_backend-api`) |
| **Notificaciones** | Firebase Cloud Messaging (FCM) para alertas Push de turnos |
| **Ubicación y Mapas** | Google Maps SDK API para geolocalización de negocios cercanos |

---

## 📲 Funcionalidades Clave

1. **Geolocalización Inmediata**: Identificación de comercios y prestadores en un radio cercano en mapa o lista.
2. **Reserva Háptica en 3 Pasos**: Selección fácil e interactiva optimizada para interacción táctil.
3. **Recordatorios por Notificación Push**: Alertas previas a la hora del turno reservado.
4. **Validación de Turnos para Comercios**: Escaneo o confirmación rápida por parte del dueño.

---

## 📁 Estructura del Proyecto Objetivo

```text
Turnos_ya_frontend_mobile-main/
├── android/
├── ios/
├── lib/ / src/
│   ├── components/
│   ├── models/
│   ├── providers/
│   ├── screens/
│   └── services/
└── README.md
```
