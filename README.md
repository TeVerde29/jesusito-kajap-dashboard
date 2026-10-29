# sistema-negocios

Base de datos centralizada y dashboard para dos negocios:

- **Jesusito**: ferretería y repuestos de moto (mostrador, compras e inventario).
- **KAJAP**: venta de agua purificada a clientes mayoristas, con bidones retornables.
- **Agente BCP**: operaciones del agente bancario y sus comisiones.

Los datos de ambos locales se integran en un modelo común, para analizar ventas, compras, gastos, planilla y operaciones en un solo lugar.

## Estructura

```
sistema-negocios/
├── db/
│   └── esquema_negocios_sqlserver.sql   # creación de tablas (SQL Server)
├── docs/
│   ├── diagrama_ordenado.png            # diagrama del modelo
│   └── diagrama.dot                     # fuente editable del diagrama
├── etl/                                 # (pendiente) carga y limpieza de Excel/Access
├── backend/                             # (pendiente) API
├── frontend/                            # (pendiente) dashboard
└── README.md
```

## Requisitos

- SQL Server 2016 o superior (o SSMS / Azure Data Studio para ejecutar scripts).
- Python 3.10+ (para el ETL, cuando esté listo).

## Crear la base de datos

1. Crea la base de datos en SQL Server (por ejemplo, `NegociosDB`).
2. Abre `db/esquema_negocios_sqlserver.sql`.
3. Reemplaza el `USE` por el nombre de tu base de datos y descomenta esa línea.
4. Ejecuta el script completo. Crea 17 tablas con sus claves, restricciones y columnas calculadas.

## Modelo de datos

| Área | Tablas |
|---|---|
| Catálogos | `local`, `medio_pago`, `estado_pago`, `tipo_operacion_bcp`, `concepto_gasto` |
| Maestros | `producto`, `cliente`, `proveedor`, `empleado`, `planilla_mensual` |
| Ventas | `venta_mostrador`, `despacho_agua`, `movimiento_bidon` |
| Agente BCP | `operacion_bcp` |
| Compras, inventario y gastos | `compra_proveedor`, `movimiento_inventario`, `gasto` |

Algunas decisiones de diseño:

- Los DNI y RUC se guardan como texto para conservar los ceros a la izquierda.
- Los totales (`total`, `total_pedido`) son columnas calculadas.
- El stock sale de sumar los movimientos de inventario, no de un número fijo.
- El saldo de bidones se calcula desde los préstamos y devoluciones.

## Hoja de ruta

- [x] Diseño del modelo de datos
- [x] Script de creación de tablas (SQL Server)
- [ ] ETL: carga de Excel y Access, con corrección de fechas, DNI y gastos mal separados
- [ ] Backend (API)
- [ ] Dashboard (frontend)

## Privacidad

Este repositorio manipula datos personales (DNI, sueldos, operaciones de terceros). No subas archivos con datos reales al repositorio. Los archivos de datos deben quedar fuera de Git (ver `.gitignore`).

## Licencia

Uso interno.
