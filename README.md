# Procurement Analytics Case Study — Iluma Alliance
**Evaluación de Compras de Oportunidad (Spot vs. Contrato) y Modelo Analítico**

Este repositorio contiene la resolución técnica del caso de estudio de adquisiciones de Iluma Alliance. El objetivo del proyecto es responder con rigor analítico y criterio de abastecimiento si las compras de oportunidad (`BuyingMode = "Spot"`) representaron un beneficio real frente a los acuerdos prenegociados (`BuyingMode = "Contract"`), considerando fletes, costos de almacenamiento, capacidad instalada de planta y el impacto financiero de las mermas por vencimiento.

---

## 1. Resumen Ejecutivo y Diagnóstico del Negocio

Las compras de oportunidad (Spot) sí representan una palanca legítima de ahorro y flexibilidad operativa, pero su rentabilidad está estrictamente condicionada a la categoría del insumo, la rotación de inventarios y la capacidad física instalada. El análisis demuestra una marcada bifurcación entre categorías:

* **Insumos No Perecederos (Packaging y MRO / Spare Parts):** El modelo Spot operó sin riesgo y capturó valor, registrando **$0,00 USD en pérdidas por vencimiento** (0 lotes vencidos) y costos unitarios controlados entre **12,33 y 17,08 USD**. En estas líneas, vidas útiles de entre 800 y 2.300 días y tarifas marginales de bodegaje blindan el valor del inventario.
* **Materias Primas Perecederas (Raw Materials):** La compra Spot concentró el 100% de las fallas operativas, generando **$2.963.030,80 USD en mermas por vencimiento** (17 lotes vencidos de 21 pedidos) y **$656.623 USD en costos de bodegaje prolongado**. En los centros Puerto (P) y Sur (S), el 100% de las órdenes de materia prima se venció en almacenamiento, elevando el costo real por unidad consumida en la categoría a **491,08 USD** frente a los 23,8 USD facturados originalmente.
* **Quiebre de Capacidad por Sobrecompra:** Se identificó que compras masivas no coordinadas saturaron la infraestructura de planta. Como caso crítico, la orden `PO10156-N` ingresó **60.785 unidades** a la Planta Norte, superando por sí sola el 100% de la capacidad total del centro (60.000 unidades).

**Directriz de Negocio:** No se debe prohibir la modalidad Spot de forma general, sino habilitarla con agilidad en empaques y repuestos, e implementar un bloqueo estricto en materias primas perecederas sujeto a validación previa de cupo físico y una orden de producción inmediata garantizada.

---

## 2. Supuestos Metodológicos y Reglas de Negocio

1. **Costo de Transporte e Imputación Logística:** En la exploración inicial de fletes se observaron registros atípicos y órdenes incompletas. Se determinó utilizar un costo logístico promedio de **1.138 USD** para cubrir datos faltantes y normalizar la comparación de costos puestos en planta.
2. **Criterio Contable de Mermas (Write-off al 100%):** Ante la ausencia de un costo explícito de baja en las reglas de negocio, se adoptó la política conservadora de castigar contablemente al 100% el valor de cualquier lote cuyos días en bodega superaron su vida útil (`DaysInStorage > ShelfLifeDays`), absorbiendo factura de compra, flete y almacenamiento acumulado.
3. **Tratamiento de No Perecederos y Servicios:** Para las categorías de Packaging, MRO, Servicios Logísticos y consumibles desconocidos, cualquier pérdida por vencimiento se fijó estrictamente en **0**, dado que carecen de fecha de caducidad física o sus amplias vidas útiles impiden técnicamente la merma.
4. **Condición de Comparabilidad en Costos Unitarios:** En combinaciones de centros o materiales donde no coexisten compras en ambas modalidades, o en lotes donde el 100% del volumen se venció en bodega (sin unidades útiles aprovechables), las métricas comparativas devuelven `BLANK()` para evitar que un cero artificial distorsione el análisis con ahorros inexistentes.
5. **Límites de Confianza:** El análisis se basa en una muestra de 165 órdenes de compra en un periodo de nueve meses. Los días en bodega corresponden a una medición estática del lote y no a una curva de consumo progresivo, por lo que las conclusiones operan como un diagnóstico direccional para compras.

---

## 3. Estructura de Entregables del Repositorio

```text
├── README.md                                <- Resumen ejecutivo, metodología y nota de IA
├── Caso_Estudio_Iluma_Alliance.pbix         <- Modelo analítico, Power Query y medidas DAX
└── Presentacion_Ejecutiva_Iluma.pdf         <- Documento de sustento ejecutivo (Task 1, Task 2 e IA)
