# 7. Evidencia 5 – Estructura tecnológica

EcoCusco usa un stack JavaScript de punta a punta (React, Node.js y
PostgreSQL) con servicios gratuitos o de bajo costo para el piloto. Un
solo lenguaje simplifica el trabajo del equipo y se adapta al
presupuesto de un emprendimiento universitario.

## 7.1 Componentes y responsabilidades

| Capa           | Tecnología                                  | Responsabilidad dentro de EcoCusco                                                                      |
|----------------|---------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Frontend       | React 18 con Vite                           | Interfaz de todos los roles: pantallas del ciudadano, operador, comercio, municipalidad y administrador |
| Frontend       | Tailwind CSS                                | Diseño responsive, pensado primero para celular                                                         |
| Frontend       | React Router                                | Navegación y rutas protegidas según el rol                                                              |
| Frontend       | vite-plugin-pwa (service worker y manifest) | Instalación en el celular y caché sin conexión de rutas, horarios y puntos limpios                      |
| Frontend       | Leaflet y react-leaflet                     | Mapas de puntos limpios y de reportes, y selección de ubicación al reportar                             |
| Frontend       | qrcode.react y html5-qrcode                 | Mostrar el QR personal del usuario y escanearlo con la cámara del operador y del comercio               |
| Backend        | Node.js 20 y Express                        | API REST en formato JSON con la lógica de negocio: cálculo de puntos, límites, canjes y retiros         |
| Backend        | JWT y bcrypt                                | Autenticación, cifrado de contraseñas y autorización por rol                                            |
| Backend        | Zod                                         | Validación de los datos que llegan a la API                                                             |
| Backend        | Helmet, CORS y express-rate-limit           | Cabeceras seguras, orígenes permitidos y límite de peticiones                                           |
| Backend        | Prisma ORM                                  | Acceso a la base de datos, migraciones versionadas y transacciones                                      |
| Backend        | Swagger (OpenAPI)                           | Documentación de los endpoints para el frontend y las pruebas                                           |
| Base de datos  | PostgreSQL 16                               | Almacenamiento relacional de las 18 entidades, con integridad referencial y transacciones               |
| Almacenamiento | Supabase Storage                            | Guardado de las fotos de los reportes de puntos críticos                                                |
| Mapas base     | OpenStreetMap                               | Mosaicos del mapa, sin costo de licencia                                                                |

## 7.2 Despliegue y herramientas de trabajo

| Necesidad                  | Herramienta           | Uso                                                              |
|----------------------------|-----------------------|------------------------------------------------------------------|
| Hosting del frontend       | Vercel                | Publica la PWA con HTTPS automático                              |
| Hosting del backend        | Render                | Ejecuta la API Node.js                                           |
| Base de datos gestionada   | Supabase (PostgreSQL) | Base de datos con copias de seguridad automáticas                |
| Control de versiones       | Git y GitHub          | Repositorio, ramas y revisión de cambios                         |
| Gestión del trabajo        | GitHub Projects       | Tablero Kanban con las tareas de cada requerimiento              |
| Integración continua       | GitHub Actions        | Ejecuta el análisis de código y las pruebas en cada pull request |
| Calidad de código          | ESLint y Prettier     | Estilo uniforme en todo el equipo                                |
| Pruebas                    | Vitest y Supertest    | Pruebas de la lógica de negocio y de la API                      |
| Pruebas manuales de la API | Postman               | Verificación de cada endpoint                                    |
| Diseño de pantallas        | Figma                 | Prototipos antes de programar                                    |
| Diagramas                  | Mermaid y draw.io     | Diagramas de datos y arquitectura mantenidos junto al código     |
