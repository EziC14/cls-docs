# Gestionar reglas de correo en Exchange

Esta guía explica cómo editar y configurar reglas de Mail Flow en Microsoft Exchange para bloquear dominios de remitentes.

---

## 1. Acceder al Admin de Exchange

- Abre tu navegador y ve a:
  ```
  https://admin.exchange.microsoft.com
  ```
- Inicia sesión con tu cuenta de administrador.

## 2. Navegar a Mail Flow → Rules

- En el panel izquierdo, selecciona **Mail Flow**.
- Luego haz clic en **Rules**.
- Se mostrará la lista de reglas de correo configuradas.

## 3. Editar una regla existente

- Localiza la regla de advertencia ya creada en la lista.
- Haz clic en el nombre de la regla para abrirla y editarla.

## 4. Agregar un nuevo dominio a bloquear

- Dentro de la configuración de la regla, busca la sección de **Sender domain** (Dominio de remitente) o similar.
- Agrega el nuevo dominio que deseas bloquear a la lista existente.
- Asegúrate de seguir el formato correcto: `dominio.com`

## 5. Guardar los cambios

- Haz clic en **Save** o **Guardar**.
- La regla se actualizará y entrará en vigencia inmediatamente.

---

**Notas:**
- Los cambios en las reglas de Mail Flow se aplican a todos los correos entrantes después de guardar.
- Verifica que el dominio nuevo esté correctamente escrito para evitar bloqueos innecesarios.
- Si necesitas deshacer cambios, puedes editar la regla nuevamente.
