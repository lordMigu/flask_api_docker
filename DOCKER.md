## 🧱 1. Construcción de la Imagen Docker para un microservicio en contenedor y base de datos en local

```bash
docker build --force-rm -t flask-api . --no-cache
```

🔍 **¿Qué hace este comando?**

- **`docker build`**: Inicia la construcción de una imagen Docker.
- **`--force-rm`**: Elimina contenedores intermedios creados durante la construcción.
- **`-t flask-api`**: Asigna el nombre `flask-api` a la imagen.
- **`.`**: Indica que el `Dockerfile` está en el directorio actual.
- **`--no-cache`**: Fuerza una construcción limpia, sin usar archivos en caché.

📦 Resultado: Se crea una imagen llamada `flask-api` que contiene tu aplicación Flask y sus dependencias.

---

## 🚀 2. Ejecución del Contenedor (para pegar en terminal de vs code eliminar los \)

```bash
docker run \
  --env=SECRET_KEY=B!3w6*NAt2T^%kvhUI*S^_ \
  --env=MYSQL_HOST=host.docker.internal \
  --env=MYSQL_USER=root \
  --env=MYSQL_PASSWORD= \
  --env=MYSQL_DB=flask_jwt_logs \
  --env=JWT_KEY=D8*F?_1?-d$f*5 \
  -p 5000:5000 \
  -d \
  --name FlaskApiDocker \
  flask-api:latest
```

🔍 **¿Qué hace este comando?**

- **`docker run`**: Crea y ejecuta un contenedor a partir de la imagen `flask-api`.
- **`--env=...`**: Define variables de entorno que tu aplicación Flask usará:
  - `SECRET_KEY`: Clave secreta para sesiones o seguridad.
  - `MYSQL_HOST`: Dirección del servidor MySQL (en este caso, el host local desde Docker).
  - `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`: Credenciales y nombre de la base de datos.
  - `JWT_KEY`: Clave para generar y validar tokens JWT.
- **`-p 5000:5000`**: Mapea el puerto interno del contenedor (5000) al puerto externo de tu máquina (5000).
- **`-d`**: Ejecuta el contenedor en segundo plano (*detached mode*).
- **`--name FlaskApiDocker`**: Asigna un nombre personalizado al contenedor.
- **`flask-api:latest`**: Usa la imagen recién creada con el tag `latest`.

📡 Resultado: Tu aplicación Flask queda corriendo en un contenedor Docker, accesible desde `http://localhost:5000`, y conectada a una base de datos MySQL que corre en tu máquina local.

---

## 🧠 En resumen

Estás haciendo lo siguiente:

1. **Construyendo** una imagen Docker con tu aplicación Flask.
2. **Ejecutando** esa imagen como contenedor.
3. **Configurando** variables de entorno para seguridad y conexión a MySQL.
4. **Exponiendo** el puerto 5000 para acceder a la API desde tu navegador o herramientas como Postman.
