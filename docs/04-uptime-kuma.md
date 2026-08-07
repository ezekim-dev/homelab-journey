# ⏱️ Uptime Kuma (Monitoreo)

Uptime Kuma es una herramienta de monitoreo autohospedada, similar a "Uptime Robot". La utilizamos como nuestro primer servicio para vigilar la salud del servidor y de los futuros contenedores que instalemos.

## Proceso de Instalación (Vía Coolify)

1. En el panel de Coolify, dentro de nuestro proyecto principal, seleccionamos **"Añadir nuevo recurso" -> "Service"**.
2. Buscamos **"Uptime Kuma"**.
3. Seleccionamos la versión base (Standalone / SQLite) ya que es ligera y perfecta para nuestras necesidades.
4. En la pantalla de configuración, cambiamos el **Service Name** generado aleatoriamente a un nombre más limpio, como `uptime-kuma`.
5. Hacemos clic en **"Save"** (Guardar) y luego en **"Deploy"** (Desplegar).
6. Una vez que el estado cambia a "Running", podemos acceder a la interfaz web de Uptime Kuma haciendo clic en el enlace generado automáticamente (se encuentra en la pestaña **"Links"** en la parte superior, o en el campo **Domains** dentro de la configuración General).
7. En la pantalla inicial de bienvenida de Uptime Kuma, seleccionamos **"SQLite"** como base de datos y continuamos para crear el usuario administrador. Esto mantiene el servicio lo más ligero posible.
