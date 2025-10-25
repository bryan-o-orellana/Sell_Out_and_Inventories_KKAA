# 📊 DAX Measures — Sell Out & Inventory KKAA

A collection of DAX measures used to calculate **Sell Out MTD performance** and **growth vs. last year** in the Power BI report.

---

## 1️⃣ Sell Out MTD
Calculates the **month-to-date (MTD)** sales up to the current day of the month.

```DAX
Sell Out MTD = 
VAR _diaMes = [DiasTranscurridos]
VAR _mesActual = [MesMTDCalculos]
VAR _resultado = 
    CALCULATE(
        [Total Sell Out],
        DAY(Facturas[Fecha]) <= _diaMes,
        MONTH(Facturas[Fecha]) = _mesActual
    )
RETURN 
    _resultado

Sell Out MTD LY =
VAR _maxFechaContext = MIN( MAX( DimFechas[Fecha] ), TODAY() )
VAR _dia = DAY( _maxFechaContext )
VAR _mes = MONTH( _maxFechaContext )
VAR _anioAnterior = YEAR( _maxFechaContext ) - 1

VAR _startLY = DATE( _anioAnterior, _mes, 1 )
VAR _endLY = DATE( _anioAnterior, _mes, _dia )

RETURN
CALCULATE(
    [Total Sell Out],
    FILTER(
        ALL( DimFechas ),
        DimFechas[Fecha] >= _startLY &&
        DimFechas[Fecha] <= _endLY
    )
)

Crecimiento Sell Out MTD = 
[Sell Out MTD] - [Sell Out MTD LY]

% Crecimiento Sell Out MTD = 
DIVIDE(
    [Sell Out MTD] - [Sell Out MTD LY],
    [Sell Out MTD LY]
)

MesMTDCalculos = VALUE(MONTH([MaxFechaCalculos]))


