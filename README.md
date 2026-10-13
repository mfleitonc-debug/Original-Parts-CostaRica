# Original Parts Costa Rica — prueba en Render

## Qué cambia
Se admite PostgreSQL con `DATABASE_URL` y SQLite solo en desarrollo local sin esa variable. **No se migran automáticamente los datos del SQLite anterior**. La prueba en Render empieza con base vacía. No cargar datos reales.

## Publicación de prueba
1. Crear repositorio **privado** en GitHub con los archivos de esta carpeta. No subir `inventario.db`, `.env` ni claves.
2. Crear una base PostgreSQL gratuita de prueba en Render, si está disponible. Copiar su **Internal Database URL**. Las bases gratuitas pueden caducar y no son adecuadas para datos reales.
3. Render > New > Blueprint, conectar repositorio y aplicar `render.yaml`. En `DATABASE_URL` pegar la URL interna de la base; `SECRET_KEY` se genera automáticamente. Revisar que la base y el web service estén en la misma región.
4. Tras el despliegue, abrir Shell de Render si el plan lo permite y ejecutar `flask --app app crear-usuario administrador --rol admin`. Si el plan gratuito no ofrece Shell, ejecutar **desde una terminal local**, con `DATABASE_URL` configurada temporalmente con la **External Database URL**, `python -m flask --app app crear-usuario administrador --rol admin`; no guardar ni publicar la URL. Si la URL externa no funciona, usar una tarea temporal privada con el mismo entorno.
5. Abrir la URL HTTPS que muestra Render y probar inventario, clientes, carrito, comprobante y reportes con datos ficticios.

## Local
`python -m pip install -r requirements.txt`; `python -m flask --app app crear-usuario administrador --rol admin`; `python app.py`.

## Precauciones
- La app usa una clave de sesión estable en Render; contraseñas hasheadas, roles y CSRF.
- Aún requiere limitación de intentos de login, políticas de privacidad, copias de seguridad y restauración probada antes de usar datos reales.
- Los comprobantes no son facturas electrónicas.
- La base gratuita de Render puede caducar: no almacenar datos importantes.
