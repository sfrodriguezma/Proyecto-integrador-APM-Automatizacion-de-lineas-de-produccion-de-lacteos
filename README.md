# INTEGRAFLUX
## Informe de avance: automatización de la línea de quesos

**Curso:** Automatización de Procesos de Manufactura · Universidad Nacional de Colombia · 2026-2S
**Proyecto:** Integrador, transformación digital industrial (TDI) de una planta láctea de referencia (Sopó)
**Fecha de corte:** octubre de 2026 · **Versión:** avance preliminar

> **Nota sobre los datos.** La planta es una referencia académica. Ningún valor corresponde a datos reales de la empresa de referencia. Cada cifra se clasifica como `REFERENCE`, `ASSUMED`, `CALCULATED`, `PROPOSED` o `PENDING` y debe validarse en planta antes de cualquier decisión de inversión.

---

## 1. Equipo y roles

| Rol | Integrante | Responsabilidades y aportes |
|---|---|---|
| **CEO** | Sergio Felipe Rodríguez Mayorga | Acotamiento y planteamiento del problema; simulación en Tecnomatix |
| **CTO** | Juan Manuel Barrero Mendoza | Apoyo en la caracterización de la línea UHT; demanda e indicadores; apoyo en los VSM de los tres productos |
| **CFO** | Juliana Góngora Rasmussen | Apoyo en yogures; matriz de decisión; apoyo en diagramas de flujo, PFD, DAP y VSM; EDT y Gantt del proyecto |
| **COO** | Sergio Avellaneda Piñeros | Apoyo en la caracterización de quesos; diagrama general de la planta; layout de las tres líneas |
| **CCO** | Juan Andrés Mora Henao | Caracterización de la línea de yogures; presupuesto preliminar; flujos de caja |
| **CMO** | Andrés Gustavo Pinilla Martínez | Línea UHT; ISA-88; ISA-95; página web |

---

## 2. Introducción y alcance

INTEGRAFLUX es una empresa integradora de soluciones TDI. El caso analiza una planta con tres líneas por lotes (yogures, leche UHT y quesos). La **matriz de decisión** seleccionó la **línea de quesos** como objeto de automatización. El trabajo aplica el modelo de **12 capas** (contexto, materias primas, flujo, proceso, equipos, variables, sensores, tiempos, capacidades, recursos, recetas y batches) documentado en `Linea_de_quesos.md`, `Linea_de_yogures.md` y `Linea_de_UHT.md`.

---

## 3. Caracterización de las líneas

- **Yogures:** línea por lotes con fermentación y envasado. Se caracterizó con el modelo de 12 capas.
- **Leche UHT:** línea de tratamiento térmico y envasado aséptico. Se caracterizó con el mismo modelo.
- **Quesos (línea seleccionada):** 3 referencias, lote de 4.500 kg de leche, 7 lotes diarios (3 Campesino, 2 Mozzarella, 2 Sopó) y 3 tinas de elaboración.

| Referencia | Presentación | Queso por lote | Ruta diferenciadora |
|---|---|---|---|
| Campesino (Q-PRD-001) | Cuña 250 g | 540 kg | Salado en masa, moldeo y prensado, enfriamiento 12 h |
| Mozzarella (Q-PRD-002) | Bloque 250 g | 495 kg | Acidificación, estirado/hilado y moldeo, enfriamiento |
| Sopó (Q-PRD-003) | Cuña 180 g | 450 kg | Prensado, salmuera ≈36 h, oreo y maduración ≈150 d |

---

## 4. Arquitectura: ISA-88 e ISA-95 (estado actual)

Se levantó el modelo físico, procedimental y de recetas (ISA-88) y la ubicación de la planta en los niveles N0–N5 (ISA-95), con la separación OT/IT. El análisis de brechas por nivel (N1–N3) está **en elaboración**. Los entregables 3.4 y 3.5 están por confirmar.

---

## 5. Diagnóstico del estado actual de la línea de quesos

### 5.1 Planta, layout y flujo
Se cuenta con el diagrama general de la planta y el layout de las tres líneas. El flujo de la línea de quesos se separa en un **frente común** (recepción, estandarización y pasteurización) y tres ramas por referencia. Cada rama tiene diagrama de flujo y PFD.

### 5.2 Diagrama de análisis de proceso (DAP)
Se usan los símbolos ASME (○ operación, → transporte, D demora, □ inspección, ▽ almacenamiento).

| | Campesino | Mozzarella | Sopó |
|---|---|---|---|
| Tiempo total | 6.375 min (≈4,4 d) | 5.672 min (≈3,9 d) | 227.764 min (≈158 d) |
| Distancia | 97 m | 84 m | 147 m |
| ○ · → · D · □ · ▽ | 14 · 6 · 2 · 4 · 2 | 14 · 5 · 2 · 5 · 2 | 16 · 7 · 1 · 5 · 2 |

### 5.3 Mapa de la cadena de valor (VSM) del estado actual
| | Campesino | Mozzarella | Sopó |
|---|---|---|---|
| Lead time con empaque | 4,34 d | 3,86 d | 158,1 d |
| Valor agregado | 2,9 % | 3,2 % | 0,09 % |
| Cuello de botella | Enfriamiento y acondicionamiento | Tina de elaboración (compartida) | Cámara de maduración |

Indicadores de línea (`CALCULATED`): takt 94,7 min/lote, ciclo efectivo 75,2 min, **OEE 67,1 %** (disponibilidad 84,6 % × rendimiento 81,0 % × calidad 98 %), uso de tinas ≈79 %, empaque ≈88 %.


### 5.4 Modelo de simulación en Tecnomatix
Se construyó un modelo del proceso actual. Su validación contra el VSM y el DAP está en curso (entregable 5.9).


---

## 6. Propuesta preliminar

Esta propuesta es **`PROPOSED`**. Debe validarse con datos de planta y con el avance de los paquetes 6 a 11 del EDT.

### 6.1 Concepto
Reducir los tiempos de espera y elevar el OEE de la línea de quesos mediante una arquitectura TDI por niveles ISA-95 y una celda robotizada en la zona de empaque y paletizado.

### 6.2 Componentes
| Componente | Propuesta | Problema que atiende |
|---|---|---|
| Instrumentación y control (N1–N2) | Sensores de temperatura, pH y nivel en tinas y cámaras; controladores programados con Grafcet/Ladder, recetas ISA-88 | Variabilidad de proceso y dependencia del operario |
| SCADA (N2–N3) | Supervisión de tinas, enfriamiento y maduración con alarmas y tendencias | Falta de visibilidad en tiempo real |
| MES/ERP (N3–N4) | Trazabilidad de lotes, programación de las tres tinas y cálculo automático de OEE | Cuello de botella en la tina compartida |
| Celda robotizada | Empaque y paletizado (simulación en RobotStudio) | Empaque al ≈88 % de uso, segundo punto crítico en las tres referencias |
| Gemelo digital | Modelo en Tecnomatix para probar escenarios antes de invertir | Riesgo de implementación |

### 6.3 Evaluación económica preliminar (`ASSUMED`/`CALCULATED`)
| Concepto | Valor |
|---|---|
| Inversión total | 2.807 M COP |
| Ingeniería | 287 M COP |
| Celda robotizada | 1.726 M COP |
| VPN | +1.857 M COP |
| TIR | 28,6 % |
| Punto de equilibrio | 61 % |
| Recuperación de la inversión | 3,3 años |

Estas cifras salen del presupuesto preliminar y del flujo de caja, con supuestos de precios, tasa de descuento y beneficios aún por validar.

---

## 7. Plan del proyecto

El EDT tiene **12 paquetes y 70 entregables**. El cronograma dura **49 semanas** (3 ago 2026 → 15 jul 2027) y tiene **16 hitos** (H1–H16).

| Fecha | Evento |
|---|---|
| 14 y 16 oct 2026 | Presentación de avance (según calendario V2) |
| 7 dic 2026 | Entrega final |
| 9 y 11 dic 2026 | Sustentaciones finales |
| 17 dic 2026 | H11: decisión de inversión del cliente |
| 15 jul 2027 | H16: cierre y aceptación final |

*Observación:* el documento del proyecto integrador indica entrega intermedia el 5 de octubre. Conviene confirmar con el docente cuál fecha rige.

### Avance frente al EDT

| Paquete | Estado | Entregables |
|---|---|---|
| 1. Modelo de 12 capas | ✅ Hecho | 1.1 Análisis de las 12 capas de las tres líneas |
| 2. Selección de la línea | ✅ Hecho | 2.1 Matriz de decisión (quesos seleccionada) |
| 3. Arquitectura TDI |✅ Hecho| ISA-88 e ISA-95  |
| 4. Planeación | ✅ Hecho | 4.1 EDT · 4.2 Cronograma y calendario |
| 5. Diagnóstico del estado actual | ✅ Hecho | ✅ 5.1 Diagramas de proceso · 5.2 PFD · 5.3 Layout · 5.5 VSM · 5.6 DAP · ✅ 5.7 Tiempos y capacidades · 5.8 Indicadores · ⬜ 5.4 Espagueti · ✅ 5.9 Simulación en Tecnomatix |
| 6. Diseño de la solución | 🟡 Adelantado | 6.8 Presupuesto preliminar y 6.9 Flujo de caja (en elaboración) |
| 7–12 | ⬜ Por iniciar | Simulación, celda robotizada, control y SCADA, evaluación técnico-económica, adquisiciones y puesta en marcha |

✅ hecho · 🟡 parcial o en curso · ⬜ no iniciado. El estado de 5.3 a 5.9 se dedujo de las contribuciones del equipo y debe confirmarse.

---

## 8. Aprendizajes

1. **El cuello de botella no siempre está en la transformación.** En las tres referencias pesan el enfriamiento, la tina compartida, la maduración y el empaque, no las operaciones directas.
2. **La mayor parte del lead time es espera.** El valor agregado va de 0,09 % a 3,2 %, así que la mejora está en eliminar demoras, no en acelerar equipos.
3. **Las herramientas deben conciliarse entre sí.** DAP, VSM y el modelo de 12 capas deben dar los mismos totales. Al cuadrarlos aparecieron actividades en cero, una acidificación omitida y un bloque de 60 min sin repartir.
4. **Las referencias comparten equipos pero no ruta.** Programar tres productos en tres tinas exige tratar a la tina como recurso restringido.
5. **Los supuestos deben ser visibles.** Etiquetar cada dato (`REFERENCE`, `ASSUMED`…) evita presentar estimaciones como datos reales.
6. **La simulación respalda la inversión.** Un modelo validado en Tecnomatix permite mostrar el beneficio de la celda antes de comprometer capital.
7. **El trabajo en equipo por roles acelera el avance,** pero requiere una versión única de cada documento (por ejemplo, el VSM).

---


