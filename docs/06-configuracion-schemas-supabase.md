# Guía: Exponer Nuevos Schemas en Supabase (Autoalojado)

En la versión autoalojada de Supabase (vía Coolify o Docker), la API REST (PostgREST) no expone nuevos schemas automáticamente por motivos de seguridad y arquitectura. PostgREST requiere una lista explícita de schemas permitidos en sus variables de entorno. 

No existe un "comodín" (wildcard) para decirle que acepte todos los schemas futuros. Por lo tanto, cada vez que crees un proyecto nuevo con un schema aislado (ej. `cobranzas`), **debes realizar este proceso manual una única vez**.

## Paso 1: Crear el Schema y dar Permisos (SQL)

Para aislar proyectos, es buena práctica crear un esquema nuevo. Además, la API no puede leer un schema si no tiene permisos, y es crítico incluir al rol `authenticator` para que Supabase pueda gestionar las conexiones.

Usa la siguiente plantilla para crear el schema y asignar los permisos correctamente:

```sql
-- ==========================================
-- CREACIÓN DEL SCHEMA
-- ==========================================
-- Crear el schema para aislar este proyecto
CREATE SCHEMA IF NOT EXISTS nombre_del_schema;

-- ==========================================
-- PERMISOS PARA LA API DE SUPABASE
-- ==========================================
-- Dar permisos de uso del schema a los roles de la API (¡INCLUYENDO authenticator!)
GRANT USAGE ON SCHEMA nombre_del_schema TO authenticator, anon, authenticated, service_role;

-- Dar permisos sobre todas las tablas actuales
GRANT ALL ON ALL TABLES IN SCHEMA nombre_del_schema TO anon, authenticated, service_role;

-- Dar permisos sobre todas las secuencias (IDs autoincrementales)
GRANT ALL ON ALL SEQUENCES IN SCHEMA nombre_del_schema TO anon, authenticated, service_role;

-- Asegurar que las tablas futuras también tengan permisos
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA nombre_del_schema GRANT ALL ON TABLES TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA nombre_del_schema GRANT ALL ON SEQUENCES TO anon, authenticated, service_role;
```

## Paso 2: Exponer el Schema en Coolify

Para que la API despierte y vea el schema, debes agregarlo a la lista blanca:

1. Ingresa a tu panel de **Coolify**.
2. Navega hacia tu Proyecto/Entorno y abre el servicio de **Supabase**.
3. Ve a la pestaña **Environment Variables** (Variables de Entorno) o **Secrets**.
4. Busca la variable llamada `PGRST_DB_SCHEMAS` (a veces aparece como `DB_SCHEMAS`).
5. Su valor por defecto suele ser `public, storage, graphql_public`. 
6. **Agrega tu nuevo schema al final de la lista**, separado por una coma. Por ejemplo:
   ```text
   public, storage, graphql_public, cobranzas
   ```
7. Guarda los cambios.
8. **Reinicia el contenedor** de Supabase (en Coolify, puedes darle a Restart al servicio completo).

## Paso 3: Validar en n8n

Una vez reiniciado, ve a n8n, entra a tu nodo de Supabase, activa la opción **Use Custom Schema**, escribe el nombre exacto del schema y dale a "Execute step". Si no devuelve un error `503 Service Unavailable`, ¡felicidades, el schema está expuesto!
