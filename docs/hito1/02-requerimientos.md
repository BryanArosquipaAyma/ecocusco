# 3. Evidencia 1 – Documento de requerimientos

EcoCusco tiene 19 requerimientos funcionales, 10 no funcionales y 13
reglas de negocio, todos derivados de las cuatro funciones de la
propuesta aprobada.

## 3.1 Actores del sistema

| Actor                    | Descripción                                                                               | Acceso                             |
|--------------------------|-------------------------------------------------------------------------------------------|------------------------------------|
| Ciudadano                | Vecino o familia de Santiago que consulta información, reporta, recicla y gana puntos     | Registro propio                    |
| Comercio afiliado        | Bodega, restaurante o negocio que publica promociones y valida canjes                     | Alta aprobada por el administrador |
| Operador de punto limpio | Reciclador asociado que pesa y registra las entregas en un punto limpio                   | Cuenta creada por el administrador |
| Personal municipal       | Funcionario de la municipalidad distrital que publica rutas y atiende reportes            | Cuenta creada por el administrador |
| Administrador EcoCusco   | Equipo del proyecto que configura tarifas, aprueba cuentas y registra ventas y pagos      | Cuenta interna                     |
| Empresa recicladora      | Compradora del material; en el MVP no tiene acceso, el administrador registra sus compras | Sin acceso                         |

## 3.2 Requerimientos funcionales

Prioridad: Alta = indispensable para el MVP, Media = incluida en el MVP
pero puede entregarse al final.

| Código | Módulo          | Requerimiento                                                                                                                          | Actor                    | Prioridad |
|--------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|-----------|
| RF-01  | Cuentas         | Registrar un usuario con nombres, correo, teléfono y contraseña; el sistema genera su código QR personal único                         | Ciudadano, Comercio      | Alta      |
| RF-02  | Cuentas         | Iniciar y cerrar sesión, con acceso a las pantallas según el rol                                                                       | Todos                    | Alta      |
| RF-03  | Cuentas         | Ver y editar el perfil, incluido el teléfono para Yape/Plin                                                                            | Ciudadano, Comercio      | Media     |
| RF-04  | Recolección     | Crear, editar y desactivar rutas y sus horarios (día, hora de inicio y fin)                                                            | Municipal                | Alta      |
| RF-05  | Recolección     | Consultar las rutas y horarios de recojo del distrito, filtrando por día                                                               | Ciudadano                | Alta      |
| RF-06  | Puntos críticos | Reportar un punto crítico con foto, ubicación en el mapa y descripción                                                                 | Ciudadano                | Alta      |
| RF-07  | Puntos críticos | Ver los reportes en lista y mapa, y cambiar su estado (pendiente, en atención, resuelto, rechazado) con observación                    | Municipal                | Alta      |
| RF-08  | Puntos críticos | Consultar el estado y el historial de los reportes propios                                                                             | Ciudadano                | Media     |
| RF-09  | Puntos limpios  | Ver en un mapa interactivo los puntos limpios con dirección, horario y materiales que aceptan                                          | Ciudadano                | Alta      |
| RF-10  | Puntos limpios  | Crear y editar puntos limpios y asignar o retirar operadores                                                                           | Administrador            | Alta      |
| RF-11  | Reciclaje       | Escanear el QR del ciudadano, registrar material y peso de cada ítem entregado y acreditar los puntos calculados con la tarifa vigente | Operador                 | Alta      |
| RF-12  | Reciclaje       | Consultar saldo de puntos e historial de movimientos (entregas, canjes, retiros)                                                       | Ciudadano                | Alta      |
| RF-13  | Reciclaje       | Administrar el catálogo de materiales y sus tarifas (puntos por kg y precio de compra)                                                 | Administrador            | Alta      |
| RF-14  | Antifraude      | Aplicar el límite diario de entrega por usuario y permitir anular una entrega con reversión de sus puntos                              | Sistema, Administrador   | Alta      |
| RF-15  | Canje           | Crear y editar promociones con puntos requeridos, stock y vigencia                                                                     | Comercio                 | Media     |
| RF-16  | Canje           | Canjear puntos por una promoción, generar un código de canje y validarlo en el comercio                                                | Ciudadano, Comercio      | Alta      |
| RF-17  | Retiro          | Solicitar el retiro de puntos en soles a Yape/Plin y registrar su pago                                                                 | Ciudadano, Administrador | Media     |
| RF-18  | Sostenibilidad  | Registrar un lote de venta a la empresa recicladora, asociar las entregas incluidas y calcular el aporte al fondo de recompensas       | Administrador            | Media     |
| RF-19  | Impacto         | Mostrar un tablero con kg por material, usuarios activos, puntos emitidos, reportes resueltos y tiempo promedio de atención            | Administrador, Municipal | Media     |

**Fuera del alcance del MVP:** sensores IoT en contenedores, ubicación
de camiones en tiempo real, encuestas de percepción, pago automático por
API de billeteras digitales y recepción de residuos electrónicos.

## 3.3 Requerimientos no funcionales

| Código | Categoría         | Requerimiento                                                  | Criterio de verificación                                                                                                                      |
|--------|-------------------|----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| RNF-01 | Rendimiento       | Carga ágil en celular con conexión 4G                          | Pantalla inicial en 3 s o menos; el 95% de las consultas de la API responde en 1 s o menos                                                    |
| RNF-02 | Usabilidad        | Diseño responsive, pensado primero para celular                | Funcional desde 360 px de ancho; reportar un punto crítico en 3 pasos o menos; interfaz en español                                            |
| RNF-03 | Seguridad         | Protección de cuentas y datos                                  | Contraseñas con hash bcrypt, HTTPS, sesión con token JWT con expiración, permisos por rol y validación de entradas contra inyección SQL y XSS |
| RNF-04 | Privacidad        | Cumplimiento de la Ley 29733 de Protección de Datos Personales | Solo se piden los datos necesarios (sin DNI), con consentimiento explícito al registrarse; la ubicación se usa solo al reportar               |
| RNF-05 | Integridad        | Trazabilidad de los puntos                                     | El libro de movimientos solo admite inserciones; las correcciones son movimientos nuevos; cada acreditación se guarda en una transacción      |
| RNF-06 | Disponibilidad    | Servicio estable durante el piloto                             | Disponibilidad objetivo de 99% mensual con hosting gestionado y copias de seguridad automáticas de la base de datos                           |
| RNF-07 | Compatibilidad    | Funcionamiento en los navegadores comunes                      | Chrome, Edge, Firefox y Safari en sus dos últimas versiones mayores                                                                           |
| RNF-08 | Baja conectividad | Uso con internet inestable                                     | Rutas, horarios y puntos limpios disponibles sin conexión mediante caché de la PWA; fotos comprimidas a 1 MB o menos antes de subirlas        |
| RNF-09 | Mantenibilidad    | Código ordenado y documentado                                  | Arquitectura por capas, API documentada con OpenAPI, migraciones de base de datos versionadas y commits convencionales                        |
| RNF-10 | Escalabilidad     | Ampliación a otros distritos sin rediseñar                     | Todos los datos territoriales dependen de la entidad Distrito                                                                                 |

## 3.4 Reglas de negocio

Los valores numéricos son parámetros iniciales que el administrador
puede ajustar y que el equipo validará con la empresa recicladora.

| Código | Regla                                                                                                                                                                                        |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RN-01  | Cada usuario tiene un solo rol: ciudadano, comercio, operador, municipal o administrador                                                                                                     |
| RN-02  | Un punto equivale a S/ 0.10, y el valor en soles de los puntos que se otorgan por un material no puede superar el 50% de su precio de compra vigente; el resto cubre los costos de operación |
| RN-03  | Solo un operador asignado a un punto limpio puede registrar entregas en ese punto, y siempre a partir del QR del ciudadano                                                                   |
| RN-04  | Solo se aceptan materiales activos del catálogo (PET, aluminio, vidrio, papel y cartón); el peso se toma de la balanza del punto y debe ser mayor que cero                                   |
| RN-05  | Cada usuario puede entregar como máximo 20 kg de material por día                                                                                                                            |
| RN-06  | Una entrega validada no se edita; solo el administrador puede anularla y el sistema registra la reversión de sus puntos                                                                      |
| RN-07  | El saldo de un usuario es la suma de sus movimientos y nunca puede ser negativo                                                                                                              |
| RN-08  | Al canjear una promoción se descuentan los puntos y se genera un código de un solo uso que vence a los 7 días; si vence sin usarse, los puntos se devuelven                                  |
| RN-09  | El retiro en soles exige un mínimo de 100 puntos (S/ 10)                                                                                                                                     |
| RN-10  | Los retiros se pagan solo hasta el límite de liquidación diaria y mientras el fondo disponible los cubra; el fondo es el aporte acumulado de los lotes vendidos menos los retiros pagados    |
| RN-11  | Al cerrar un lote de venta, el 50% del ingreso se destina al fondo de recompensas y el resto a costos de operación; cada entrega validada pertenece a un solo lote                           |
| RN-12  | Un ciudadano puede enviar hasta 3 reportes de puntos críticos por día, y solo el personal municipal del mismo distrito puede cambiar su estado; marcarlo como resuelto exige una observación |
| RN-13  | Un comercio solo puede publicar promociones después de que el administrador apruebe su cuenta                                                                                                |
