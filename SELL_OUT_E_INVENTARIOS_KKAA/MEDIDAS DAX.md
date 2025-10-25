//---------------------------------------------//
// ⚙️ POWER BI DAX MEASURES
// Sell Out & Inventory KKAA Report
//---------------------------------------------//

// 1️⃣ Sell Out MTD
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


//---------------------------------------------//
// 2️⃣ Sell Out MTD LY
//---------------------------------------------//
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


//---------------------------------------------//
// 3️⃣ Crecimiento Sell Out MTD
//---------------------------------------------//
Crecimiento Sell Out MTD = 
[Sell Out MTD] - [Sell Out MTD LY]


//---------------------------------------------//
// 4️⃣ % Crecimiento Sell Out MTD
//---------------------------------------------//
% Crecimiento Sell Out MTD = 
DIVIDE(
    [Sell Out MTD] - [Sell Out MTD LY],
    [Sell Out MTD LY]
)


//---------------------------------------------//
// 5️⃣ MesMTDCalculos
//---------------------------------------------//
MesMTDCalculos = 
VALUE( MONTH( [MaxFechaCalculos] ) )

//---------------------------------------------//
// 🧩 Notes
// - Measures designed for monthly comparisons (MTD)
// - Uses DimFechas[Fecha], [MaxFechaCalculos], [Total Sell Out]
// - Ensure DimFechas is marked as a Date Table
//---------------------------------------------//

// 📘 Author: Bryan O. Orellana Chávez
// 📅 Last Updated: October 2025



