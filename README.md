# docs-infra (sistema de documentación interna)

Este repo contiene la documentación interna convertida en un sitio estático con **MkDocs**.

## Flujo rápido
1. Edita/añade archivos en `docs/` como Markdown.
2. Prueba localmente: `mkdocs serve` (ver instrucciones abajo).
3. Haz push a `main` → GitHub Actions compila y publica en `gh-pages`.

## Comandos útiles
- Crear entorno y dependencias:
  ```bash
  python -m venv .venv
  source .venv/bin/activate   # Linux / macOS
  .venv\Scripts\activate    # Windows PowerShell
  pip install -r requirements.txt
  ```
- Servir localmente:
  ```bash
  mkdocs serve -a 0.0.0.0:8000
  ```
- Construir estático:
  ```bash
  mkdocs build
  ```

## Notas
- Por seguridad, **NUNCA** ejecutar `UPDATE` o `DELETE` en producción sin respaldo y sin aprobación.
- Ajusta `mkdocs.yml` y `repo_name` al nombre real del repositorio en GitHub.