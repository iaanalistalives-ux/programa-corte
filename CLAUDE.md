# CLAUDE.md — Programa de Corte PCP (programa-corte)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`index.html` — "Programa de Corte — PCP" (Órdenes · Avance de ruta · Tizado · Medidas): tablero de programación diaria con edición en línea y subida del listado de corte.
- Muestra solo las órdenes de corte con PIEZA = "CUERPO" y PRENDAS REALES = 0 (lo que falta cortar). Cada orden se identifica por O/C + partida + pieza + grupo.
- Filtros: búsqueda (grupo, OC, partida, color, taller), prioridad (URGENTE y 1 a 3), avance (pendiente, ruta, tizado, medidas), Servicio o Planta, módulo y fecha. KPIs: órdenes, O/C únicas, prendas, kg, % Servicios y % Planta, y % de Ruta, Tizado y Medidas.
  - Ruta OK = tiene fecha de asignación de molde; Tizado OK = tiene fecha de tizado; Medidas OK = tiene fecha de medidas de tendido o la ruta empieza con "CORTE".
- Tabla editable por orden: tipo (Servicio o Planta), fecha programada, módulo (1 a 3), taller de costura, servicio de corte, reposo, envío y observación. Regla 500 sin filtros.
- Paneles: Planta por módulo (minutos contra la capacidad de 1346.6 min por módulo y día), Servicios de corte, Reposo y envíos, y Programación por capacidad diaria (Gantt o por día, sin domingos).
- Reportes en ventana (PDF o PNG): Dashboard, Programa reposo, Programa envíos y Exportar vista. Excel LIVES con resumen y detalle de 24 columnas.
- **Subir listado del día** (`ocorte_Listado2.xlsx`, solo .xlsx; el lector propio requiere Chrome o Edge modernos): reemplaza el listado anterior. Usa la primera hoja con encabezados "GRUPO" y "O/C".

## Base de datos (Supabase lives-datos)
- `pcorte_leer` (asignaciones y listado), `pcorte_cargar` (reemplaza el listado) y `pcorte_guardar` (una asignación, 0,6 s después de cada cambio).
- Tablas: `pcorte_listado`, `pcorte_cargas`, `pcorte_asignaciones`, `pcorte_historial`.
- No usa Storage; no guarda nada en el navegador.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 1 persona (Analista de Planeamiento).

## Quién puede editar
- Lo decide la base: `pcorte_puede_editar_` (con la tabla `permisos`). Sin permiso queda en "Solo consulta": los controles de las filas se desactivan y no aparece la subida.
- A Sistemas: `pcorte_puede_editar_` es la pieza que reemplazan con su seguridad; el listado debería salir directo de sus sistemas.

## Pendientes vistos al escribir esto (21/10/2026)
- Dos cálculos de minutos distintos: la programación por capacidad usa prendas × minutos de programa, pero el panel Planta por módulo, el aviso de sobrecarga y la hoja de Excel usan solo la suma de minutos. El % de uso puede no coincidir entre vistas.
- Las listas de talleres de costura (21) y de servicios de corte (8) están escritas a mano en el código.
- Código sin uso: `generateReport()` y `dlCsv()`.
