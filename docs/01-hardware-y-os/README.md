# 🖥️ Hardware y Sistema Operativo

En esta sección se documenta el hardware utilizado para el servidor y los detalles de la instalación del sistema operativo base.

## Hardware
- **CPU:** AMD Ryzen 5 2600 Six-Core Processor (3.4GHz)
- **RAM:** 32GB
- **GPU:** NVIDIA GeForce GTX 1060 3GB VRAM
- **Almacenamiento (Activo):** Disco SSD de 240GB. *(En este primer periodo de pruebas, Ubuntu Server comparte el disco con Windows asignándole solo 60GB, para evitar romper instalaciones no respaldadas. Próximamente se evaluará pasar el 100% del SSD a Ubuntu).*
- **Almacenamiento (Secundario):** 2 Discos Duros HDD (300GB y 1TB). *(Aún no formateados para Linux. Tras respaldar su contenido actual, se utilizarán para almacenamiento masivo y backups, ej. un servidor multimedia de películas).*

## Sistema Operativo
Se optó por instalar **Ubuntu Server (edición sin interfaz gráfica / CLI)**.

### ¿Por qué sin interfaz gráfica (CLI)?
- **Ahorro de recursos:** Una interfaz gráfica (GUI) consume RAM y CPU innecesariamente (fácilmente 1GB de RAM o más solo para dibujar ventanas). En un servidor, queremos que todos los recursos se dediquen a las aplicaciones.
- **Mejor aprendizaje:** Me obliga a perderle el miedo a la terminal y a aprender cómo funcionan las cosas por debajo, que es el estándar en entornos profesionales de servidores y nube.
- **Seguridad:** Menos paquetes instalados significan menos posibles vulnerabilidades.

### Los primeros pasos que realicé (Configuración Inicial)
Una vez instalado el SO base, guiado fuertemente con la ayuda de IA para acelerar la curva de aprendizaje, los primeros pasos fueron:
1. Asegurar que el sistema estuviera actualizado (`sudo apt update && sudo apt upgrade`).
2. Configurar una IP estática en la red local (para que el servidor siempre tenga la misma dirección).
3. Configurar el acceso remoto mediante SSH.
4. Configurar **Tailscale** para poder acceder por SSH desde fuera de mi red local.

*(Nota sobre el entorno: Aunque el servidor físico es la PC mencionada arriba, todo el trabajo de administración se realiza mediante SSH desde mi notebook personal, simulando de manera realista la administración de un VPS o servidor en la nube).*

## 🛠️ Comandos Útiles de Administración Básica
Durante los primeros días administrando un servidor por línea de comandos, estos son los comandos más esenciales de supervivencia:

- **Conectarse al servidor (desde otro PC en la misma red WiFi):** `ssh ezekim@192.168.1.15`
- **Conectarse al servidor (desde CUALQUIER lugar del mundo vía Tailscale):** `ssh ezekim@100.66.226.31`
- **Apagar el servidor de forma segura (cierra los contenedores de Docker correctamente):** `sudo shutdown now` (o `sudo poweroff`)
- **Reiniciar el servidor:** `sudo reboot` (o `sudo shutdown -r now`)
- **Actualizar lista de repositorios:** `sudo apt update`
- **Instalar actualizaciones:** `sudo apt upgrade`
- **Limpiar paquetes obsoletos:** `sudo apt autoremove`
