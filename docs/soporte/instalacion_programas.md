# Guía de Instalación de Programas para PC/Laptop

## 1. Preparación Inicial

### Acceso al Servidor
- **Servidor:** `192.168.1.20`
- **Ruta de programas:** `Emergentes/PROGRAMAS`

### Credenciales
- Si no conoces las credenciales, accede a: `192.168.1.20:5000`
- Cambia la contraseña del usuario desde esta interfaz

---

## 2. Instalación de Java

1. Navegar a `\\192.168.1.20\Emergentes\PROGRAMAS`
2. Instalar el archivo de Java

---

## 3. Instalación de IBM Access Client Solutions (ACS)

### IBM v1r1 (4)
1. Buscar la carpeta: `IBM v1r1 (4)\Windows Application`
2. Ejecutar: `install-acs-64_allusers`
3. **Configuración:**
   - Aceptar todo durante la instalación
   - **IMPORTANTE:** Cuando aparezca la opción de SSL → seleccionar **"No"**
   - Para instalación de administrador → seleccionar **"Sí"** a todo

### IBM Windows Imagen 64
1. Instalar IBM Windows Imagen 64
2. Ejecutar el archivo `Setup` de la aplicación

---

## 4. Instalación de Módulos del Sistema

### Consulta Venta y Compra
1. Entrar a la carpeta `Consulta venta y compra`
2. Ejecutar `setup`
3. **Aparecerá un mensaje de error** → Es normal, hacer clic en **Omitir**

### Cotización
1. Entrar a: `Cotización\Aplicación`
2. Ejecutar `setup`
3. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**

### Lista de Precio
1. Entrar a la carpeta `Lista de precio`
2. Ejecutar `setup`
3. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**

### Módulo de Consignación
1. Entrar a: `Modulo de consignación\pqtModulo`
2. Ejecutar `setup`
3. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**
4. **Aparecerá un mensaje de error al final** → Es normal, hacer clic en **Omitir**

### Orden de Compra
1. Entrar a la carpeta `Orden de compra`
2. Ejecutar `setup`
3. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**

---

## 5. Instalación de SPEED

### SPEED 400
1. Entrar a la carpeta `Speed 400`
2. Ejecutar `setup`
3. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**

### SPEED Advance
1. Entrar a: `Speed Advance\Instaladores speed advance`
2. Instalar **DV** (para sistemas 32 bits)
3. Ejecutar `setup`
4. Cuando aparezca ventana de **"Conflicto de versiones"** → seleccionar **"Sí a todo"**

---

## 6. Configuración de ODBC Data Sources

### ODBC 32 bits - DNS de Sistema

1. Abrir el buscador de Windows y escribir: **ODBC Data Sources (32-bit)**
2. Ir a la pestaña **"DNS de Sistema"**
3. Click en **"Agregar"**
4. Seleccionar **"iSeries Access ODBC Driver"**
5. **Configuración:**
   - **Data source name:** `SPEED`
   - **System:** `192.168.1.5`
   - **Package:** Deseleccionar el check de **"Enable extended"**
   - Ir a la pestaña **"Server"**:
     - **Naming convention:** `System naming convention`
     - **Default SQL:** Dejar vacío
     - **Connection type:** `Use ODBC Access mode`
6. Click en **Aplicar** y **Aceptar**

### ODBC 64 bits - DNS de Usuario

#### Configuración CLS
1. Abrir el buscador de Windows y escribir: **ODBC Data Sources (64-bit)**
2. Ir a la pestaña **"DNS de Usuario"**
3. Click en **"Agregar"**
4. Seleccionar **"iSeries Access ODBC Driver"**
5. **Configuración:**
   - **Data source name:** `CLS`
   - **System:** `192.168.1.5`
   - **Package:** Deseleccionar el check de **"Enable extended"**
   - Ir a la pestaña **"Server"**:
     - **Naming convention:** `SQL naming convention`
     - **Default SQL:** `SPEED400CS`
     - **Connection type:** `Use ODBC Access mode`
6. Click en **Aplicar** y **Aceptar**

#### Configuración CLS_RP
1. En **"DNS de Usuario"** → Click en **"Agregar"**
2. Seleccionar **"iSeries Access ODBC Driver"**
3. **Configuración:**
   - **Data source name:** `CLS_RP`
   - **System:** `192.168.1.5`
   - **Package:** Deseleccionar el check de **"Enable extended"**
   - Ir a la pestaña **"Server"**:
     - **Naming convention:** `SQL naming convention`
     - **Default SQL:** `SPEED400CS`
     - **Connection type:** `Use ODBC Access mode call allowed`
4. Click en **Aplicar** y **Aceptar**

---

## 7. Instalación de Aplicaciones Específicas

### Crear Accesos Directos
1. **Preguntar al usuario** qué aplicaciones necesita
2. Acceder a: `\\192.168.1.20\Aplicaciones`
3. Buscar dentro de cada módulo o aplicación el archivo `.exe` principal
4. Click derecho → **"Enviar a"** → **"Escritorio (crear acceso directo)"**

### Aplicaciones SPEED
Las siguientes aplicaciones se encuentran en: `\\192.168.1.20\Aplicaciones\Speed Advance`
- **SPEEDADV**
- **SPEEDREP**

Crear accesos directos de ambas al escritorio.

---

### Aplicacion LOGISTI
Las siguientes aplicaciones se encuentran en: `\\192.168.1.20\Aplicaciones\Speed400`
- **LOGISTI7**

Crear acceso directo al escritorio.

---

## 8. Automatización FE (Facturación Electrónica)

### Instalación
1. Navegar a: `\\192.168.1.20\Emergentes\Programas\Automatización FE`
2. Ejecutar `setup` para instalar

### Solución de Problemas con Antivirus
- Si el antivirus bloquea la instalación:
  1. Desactivar temporalmente la **protección en tiempo real** del antivirus
  2. Completar la instalación
  3. Volver a activar el antivirus

---

## Notas Importantes

> ⚠️ **Mensajes de error esperados:**
> - Al instalar "Consulta venta y compra" → Omitir error
> - Al instalar "Módulo de consignación" → Omitir error al final

> ℹ️ **Conflictos de versiones:**
> - Siempre seleccionar **"Sí a todo"** cuando aparezca esta ventana

> 🔒 **Seguridad:**
> - Cambiar credenciales predeterminadas en `192.168.1.20:5000`
> - Mantener actualizado el antivirus

---
