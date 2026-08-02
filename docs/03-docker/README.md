# 🐳 Docker y Orquestación (Coolify)

En lugar de instalar y gestionar Docker de forma manual, hemos decidido utilizar **Coolify**, una plataforma de alojamiento en la nube autogestionada (PaaS) de código abierto.

## ¿Qué hace Coolify por nosotros?
1. **Instalación de Docker:** Al instalar Coolify, automáticamente se configura el motor de Docker bajo el capó.
2. **Gestor de Proxy Inverso:** Configura automáticamente Traefik (o Caddy) para rutear el tráfico a los contenedores correctos basándose en nombres de dominio.
3. **Gestión de SSL (Opcional):** Tiene la capacidad de generar automáticamente certificados HTTPS gratuitos usando Let's Encrypt. *(Sin embargo, en nuestra arquitectura delegamos la encriptación HTTPS a Cloudflare Tunnels, por lo que usaremos HTTP internamente)*.
4. **Despliegues sencillos:** Nos da un panel web donde podemos levantar bases de datos y aplicaciones preconfiguradas con un par de clics, o hacer *deploy* de nuestro propio código de GitHub.

## Instalación de Coolify

La instalación se realiza ejecutando el script oficial directamente en el servidor (como usuario root o con permisos sudo).

**Comando de instalación oficial:**
```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

*(Nota: Este script tarda varios minutos ya que descarga e instala Docker, descarga las imágenes de contenedores de la base de datos interna de Coolify, el proxy y la propia aplicación).*

## Acceso al Panel de Control

Una vez finalizada la instalación exitosamente, el panel de control estará disponible en el puerto `8000` de la IP de nuestro servidor.

- **URL de acceso local:** `http://192.168.1.15:8000`

## Configuración Inicial

Al acceder por primera vez al panel de Coolify, el sistema nos pedirá crear la cuenta de usuario principal (**Root User Setup**).

1. Llenamos los datos solicitados: Nombre, Email y una Contraseña segura.
2. Este usuario tendrá acceso total de administrador sobre la instancia de Coolify, por lo que las credenciales deben guardarse en un lugar seguro (por ejemplo, en un gestor de contraseñas).
3. Tras crear la cuenta, ingresaremos al dashboard principal de Coolify.
4. En la pantalla **"Elegir Tipo de Servidor"**, seleccionamos **"Esta máquina"** (Localhost). Esto le indica a Coolify que debe desplegar nuestros contenedores y servicios en este mismo servidor físico donde está instalado (la configuración típica para un Homelab de un solo nodo).
5. Finalmente, hacemos clic en **"Crea 'Mi primer proyecto'"**. Los proyectos en Coolify funcionan como "carpetas" para agrupar lógicamente nuestras aplicaciones (ej. un proyecto para "Media", otro para "Domótica", etc.).
6. En la pantalla de **"¡Montaje completado!"**, finalizamos haciendo clic en **"Ir al Panel de Control"** para acceder a la vista general de nuestra instancia. Con esto concluimos el asistente inicial y el servidor está listo para trabajar.

## Notificaciones (Discord)

Para mantener el monitoreo del estado de las aplicaciones y despliegues, configuramos notificaciones automáticas hacia un servidor privado de Discord.

1. **Creación del Canal:** Se creó un canal específico (ej. `#alertas-coolify`) en Discord. Es buena práctica hacer este canal **Privado** para que, si en el futuro se invita a otros usuarios al servidor, no tengan acceso a los logs del sistema.
2. **Webhook:** Desde los ajustes de Integraciones del canal en Discord, se generó un Webhook.
3. **Integración en Coolify:** La URL de ese Webhook se vincula en los ajustes de notificaciones de Coolify para que despache los eventos (despliegues exitosos, caídas de contenedores, etc.) directamente al chat.
