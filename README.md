# Tarea11_SXE

---

## Preparar el IDE:

Lo primero que voy a hacer es instalar una serie de plugins en mi IDE, que me van a ayudar a la hora de crear nuevos documentos YML:

### `Docker`

<img width="987" height="218" alt="image" src="https://github.com/user-attachments/assets/8aed2e0f-570b-4c7d-971f-01f733ea0040" />

Plugin de `“Docker”`: mejora la edición de los archivos Dockerfile en PyCharm añadiendo resaltado de sintaxis, 
autocompletado de instrucciones y detecta errores de estructura o sintaxis del Dockerfile. 

<img width="954" height="327" alt="image" src="https://github.com/user-attachments/assets/fb64b241-75ef-447f-b22f-e29557fa674a" />


Ahora cuando creamos un nuevo documento yml, tenemos que crearlo como un Dockerfile

### `YAML`

<img width="987" height="218" alt="image" src="https://github.com/user-attachments/assets/8b56fcfa-6b79-4614-a0c1-fd9494fa27b7" />


Plugin de “YAML”: facilita el trabajo con archivos .yml y .yaml añadiendo resaltado de sintaxis,
formateado automático y autocompletado.

---

## Instalar Odoo 18 Community y PgAdmin con Docker Compose

## `Docker-compose.yml`

### `Postgres`

- Primero declaro la imagen y el nombre del contenedor.
- Despues en el enviroment, meto las variables de entorno necesarias
- Por ultimo añado los volumenes las networks y el restart


```bash
  postgresql:
    image: postgres:15
    container_name: postgresql-odoo18
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - ./postgresql/data:/var/lib/postgresql/data
    networks:
      - odoo-network
    restart: unless-stopped
```

### `Odoo`

- Primero meto la imagen de odoo 18 y l epongo el nombre al contenedor.
- A continuacion le meto las dependencias con la base de datos.
- Continuo con el enviroment, en el que añado las variables de entorno necesarias: https://hub.docker.com/_/odoo
- Creo los volumenes para guardar datos

<img width="950" height="195" alt="image" src="https://github.com/user-attachments/assets/7ab3087f-284d-4b16-9eb7-4d84c24aac04" />

<img width="950" height="195" alt="image" src="https://github.com/user-attachments/assets/583e48cd-173d-47fb-b5f0-2cb05a422cfc" />

- Declaro el puerto, en este caso el 8069 que es el que viene por defecto con Odoo.
- Por ultimo las networks y el restart


```bash
  odoo:
    image: odoo:18.0
    container_name: odoo18
    depends_on:
      - postgresql
    environment:
      - HOST=postgresql
      - USER=odoo
      - PASSWORD=odoo
    volumes:
      - ./odoo/addons:/mnt/extra-addons
      - ./odoo/config:/etc/odoo
    ports:
      - "8069:8069"
    networks:
      - odoo-network
    restart: unless-stopped
```

### `Pgadmin`

- Primero meto la imagen y le pongo nombre al contenedor
- A continuacion en el enviroment declaro las variables de entorno
- Despues declaro el puerto 8080, que es el que viene por defecto.
- Declaro tambien las networks y el restart.
- Por ultimo declaro la dependencia del gestor con la BD.

```bash
  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: pgadmin-odoo
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=admin
    ports:
      - "8080:80"
    networks:
      - odoo-network
    restart: unless-stopped
    depends_on:
      - postgresql
```
