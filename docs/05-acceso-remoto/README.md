# 🌍 Acceso Remoto (Cloudflare Tunnels)

Para poder acceder a los servicios de nuestro Homelab desde internet (ej. estando fuera de casa o usando datos móviles), hemos decidido utilizar **Cloudflare Tunnels**.

## Decisión Arquitectónica
- **Método:** Cloudflare Tunnels (Zero Trust).
- **Razonamiento:** Es la opción más segura ya que no requiere abrir puertos (Port Forwarding) en el router, protegiendo al servidor de escaneos automáticos de bots y vulnerabilidades directas.
- **Dominio:** En lugar de comprar un dominio nuevo inicialmente, utilizaremos un **subdominio** de un dominio corporativo existente (`empresa.cl`).
- **Implementación DNS:** El dominio raíz está registrado en NIC Chile y apunta a Cloudflare. Luego, Cloudflare apunta al hosting original (ej. HostGator) para mantener la página web oficial ya existente. La configuración de nuestro laboratorio (`lab.empresa.cl` o subdominios específicos) se realizará directamente en la "Zona DNS" de Cloudflare mediante un registro `CNAME` apuntando al túnel de Zero Trust.

*(Esta configuración garantiza que la infraestructura crítica de la empresa, como su sitio web principal y correos electrónicos, permanezca intacta y segura en su hosting comercial, mientras que el laboratorio opera de forma independiente bajo subdominios específicos).*

## Pasos de Configuración (Zero Trust)
1. En el panel de **Cloudflare Zero Trust**, nos dirigimos a **Redes -> Túneles**.
2. Creamos un nuevo túnel (tipo Cloudflared) y le asignamos un nombre (ej. `Homelab`).
3. Cloudflare generará un comando de instalación. Este comando instalará un pequeño agente o "conector" como servicio dentro de nuestro Ubuntu Server.

### Enrutamiento y Traefik (Mejores Prácticas)
Para mantener una arquitectura limpia y aprovechar las capacidades de proxy reverso de Coolify:
- En Cloudflare Zero Trust, **todas** las rutas (Public Hostnames) apuntan a `localhost:80` (HTTP). Incluso el panel de Coolify apunta al 80, ya que Traefik interno se encarga de recibir el tráfico y redirigirlo al contenedor correcto, sin importar qué puertos internos estén usando (solucionando así conflictos con puertos bloqueados como el 6xxx).
- Esto delega el enrutamiento interno a **Traefik**, el cual lee el subdominio y redirige el tráfico.
- **Subdominios Independientes:** Cada aplicación o servicio requiere su propio subdominio configurado tanto en Cloudflare como en Coolify. (Ejemplos: `control.empresa.cl` para Coolify, `estado.empresa.cl` para Uptime Kuma, y `mi-app.empresa.cl` para el portafolio).
- **Importante (Terminación SSL):** En la configuración de dominios dentro de Coolify, los dominios deben escribirse **sin HTTPS** (ej. `http://monitor.empresa.cl`). Esto previene el error `ERR_TOO_MANY_REDIRECTS`, ya que Cloudflare se encarga de la seguridad SSL (el candado verde) en el exterior, y la comunicación interna ocurre en HTTP plano seguro.
- Para que Traefik registre los cambios de dominio de cualquier aplicación, es obligatorio presionar el botón **Deploy** o **Restart** en Coolify.
