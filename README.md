# Projeto Modulo 4

## Desafio 1. 
Diagrama dimensional – star schema – com base no diagrama relacional disponibilizado.
Foco: Professor – objeto de análise.

## Desafio 2. 
Modelagem Star Schema utilizando como base a tabela Financial Sample. 

Tabelas criadas
Fato_Vendas
Dim_Descontos
Dim_Produtos 
Dim_Detalhes_Vendas
Dim_Detalhes_Produtos 
Dim_Data

### Relacionamentos 
No esquema estrela, a tabela fato Fato_Vendas estará no centro, conectada a cada uma das tabelas de dimensão.

A tabela Dim_Data foi criada utilizado as funções DAX: 

Dim_Data = 
VAR dataMinVta = MIN(Fato_Vendas[Date])
VAR YeardataMin = YEAR(dataMinVta)
VAR dataMaxVta = MAX(Fato_Vendas[Date])
VAR YeardataMax = YEAR(dataMaxVta)
VAR Tabela = 
FILTER(
    CALENDARAUTO(),
    YEAR([Date]) >= YeardataMin &&
    YEAR([Date]) <= YeardataMax
)
VAR Calendario =
    SELECTCOLUMNS (
        Tabela,
        "Data",[Date],
        "Ano", YEAR ( [Date] ),
        "Mes", FORMAT ( [Date],"MMMM"),
        "Periodo", INT(FORMAT([Date], "YYYYMM")),
        "Mes-Ano", COMBINEVALUES("-", FORMAT ( [Date],"MMM"), YEAR ( [Date] )),   
        "MesNro",INT ( FORMAT ( [Date],"M" ) ),
        "NroDia",INT ( FORMAT ( [Date],"d" ) ),
        "Trimestre", "T" & ROUNDUP ( MONTH ( [Date] ) / 3,0 ),
        "NroTrimestre",ROUNDUP ( MONTH ( [Date] ) / 3,0 ),
        "DiaSemana",WEEKDAY ( [Date],2 ),
        "Semana", WEEKNUM ( [Date],2),
        "Nome Dia", FORMAT ( [Date],"DDDD"),
        "Ano-Trimestre", COMBINEVALUES ("-",YEAR ( [Date] ), "T" & ROUNDUP ( MONTH ( [Date] ) / 3,0 )),
        "MesCurto", FORMAT ( [Date],"MMM" ),
        "Dia",DAY([Date]),
        "Mes Atual",MONTH ( TODAY () ),
        "Trimestre Atual",ROUNDUP ( MONTH ( TODAY () ) / 3,0 ),
        "Semestre", ROUNDUP ( INT ( FORMAT ( [Date],"M" ) ) * 2 / 12,0 ),
        "Ano-Mes",COMBINEVALUES ( "-",YEAR ( [Date] ),FORMAT ( [Date],"MM" ) ),
        "ID_Data", YEAR ( [Date] ) * 10000 + MONTH ( [Date] ) * 100 + DAY ( [Date] )
    )
RETURN Calendario
