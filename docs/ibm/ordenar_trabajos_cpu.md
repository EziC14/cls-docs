# Ordenar trabajos por uso de CPU en IBM i (AS/400)

Esta guía explica cómo visualizar, ordenar y gestionar los trabajos activos según su consumo de CPU usando el comando WRKACTJOB en IBM i (AS/400).

---

## 1. Ingresar al sistema
- Usuario: `QSECOFR` (o tu usuario con permisos de administración)

## 2. Ver trabajos activos
- Para ver todos los subsistemas:
  ```
  WRKACTJOB SBS(*ALL)
  ```
- Para un subsistema específico y ordenar por CPU:
  ```
  WRKACTJOB SBS(QUSRWRK) SEQ(*CPU)
  ```

## 3. Reiniciar estadísticas de uso
- Presiona `F10` (Restart statistics) para poner a cero los contadores de CPU y obtener datos frescos.

## 4. Ordenar por uso de CPU
- Presiona `F16` (Change sort order) o `Shift+F4`.
- Elige el criterio de ordenamiento: `CPU%` o `CPU time` (según versión del sistema).
- Los trabajos con mayor uso de CPU aparecerán en la parte superior.

## 5. Finalizar un trabajo (si es necesario)
- Escribe `4` (End job) en la columna de opción junto al trabajo/usuario que deseas finalizar.
- Presiona `Enter` para ejecutar la acción.

---

**Notas:**
- Utiliza estas acciones solo si tienes autorización y comprendes el impacto de finalizar trabajos activos.
- Reiniciar estadísticas es útil para monitorear picos recientes de consumo.
