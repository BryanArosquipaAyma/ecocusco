# 9. Trazabilidad y cumplimiento de la rúbrica

Cada requerimiento funcional tiene entidades que lo sostienen en la base
de datos y un servicio que lo implementa, así que el diseño es
construible sin vacíos.

## 9.1 Matriz de trazabilidad

| Módulo                   | Requerimientos | Entidades                                                              | Pantallas del frontend                                                           | Servicios del backend             |
|--------------------------|----------------|------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------|
| Cuentas                  | RF-01 a RF-03  | USUARIO, COMERCIO                                                      | Registro, inicio de sesión, perfil, QR personal                                  | auth, usuarios                    |
| Recolección              | RF-04, RF-05   | RUTA_RECOJO, HORARIO_RUTA                                              | Panel municipal de rutas; consulta de horarios                                   | rutas                             |
| Puntos críticos          | RF-06 a RF-08  | REPORTE_CRITICO                                                        | Formulario con mapa y foto; bandeja municipal; mis reportes                      | reportes, almacenamiento de fotos |
| Puntos limpios           | RF-09, RF-10   | PUNTO_LIMPIO, ASIGNACION_OPERADOR, MATERIAL                            | Mapa de puntos limpios; administración de puntos                                 | puntos                            |
| Reciclaje y puntos       | RF-11 a RF-14  | ENTREGA, DETALLE_ENTREGA, TARIFA_MATERIAL, MATERIAL, MOVIMIENTO_PUNTOS | Escáner QR y registro del operador; saldo e historial; administración de tarifas | entregas, puntos, tarifas         |
| Canje y retiro           | RF-15 a RF-17  | PROMOCION, CANJE, RETIRO, MOVIMIENTO_PUNTOS                            | Promociones; canje; validación en el comercio; solicitud y pago de retiros       | canjes, retiros                   |
| Sostenibilidad e impacto | RF-18, RF-19   | LOTE_VENTA, EMPRESA_RECICLADORA, ENTREGA                               | Registro de lotes; tablero de impacto                                            | lotes, indicadores                |
