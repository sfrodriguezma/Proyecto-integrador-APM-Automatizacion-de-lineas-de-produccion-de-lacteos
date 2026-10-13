# Módulo 0 · Caracterización de las líneas: modelo de 12 capas

**Proyecto:** INTEGRAFLUX · Automatización de Procesos de Manufactura · UNAL 2026-2S
**Responsables:** CMO Pinilla (UHT) · CCO Mora y CFO Góngora (yogures) · COO Avellaneda y CTO Barrero (quesos y UHT)
**Estado:** 🟢 Las tres líneas tienen las 12 capas definidas. La capa 10 de yogures y UHT queda parcial por los consumos de servicios.

> La planta de Sopó es una referencia académica. Ningún dato es real de la empresa. Cada valor lleva una etiqueta: `PUBLIC`, `REFERENCE`, `ASSUMED`, `CALCULATED`, `PROPOSED` o `PENDING`.

---

## 1. Objetivo

Describir cada línea de la planta con la misma estructura de 12 capas y con tres productos por línea, para tener nueve productos comparables. Esta caracterización es la base de los módulos 1 a 7 (arquitectura ISA-88/95, VSM y simulación, presupuesto, celda, gemelo digital, control y SCADA).

## 2. Qué significa cada capa

| # | Capa | Pregunta que responde | Qué contiene | Alimenta a |
|---|---|---|---|---|
| 1 | **Contexto** | ¿Qué línea es y qué fabrica? | Empresa, planta, línea, productos, justificación y alcance | Todo el modelo |
| 2 | **Materias primas** | ¿Con qué se fabrica? | Leche, ingredientes, aditivos, cultivos, material de empaque, fórmula base | Recetas, balance de masa |
| 3 | **Flujo general** | ¿Por dónde pasa el producto? | Diagrama de flujo común y por producto, entradas, salidas, flujo de información, almacenamientos | Layout, PFD, VSM |
| 4 | **Proceso detallado** | ¿Qué operaciones hay y con qué parámetros? | Process Master (`OP`), parámetros por operación, diferencias entre productos | Equipos, variables, DAP |
| 5 | **Equipos** | ¿Con qué máquinas? | Inventario (`EQ`) con función, capacidad y operación asociada | Capacidades, presupuesto |
| 6 | **Variables** | ¿Qué hay que medir o controlar? | Variables (`VAR`) con unidad, rango y criticidad | Instrumentación, SCADA |
| 7 | **Sensores y actuadores** | ¿Con qué se mide y se actúa? | Sensores (`SEN`), actuadores (`ACT`), escenario base frente a propuesto | Arquitectura ISA-95, control |
| 8 | **Tiempos** | ¿Cuánto dura cada cosa? | Tiempos por operación (`TMP`), limpieza, cambios, ocupación de equipos | VSM, DAP, takt |
| 9 | **Capacidades** | ¿Cuánto se puede producir? | Capacidad por equipo, lote, turno y día (`CAP`), utilización, cuello de botella | Simulación, OEE |
| 10 | **Recursos** | ¿Qué se necesita para operar? | Personal, servicios, limpieza, infraestructura, sistemas de información (`RES`) y recursos compartidos (`PL`) | OPEX, integración de planta |
| 11 | **Recetas** | ¿Cómo se fabrica cada producto? | Fórmula por lote, procedimiento, parámetros objetivo y criterios de liberación (`REC`) | ISA-88, controladores |
| 12 | **Batches y trazabilidad** | ¿Cómo se registra cada ejecución? | Formato de lote, campos registrados, estados, genealogía | MES, ERP |

Cadena lógica: **Producto → Receta → Batch** (qué se fabrica → cómo → cada ejecución concreta).

## 3. Identificación y origen de los datos

| Elemento | Quesos | Yogures | UHT |
|---|---|---|---|
| Prefijo de línea | `Q` | `Y` | `U` |
| Formato de ID | `Q-OP-001`, `Q-EQ-001`, `Q-VAR-001`… | `Y-OP-001`… | `U-OP-001`… |
| Formato de batch | `AAAAMMDD-Q-NNN-XXX` | `AAAAMMDD-Y-PPP-###` | `AAAAMMDD-U-PPP-###` |
| Recursos compartidos | `PL-EQ-001` a `005` y `PL-RES-001/002`: recepción y silos, CIP, vapor, agua helada, aire comprimido, agua y mantenimiento | | |

| Etiqueta | Significado |
|---|---|
| `PUBLIC` | Información pública verificable |
| `REFERENCE` | Literatura o fuente técnica |
| `ASSUMED` | Supuesto del modelo académico |
| `CALCULATED` | Resultado de un cálculo |
| `PROPOSED` | Propuesta del proyecto |
| `PENDING` | Registro definido, valor sin obtener |

## 4. Los nueve productos

| Línea | ID | Producto | Presentación | Rasgo que lo diferencia | Receta | Lote | Producción por lote | Lotes/día |
|---|---|---|---|---|---|---|---|---|
| Quesos | `Q-PRD-001` | Queso Campesino | Cuña 250 g | Salado en masa, prensado 60 min, enfriamiento 12 h | `Q-REC-001` | 4.500 kg de leche | 540 kg · 2.160 un. | 3 |
| Quesos | `Q-PRD-002` | Queso Mozzarella | Bloque 250 g | Pasta filata: estirado a 65–70 °C | `Q-REC-002` | 4.500 kg de leche | 495 kg · 1.980 un. | 2 |
| Quesos | `Q-PRD-003` | Queso Sopó | Cuña 180 g | Salmuera ≈36 h y maduración ≈5 meses a 9–11 °C | `Q-REC-003` | 4.500 kg de leche | 450 kg · 2.500 un. | 2 |
| Yogures | `Y-PRD-001` | Yogur semidescremado fresa | Vaso 150 g | Fermentación termófila 4–5 h y fruta en línea (15 %) | `Y-REC-001` | 4.000 kg de base | 4.706 kg · 31.373 vasos | 3 |
| Yogures | `Y-PRD-002` | Yogur griego natural | Vaso 150 g | Ultrafiltración (factor ≈2,7) hasta ≥8 % de proteína | `Y-REC-002` | 4.000 kg de base | 1.481 kg · 9.877 vasos | 2 |
| Yogures | `Y-PRD-003` | Kumis dulce entero | Botella 200 g | Fermentación mesófila a 28–30 °C durante 16–20 h | `Y-REC-003` | 4.000 kg de base | 4.000 kg · 20.000 botellas | 2 |
| UHT | `U-PRD-001` | Leche entera UHT | Caja aséptica 1 L | Ruta base: UHT a 138–142 °C | `U-REC-001` | 10.000 kg | 9.709 cajas | 3 |
| UHT | `U-PRD-002` | Leche deslactosada semidescremada UHT | Caja aséptica 1 L | Dosificación aséptica de lactasa; hidrólisis ≈24 h | `U-REC-002` | 10.000 kg | 9.709 cajas | 1 |
| UHT | `U-PRD-003` | Bebida láctea achocolatada UHT | Caja aséptica 200 mL | Hidratación de polvos; sólidos 17–19 % | `U-REC-003` | 10.000 kg | 48.544 cajas | 1 |

Los productos de cada línea se diferencian en ingredientes, parámetros, tiempos, envase, inspección y trazabilidad, como pide el documento del integrador.

## 5. Resumen por línea

| | **Quesos** (`Q`) | **Yogures** (`Y`) | **UHT** (`U`) |
|---|---|---|---|
| Naturaleza | Batch con espera larga (cuajada, maduración) | Continuo + batch (fermentación) + discreto (envasado) | Continuo + envasado aséptico discreto |
| Operaciones | 20 | 26 | 20 |
| Equipos | 18 | 24, más 5 de planta | 17, más 5 de planta |
| Variables | 22 | 25 | 20 |
| Sensores · actuadores | 12 · 15 | 24 · 22 | 16 · 14 |
| Producción diaria | 3.510 kg/día | 25.081 kg/día | 50.000 kg/día |
| Leche cruda por día | ≈ 31.500 kg (7 × 4.500) | ≈ 27.162 kg | ≈ 50.505 kg |
| Cuello de botella | Tinas (87 % de uso) y cámara de maduración | Llenadora de vasos (101,4 %) | Llenadora de 1 L (62,2 %, con holgura) |
| Cadena de frío | Sí | Sí | No; cuarentena de esterilidad de 72 h |
| Subproducto | Suero | Suero ácido y crema | Crema |

Estado de las capas: quesos y UHT con las 12 capas definidas. Yogures igual, con la capa 10 en 🟡 por los consumos de servicios, que se calculan en el módulo de presupuesto.

## 6. Líneas compartidas: recursos de planta

Las tres líneas usan los mismos recursos de planta, que pueden limitar la producción global: silos de leche cruda (`PL-EQ-001`), CIP (`PL-EQ-002`), vapor o agua caliente (`PL-EQ-003`), agua helada (`PL-EQ-004`), aire comprimido (`PL-EQ-005`), laboratorio, mantenimiento y cámaras de frío. La matriz de recursos compartidos consolidada de las tres líneas sigue pendiente.

## 7. Pendientes y advertencias

- **Versión del documento de quesos:** el archivo del proyecto en Claude (`Linea de quesos.md`) está en una versión anterior, con las capas 8 a 12 en `PENDING`. Esta caracterización usa la versión actualizada, con tiempos, capacidades y recetas. Conviene reemplazar el archivo del proyecto.
- **Uso de tinas:** la capa 9 de quesos indica 87 % y el VSM más reciente indica ≈79 %. Hay que unificar la cifra.
- **Supuestos:** el tamaño de lote, el número de tanques, el OEE base de 65 % y las velocidades de llenadoras son `ASSUMED` y deben validarse en la visita industrial.
- **Consumos de servicios** (vapor, agua helada, aire, energía): pendientes para el presupuesto.
- **Matriz de recursos compartidos** de las tres líneas: por consolidar.
- **Normativa:** verificar si la Res. 1407 de 2022 modificó artículos fisicoquímicos de la Res. 2310 de 1986.

## 8. Archivos fuente

`Linea_de_quesos.md` · `Linea_de_yogures.md` · `Linea_de_UHT.md` (cada uno con sus 12 capas, tablas de IDs y fuentes).
