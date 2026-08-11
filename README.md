# Documentación Turnos_ya API (Modular)

Este repositorio contiene la especificación de la nueva API RESTful para el proyecto `Turnos_ya`. Se ha refactorizado la documentación para usar arquitectura basada en dominios (DDD) y OpenAPI 3.0.

## Estructura
- `openapi.yaml`: El punto de entrada (entrypoint).
- `components/`: Contiene los schemas transversales (ej. `errors.yaml` según RFC 7807, `pagination.yaml`, seguridad).
- `modules/`: Agrupación lógica de dominios (auth, users, businesses, appointments, etc.). Cada uno con sus schemas y paths separados.
- `postman_collection.json`: Una colección exportada lista para importar en Postman y probar los flujos directamente (incluye scripts de Login para setear el Token).

## Cómo visualizar localmente
Dado que los archivos están particionados usando referencias relativas `$ref`, puedes visualizarlos mediante las herramientas definidas en el `package.json`.

Primero, instala las dependencias:
```bash
npm install
```

Luego, puedes ejecutar los siguientes comandos:

- **Mock Server**: Levanta un servidor simulado en `http://localhost:4040` usando Prism, respondiendo con datos falsos pero conformes al contrato.
  ```bash
  npm run mock
  ```
- **Linter**: Valida la especificación contra reglas estrictas de diseño usando Spectral.
  ```bash
  npm run lint
  ```
- **Generación de Tipos**: Compila el contrato YAML y genera un archivo de declaración estricta para TypeScript.
  ```bash
  npm run generate:types
  ```
- **Preview de Documentación**: Abre un servidor local en el navegador usando Redocly, compilando al vuelo los `$ref`.
  ```bash
  npm run docs:preview
  ```

## Postman
Importa `postman_collection.json` en Postman. Asegúrate de ejecutar el request **Login** en la carpeta Auth, esto guardará automáticamente el JWT en la variable `{{authToken}}` para su uso en los siguientes endpoints (ej. **Hold Appointment**).