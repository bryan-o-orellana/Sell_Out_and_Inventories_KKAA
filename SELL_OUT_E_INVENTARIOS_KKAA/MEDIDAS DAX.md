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


