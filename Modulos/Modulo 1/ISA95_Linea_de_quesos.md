# ISA-95 · Línea de quesos: del proceso al ERP

Proyecto Integrador · Automatización de Procesos de Manufactura 2026-2 · IntegraFlux

Este documento describe cómo se organiza la información de la línea de quesos (campesino, mozzarella y Sopó) según los seis niveles de ISA-95, del proceso (nivel 0) al ERP (nivel 5). Resume lo que hay hoy en cada nivel y lo que el proyecto desarrolla. Los valores numéricos son supuestos de diseño del equipo (`ASSUMED`) o cálculos sobre ellos (`CALCULATED`), definidos en las 12 capas de la línea de quesos.

## 1. Idea general

- Los niveles 0 a 3 son la parte de operación (OT): el proceso, los sensores y actuadores, el control y la supervisión.
- Los niveles 4 y 5 son la parte de información (IT): la gestión de la producción (MES) y la gestión empresarial (ERP).
- **Hacia arriba** suben las mediciones, los estados, los lotes y la producción realizada.
- **Hacia abajo** bajan las órdenes, las recetas y las consignas.

```
N5  ERP                      ▲ producción realizada y consumos      ▼ programación y cantidades
N4  MES                      ▲ cantidades, calidad, paradas         ▼ órdenes y lotes programados
N3  SCADA                    ▲ alarmas, tendencias, estado del lote ▼ comandos del operador y receta
N2  Control                  ▲ estado de equipos y secuencia        ▼ consignas y parámetros de la receta
N1  Sensores y actuadores    ▲ mediciones y estados                 ▼ órdenes a válvulas, bombas y variadores
N0  Proceso                  ▲ temperatura, caudal, nivel, presión  ▼ materias primas y producto según la receta
```

## 2. Qué hay hoy y qué se desarrolla en cada nivel

| Nivel | Situación inicial | Lo que se desarrolla |
|---|---|---|
| **N0 · Proceso** | 20 operaciones en tres rutas (16, 15 y 17 etapas); 11 unidades y 18 equipos, con una máquina por proceso compartido, tres tinas de 5.000 L y tres empacadoras, una por producto | Línea de quesos por lotes, que combina operaciones continuas, por lotes y discretas |
| **N1 · Sensores y actuadores** | 22 variables de proceso definidas, con 12 sensores y 15 actuadores de referencia. Varias variables se miden de forma manual | Sensores y actuadores virtuales, representados en el gemelo digital |
| **N2 · Control** | Control local en cada equipo, sin controlador de línea. El operador ajusta los parámetros de la receta | Control lógico-secuencial diseñado en Grafcet e implementado en Ladder con Logix Emulate; gestiona los parámetros de cada receta |
| **N3 · SCADA** | Sin SCADA: cada equipo se opera desde su control local y los registros van en papel | Supervisión y operación con una interfaz HMI bajo ISA-101 en Ignition, comunicada por OPC |
| **N4 · MES** | Órdenes, batches, calidad, tiempos y trazabilidad definidos como información (Capa 12), con el mantenimiento incluido | Gestión de la producción: órdenes, lotes, recetas, calidad, paradas, trazabilidad e indicadores. Se plantea de forma conceptual |
| **N5 · ERP** | Plan diario de 7 lotes (31.500 kg de leche y 3.510 kg de queso), inventarios (≈ 15 t retenidas y ≈ 116 t en maduración) y consumos por lote | Gestión empresarial: recibe el desempeño de la producción y entrega la programación. Se plantea de forma conceptual |

## 3. Detalle de cada nivel

### N0 · Proceso
Recepción de leche, tratamiento térmico, elaboración en tina, salado, moldeado y prensado, estirado (mozzarella), maduración (Sopó), enfriamiento, empaque, almacenamiento y despacho.
- **Sube:** temperatura, caudal, nivel y presión; producto, lote y calidad.
- **Baja:** materias primas y producto según la receta.

### N1 · Sensores y actuadores
Instrumentación que mide y acciona el proceso (por ejemplo, temperatura de pasteurización, temperatura y pH en la tina, presión de prensado, pesaje en el empaque).
- **Sube:** mediciones de las variables críticas y estados de válvulas, bombas y motores.
- **Baja:** órdenes a válvulas, bombas y variadores.

### N2 · Control
Control lógico-secuencial por unidad, con los parámetros de cada receta (temperaturas de coagulación y cocción, tiempos, presiones).
- **Sube:** estado de equipos y de la secuencia; variables de proceso.
- **Baja:** consignas y parámetros de la receta.

### N3 · SCADA
Interfaz del operador: visualiza el proceso, las alarmas y las tendencias, y permite seleccionar la receta y el lote.
- **Sube:** alarmas y tendencias; producto o receta y estado del lote.
- **Baja:** comandos del operador y selección de receta.

### N4 · MES
Gestión de la producción por lotes, con los registros de la Capa 12 de las 12 capas: identificador del lote, receta y versión, equipos usados, parámetros reales, resultados de calidad y liberación.
- **Sube:** cantidades producidas y calidad; paradas, trazabilidad e indicadores.
- **Baja:** órdenes de producción y lotes programados.

### N5 · ERP
Programación y cantidades a producir, inventarios y consumos.
- **Sube:** producción realizada y consumos.
- **Baja:** programación y cantidades a producir.

## 4. Relación con ISA-88

| ISA-95 | ISA-88 |
|---|---|
| N0 · Proceso | Modelo físico: 11 unidades (por ejemplo, tina de elaboración, prensa, cámara de maduración, empacadoras) y sus equipos |
| N2 · Control | Procedimientos de unidad y operaciones (10 procedimientos y 20 operaciones) ejecutados sobre cada unidad |
| N4 · MES | Recetas maestras (Q-REC-001, 002 y 003) y registro de cada batch con la versión de receta utilizada |
| N5 · ERP | Plan diario de lotes (3 de campesino, 2 de mozzarella y 2 de Sopó) |

## 5. Alcance del proyecto en este tema

- **Se desarrolla:** control (Grafcet y Ladder en Logix Emulate) y supervisión (Ignition con HMI bajo ISA-101, comunicada por OPC).
- **Se plantea de forma conceptual:** MES y ERP.
- **Ya definido como información:** órdenes, lotes, recetas, calidad, tiempos y trazabilidad (Capa 12), y el plan diario.

## 6. Supuestos

- Los valores del plan diario (7 lotes, 31.500 kg de leche, 3.510 kg de queso) son supuestos de diseño.
- Las cantidades de sensores y actuadores (12 y 15) son de referencia, no una lista definitiva de instrumentación.
- No hay datos reales de la planta: el caso es representativo y no corresponde a una empresa específica.
