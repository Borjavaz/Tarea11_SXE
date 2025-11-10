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

---

## Levantar contenedores

Tenemos que iniciar docker

Despues entramos a la carpeta donde esta el .yml y ejecutar el siguiente comando:

```bash
docker compose up
```

<img width="731" height="78" alt="image" src="https://github.com/user-attachments/assets/ac597337-d2b1-4e92-a369-2ce4c60c8e54" />

En docker se deveria ver asi:

<img width="956" height="413" alt="image" src="https://github.com/user-attachments/assets/fcecbe88-d280-489e-80d1-4f47e9a01259" />

A continuacion tenemos que entrar el los puertos que definimos antes:

### `8080`

Cuando entramos a este puerto, se nos abre este menu.
Tenemos que meter los datos que declaramos anteriormente en el .yml para el gestor de DB.

<img width="953" height="719" alt="image" src="https://github.com/user-attachments/assets/491e8663-e15a-4151-9a85-08be0dd36f5e" />

Cuando entramos en el gestor esto es lo que se vería:

<img width="953" height="719" alt="image" src="https://github.com/user-attachments/assets/d15f32b1-52bf-4e49-ab94-fbfc3fb5ebbf" />


### `8069`

Cuando entramos a este puerto, se nos abre este menu.

Rellenamos los campos con nuestra información para crear una base de datos.

Es importante seleccionar la casilla de `Demo Data`.

<img width="915" height="889" alt="image" src="https://github.com/user-attachments/assets/87c02540-310f-486b-82db-3d4c540ac852" />

Cuando creamos la base de datos, nos lleva a otra pagina para registrarnos.

Completamos estos campos con la información que pusimos antes, para iniciar sesion.


<img width="306" height="428" alt="image" src="https://github.com/user-attachments/assets/cd898776-66eb-40ea-a40c-3bf6039d1406" />


---

## Explorar Odoo con Datos de Demo

Cuando iniciamos sesion en Odoo, veremos algo así:

<img width="1920" height="516" alt="image" src="https://github.com/user-attachments/assets/f3a11338-edab-4183-9fcb-c1426c01d6cf" />

Cuando entro al apartado de ventas se me abre esta ventana:

<img width="958" height="585" alt="image" src="https://github.com/user-attachments/assets/77e35f97-f52a-4cb2-b786-45d3942789b4" />

<img width="958" height="585" alt="image" src="https://github.com/user-attachments/assets/8aa01fa9-40eb-4277-a947-ff98e9afb8a2" />
<img width="958" height="585" alt="image" src="https://github.com/user-attachments/assets/0362217a-a98e-4f36-ae2a-5a83e6104573" />
<img width="958" height="379" alt="image" src="https://github.com/user-attachments/assets/c89e0005-16ab-4f5f-a77d-7e4f190f9d0c" />


<img width="956" height="1011" alt="image" src="https://github.com/user-attachments/assets/4a81d1f9-b574-4c2f-b611-abd2b0c1ed9a" />

<img width="957" height="399" alt="image" src="https://github.com/user-attachments/assets/c9e79f55-8c9c-4476-a0cb-2d936424972e" />

<img width="957" height="622" alt="image" src="https://github.com/user-attachments/assets/540e6e9f-0563-4106-8525-4a29e2afb4aa" />

<img width="1920" height="867" alt="image" src="https://github.com/user-attachments/assets/5983bcb4-dbfe-4880-9f5b-67ee908575d8" />

<img width="1920" height="554" alt="image" src="https://github.com/user-attachments/assets/af86df4c-b176-4012-bff3-0396400c920a" />



