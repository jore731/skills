# Tratamiento fiscal de inversiones y comisiones de brokers

Cómo tributan las inversiones (acciones, ETFs, fondos, CFDs, criptomonedas) y las comisiones de brokers como eToro, DEGIRO, Interactive Brokers, etc. en la Renta española (IRPF).

## Dónde tributan las inversiones

| Tipo de rendimiento | Base | Capítulo del manual |
|---------------------|------|---------------------|
| Dividendos, intereses, cupones | Ahorro — Rendimientos capital mobiliario | Cap. 5 |
| Ganancias/pérdidas por venta de acciones, ETFs, fondos, cripto | Ahorro — Ganancias patrimoniales | Cap. 11 |
| CFDs, futuros, opciones (especulativos) | Ahorro — Ganancias patrimoniales | Cap. 11 (Art. 37.1.m) |

## Ganancias y pérdidas patrimoniales por transmisión de valores

### Cálculo (Art. 34-36 Ley IRPF)

```
Ganancia = Valor de transmisión − Valor de adquisición
```

**Valor de adquisición** = Precio de compra + gastos inherentes (comisiones, tasas, impuestos) − amortizaciones

**Valor de transmisión** = Precio de venta − gastos inherentes (comisiones, tasas, impuestos)

> **Los intereses NO forman parte** del valor de adquisición ni de transmisión.

🔗 [Reglas generales de cálculo — valor de adquisición y transmisión](https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/irpf-2025/c11-ganancias-perdidas-patrimoniales/calculo-ganancias-perdidas-patrimoniales/reglas-generales-calculo.html)

### Valores cotizados (Art. 37.1.a)

Para acciones y ETFs cotizados:
- Valor de transmisión = Mayor entre precio de venta o valor de cotización a fecha de transmisión
- Aplica criterio FIFO (Art. 37.2)

🔗 [Transmisiones onerosas de valores cotizados](https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/irpf-2025/c11-ganancias-perdidas-patrimoniales/calculo-ganancias-perdidas-patrimoniales/ganancias-perdidas-patrimoniales-derivadas-transmisiones/transmisiones-elementos-patrimoniales/transmisiones-onerosas/valores-representativos-participacion-fondos/transmision-valores-admitidos-negociacion.html)

---

## Comisiones de brokers — Tratamiento fiscal

### Comisiones de compra/venta

**Son gastos inherentes a la adquisición/transmisión** (Art. 35) → reducen la ganancia patrimonial.

- Comisiones de compra → se suman al valor de adquisición
- Comisiones de venta → se restan del valor de transmisión
- El manual menciona explícitamente *"comisiones, Fedatario público, Registro, etc."*

### Spreads (eToro, brokers sin comisión explícita)

- El spread está embebido en el precio de ejecución
- Se refleja automáticamente en los valores de compra/venta
- No requiere ajuste manual — ya está incorporado en el precio

### Comisiones de custodia / administración y depósito

- **Solo deducibles de rendimientos del capital mobiliario** (dividendos), NO de ganancias patrimoniales
- Art. 26.1.a: *"gastos de administración y depósito de valores negociables"*
- **Excluidos expresamente:** gastos de gestión discrecional e individualizada de carteras

🔗 [Gastos deducibles: de administración y depósito](https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/irpf-2025/c05-rendimientos-capital-mobiliario/rendimientos-obtenidos-participacion-fondos-propios/dividendos-participaciones-beneficios-entidad/gastos-deducibles-administracion-deposito.html)

### Comisiones overnight (eToro CFDs)

- Se restan del resultado de la operación
- Forman parte del cálculo de la ganancia/pérdida patrimonial

### Comisiones de conversión de divisa

- Son un gasto inherente a la operación → reducen la ganancia patrimonial
- Se incluyen en el valor de adquisición o transmisión según corresponda

### Comisiones NO deducibles

| Comisión | ¿Deducible? | Motivo |
|----------|-------------|--------|
| Comisión de inactividad | ❌ No | No es inherente a ninguna operación |
| Comisión de retirada | ❌ No | No es inherente a la adquisición/transmisión de valores |
| Gestión de carteras (robo-advisors) | ❌ No | Excluido expresamente por Art. 26.1.a |

---

## CFDs, futuros y opciones (Art. 37.1.m)

Las operaciones en mercados de futuros y opciones (incluidos CFDs) tributan como **ganancias patrimoniales** cuando no son operaciones de cobertura vinculadas a una actividad económica.

🔗 [Operaciones en mercados de futuros y opciones](https://sede.agenciatributaria.gob.es/Sede/ayuda/manuales-videos-folletos/manuales-practicos/irpf-2025/c11-ganancias-perdidas-patrimoniales/calculo-ganancias-perdidas-patrimoniales/ganancias-perdidas-patrimoniales-derivadas-transmisiones/transmisiones-elementos-patrimoniales/transmisiones-onerosas/operaciones-realizadas-mercados-futuros-opciones.html)

---

## Reglas clave para inversiones

### Criterio FIFO (Art. 37.2)

Al vender parcialmente valores homogéneos, se consideran transmitidos los adquiridos en primer lugar.

### Regla antiaplicación (Art. 33.5.f)

No se computan como pérdida patrimonial las derivadas de transmisiones cuando se recompran valores homogéneos:
- **2 meses** antes o después → valores cotizados
- **1 año** antes o después → valores no cotizados

### Dividendos

- Tributan como rendimientos del capital mobiliario en la base del ahorro
- No existe mínimo exento (se eliminó en 2015)
- Retención en origen (extranjera) → deducción por doble imposición internacional (Art. 80)

### Criptomonedas

- Las permutas entre criptomonedas generan ganancia/pérdida patrimonial
- Se aplican las mismas reglas de cálculo que para valores
- Aplica FIFO y regla antiaplicación
- Obligación de informar sobre monedas virtuales (Modelo 721 si en el extranjero > 50.000 €)

---

## Resumen por tipo de comisión de broker

| Tipo de comisión | Dónde se aplica | Efecto fiscal |
|------------------|----------------|---------------|
| Compra/venta | Ganancias patrimoniales | ↑ valor adquisición / ↓ valor transmisión |
| Spread | Ganancias patrimoniales | Embebido en precio (automático) |
| Custodia/depósito | Rdtos. capital mobiliario | Gasto deducible de dividendos |
| Overnight (CFDs) | Ganancias patrimoniales | Se resta del resultado |
| Conversión divisa | Ganancias patrimoniales | Gasto inherente a la operación |
| Inactividad | — | No deducible |
| Retirada | — | No deducible |
| Gestión carteras | — | No deducible (excluido Art. 26.1.a) |
