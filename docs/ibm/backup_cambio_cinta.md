# Guía de Backup y Cambio de Cinta IBM

## 1. Identificación y Preparación

### Localizar la Cinta Más Antigua
1. Revisar las cintas disponibles en el estante/almacén
2. Verificar las etiquetas con las fechas de los backups
3. Seleccionar la cinta con la **fecha más antigua**

---

## 2. Cambio Físico de Cinta

### Extracción de Cinta Actual
1. Acceder físicamente al servidor IBM
2. Abrir la unidad de cinta (TAP01)
3. Extraer la cinta actual con cuidado
4. **Etiquetar la cinta** con la información del backup:
   - Fecha del backup
   - Tipo de backup
   - Cualquier observación relevante

### Inserción de Nueva Cinta
1. Tomar la cinta más antigua identificada
2. Insertar correctamente en la unidad TAP01
3. Asegurar que la cinta esté bien colocada
4. Cerrar la unidad de cinta

---

## 3. Inicialización de la Cinta

### Comando de Limpieza e Inicialización

Ejecutar el siguiente comando en el IBM i (AS/400):

```
INZTAP DEV(TAP01) NEWVOL(SIRENA) NEWOWNID(LASIRENA) CHECK(*NO) DENSITY(*DEVTYPE) CLEAR(*NO)
```

### Descripción de Parámetros

| Parámetro | Valor | Descripción |
|-----------|-------|-------------|
| `DEV` | `TAP01` | Dispositivo de cinta a inicializar |
| `NEWVOL` | `SIRENA` | Nuevo nombre de volumen para la cinta |
| `NEWOWNID` | `LASIRENA` | Identificador del propietario |
| `CHECK` | `*NO` | No verificar errores durante inicialización |
| `DENSITY` | `*DEVTYPE` | Usar densidad predeterminada del dispositivo |
| `CLEAR` | `*NO` | No borrar completamente la cinta |

> ⚠️ **Importante:** Esperar a que el comando complete exitosamente antes de continuar.

---

## 4. Ejecución del Backup

### Acceso al Menú de Salvaguarda

```
GO MENUSALV
```

### Programación del Backup

> 🌙 **Horario recomendado:** Siempre ejecutar el backup durante la noche
> 
> **Motivo:** Menor actividad del sistema, mayor velocidad de backup y menor impacto en usuarios

### Pasos para Ejecutar el Backup

1. Ejecutar comando `GO MENUSALV`
2. Seleccionar **Opción 3** (Trabajo con trabajos de salvaguarda)
3. Luego seleccionar **Opción 1** (Visualizar trabajos de salvaguarda activos)
   - Esto permite verificar si hay un backup corriendo actualmente
   - Si hay un backup en ejecución, aparecerá listado aquí
4. Esperar confirmación de finalización exitosa

---

## 6. Rotación de Cintas

### Esquema de Rotación Recomendado

```
┌─────────────┐
│  Lunes      │ → Backup
├─────────────┤
│  Martes     │ → Cambio de Cinta
├─────────────┤
│  Miércoles  │ → Backup
├─────────────┤
│  Jueves     │ → Backup
├─────────────┤
│  Viernes    │ → Backup
├─────────────┤
│  Sábado     │ → Backup
├─────────────┤
│  Domingo    │ → Backup
└─────────────┘
```

> 💡 **Tip:** Recordar que el cambio de cinta es semanal

---

## 7. Mantenimiento de Cintas

### Cuidados Básicos

- **Manipulación:**
  - No tocar la cinta magnética directamente
  - Evitar caídas o golpes
  - Mantener en sus estuches cuando no se usen

- **Limpieza de la unidad:**
  - Usar cinta de limpieza cada 50 backups aproximadamente
  - O según lo indique el fabricante

### Vida Útil de las Cintas

- **Uso normal:** 2-5 años
- **Verificar:** Errores frecuentes pueden indicar desgaste
- **Reemplazar:** Si aparecen errores de lectura/escritura recurrentes

---

## 8. Solución de Problemas

### Errores Comunes

| Error | Posible Causa | Solución |
|-------|---------------|----------|
| Cinta no detectada | Mal insertada | Reinsertar correctamente |
| Error de inicialización | Cinta dañada | Usar otra cinta |
| Backup lento | Cinta desgastada | Considerar reemplazo |
| Error de escritura | Unidad sucia | Ejecutar limpieza |

### Comandos Útiles

**Verificar si hay backup corriendo:**
```
GO MENUSALV
Opción 3: Trabajo con trabajos de salvaguarda
Opción 1: Visualizar trabajos activos
```

**Verificar estado de la unidad:**
```
WRKDEVD DEVD(TAP01)
```

**Ver trabajos de backup activos:**
```
WRKACTJOB SBS(QSYSWRK)
```

**Consultar historial de mensajes:**
```
DSPMSG QSYSOPR
```

---