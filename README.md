
#Unión de las tablas 2023 y 2024
# Para la producción 
Extracción = UNION (
        SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Producción relaves frescos (t)", Cons2023[Toneladas producidas frescos (t)],"Producción en tranque cauquenes (t)",Cons2023[Toneladas producidas cauquenes (t)],"Producción Molibdeno (lb)",Cons2023[Molybdenum production (lbs)]),
        SELECTCOLUMNS(Cons2024, "Date", Cons2024[Date], "Producción relaves frescos (t)", Cons2024[Toneladas producidas frescos (t)],"Producción en tranque cauquenes (t)",Cons2024[Toneladas producidas cauquenes (t)],"Producción Molibdeno (lb)",Cons2024[Molybdenum production (lbs)])
    )
# Creación tabla de costos 
Costos = 
    UNION (
        SELECTCOLUMNS(FR2023, "Date", FR2023[Date], "Costo Energía (USD)", FR2023[Power],"Costo bolas (USD)",FR2023[Grinding Balls],"Costos de Cal (USD)",FR2023[Lime],"Costos filtrado y secado (USD)",FR2023[Filtration and drying process],"Costos RAI (USD)",FR2023[Industrial Water (RAI)]),
        SELECTCOLUMNS(FR2024, "Date", FR2024[date], "Costo Energía (USD)", FR2024[Power],"Costo bolas (USD)",FR2024[Grinding Balls],"Costos de Cal (USD)",FR2024[Lime],"Costos filtrado y secado (USD)",FR2024[Filtration and drying process],"Costos RAI (USD)",FR2024[Industrial Water])
    ) 
# Creación tabla de energía
Energía =
        ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Consumo Energía (kWh)", Cons2023[Power consumed  (Kw/month)]),
            SELECTCOLUMNS(Cons2024, "Date", Cons2024[Date], "Consumo Energía (kWh)", Cons2024[Power consumed  (Kw/month)])
        ),
        "Toneladas Producidas", 
         LOOKUPVALUE('Extracción'[Total Producción (t)],       'Extracción'[Date], [Date]),
        "Costos Energía (USD)", 
       LOOKUPVALUE('Costos'[Costo Energía (USD)],Costos[Date], [Date])
        )
# Creación tabla de Molienda
Molienda = 
    ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Consumo bolas (t)", Cons2023[Grinding balls consumed (tonnes/month)], "Días operativos relaves frescos", Cons2023[Operating days - fresh tailings]),
            SELECTCOLUMNS(Cons2024, "DATE", Cons2024[Date], "Consumo bolas (t)", Cons2024[Grinding balls consumed (tonnes/month)], "Días operativos relaves frescos",Cons2024[Operating days - fresh tailings])
        ),
        "Toneladas Producidas", 
        LOOKUPVALUE('Extracción'[Total Producción (t)], 'Extracción'[Date], [Date]),
        "Costos Bolas (USD)", 
        LOOKUPVALUE('Costos'[Costo bolas (USD)],Costos[Date], [Date])
    )
# Creación tabla de Cal
CAL = 
    ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Consumo CAL (t)", Cons2023[Lime consumed (tonnes/month)], "Días operativos cauquenes", Cons2023[Operating days - Cauquenes tailings]),
            SELECTCOLUMNS(Cons2024, "DATE", Cons2024[Date], "Consumo CAL (t)", Cons2024[Lime consumed (tonnes/month)], "Días operativos cauquenes",Cons2024[Operating days - Cauquenes tailings])
        ),
        "Toneladas Producidas Cauquenes", 
        LOOKUPVALUE('Extracción'[Producción en tranque cauquenes (t)], 'Extracción'[Date], [Date]),
        "Costos CAL (USD)", 
        LOOKUPVALUE('Costos'[Costos de Cal (USD)],Costos[Date], [Date])
    )
# Creación tabla de Flotación
Flotación = 
    ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(
                Cons2023, 
                "Date", Cons2023[Date], 
                "Consumo Colector (T)", Cons2023[Reagents - collector consumed (tonnes/month)],
                "Costo unitario de colector (USD/t)", Cons2023[Reagents - collector cost (US$/tonne)],
                "Consumo Espumante (T)", Cons2023[Reagents - frother consumed (tonnes/month)],
                "Costo unitario de espumante (USD/t)", Cons2023[Reagents - frother cost (US$/tonne)],
                "Consumo Tiofos (T)", Cons2023[Tiofos|reagents  consumed (tonne/month)],
                "Costo unitario Tiofos (USD/t)", Cons2023[Tiofos| reagents cost (US$/tonne)],
                "Días operativos relaves frescos", Cons2023[Operating days - fresh tailings],
                "Producción Molibdeno (lb)",Cons2023[Molybdenum production (lbs)]
            ),
            SELECTCOLUMNS(
                Cons2024, 
                "Date", Cons2024[Date], 
                "Consumo Colector (T)", Cons2024[Reagents - collector consumed (tonnes/month)],
                "Costo unitario de colector (USD/t)", Cons2024[Reagents - collector cost (US$/tonne)],
                "Consumo Espumante (T)", Cons2024[Reagents - frother consumed (tonnes/month)],
                "Costo unitario de espumante (USD/t)", Cons2024[Reagents - frother cost (US$/tonne)],
                "Consumo Tiofos (T)", Cons2024[Tiofos|reagents  consumed (tonne/month)],
                "Costo unitario Tiofos (USD/t)", Cons2024[Tiofos| reagents cost (US$/tonne)],
                "Días operativos relaves frescos", Cons2024[Operating days - fresh tailings],
                "Producción Molibdeno (lb)",Cons2024[Molybdenum production (lbs)]
            )
        ),
        "Toneladas Producidas", 
        LOOKUPVALUE('Extracción'[Total Producción (t)], 'Extracción'[Date], [Date])
    )
# Creación tabla de Filtrado y secado
Filtrado/secado = 
    ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Consumo diesel (t)", Cons2023[Filtration and drying process - diesel consumed (tonnes/month)], "Costo unitario diesel (USD)",Cons2023[Filtration and drying process - diesel cost (US$/tonnes)], "Días operativos relaves frescos", Cons2023[Operating days - fresh tailings]),
            SELECTCOLUMNS(Cons2024, "DATE", Cons2024[Date], "Consumo diesel (t)", Cons2024[Filtration and drying process - diesel consumed (tonnes/month)], "Costo unitario diesel (USD)",Cons2024[Filtration and drying process - diesel cost (US$/tonnes)], "Días operativos relaves frescos", Cons2024[Operating days - fresh tailings])
        ),
        "Toneladas Producidas", 
        LOOKUPVALUE('Extracción'[Total Producción (t)], 'Extracción'[Date], [Date]),
        "Costos filtrado y secado (USD)", 
        LOOKUPVALUE('Costos'[Costos filtrado y secado (USD)],Costos[Date], [Date])
    )
# Creación tabla de RAI
RAI = 
    ADDCOLUMNS (
        UNION (
            SELECTCOLUMNS(Cons2023, "Date", Cons2023[Date], "Consumo floculantes (t)", Cons2023[Industrial water - floculant consumed (tonnes/month)], "Costo unitario de floctulante (USD/t)",Cons2023[Industrial water - floculants cost (US$/tonne)], "Días operativos relaves frescos", Cons2023[Operating days - fresh tailings]),
            SELECTCOLUMNS(Cons2024, "DATE", Cons2024[Date], "Consumo floculantes (t)", Cons2024[Industrial water - floculant consumed (tonnes/month)], "Costo unitario de floctulante (USD/t)",Cons2024[Industrial water - floculants cost (US$/tonne)], "Días operativos relaves frescos",Cons2024[Operating days - fresh tailings])
        ),
        "Toneladas Producidas", 
        LOOKUPVALUE('Extracción'[Total Producción (t)], 'Extracción'[Date], [Date]),
        "Costos RAI (USD)", 
        LOOKUPVALUE('Costos'[Costos RAI (USD)],Costos[Date], [Date])
    )





Extracción2 = 
UNION(
    SELECTCOLUMNS(Cons2023, 
        "Fecha", Cons2023[Date],
        "Tipo de Producción", "Frescos",
        "Toneladas Producidas", Cons2023[Toneladas producidas frescos (t)]
    ),
    SELECTCOLUMNS(Cons2023, 
        "Fecha", Cons2023[Date],
        "Tipo de Producción", "Cauquenes",
        "Toneladas Producidas", Cons2023[Toneladas producidas cauquenes (t)]
    ),
    SELECTCOLUMNS(Cons2024, 
        "Fecha", Cons2024[Date],
        "Tipo de Producción", "Frescos",
        "Toneladas Producidas", Cons2024[Toneladas producidas frescos (t)]
    ),
    SELECTCOLUMNS(Cons2024, 
        "Fecha", Cons2024[Date],
        "Tipo de Producción", "Cauquenes",
        "Toneladas Producidas", Cons2024[Toneladas producidas cauquenes (t)]
    )
)
