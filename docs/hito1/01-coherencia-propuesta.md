# 2. Coherencia con la propuesta inicial y cambios justificados

El diseño técnico conserva el núcleo de la propuesta aprobada
(documentos «De la idea al emprendimiento» y «Alineación del Proyecto
con el Valor Social») y solo ajusta seis puntos, cada uno justificado a
continuación.

**Lo que se mantiene**

- Nombre, problema y público: EcoCusco para los vecinos del distrito de
  Santiago, Cusco.

- Cuatro funciones: rutas y horarios, reporte de puntos críticos, mapa
  de puntos limpios e incentivos por reciclar con validación QR.

- Materiales: PET, aluminio, vidrio y papel/cartón.

- Actores: ciudadanos, comercios, recicladores como operadores de puntos
  limpios, municipalidad distrital y empresa recicladora.

- Modelo económico: los incentivos se financian con la venta del
  material, con límite de liquidación diaria, QR personal y validación
  del operador como controles antifraude.

- Implementación por fases, empezando por un piloto de 6 meses.

**Cambios respecto de la propuesta inicial**

| N.º | Propuesta inicial                                                              | Diseño del Hito 1                                                                                                   | Justificación                                                                                                                                                                                  |
|-----|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1   | «App móvil»                                                                    | Aplicación web progresiva (PWA), instalable desde el navegador                                                      | El proyecto de la asignatura es de desarrollo web. Una PWA funciona en el celular, no exige descargar nada de una tienda y usa una sola base de código para todos los roles.                   |
| 2   | Rutas y horarios «en tiempo real»                                              | Rutas y horarios publicados por la municipalidad en el panel y consultados por el ciudadano                         | El tiempo real exige GPS en los camiones y convenio de datos con la municipalidad, algo que hoy no está asegurado. La propia Guía lo ubica en una fase posterior. Queda como evolución futura. |
| 3   | Alcance: Santiago (Doc 1) y «ciudad del Cusco, piloto en dos distritos» (Guía) | Piloto en el distrito de Santiago; el modelo incluye la entidad Distrito para ampliar a otros                       | Se elimina la contradicción entre ambos documentos y se acota el piloto a lo que el equipo puede validar.                                                                                      |
| 4   | Pago de recompensas por Yape/Plin, sin detalle                                 | Los puntos se canjean en comercios afiliados o se solicitan como retiro en soles; el administrador registra el pago | No se asume una integración automática con Yape/Plin, que debe verificarse. Con el registro de solicitudes y pagos queda toda la trazabilidad y la integración se puede añadir después.        |
| 5   | Monto de incentivos S/ 15,000 sin relación con los ingresos                    | Tarifa configurable por material y un porcentaje del ingreso de cada lote asignado al fondo de recompensas          | Hace sostenible el modelo: los puntos solo se emiten según tarifas vigentes y el fondo se alimenta de la venta real de lotes.                                                                  |
| 6   | Productos tecnológicos, encuestas bimestrales y sensores IoT mencionados       | Quedan fuera del alcance del MVP                                                                                    | Los residuos electrónicos requieren manejo legal distinto, y encuestas e IoT son fases posteriores según la propia Guía. No se incluyen requerimientos que no se vayan a construir.            |
