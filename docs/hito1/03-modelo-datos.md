# 4. Evidencia 2 – Modelo de datos

El modelo relacional tiene 18 entidades en cinco grupos, pensado para
PostgreSQL, y cubre todos los requerimientos funcionales del apartado 3.

**Convenciones.** Los nombres van en minúsculas con guion bajo. Toda
clave primaria (PK) es un entero autoincremental. FK indica clave
foránea y la entidad a la que apunta, UK indica valor único y NN indica
obligatorio. Los estados se guardan como texto con restricción CHECK a
los valores indicados.

## 4.1 Territorio y cuentas

**DISTRITO** – distritos atendidos; el piloto empieza con Santiago.

| Atributo    | Tipo        | Clave / restricción | Descripción                         |
|-------------|-------------|---------------------|-------------------------------------|
| id_distrito | INT         | PK                  | Identificador                       |
| nombre      | VARCHAR(80) | UK, NN              | Nombre del distrito                 |
| provincia   | VARCHAR(80) | NN                  | Provincia                           |
| activo      | BOOLEAN     | NN                  | Si está habilitado en la plataforma |

**USUARIO** – todas las personas con cuenta; el rol define sus permisos.

| Atributo       | Tipo         | Clave / restricción | Descripción                                                   |
|----------------|--------------|---------------------|---------------------------------------------------------------|
| id_usuario     | INT          | PK                  | Identificador                                                 |
| id_distrito    | INT          | FK → DISTRITO       | Distrito de residencia o trabajo (vacío para administradores) |
| nombres        | VARCHAR(80)  | NN                  | Nombres                                                       |
| apellidos      | VARCHAR(80)  | NN                  | Apellidos                                                     |
| correo         | VARCHAR(120) | UK, NN              | Correo de acceso                                              |
| telefono       | VARCHAR(15)  | NN                  | Celular, también usado para Yape/Plin                         |
| password_hash  | VARCHAR(255) | NN                  | Contraseña cifrada con bcrypt                                 |
| rol            | VARCHAR(15)  | NN, CHECK           | ciudadano, comercio, operador, municipal, administrador       |
| codigo_qr      | VARCHAR(64)  | UK, NN              | Código personal que lee el operador                           |
| estado         | VARCHAR(12)  | NN, CHECK           | activo, inactivo, pendiente                                   |
| fecha_registro | TIMESTAMP    | NN                  | Fecha de alta                                                 |

**COMERCIO** – datos del negocio de un usuario con rol comercio.

| Atributo         | Tipo         | Clave / restricción | Descripción                      |
|------------------|--------------|---------------------|----------------------------------|
| id_comercio      | INT          | PK                  | Identificador                    |
| id_usuario       | INT          | FK → USUARIO, UK    | Usuario responsable de la cuenta |
| id_distrito      | INT          | FK → DISTRITO       | Distrito donde opera             |
| nombre_comercial | VARCHAR(100) | NN                  | Nombre del negocio               |
| ruc              | VARCHAR(11)  | UK                  | RUC, si lo tiene                 |
| direccion        | VARCHAR(150) | NN                  | Dirección                        |
| latitud          | DECIMAL(9,6) | NN                  | Coordenada                       |
| longitud         | DECIMAL(9,6) | NN                  | Coordenada                       |
| estado           | VARCHAR(12)  | NN, CHECK           | pendiente, aprobado, suspendido  |

## 4.2 Recolección y puntos críticos

**RUTA_RECOJO** – recorridos del camión recolector.

| Atributo    | Tipo        | Clave / restricción | Descripción                |
|-------------|-------------|---------------------|----------------------------|
| id_ruta     | INT         | PK                  | Identificador              |
| id_distrito | INT         | FK → DISTRITO       | Distrito de la ruta        |
| nombre      | VARCHAR(80) | NN                  | Nombre de la ruta          |
| descripcion | TEXT        |                     | Calles y zonas que recorre |
| estado      | VARCHAR(10) | NN, CHECK           | activa, inactiva           |

**HORARIO_RUTA** – días y horas en que pasa cada ruta.

| Atributo    | Tipo     | Clave / restricción | Descripción             |
|-------------|----------|---------------------|-------------------------|
| id_horario  | INT      | PK                  | Identificador           |
| id_ruta     | INT      | FK → RUTA_RECOJO    | Ruta a la que pertenece |
| dia_semana  | SMALLINT | NN, CHECK 1–7       | 1 = lunes … 7 = domingo |
| hora_inicio | TIME     | NN                  | Inicio del recojo       |
| hora_fin    | TIME     | NN                  | Fin del recojo          |

**REPORTE_CRITICO** – acumulaciones de basura reportadas por vecinos.

| Atributo        | Tipo         | Clave / restricción | Descripción                                      |
|-----------------|--------------|---------------------|--------------------------------------------------|
| id_reporte      | INT          | PK                  | Identificador                                    |
| id_usuario      | INT          | FK → USUARIO        | Ciudadano que reporta                            |
| id_distrito     | INT          | FK → DISTRITO       | Distrito del punto                               |
| id_atendido_por | INT          | FK → USUARIO        | Personal municipal que atiende (vacío al inicio) |
| descripcion     | VARCHAR(300) | NN                  | Qué se observa                                   |
| foto_url        | VARCHAR(255) | NN                  | Enlace a la foto almacenada                      |
| latitud         | DECIMAL(9,6) | NN                  | Ubicación del punto                              |
| longitud        | DECIMAL(9,6) | NN                  | Ubicación del punto                              |
| estado          | VARCHAR(12)  | NN, CHECK           | pendiente, en_atencion, resuelto, rechazado      |
| fecha_reporte   | TIMESTAMP    | NN                  | Cuándo se reportó                                |
| fecha_atencion  | TIMESTAMP    |                     | Cuándo se resolvió                               |
| observacion     | VARCHAR(300) |                     | Nota del personal municipal                      |

## 4.3 Puntos limpios y materiales

**PUNTO_LIMPIO** – lugares de acopio. Todos aceptan los materiales
activos del catálogo.

| Atributo         | Tipo         | Clave / restricción | Descripción                            |
|------------------|--------------|---------------------|----------------------------------------|
| id_punto         | INT          | PK                  | Identificador                          |
| id_distrito      | INT          | FK → DISTRITO       | Distrito donde está                    |
| nombre           | VARCHAR(100) | NN                  | Nombre del punto                       |
| direccion        | VARCHAR(150) | NN                  | Dirección                              |
| latitud          | DECIMAL(9,6) | NN                  | Coordenada                             |
| longitud         | DECIMAL(9,6) | NN                  | Coordenada                             |
| horario_atencion | VARCHAR(120) | NN                  | Texto con los días y horas de atención |
| estado           | VARCHAR(10)  | NN, CHECK           | activo, inactivo                       |

**ASIGNACION_OPERADOR** – relaciona operadores con puntos limpios (un
operador puede atender varios puntos y un punto tener varios
operadores).

| Atributo      | Tipo | Clave / restricción | Descripción                          |
|---------------|------|---------------------|--------------------------------------|
| id_asignacion | INT  | PK                  | Identificador                        |
| id_punto      | INT  | FK → PUNTO_LIMPIO   | Punto limpio                         |
| id_usuario    | INT  | FK → USUARIO        | Usuario con rol operador             |
| fecha_inicio  | DATE | NN                  | Desde cuándo atiende                 |
| fecha_fin     | DATE |                     | Hasta cuándo (vacío si sigue activo) |

**MATERIAL** – catálogo de materiales reciclables.

| Atributo    | Tipo        | Clave / restricción | Descripción                           |
|-------------|-------------|---------------------|---------------------------------------|
| id_material | INT         | PK                  | Identificador                         |
| nombre      | VARCHAR(50) | UK, NN              | PET, aluminio, vidrio, papel y cartón |
| activo      | BOOLEAN     | NN                  | Si se acepta actualmente              |

**TARIFA_MATERIAL** – tarifa vigente de cada material; conserva el
historial de precios.

| Atributo         | Tipo         | Clave / restricción | Descripción                                    |
|------------------|--------------|---------------------|------------------------------------------------|
| id_tarifa        | INT          | PK                  | Identificador                                  |
| id_material      | INT          | FK → MATERIAL       | Material al que aplica                         |
| puntos_por_kg    | DECIMAL(8,2) | NN, CHECK \> 0      | Puntos que se otorgan por kg                   |
| precio_compra_kg | DECIMAL(8,2) | NN, CHECK \> 0      | Precio en soles que paga la recicladora por kg |
| vigente_desde    | DATE         | NN                  | Inicio de vigencia                             |
| vigente_hasta    | DATE         |                     | Fin de vigencia (vacío si es la vigente)       |

Restricción adicional (regla RN-02): puntos_por_kg × 0.10 ≤ 0.50 ×
precio_compra_kg.

## 4.4 Entregas y venta del material

**EMPRESA_RECICLADORA** – compradora del material.

| Atributo     | Tipo         | Clave / restricción | Descripción         |
|--------------|--------------|---------------------|---------------------|
| id_empresa   | INT          | PK                  | Identificador       |
| razon_social | VARCHAR(120) | NN                  | Nombre legal        |
| ruc          | VARCHAR(11)  | UK, NN              | RUC                 |
| contacto     | VARCHAR(100) |                     | Persona de contacto |
| telefono     | VARCHAR(15)  |                     | Teléfono            |
| estado       | VARCHAR(10)  | NN, CHECK           | activa, inactiva    |

**LOTE_VENTA** – venta de un conjunto de entregas a la empresa
recicladora.

| Atributo         | Tipo          | Clave / restricción      | Descripción                                                      |
|------------------|---------------|--------------------------|------------------------------------------------------------------|
| id_lote          | INT           | PK                       | Identificador                                                    |
| id_empresa       | INT           | FK → EMPRESA_RECICLADORA | Empresa compradora                                               |
| fecha_venta      | DATE          | NN                       | Fecha de la venta                                                |
| total_kg         | DECIMAL(10,2) | NN                       | Peso total vendido                                               |
| ingreso_total    | DECIMAL(10,2) | NN                       | Soles recibidos                                                  |
| porcentaje_fondo | DECIMAL(5,2)  | NN, CHECK 0–100          | Porcentaje del ingreso para recompensas (50 por defecto)         |
| aporte_fondo     | DECIMAL(10,2) | NN                       | Soles que pasan al fondo: ingreso_total × porcentaje_fondo / 100 |
| estado           | VARCHAR(10)   | NN, CHECK                | abierto, cerrado                                                 |

**ENTREGA** – una visita de reciclaje validada por un operador.

| Atributo         | Tipo         | Clave / restricción | Descripción                                  |
|------------------|--------------|---------------------|----------------------------------------------|
| id_entrega       | INT          | PK                  | Identificador                                |
| id_usuario       | INT          | FK → USUARIO        | Ciudadano o comercio que entrega             |
| id_punto         | INT          | FK → PUNTO_LIMPIO   | Punto donde se entregó                       |
| id_operador      | INT          | FK → USUARIO        | Operador que pesó y validó                   |
| id_lote          | INT          | FK → LOTE_VENTA     | Lote en que se vendió (vacío hasta entonces) |
| fecha_hora       | TIMESTAMP    | NN                  | Momento de la entrega                        |
| total_kg         | DECIMAL(8,2) | NN                  | Suma de pesos del detalle                    |
| total_puntos     | INT          | NN                  | Suma de puntos del detalle                   |
| estado           | VARCHAR(10)  | NN, CHECK           | validada, anulada                            |
| motivo_anulacion | VARCHAR(200) |                     | Obligatorio si está anulada                  |

**DETALLE_ENTREGA** – cada material de una entrega.

| Atributo         | Tipo         | Clave / restricción  | Descripción                         |
|------------------|--------------|----------------------|-------------------------------------|
| id_detalle       | INT          | PK                   | Identificador                       |
| id_entrega       | INT          | FK → ENTREGA         | Entrega a la que pertenece          |
| id_material      | INT          | FK → MATERIAL        | Material entregado                  |
| id_tarifa        | INT          | FK → TARIFA_MATERIAL | Tarifa aplicada al momento          |
| peso_kg          | DECIMAL(8,2) | NN, CHECK \> 0       | Peso medido                         |
| puntos_otorgados | INT          | NN                   | peso_kg × puntos_por_kg, redondeado |

## 4.5 Incentivos y canjes

**MOVIMIENTO_PUNTOS** – libro de movimientos; el saldo de cada usuario
es la suma de sus puntos. Solo admite inserciones.

| Atributo      | Tipo         | Clave / restricción | Descripción                                              |
|---------------|--------------|---------------------|----------------------------------------------------------|
| id_movimiento | INT          | PK                  | Identificador                                            |
| id_usuario    | INT          | FK → USUARIO        | Dueño de los puntos                                      |
| tipo          | VARCHAR(20)  | NN, CHECK           | acreditacion, canje, devolucion_canje, retiro, reversion |
| puntos        | INT          | NN                  | Positivo suma y negativo resta                           |
| id_entrega    | INT          | FK → ENTREGA        | Origen si es acreditación o reversión                    |
| id_canje      | INT          | FK → CANJE          | Origen si es canje o devolución                          |
| id_retiro     | INT          | FK → RETIRO         | Origen si es retiro                                      |
| fecha_hora    | TIMESTAMP    | NN                  | Momento del movimiento                                   |
| descripcion   | VARCHAR(150) |                     | Texto para el historial                                  |

Restricción adicional: exactamente una de las tres claves de origen
(id_entrega, id_canje, id_retiro) tiene valor.

**PROMOCION** – ofertas que un comercio da a cambio de puntos.

| Atributo          | Tipo         | Clave / restricción | Descripción              |
|-------------------|--------------|---------------------|--------------------------|
| id_promocion      | INT          | PK                  | Identificador            |
| id_comercio       | INT          | FK → COMERCIO       | Comercio que la ofrece   |
| titulo            | VARCHAR(100) | NN                  | Nombre de la promoción   |
| descripcion       | VARCHAR(300) |                     | Condiciones              |
| puntos_requeridos | INT          | NN, CHECK \> 0      | Costo en puntos          |
| stock             | INT          | NN, CHECK ≥ 0       | Canjes disponibles       |
| fecha_inicio      | DATE         | NN                  | Inicio de vigencia       |
| fecha_fin         | DATE         | NN                  | Fin de vigencia          |
| estado            | VARCHAR(10)  | NN, CHECK           | activa, pausada, vencida |

**CANJE** – uso de puntos en una promoción.

| Atributo          | Tipo        | Clave / restricción | Descripción                     |
|-------------------|-------------|---------------------|---------------------------------|
| id_canje          | INT         | PK                  | Identificador                   |
| id_usuario        | INT         | FK → USUARIO        | Ciudadano que canjea            |
| id_promocion      | INT         | FK → PROMOCION      | Promoción elegida               |
| puntos_usados     | INT         | NN                  | Puntos descontados              |
| codigo_canje      | VARCHAR(12) | UK, NN              | Código de un solo uso           |
| estado            | VARCHAR(10) | NN, CHECK           | generado, validado, vencido     |
| fecha_generacion  | TIMESTAMP   | NN                  | Cuándo se generó                |
| fecha_vencimiento | TIMESTAMP   | NN                  | Siete días después de generarse |
| fecha_validacion  | TIMESTAMP   |                     | Cuándo lo validó el comercio    |

**RETIRO** – solicitud de pago en soles por Yape o Plin.

| Atributo             | Tipo         | Clave / restricción | Descripción                                          |
|----------------------|--------------|---------------------|------------------------------------------------------|
| id_retiro            | INT          | PK                  | Identificador                                        |
| id_usuario           | INT          | FK → USUARIO        | Ciudadano que solicita                               |
| id_admin             | INT          | FK → USUARIO        | Administrador que registra el pago (vacío al inicio) |
| puntos               | INT          | NN, CHECK ≥ 100     | Puntos a retirar                                     |
| monto_soles          | DECIMAL(8,2) | NN                  | Puntos × 0.10                                        |
| medio                | VARCHAR(5)   | NN, CHECK           | Yape, Plin                                           |
| telefono_destino     | VARCHAR(15)  | NN                  | Celular que recibe el pago                           |
| estado               | VARCHAR(10)  | NN, CHECK           | pendiente, pagado, rechazado                         |
| fecha_solicitud      | TIMESTAMP    | NN                  | Cuándo se pidió                                      |
| fecha_pago           | TIMESTAMP    |                     | Cuándo se pagó                                       |
| referencia_operacion | VARCHAR(40)  |                     | Número de operación de la billetera                  |

## 4.6 Decisiones de diseño

- **Roles en un solo campo.** El rol es un campo de USUARIO (regla
  RN-01) porque cada persona tiene un único rol y así se evita una tabla
  intermedia.

- **Libro de movimientos.** El saldo no se guarda como columna: se
  calcula con MOVIMIENTO_PUNTOS, lo que da trazabilidad completa y evita
  saldos inconsistentes.

- **Tarifa guardada en cada detalle.** DETALLE_ENTREGA conserva la
  tarifa aplicada, de modo que cambiar precios no altera entregas
  pasadas.

- **Distrito como eje territorial.** Rutas, puntos limpios, comercios,
  reportes y usuarios se ligan a DISTRITO para ampliar el piloto.

- **Varias claves hacia USUARIO.** ENTREGA (ciudadano y operador),
  REPORTE_CRITICO (quien reporta y quien atiende) y RETIRO (quien
  solicita y quien paga) usan dos claves foráneas hacia USUARIO, cada
  una con un rol distinto.

- **Datos mínimos.** No se almacena DNI ni datos bancarios; el teléfono
  basta para Yape/Plin y reduce el riesgo según la Ley 29733.
