# Línea de Quesos — ISA-88 y recetas

Documento complementario de las 12 capas de la línea de quesos. No introduce información nueva de proceso: ordena la ya definida con los términos del estándar ISA-88 (control por lotes) y deja las tres recetas maestras completas en su versión 1.0.

Qué parte sale de dónde:

| Parte de ISA-88 | Se construye con |
|---|---|
| Modelo físico | Capa 5 (equipos), Capa 7 (sensores y actuadores) y Capa 10 (recursos) |
| Modelo procedimental | Capa 3 (flujo), Capa 4 (operaciones y parámetros) y Capa 8 (tiempos) |
| Modelo de recetas | Capa 2 (materias primas), Capa 11 (recetas), Capa 9 (rendimientos) y Capa 12 (batch) |

**Origen de los datos.** Se mantienen las etiquetas del modelo (`PUBLIC`, `REFERENCE`, `ASSUMED`, `CALCULATED`, `PROPOSED`, `PENDING`). Los elementos nuevos de este documento (unidades, módulos de equipo, procedimientos de unidad y fases) son `PROPOSED`: son una forma de organizar lo existente, no una verificación en la planta. Las unidades describen la situación actual; los módulos de equipo, los procedimientos de unidad y las fases son la base de la propuesta (ver la sección 8).

**Prefijos nuevos** (no chocan con los existentes):

| Prefijo | Elemento ISA-88 | Ejemplo |
|---|---|---|
| `Q-UN-` | Unidad | `Q-UN-003` |
| `Q-EM-` | Módulo de equipo | `Q-EM-005` |
| `Q-UP-` | Procedimiento de unidad | `Q-UP-003` |
| `Q-FS-` | Fase | `Q-FS-012` |

---

## 1. Lectura rápida de ISA-88

ISA-88 separa **qué se fabrica** (la receta) de **con qué se fabrica** (el equipo). Eso permite fabricar varios productos en los mismos equipos cambiando solo la receta, que es exactamente la situación de la línea: tres quesos en las mismas tinas, prensas y líneas de empaque.

Tiene tres modelos que se relacionan:

1. **Modelo físico:** jerarquía de equipos.
2. **Modelo procedimental:** jerarquía de lo que el equipo ejecuta.
3. **Modelo de recetas:** información que le dice al equipo qué producto hacer y con qué valores.

---

## 2. Modelo físico

### 2.1. Jerarquía

| Nivel ISA-88 | En este proyecto |
|---|---|
| Empresa | Empresa láctea del caso de estudio |
| Sitio | Planta láctea (tres líneas por lotes: yogures, quesos y UHT) |
| Área | Línea de quesos |
| Celda de proceso | Celda de elaboración de quesos: todo lo necesario para fabricar un batch de cualquiera de las tres referencias |
| Unidad | Equipo principal que ejecuta operaciones sobre el lote (ver 2.2) |
| Módulo de equipo | Grupo de elementos que ejecuta una función concreta (dosificar, agitar, regular temperatura; ver 2.3) |
| Módulo de control | Sensor o actuador individual (`Q-SEN-` y `Q-ACT-` de la Capa 7) |

### 2.2. Unidades

| ID | Unidad | Equipos de la Capa 5 | Cantidad | Usada por | Dimensionamiento de diseño | Estado |
|---|---|---|---|---|---|---|
| `Q-UN-001` | Recepción y almacenamiento de leche | `Q-EQ-001`, `Q-EQ-002` | 1 | Todas | Leche a ≤ 4 °C, ≈ 12 h de permanencia; 31.500 kg al día | PROPOSED |
| `Q-UN-002` | Preparación y tratamiento térmico | `Q-EQ-003`, `Q-EQ-004` | 1 | Todas | Pasteurizador de 5.000 kg/h | ASSUMED |
| `Q-UN-003` | Tina de elaboración (A, B y C) | `Q-EQ-005` a `Q-EQ-009` | 3 | Todas | 5.000 L, 4.500 kg útiles por lote | ASSUMED |
| `Q-UN-004` | Salado | `Q-EQ-010` | 1 | Campesino (en masa) y Sopó (salmuera) | Salmuera para ≈ 1,35 t de queso al tiempo | CALCULATED |
| `Q-UN-005` | Moldeado y prensado | `Q-EQ-011`, `Q-EQ-012` | 1 | Todas (la mozzarella solo usa el moldeado) | Por definir | PROPOSED |
| `Q-UN-006` | Estirado / hilado | `Q-EQ-013` | 1 | Mozzarella | ≈ 60 min por lote de 495 kg de queso | ASSUMED |
| `Q-UN-007` | Cámara de maduración | `Q-EQ-014` | 1 | Sopó | ≈ 116 t (≈ 25.700 hormas de 4,5 kg) a 9–11 °C | CALCULATED / ASSUMED |
| `Q-UN-008` | Enfriamiento y acondicionamiento | `Q-EQ-015` | 1 | Todas | Espacio refrigerado compartido, para varios lotes a la vez; capacidad por definir | PROPOSED |
| `Q-UN-009` | Empaque | `Q-EQ-016` | 3 (una por producto) | Una por producto: el sistema de lotes dirige cada batch a la suya | 600, 720 y 450 unidades/h; carga diaria de cada una sobre las 16 h: ≈ 83 %, ≈ 42 % y ≈ 86 % | ASSUMED / CALCULATED |
| `Q-UN-010` | Almacenamiento de producto terminado | `Q-EQ-017` | 1 | Todas | ≈ 15 t a 4 °C | ASSUMED / CALCULATED |
| `Q-UN-011` | Despacho y manipulación | `Q-EQ-018` | 1 | Todas | Por definir | PROPOSED |

**Unidades compartidas.** Las tres tinas, el pasteurizador, la salmuera, la prensa y el enfriamiento los usan varias referencias. En ISA-88 esto implica **arbitrar el uso**: el sistema de lotes decide qué batch ocupa qué tina y en qué orden. Las tres líneas de empaque son una por producto, y el sistema de lotes dirige cada batch a la suya. La tina se ocupa 224 min por lote en campesino y mozzarella, y 254 min en Sopó (Capa 8), y se limpia con CIP entre lotes.

### 2.3. Módulos de equipo

| ID | Módulo de equipo | Función | Módulos de control (Capa 7) | Unidad | Estado |
|---|---|---|---|---|---|
| `Q-EM-001` | Transferencia de leche | Mover leche entre recepción, almacenamiento, preparación y tinas | `Q-ACT-001`, `Q-ACT-002`, `Q-SEN-001` | `Q-UN-001`, `Q-UN-002` | PROPOSED |
| `Q-EM-002` | Regulación térmica del tratamiento | Mantener la temperatura de pasteurización | `Q-ACT-003`, `Q-SEN-002` | `Q-UN-002` | PROPOSED |
| `Q-EM-003` | Dosificación de cultivos | Dosificar el cultivo según la receta | `Q-ACT-004` | `Q-UN-003` | PROPOSED |
| `Q-EM-004` | Dosificación de cuajo | Dosificar el coagulante según la receta | `Q-ACT-005` | `Q-UN-003` | PROPOSED |
| `Q-EM-005` | Corte y agitación | Cortar la cuajada y agitarla | `Q-ACT-006`, `Q-ACT-007`, `Q-SEN-005`, `Q-SEN-006` | `Q-UN-003` | PROPOSED |
| `Q-EM-006` | Desuerado | Evacuar y medir el suero | `Q-ACT-008`, `Q-SEN-008` | `Q-UN-003` | PROPOSED |
| `Q-EM-007` | Dosificación de sal | Dosificar sal en masa | `Q-ACT-009`, `Q-SEN-007` | `Q-UN-004` | PROPOSED |
| `Q-EM-008` | Prensado | Aplicar la presión de prensado | `Q-ACT-010`, `Q-SEN-009` | `Q-UN-005` | PROPOSED |
| `Q-EM-009` | Estirado | Accionar el estirado y regular su temperatura | `Q-ACT-011` y sensor de temperatura | `Q-UN-006` | PROPOSED |
| `Q-EM-010` | Clima de la cámara | Regular temperatura y humedad relativa | `Q-ACT-012`, `Q-ACT-013`, `Q-SEN-010`, `Q-SEN-011` | `Q-UN-007` | PROPOSED |
| `Q-EM-011` | Enfriamiento | Enfriar el producto | `Q-ACT-014`, `Q-SEN-001` | `Q-UN-008` | PROPOSED |
| `Q-EM-012` | Empaque y verificación | Empacar y verificar masa o cantidad | `Q-ACT-015`, `Q-SEN-007`, `Q-SEN-012` | `Q-UN-009` | PROPOSED |
| `Q-EM-013` | Limpieza CIP | Limpiar tina y circuitos; es un recurso de planta, se confirmará como `PL-` en la integración | Por definir | `Q-UN-001`, `Q-UN-002`, `Q-UN-003` | PROPOSED |

Los sensores de pH (`Q-SEN-004`) y de registro de tiempo (`Q-SEN-003`) pertenecen a la unidad `Q-UN-003`, sin módulo propio. La dosificación de cloruro de calcio no tiene actuador en la Capa 7: se asume manual hasta validarla.

---

## 3. Modelo procedimental

### 3.1. Jerarquía

| Nivel ISA-88 | Qué es aquí | Dónde está definido |
|---|---|---|
| Procedimiento | Todo lo que se hace para fabricar un batch de una referencia | La receta maestra (`Q-REC-001`, `002`, `003`) |
| Procedimiento de unidad | Lo que se hace dentro de una unidad | Sección 3.2 de este documento |
| Operación | Un conjunto de acciones con un resultado de proceso | `Q-OP-001` a `Q-OP-020` (Capa 4) |
| Fase | La acción más pequeña que el control ejecuta como bloque | Sección 3.3 de este documento |

Las 20 operaciones de la Capa 4 son el nivel de operación. Las fases son el único nivel que no estaba definido antes.

### 3.2. Procedimientos de unidad

| ID | Procedimiento de unidad | Unidad | Operaciones |
|---|---|---|---|
| `Q-UP-001` | Recepción y almacenamiento de leche | `Q-UN-001` | `Q-OP-001`, `Q-OP-002`, `Q-OP-003` |
| `Q-UP-002` | Preparación y tratamiento térmico | `Q-UN-002` | `Q-OP-004`, `Q-OP-005` |
| `Q-UP-003` | Elaboración de la cuajada | `Q-UN-003` | `Q-OP-006`, `Q-OP-007`, `Q-OP-008`, `Q-OP-009`, `Q-OP-010`, `Q-OP-011` |
| `Q-UP-004` | Salado | `Q-UN-004` | `Q-OP-012` |
| `Q-UP-005` | Moldeado y prensado | `Q-UN-005` | `Q-OP-013`, `Q-OP-014` |
| `Q-UP-006` | Estirado / hilado | `Q-UN-006` | `Q-OP-015` |
| `Q-UP-007` | Maduración | `Q-UN-007` | `Q-OP-016` |
| `Q-UP-008` | Enfriamiento y acondicionamiento | `Q-UN-008` | `Q-OP-017` |
| `Q-UP-009` | Empaque | `Q-UN-009` | `Q-OP-018` |
| `Q-UP-010` | Almacenamiento y despacho | `Q-UN-010`, `Q-UN-011` | `Q-OP-019`, `Q-OP-020` |

### 3.3. Fases

Todas son `PROPOSED`. Los valores entre paréntesis salen de la Capa 4, la Capa 8 y la Capa 11, con su origen. Los límites aceptables y los valores de pH siguen pendientes.

| ID | Fase | Procedimiento de unidad | Operación | Módulos de equipo | Parámetros y valores de diseño | Aplica a |
|---|---|---|---|---|---|---|
| `Q-FS-001` | Recibir leche de la cisterna | `Q-UP-001` | `Q-OP-001` | `Q-EM-001` | 30 min por cisterna (ASSUMED) | Todas |
| `Q-FS-002` | Muestrear y analizar la leche | `Q-UP-001` | `Q-OP-002` | `Q-EM-001` | Temperatura, acidez y antibióticos; 20 min por cisterna (ASSUMED) | Todas |
| `Q-FS-003` | Aprobar o rechazar la leche | `Q-UP-001` | `Q-OP-002` | — | Resultado: aprobada o rechazada | Todas |
| `Q-FS-004` | Almacenar la leche | `Q-UP-001` | `Q-OP-003` | `Q-EM-001` | ≤ 4 °C, ≈ 12 h (ASSUMED) | Todas |
| `Q-FS-005` | Preparar y estandarizar la leche | `Q-UP-002` | `Q-OP-004` | `Q-EM-001` | Incluida en el llenado de tina (54 min con el tratamiento) | Todas |
| `Q-FS-006` | Pasteurizar | `Q-UP-002` | `Q-OP-005` | `Q-EM-002` | 72–73 °C durante 15–20 s (REFERENCE) | Todas |
| `Q-FS-007` | Enfriar y transferir a la tina | `Q-UP-002` | `Q-OP-005` | `Q-EM-001`, `Q-EM-002` | Enfriar a ≈ 30 °C; 4.500 kg a 5.000 kg/h (CALCULATED) | Todas |
| `Q-FS-008` | Llenar la tina | `Q-UP-003` | `Q-OP-006` | `Q-EM-001` | 4.500 kg de leche (ASSUMED) | Todas |
| `Q-FS-009` | Ajustar la temperatura de coagulación | `Q-UP-003` | `Q-OP-006` | `Q-EM-002` | 31–36 °C; campesino y Sopó 33 °C, mozzarella 35 °C (REFERENCE) | Todas |
| `Q-FS-010` | Dosificar cloruro de calcio | `Q-UP-003` | `Q-OP-006` | Manual | 225–900 g por lote, por validar (REFERENCE) | Todas |
| `Q-FS-011` | Dosificar cultivo e inocular | `Q-UP-003` | `Q-OP-006` | `Q-EM-003` | ≈ 22–45 g por lote; 30 min con reposo (ASSUMED) | Todas |
| `Q-FS-012` | Dosificar cuajo y coagular | `Q-UP-003` | `Q-OP-007` | `Q-EM-004` | Hasta 1,35 L por lote; ≈ 30 min a 31–36 °C (REFERENCE) | Todas |
| `Q-FS-013` | Cortar la cuajada | `Q-UP-003` | `Q-OP-008` | `Q-EM-005` | Grano de 1 a 15 mm según el queso; ≈ 20 min de corte y agitación inicial (REFERENCE) | Todas |
| `Q-FS-014` | Agitar y cocer la cuajada | `Q-UP-003` | `Q-OP-009`, `Q-OP-011` | `Q-EM-005`, `Q-EM-002` | Campesino 39–40 °C, 10–15 min; Sopó 50–56 °C; mozzarella por definir (REFERENCE) | Todas |
| `Q-FS-015` | Desuerar | `Q-UP-003` | `Q-OP-010` | `Q-EM-006` | Suero ≈ 3.960 kg (campesino), 4.005 kg (mozzarella) y 4.050 kg (Sopó) (CALCULATED) | Todas |
| `Q-FS-016` | Vaciar la tina a la unidad siguiente | `Q-UP-003` | `Q-OP-010` | `Q-EM-006` | Del cuajo al vaciado 90 min (campesino y mozzarella) y 120 min (Sopó); espera de 10 min hasta el moldeo (ASSUMED) | Todas |
| `Q-FS-017` | Limpiar la tina (CIP) | `Q-UP-003` | — | `Q-EM-013` | 50 min por lote (ASSUMED) | Todas |
| `Q-FS-018` | Salar en masa | `Q-UP-004` | `Q-OP-012` | `Q-EM-007` | 1,5–1,8 % del queso (8,1–9,7 kg por lote); 15 min, antes de moldear (REFERENCE) | Campesino |
| `Q-FS-019` | Salar en salmuera | `Q-UP-004` | `Q-OP-012` | Tanque de salmuera | 20 °Bé (≈ 22 % de sal) a 10–12 °C; ≈ 36 h por horma de 4,5 kg; espera de 15 min antes (REFERENCE / ASSUMED) | Sopó |
| `Q-FS-020` | Orear | `Q-UP-004` | `Q-OP-012` | — | ≈ 12 h tras la salmuera (ASSUMED) | Sopó |
| `Q-FS-021` | Moldear | `Q-UP-005` | `Q-OP-013` | — | Mozzarella: masa a 55–65 °C al entrar a los moldes (REFERENCE) | Todas |
| `Q-FS-022` | Prensar | `Q-UP-005` | `Q-OP-014` | `Q-EM-008` | 60 min (campesino) y 100 min (Sopó); 50–450 g/cm² según el queso; espera de 15 min antes (REFERENCE) | Campesino y Sopó |
| `Q-FS-023` | Desmoldar y verificar peso y forma | `Q-UP-005` | `Q-OP-014` | `Q-EM-008` | Inspección de peso y forma | Campesino y Sopó |
| `Q-FS-024` | Estirar o hilar | `Q-UP-006` | `Q-OP-015` | `Q-EM-009` | En agua a 65–70 °C; ≈ 60 min por lote; amasado a ≈ 60 °C (ASSUMED / REFERENCE) | Mozzarella |
| `Q-FS-025` | Cargar la cámara | `Q-UP-007` | `Q-OP-016` | — | Hasta ≈ 200 hormas al día | Sopó |
| `Q-FS-026` | Mantener las condiciones de maduración | `Q-UP-007` | `Q-OP-016` | `Q-EM-010` | 9–11 °C y 80–85 % de humedad relativa durante ≈ 150 días (ASSUMED / REFERENCE) | Sopó |
| `Q-FS-027` | Inspeccionar al salir de maduración | `Q-UP-007` | `Q-OP-016` | — | Inspección sensorial y de humedad (máximo 40 %) | Sopó |
| `Q-FS-028` | Enfriar y acondicionar | `Q-UP-008` | `Q-OP-017` | `Q-EM-011` | Mozzarella ≈ 75 min en agua o salmuera fría hasta núcleo de 7–13 °C; campesino ≈ 12 h en refrigeración; Sopó ≈ 150 min (ASSUMED) | Todas |
| `Q-FS-029` | Empacar | `Q-UP-009` | `Q-OP-018` | `Q-EM-012` | Cuña de 250 g a 600 u/h; bloque de 250 g a 720 u/h; cuña de 180 g a 450 u/h; mozzarella espera 30 min antes (ASSUMED) | Todas |
| `Q-FS-030` | Verificar peso, sello y codificación | `Q-UP-009` | `Q-OP-018` | `Q-EM-012` | Inspección de masa, sello y código de lote | Todas |
| `Q-FS-031` | Encajonar | `Q-UP-009` | `Q-OP-018` | — | Empaque secundario según agrupación logística (PENDING) | Todas |
| `Q-FS-032` | Almacenar hasta la liberación | `Q-UP-010` | `Q-OP-019` | — | ≈ 15 t a 4 °C; 3 días (campesino y mozzarella) y 5 días (Sopó) (ASSUMED) | Todas |
| `Q-FS-033` | Liberar y despachar | `Q-UP-010` | `Q-OP-020` | — | Liberación del batch según Capa 12; tiempo de despacho PENDING | Todas |

### 3.4. Qué ejecuta cada nivel

| Nivel procedimental | Quién lo ejecuta | Nota |
|---|---|---|
| Procedimiento (receta) | Se selecciona en el nivel 3 (sistema de producción) | Con el supuesto de que quesos no tiene SCADA, la selección se hace desde una HMI local o desde el sistema de producción |
| Procedimiento de unidad y operación | PLC de cada unidad | Se modela como secuencia en Grafcet y se implementa en Ladder |
| Fase | PLC, como un bloque de la secuencia | Cada fase es un paso o una macroetapa del Grafcet; su resultado se registra en el batch |

---

## 4. Estados y modos

ISA-88 define un modelo de estados para los elementos procedimentales. Se relaciona con los estados del batch de la Capa 12 así:

| Estado del batch (Capa 12) | Estado ISA-88 equivalente |
|---|---|
| PLANIFICADO | Inactivo (*Idle*) |
| EN_PROCESO | En ejecución (*Running*) |
| EN_ESPERA | En pausa o detenido temporalmente (*Paused* o *Held*) |
| COMPLETADO | Completo (*Complete*) |
| CANCELADO | Abortado o detenido (*Aborted* o *Stopped*) |
| EN_REVISION, LIBERADO, RETENIDO, RECHAZADO | Sin equivalente directo: son estados de calidad y negocio, no de ejecución |

Esta separación es útil: ISA-88 describe cómo se ejecuta el batch, y los estados de calidad se manejan aparte en el nivel de operaciones.

Los modos de operación de las unidades son **automático**, **semiautomático** y **manual**. En la línea de quesos, el cloruro de calcio, la verificación del grano, el encajonado y varias inspecciones son manuales o semiautomáticos.

---

## 5. Modelo de recetas

### 5.1. Los cuatro tipos de receta

| Tipo ISA-88 | Qué contiene | Equivalente en el proyecto |
|---|---|---|
| General | Producto y proceso sin ligarlos a una planta | La ruta de cada queso en la Capa 3 y los parámetros de referencia de la Capa 4 |
| De sitio | Adaptada a la planta | Los valores de diseño de la planta (lote de 4.500 kg, plan de 7 lotes, equipos dimensionados) |
| Maestra | Adaptada a la línea y sus equipos, con valores objetivo | `Q-REC-001`, `Q-REC-002`, `Q-REC-003` (sección 6) |
| De control | Una instancia de la maestra, para un batch concreto, con los valores reales | El batch (Capa 12), que conserva la versión de receta usada |

### 5.2. Partes de una receta

Cada receta maestra tiene cuatro partes. Se corresponden con los campos de la Capa 11:

| Parte ISA-88 | Qué incluye | Campos de la Capa 11 |
|---|---|---|
| Cabecera | Identificación, versión, producto, tamaño de lote, estado | ID de receta, versión, producto asociado, tamaño de lote, estado de aprobación, origen |
| Fórmula | Entradas, salidas y parámetros de proceso | Materias primas, cantidades, parámetros de proceso |
| Procedimiento | Secuencia de procedimientos de unidad, operaciones y fases | Secuencia de operaciones |
| Requisitos de equipo | Qué unidades necesita la receta | Equipos requeridos |

Los criterios de calidad y las condiciones de almacenamiento de la Capa 11 se incluyen dentro de la fórmula, como parámetros de salida.

### 5.3. Valor objetivo, límites y valor real

Cada parámetro tiene tres valores distintos: el **objetivo** y los **límites** viven en la receta maestra; el **valor real** se registra en el batch y nunca en la maestra. En esta versión 1.0 se fijan los objetivos o rangos de diseño; los **límites aceptables siguen por definir**.

---

## 6. Recetas maestras (versión 1.0)

Todas son por lote de **4.500 kg de leche** (≈ 4.370 L). La versión 1.0 es la primera estructura del modelo, no una receta industrial validada. Estado de aprobación: **borrador**. Cada batch conserva la versión con la que se fabricó.

### 6.1. Queso Campesino — `Q-REC-001`

**Cabecera**

| Campo | Valor |
|---|---|
| Producto | `Q-PRD-001` — Queso Campesino, cuña de 250 g (queso fresco) |
| Versión | 1.0 |
| Tamaño de lote | 4.500 kg de leche; 3 lotes al día |
| Origen | PROPOSED |

**Fórmula**

| Tipo | Material | Cantidad por lote | Estado |
|---|---|---|---|
| Entrada | Leche de vaca (`Q-MP-001`) | 4.500 kg | ASSUMED |
| Entrada | Cultivos lácticos (`Q-MP-002`) | ≈ 22–45 g (por validar) | ASSUMED |
| Entrada | Cuajo (`Q-MP-003`) | Hasta 1,35 L | REFERENCE |
| Entrada | Sal (`Q-MP-004`) | 8,1–9,7 kg (1,5–1,8 % del queso) | REFERENCE / CALCULATED |
| Entrada | Cloruro de calcio (`Q-MP-005`) | 225–900 g (por validar) | REFERENCE |
| Entrada | Empaque primario y etiqueta (`Q-MA-001`, `Q-MA-002`) | 2.160 unidades | CALCULATED |
| Salida | Queso campesino | 540 kg (rendimiento 0,12); 2.160 cuñas de 250 g | CALCULATED |
| Salida | Suero y otras salidas | ≈ 3.960 kg | CALCULATED |

**Procedimiento**

| Orden | Procedimiento de unidad | Fases | Parámetros clave |
|---|---|---|---|
| 1 | `Q-UP-001` | `Q-FS-001` a `004` | Leche aprobada; ≤ 4 °C |
| 2 | `Q-UP-002` | `Q-FS-005` a `007` | 72–73 °C × 15–20 s; enfriar a ≈ 30 °C |
| 3 | `Q-UP-003` | `Q-FS-008` a `017` | Coagulación a 33 °C; cocción a 39–40 °C durante 10–15 min; del cuajo al vaciado 90 min |
| 4 | `Q-UP-004` | `Q-FS-018` | Sal en masa, 15 min, antes de moldear |
| 5 | `Q-UP-005` | `Q-FS-021` a `023` | Moldeado y prensado ligero, 60 min |
| 6 | `Q-UP-008` | `Q-FS-028` | Refrigeración ≈ 12 h |
| 7 | `Q-UP-009` | `Q-FS-029` a `031` | Cuñas de 250 g; 600 unidades/h |
| 8 | `Q-UP-010` | `Q-FS-032`, `033` | 4 °C; liberación a los 3 días |

**Requisitos de equipo:** `Q-UN-001`, `Q-UN-002`, `Q-UN-003` (una de las tres tinas), `Q-UN-004`, `Q-UN-005`, `Q-UN-008`, `Q-UN-009` (línea de campesino), `Q-UN-010`, `Q-UN-011`. Ocupación de la tina: 224 min por lote.

**Calidad y almacenamiento**

| Criterio | Valor | Estado |
|---|---|---|
| Clase | Semiblando; humedad máxima 65 % (Res. 1804 de 1989) | PUBLIC |
| Humedad objetivo de diseño | ≈ 60 % | ASSUMED / REFERENCE |
| Grasa mínima en extracto seco | Graso 45 %, semigraso 20 % | PUBLIC |
| Microbiología | Resolución 1407 de 2022 | PUBLIC |
| Rotulado | Resolución 810 de 2021, modificada por la 2492 de 2022 | PUBLIC |
| Almacenamiento | 4 °C; retención de 3 días hasta la liberación | ASSUMED |

---

### 6.2. Queso Mozzarella — `Q-REC-002`

**Cabecera**

| Campo | Valor |
|---|---|
| Producto | `Q-PRD-002` — Queso Mozzarella, bloque de 250 g (queso fresco de pasta hilada) |
| Versión | 1.0 |
| Tamaño de lote | 4.500 kg de leche; 2 lotes al día |
| Origen | PROPOSED |

**Fórmula**

| Tipo | Material | Cantidad por lote | Estado |
|---|---|---|---|
| Entrada | Leche de vaca (`Q-MP-001`) | 4.500 kg | ASSUMED |
| Entrada | Cultivos lácticos (`Q-MP-002`) | ≈ 22–45 g (por validar) | ASSUMED |
| Entrada | Cuajo (`Q-MP-003`) | Hasta 1,35 L | REFERENCE |
| Entrada | Sal (`Q-MP-004`) | No se sala en la ruta | REFERENCE |
| Entrada | Cloruro de calcio (`Q-MP-005`) | 225–900 g (por validar) | REFERENCE |
| Entrada | Agua para el estirado | A 65–70 °C; volumen por definir | REFERENCE / PENDING |
| Entrada | Empaque primario y etiqueta (`Q-MA-001`, `Q-MA-002`) | 1.980 unidades | CALCULATED |
| Salida | Queso mozzarella | 495 kg (rendimiento 0,11); 1.980 bloques de 250 g | CALCULATED |
| Salida | Suero y otras salidas | ≈ 4.005 kg (incluye el suero de hilado) | CALCULATED |

**Procedimiento**

| Orden | Procedimiento de unidad | Fases | Parámetros clave |
|---|---|---|---|
| 1 | `Q-UP-001` | `Q-FS-001` a `004` | Leche aprobada; ≤ 4 °C |
| 2 | `Q-UP-002` | `Q-FS-005` a `007` | 72–73 °C × 15–20 s; enfriar a ≈ 30 °C |
| 3 | `Q-UP-003` | `Q-FS-008` a `017` | Coagulación a 35 °C; cocción por definir; del cuajo al vaciado 90 min |
| 4 | `Q-UP-006` | `Q-FS-024` | Estirado en agua a 65–70 °C, ≈ 60 min |
| 5 | `Q-UP-005` | `Q-FS-021` | Moldeado con la masa a 55–65 °C |
| 6 | `Q-UP-008` | `Q-FS-028` | Agua o salmuera fría, ≈ 75 min, núcleo de 7–13 °C |
| 7 | `Q-UP-009` | `Q-FS-029` a `031` | Bloques de 250 g; 720 unidades/h; espera de 30 min antes |
| 8 | `Q-UP-010` | `Q-FS-032`, `033` | 4 °C; liberación a los 3 días |

**Requisitos de equipo:** `Q-UN-001`, `Q-UN-002`, `Q-UN-003`, `Q-UN-006`, `Q-UN-005` (solo moldeado), `Q-UN-008`, `Q-UN-009` (línea de mozzarella), `Q-UN-010`, `Q-UN-011`. Ocupación de la tina: 224 min por lote.

**Calidad y almacenamiento**

| Criterio | Valor | Estado |
|---|---|---|
| Clase | Semiduro; humedad máxima 55 %. La Resolución 1804 no trae requisitos específicos de mozzarella | PUBLIC / ASSUMED |
| Humedad objetivo de diseño | ≈ 50 % (una receta académica reportó 50,7 %) | ASSUMED / REFERENCE |
| Microbiología y rotulado | Resoluciones 1407 de 2022 y 810 de 2021 | PUBLIC |
| Almacenamiento | 4 °C; retención de 3 días hasta la liberación | ASSUMED |

---

### 6.3. Queso Sopó — `Q-REC-003`

**Cabecera**

| Campo | Valor |
|---|---|
| Producto | `Q-PRD-003` — Queso Sopó, cuña de 180 g (queso madurado) |
| Versión | 1.0 |
| Tamaño de lote | 4.500 kg de leche; 2 lotes al día |
| Origen | PROPOSED |

**Fórmula**

| Tipo | Material | Cantidad por lote | Estado |
|---|---|---|---|
| Entrada | Leche de vaca (`Q-MP-001`) | 4.500 kg | ASSUMED |
| Entrada | Cultivos lácticos (`Q-MP-002`) | ≈ 22–45 g (por validar) | ASSUMED |
| Entrada | Cuajo (`Q-MP-003`) | Hasta 1,35 L | REFERENCE |
| Entrada | Sal (`Q-MP-004`) | Salmuera de 20 °Bé (≈ 22 % de sal); volumen por definir | REFERENCE / PENDING |
| Entrada | Cloruro de calcio (`Q-MP-005`) | 225–900 g (por validar) | REFERENCE |
| Entrada | Empaque primario y etiqueta (`Q-MA-001`, `Q-MA-002`) | 2.500 unidades | CALCULATED |
| Salida | Queso Sopó | 450 kg (rendimiento 0,10); ≈ 100 hormas de 4,5 kg al salir de la tina; 2.500 cuñas de 180 g tras maduración y corte | CALCULATED |
| Salida | Suero y otras salidas | ≈ 4.050 kg (incluye la pérdida de humedad durante la maduración) | CALCULATED |

**Procedimiento**

| Orden | Procedimiento de unidad | Fases | Parámetros clave |
|---|---|---|---|
| 1 | `Q-UP-001` | `Q-FS-001` a `004` | Leche aprobada; ≤ 4 °C |
| 2 | `Q-UP-002` | `Q-FS-005` a `007` | 72–73 °C × 15–20 s; enfriar a ≈ 30 °C |
| 3 | `Q-UP-003` | `Q-FS-008` a `017` | Coagulación a 33 °C; cocción a 50–56 °C; del cuajo al vaciado 120 min |
| 4 | `Q-UP-005` | `Q-FS-021` a `023` | Moldeado y prensado, 100 min |
| 5 | `Q-UP-004` | `Q-FS-019`, `020` | Salmuera de 20 °Bé a 10–12 °C, ≈ 36 h por horma de 4,5 kg; oreo de ≈ 12 h |
| 6 | `Q-UP-007` | `Q-FS-025` a `027` | 9–11 °C, 80–85 % HR, ≈ 150 días |
| 7 | `Q-UP-008` | `Q-FS-028` | Acondicionamiento ≈ 150 min |
| 8 | `Q-UP-009` | `Q-FS-029` a `031` | Cuñas de 180 g; 450 unidades/h |
| 9 | `Q-UP-010` | `Q-FS-032`, `033` | 4 °C; liberación a los 5 días |

**Requisitos de equipo:** `Q-UN-001`, `Q-UN-002`, `Q-UN-003`, `Q-UN-005`, `Q-UN-004` (salmuera), `Q-UN-007`, `Q-UN-008`, `Q-UN-009` (línea de Sopó), `Q-UN-010`, `Q-UN-011`. Ocupación de la tina: 254 min por lote.

**Calidad y almacenamiento**

| Criterio | Valor | Estado |
|---|---|---|
| Clase | Duro; humedad máxima 40 % (Res. 1804 de 1989) | PUBLIC |
| Humedad objetivo de diseño | ≈ 38 % | ASSUMED |
| Maduración mínima | Al menos 20 días con leche higienizada (norma); el modelo adopta ≈ 5 meses | PUBLIC / ASSUMED |
| Microbiología y rotulado | Resoluciones 1407 de 2022 y 810 de 2021 | PUBLIC |
| Almacenamiento | 4 °C; retención de 5 días hasta la liberación | ASSUMED |

---

### 6.4. Comparación

| Aspecto | Campesino | Mozzarella | Sopó |
|---|---|---|---|
| Queso por lote | 540 kg | 495 kg | 450 kg |
| Unidades por lote | 2.160 | 1.980 | 2.500 |
| Lotes al día | 3 | 2 | 2 |
| Unidades de proceso | `Q-UN-004`, `005`, `008` | `Q-UN-006`, `005` (moldeado), `008` | `Q-UN-005`, `004`, `007`, `008` |
| Fases propias | `Q-FS-018`, `022`, `023` | `Q-FS-024` | `Q-FS-019`, `020`, `022`, `023`, `025` a `027` |
| Ocupación de la tina | 224 min | 224 min | 254 min |

---

## 7. Relación del control de recetas con el batch

| Elemento | Receta maestra | Receta de control (batch) |
|---|---|---|
| Identificador | `Q-REC-00X` y versión | ID del batch, producto, receta y versión (Capa 12) |
| Valores | Objetivo y límites | Valores reales medidos |
| Origen de los valores | Diseño | Sensores (`Q-SEN-`), entradas manuales y registros de calidad |
| Vida | Permanente, con control de versiones | Una sola ejecución |
| Se modifica | Con una nueva versión aprobada | Nunca después de cerrado |

El flujo es: se selecciona el producto y la receta, se crea el batch, la receta se descarga a las unidades, las fases se ejecutan y cada una registra equipos, tiempos, variables y cantidades. Al final se verifica la calidad y el batch se libera o se retiene.

---

## 8. Relación con ISA-95

Se usa la numeración de 0 a 5 de la pirámide del proyecto. ISA-88 y ISA-95 se conectan en la receta y en el batch:

| Nivel ISA-95 | Qué pasa con la receta y el batch |
|---|---|
| N5 Empresa (ERP) | Solo conoce la orden de producción (producto, cantidad y fecha), los inventarios y los costos, y recibe la cantidad producida y los consumos |
| N4 Operaciones (MES) | Guarda la receta maestra y su versión, crea el batch y registra sus valores reales, la calidad y la trazabilidad |
| N3 Supervisión (SCADA / HMI) | Selecciona la receta y muestra el estado del batch. **En la situación actual no existe: se asume que quesos no tiene SCADA** |
| N2 Control | Ejecuta los procedimientos de unidad y las fases con los parámetros de la receta. **Hoy es el control local de cada equipo** |
| N1 Sensores y actuadores | Miden las variables y accionan los equipos |
| N0 Proceso | Las unidades físicas del modelo físico (sección 2) |

**Qué es de hoy y qué es de la propuesta.**

| Elemento | Situación actual | Propuesta |
|---|---|---|
| Unidades (`Q-UN-`) | Sí: salen de los equipos de las capas, con una máquina por proceso compartido | Se mantienen |
| Módulos de equipo (`Q-EM-`) | No: son una forma de agrupar los sensores y actuadores | Base para el diseño del control |
| Procedimientos de unidad y fases (`Q-UP-`, `Q-FS-`) | No: son un nivel nuevo que no existía en las capas | Base para el Grafcet y el Ladder |
| Recetas maestras (`Q-REC-`) | Sí: son información de producción, hoy en papel | Se mantienen |
| Receta de control (batch) | Sí: es la información que registra cada lote | Se mantiene |

El flujo es: el N5 baja la orden, el N4 asigna la receta y crea el batch, la receta llega al control de cada equipo (por el operador en la situación actual, desde el SCADA en la propuesta), y los valores reales suben hasta el MES para la trazabilidad.

Ver el entregable de ISA-95 para la pirámide completa de la situación actual.

---

## 9. Pendientes y supuestos

| Tema | Estado |
|---|---|
| Límites aceptables de cada parámetro (mínimo y máximo) | PENDING |
| pH de la leche, la cuajada y el estirado | PENDING |
| Cocción de la cuajada de la mozzarella | PENDING |
| Dosis de cultivos validada con la ficha de un proveedor | PENDING |
| Volumen de agua del estirado y de salmuera del Sopó | PENDING |
| Empaque secundario y tiempo de despacho | PENDING |
| Dimensionamiento de moldeado, prensado y enfriamiento | PENDING |
| Tiempos de espera y transferencia entre unidades distintos de los ya asumidos | PENDING |
| Fases: refinar y validar con el diseño del control (Grafcet) | PROPOSED |
| Quesos sin SCADA: selección de receta desde HMI local o el sistema de producción | Supuesto de diseño, por confirmar con el equipo |
| Recursos compartidos (CIP, refrigeración, agua) con las otras líneas | Por revisar en la integración de planta (`PL-`) |

Los valores de las secciones anteriores son supuestos académicos, referencias técnicas o cálculos del modelo. No describen la operación real de ninguna planta.
