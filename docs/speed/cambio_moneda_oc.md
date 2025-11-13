# Cambio de moneda en Orden de Compra (OC)

**Objetivo:** cambiar la moneda de una OC específica en Speed utilizando DFU en el AS400.

**Precondición:**
- Tener acceso al AS400 con permisos para usar `UPDDTA` (DFU - Data File Utility).
- Conocer el número de OC proporcionado por el usuario o solicitante.

**Riesgo:** Modificar la moneda de una OC ya procesada puede afectar cálculos contables. Ejecutar solo con autorización.

---

## Variables utilizadas

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `@nroOC` | Número de la Orden de Compra | `0240005225` |

**Códigos de moneda:**
- `0` = Soles (PEN)
- `1` = Dólares (USD)

---

## Paso 1: Consultar la OC en Speed

Antes de modificar, verificar la OC y su estado actual:

```sql
SELECT * FROM SPEED400CS.TOCOH WHERE TOCNRO = '@nroOC';
```

**Campos importantes:**
- `TOCNRO`: Número de OC
- `TOCMON`: Moneda de origen (0=Soles, 1=Dólares)
- `TOCSIT`: Situación de la OC
- `TOCFEC`: Fecha de emisión

**Ejemplo:**
```sql
SELECT * FROM SPEED400CS.TOCOH WHERE TOCNRO = '0240005225';
```

---

## Paso 2: Revisar detalle de la OC (opcional)

Para ver los ítems/productos asociados a la OC:

```sql
SELECT * FROM SPEED400CS.TOCOD WHERE TODNRO = '@nroOC';
```

**Ejemplo:**
```sql
SELECT * FROM SPEED400CS.TOCOD WHERE TODNRO = '0240005225';
-- Producto ejemplo: 210100125 - GRASERA RECTA NPT ACERO INOX 1/8" ALEMITE 1961-B
```

---

## Paso 3: Cambiar la moneda con DFU (pantalla negra AS400)

1. **Iniciar sesión en el AS400** con un cliente 5250 (Mocha TN5250, IBM Client Access, etc.).

2. **Ejecutar el comando DFU:**
   ```
   UPDDTA TOCOH
   ```
   - Presionar `Enter`.
   - El sistema mostrará la pantalla de actualización del archivo `TOCOH`.

3. **Buscar el registro de la OC:**
   - En el campo de búsqueda (normalmente `TOCNRO`), ingresar el número de OC: `@nroOC`.
   - Presionar `Enter` para ubicar el registro.

4. **Modificar el campo `Moneda de Origen` (`TOCMON`):**
   - Ubicar el campo etiquetado como **"Moneda de Origen"** o `TOCMON`.
   - Cambiar el valor:
     - `0` para **Soles (PEN)**
     - `1` para **Dólares (USD)**
   - Ejemplo: Si actualmente es `0` y necesitas cambiarlo a dólares, ingresa `1`.

5. **Confirmar los cambios:**
   - Presionar `Enter` dos veces para confirmar la actualización.
   - Al salir, el sistema solicitará validación.
   - Presionar `Y` (Yes) para confirmar y guardar los cambios.

6. **Salir de DFU:**
   - Presionar `F3` o `F12` para salir de la utilidad.

---

## Paso 4: Verificar el cambio

Ejecutar nuevamente la consulta para confirmar que la moneda se actualizó correctamente:

```sql
SELECT TOCNRO, TOCMON, TOCSIT, TOCFEC 
FROM SPEED400CS.TOCOH 
WHERE TOCNRO = '@nroOC';
```

**Validación esperada:**
- El campo `TOCMON` debe reflejar el nuevo valor (0 o 1).

---

## Paso 5: Consulta de tipo de cambio (opcional)

Si necesitas revisar el tipo de cambio vigente para una fecha específica:

```sql
SELECT * FROM SPEED400CS.TTICA WHERE MONFEC = 'YYYYMMDD';
```

**Ejemplo:**
```sql
SELECT * FROM SPEED400CS.TTICA WHERE MONFEC = '20250909';
```

**Campos relevantes:**
- `MONFEC`: Fecha del tipo de cambio (formato AAAAMMDD)
- `MONCOM`: Tipo de cambio de compra
- `MONVEN`: Tipo de cambio de venta

---

## Ejemplo completo: OC 0240005225

**Contexto:** Cambiar OC de soles a dólares para la compra de una grasera recta.

```sql
-- 1. Consultar OC
SELECT * FROM SPEED400CS.TOCOH WHERE TOCNRO = '0240005225';

-- 2. Consultar detalle (producto)
SELECT * FROM SPEED400CS.TOCOD WHERE TODNRO = '0240005225';
-- Producto: 210100125 - GRASERA RECTA NPT ACERO INOX 1/8" ALEMITE 1961-B

-- 3. Consultar tipo de cambio del día
SELECT * FROM SPEED400CS.TTICA WHERE MONFEC = '20250909';
```

**Acción en AS400:**
```
UPDDTA TOCOH
→ Buscar OC: 0240005225
→ Campo TOCMON: cambiar de 0 a 1
→ Enter, Enter
→ Validar con Y
```

**Verificación:**
```sql
SELECT TOCNRO, TOCMON FROM SPEED400CS.TOCOH WHERE TOCNRO = '0240005225';
-- TOCMON debe ser 1 (Dólares)
```

---

## Rollback (reversión)

Si el cambio fue incorrecto o necesitas revertir:

1. Repetir el proceso de DFU (`UPDDTA TOCOH`).
2. Cambiar `TOCMON` al valor original.
3. Confirmar con `Y`.

**Alternativa (si tienes backup):**
```sql
-- Restaurar desde respaldo si existe
-- (No aplica si no se generó backup previo)
```

---

## Buenas prácticas

- **Antes de cambiar:** Tomar screenshot o exportar el registro actual de `TOCOH` a CSV.
- **Documentar:** Anotar quién realizó el cambio, cuándo y por qué (número de ticket o solicitud).
- **Validar con finanzas:** Asegurarse que el cambio de moneda no afecte cierres contables en curso.
- **Horario recomendado:** Fuera de horas pico de facturación/compras.

---

## Comandos de referencia rápida

| Acción | Comando AS400 | SQL alternativo |
|--------|---------------|-----------------|
| Actualizar TOCOH con DFU | `UPDDTA TOCOH` | N/A (usar DFU) |
| Ver registro de OC | N/A | `SELECT * FROM SPEED400CS.TOCOH WHERE TOCNRO='...'` |
| Ver detalle de OC | N/A | `SELECT * FROM SPEED400CS.TOCOD WHERE TODNRO='...'` |
| Tipo de cambio | N/A | `SELECT * FROM SPEED400CS.TTICA WHERE MONFEC='...'` |

---

## Ver también

- [Vinculación de guías](vinculacion_guias.md) - Para procesos relacionados con facturación
- [Guía de comandos](../guia_comandos.md) - Comandos generales de IBM 5250
- [Speed - Índice](index.md) - Otros procesos de Speed
