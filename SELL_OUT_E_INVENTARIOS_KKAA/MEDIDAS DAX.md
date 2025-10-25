# ⚙️ Power BI DAX Measures

This document contains key DAX measures used in the **Sell Out Report** for performance tracking and monthly comparisons.

---

## 📅 1️⃣ Sell Out MTD

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
MesMTDCalculos = 
VALUE( MONTH( [MaxFechaCalculos] ) )

>>------------------
🧩 Notes

These measures are designed for monthly comparisons (MTD) in Power BI.

They rely on supporting fields like DimFechas[Fecha], [MaxFechaCalculos], and [Total Sell Out].

Make sure the date table (DimFechas) is properly marked as a Date Table in Power BI for time intelligence to work correctly.

📘 Author: Bryan O. Orellana Chávez
📅 Last Updated: October 2025
