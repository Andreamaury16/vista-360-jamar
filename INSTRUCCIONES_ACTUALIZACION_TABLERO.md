# Reglas permanentes · "Actualizar tablero"
**Vigencia:** desde 14 jul 2026 · Definidas por Andrea (CMO Jamar)

## 1. Trigger
Cuando Andrea diga **"Actualizar tablero"** (o "actualizamos", "actualiza"), ejecutar TODO este proceso sin omisiones.

## 2. Fuentes obligatorias a consultar (Tableau · vía MCP directo, NO screenshots)

| Fuente | View ID | Uso |
|---|---|---|
| Funnel Cliente Nuevo-Conocido — Tipo Cliente | `718acc68-42df-4f58-b300-d9fedf93c903` | VISOR, COTIZ, ORDEN, FACT + YoY |
| Funnel — TRÁFICO (Visor x Agencia) | `676d8675-49ab-4139-9c76-65838b67db5d` | VISOR POR AGENCIA (30 agencias) |
| Funnel — FACTURACIÓN (por tipo venta) | `e83bd0e7-94a8-42d7-83b4-de670758a09e` | CONTADO, CRÉDITO, LÍNEAS |
| Cliente 360 v1 | `d58354dc-e54a-441c-af8a-8e504f218d93` | ESTRATO, EDAD, LÍNEA |
| Tablero de Ventas NonBank | `34762000-ee23-472e-aa59-84fff8a349cc` | VENTAS Y CUMPLIMIENTO |
| Tráfico ecommerce v2 — TRÁFICO DIGITAL | `4d4907e9-4b04-40eb-b5b3-d4357c3db7e1` | EMAIL, SMS, PRESENCIAL |
| Puntos de Contacto NP — Tracking Leads | `7d473866-1ad2-48f0-b95c-2007b9789319` | CNP (Base de Datos: relacional conocido/contactable/otros) |
| Visibilidad Relacional (index.html mkt) | — | MEDIOS (SMS/Email/WhatsApp/Robocall/Pauta) |

Filtro global: `Fecha = 14/07/2026` (o la fecha actual — corte del día).

## 3. Reintentos y reporte de errores
1. Si una consulta falla, reintentar 2 veces antes de continuar.
2. Si después de reintentos sigue fallando, reportar:
   - Nombre del informe/fuente
   - Error exacto
   - Qué información no pudo actualizarse
   - Acciones intentadas
3. **Nunca omitir sin informar.** Toda excepción se reporta.

## 4. Protección de meses cerrados (histórico congelado)
- Los meses cerrados (ene, feb, mar, abr, may, jun 2026) son **HISTÓRICOS INMUTABLES**.
- **NO modificar** `FUNNEL_ALL_MONTHS[1..6]`, `VENTAS_ALL_MONTHS[6]`, `CNP_ALL_MONTHS[6]`, `AGENCIAS_BY_MONTH[6]`, `ESTRATOS_BY_MONTH[6]`, `EDADES_BY_MONTH[6]`, `LINEAS_BY_MONTH[6]` bajo NINGUNA circunstancia.
- Solo actualizar el **mes en curso** (actualmente julio → índice 7).
- Al pasar a un nuevo mes (ej. agosto), **congelar julio como cierre definitivo** y comenzar a actualizar el índice 8.
- Si detecto que una actualización podría cambiar un mes cerrado, DETENER y notificar a Andrea antes de proceder.

## 5. Conservación del tablero (diseño intacto)
Prohibido en cada actualización:
- ❌ Modificar diseño/estilos CSS
- ❌ Cambiar estructura de tablas
- ❌ Renombrar campos
- ❌ Eliminar columnas
- ❌ Modificar cálculos o fórmulas
- ❌ Alterar relaciones entre tablas
- ❌ Cambiar filtros o lógica de negocio

**Solo refrescar los DATOS** en las estructuras JS existentes (`_ALL_MONTHS[7]`, `_BY_MONTH[7]`, etc).

## 6. Validación pre-finalización
Antes de terminar:
- [ ] Todas las fuentes fueron consultadas
- [ ] Todas las tablas y visualizaciones fueron actualizadas
- [ ] Meses históricos permanecen idénticos (verificar con diff estructural)
- [ ] Si algún informe de Tableau cambió su estructura (nuevo campo, columna renombrada, filtro nuevo), **notificar a Andrea antes de aplicar cambios**
- [ ] `node --check` del JS pasa sin errores
- [ ] JSDOM verifica que renderAll() puebla todas las tablas

## 7. Resumen final obligatorio
Al terminar toda actualización, entregar:

```
Estado general: ✅ Actualización completa / ⚠️ Actualización parcial
Fuentes consultadas: X / Y
Fuentes actualizadas: X / Y
Tablas y visualizaciones actualizadas: X
Meses históricos: ✅ intactos / ⚠️ modificados en [...]
Fuentes con errores: [lista o "ninguna"]
Recomendaciones/acciones: [solo si aplica]
```

Una actualización solo se considera **finalizada** cuando:
1. Todas las fuentes disponibles fueron consultadas
2. Todas las tablas fueron verificadas
3. Se confirmó que ningún mes histórico fue alterado
4. Se entregó el resumen final a Andrea
