# 🌐 Redes y Acceso Remoto

En esta sección documentamos la configuración de red del servidor, cómo asignar una IP estática y cómo acceder de forma remota a través de SSH.

## 1. Configuración de IP Estática (Netplan)

Para que los servicios (como un servidor web o un DNS local) funcionen correctamente, el servidor debe tener siempre la misma dirección IP dentro de la red local.

- **IP Asignada (Fija):** `192.168.1.15`
- **Puerta de enlace (Router):** `192.168.1.1` (Asumido por defecto)
- **Interfaz de red:** `enp5s0`

### Pasos para fijar la IP en Ubuntu Server
Ubuntu Server usa **Netplan** para gestionar la red. Los archivos de configuración están en `/etc/netplan/`.

1. **Identificar la interfaz y el archivo actual:**
   - Para ver la interfaz se usa `ip a` (en nuestro caso es `enp5s0`).
   - Listar los archivos de Netplan: `ls /etc/netplan/` (suele llamarse `50-cloud-init.yaml`, `01-netcfg.yaml` o `00-installer-config.yaml`).

2. **Editar el archivo de configuración:**
   ```bash
   sudo nano /etc/netplan/TU_ARCHIVO.yaml
   ```

3. **Estructura del archivo (Ejemplo):**
   *(Importante: En YAML la indentación con espacios es obligatoria, no usar Tabulador).*
   ```yaml
   network:
     ethernets:
       enp5s0:
         dhcp4: false
          addresses:
            - 192.168.1.15/24
         routes:
           - to: default
             via: 192.168.1.1
         nameservers:
           addresses: [8.8.8.8, 1.1.1.1]
     version: 2
   ```

4. **Aplicar los cambios:**
   ```bash
   sudo netplan apply
   ```
   *(Nota: Si cambias la IP mientras estás conectado por SSH, la sesión actual podría caerse y deberás reconectar con la nueva IP).*

---

## 2. Acceso Remoto (SSH)

SSH (Secure Shell) permite controlar el servidor desde tu computadora principal usando la terminal, sin necesidad de monitor o teclado físico en el servidor.

- **Servicio:** OpenSSH Server
- **Puerto:** `22`

### Instalación y Verificación
Si no vino preinstalado, se instala y verifica de la siguiente manera:
```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh   # Asegura que SSH inicie automáticamente al prender el PC
sudo systemctl start ssh    # Inicia el servicio ahora mismo
sudo systemctl status ssh   # Verifica que esté corriendo (Letra 'q' para salir)
```

### Conexión desde la PC Principal
Para conectarnos desde nuestra computadora principal al servidor, usamos la terminal (PowerShell o CMD en Windows) con el siguiente comando:
```bash
```bash
ssh ezekim@192.168.1.15
```

## 3. Acceso Remoto Global (Tailscale)
Para acceder al servidor desde fuera de nuestra red WiFi local (ej. desde otra ciudad) sin necesidad de abrir puertos inseguros en el router, configuramos **Tailscale** (una VPN Zero Configuration).

### Instalación en el Servidor
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```
*(Se inicia sesión con la cuenta vinculada para agregar el servidor a la red privada virtual).*

### Conexión desde el Notebook Personal
Se debe instalar Tailscale en el equipo cliente (notebook) e iniciar sesión con la misma cuenta. Una vez hecho esto, Tailscale asignará una IP estática inamovible (ej. `100.x.x.x`) al servidor.
Nos conectamos desde cualquier parte del mundo con:
```bash
ssh ezekim@100.66.226.31
```
