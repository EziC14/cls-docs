# Vinculación de guías (Speed)

**Objetivo:** centralizar los pasos comunes para vincular guías, corregir facturación errónea y cambios de OC.

> **Precaución:** Ejecutar queries de `UPDATE` o `DELETE` sólo después de un respaldo y con autorización.

## 1) Visualizar pedidos y productos
```sql
SELECT * FROM SPEED400CS.TPEDD WHERE PDPVTA = X AND PDNUME = XXXXX;
SELECT * FROM SPEED400CS.TPEDH WHERE PHPVTA = X AND PHNUME = XXXXX;
```

## 2) Vinculación de guías (pasos)
1. Ver detalle del pedido:
   ```sql
   SELECT * FROM SPEED400CS.TPEDD WHERE PDPVTA = X AND PDNUME = XXXXX;
   ```
2. Actualizar el detalle con la guía y usuario que realizó la acción:
   ```sql
   UPDATE SPEED400CS.TPEDD
   SET PDGUIA = XXXX, PDFECG = XXXXX, PDHORG = XXXX, PDUSAG = 'USER', PDREF2 = 'XXX'
   WHERE PDPVTA = X AND PDNUME = XXXXX;
   ```
3. Ver la cabecera:
   ```sql
   SELECT * FROM SPEED400CS.TPEDH WHERE PHPVTA = X AND PHNUME = XXXXX;
   ```
4. Asegurarse que `PHSITU = '04'` para permitir volver a facturar:
   ```sql
   UPDATE SPEED400CS.TPEDH SET PHSITU = '04' WHERE PHPVTA = X AND PHNUME = XXXXX;
   ```

## 3) Facturación errónea (limpieza)
- Ver registros relacionados:
  ```sql
  SELECT * FROM SPEED400CS.TREGV WHERE RVNDOC = '0090057960';
  SELECT * FROM SPEED400CS.TPEDD WHERE PDFABO = '57960';
  SELECT * FROM SPEED400CS.TCORU WHERE CURTIP = 'FC' AND CURSER = 9 AND CURNRO = 57960;
  ```
- Eliminar el registro duplicado/erróneo en `TCORU`:
  ```sql
  DELETE FROM SPEED400CS.TCORU WHERE CURTIP = 'FC' AND CURSER = 9 AND CURNRO = 57960;
  ```

## 4) Cambio de Nro de OC
1. Ver la cabecera:
   ```sql
   SELECT * FROM FERRDAT.TGENCOTH WHERE NROREP = 'RXXXXXXXX';
   SELECT * FROM FERRDAT.TGENCOTH WHERE SERCOT = X AND NROCOT = XXXXX;
   ```
2. Actualizar NRO de OC:
   ```sql
   UPDATE FERRDAT.TGENCOTH SET NROOCC = 'XXXXXXXXX' WHERE SERCOT = X AND NROCOT = XXXXX;
   ```
3. Reprocesar la OC en el servidor remoto: conectarse con `mstsc /admin` al .22 y ejecutar el job `REPRO OC` en el Programador de Tareas.

## 5) Buenas prácticas al ejecutar cambios
- Siempre **exportar/respaldar** las tablas afectadas (ej. SELECT ... INTO backup_table) o volcar a CSV antes de UPDATE/DELETE.
- Documentar en el PR/comentario quién ejecutó el cambio y por qué.
- Preferir cambios programados (ventana de mantenimiento) para operaciones que afectan contabilidad/ventas.