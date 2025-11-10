# API REST con Flask, JWT y Logs

Implementación de una API REST modular usando Flask, con autenticación JWT, sistema de logs y variables de entorno. Incluye containerización con Docker para despliegue simplificado.

## Características Principales

- ✨ Autenticación mediante JWT
- 📁 Arquitectura modular con Blueprints
- 📝 Sistema de logs detallado
- 🔐 Gestión segura de configuración (.env)
- 🗃️ Conexión a MySQL
- ✅ Tests unitarios
- 🐳 Containerización con Docker

## Arquitectura y Patrones

### Estructura del Proyecto
```plaintext
src/
├── routes/          # Endpoints de la API
│   ├── AuthRoutes.py
│   ├── IndexRoutes.py
│   └── LanguageRoutes.py
├── services/        # Lógica de negocio
│   ├── AuthService.py
│   └── LanguageService.py
├── models/         # Modelos de datos
├── utils/          # Utilidades
│   ├── Security.py
│   └── Logger.py
└── database/       # Capa de datos
    └── db_mysql.py
```

### Patrones Implementados
- **Repository Pattern**: Abstracción de acceso a datos
- **Service Layer**: Encapsulamiento de lógica de negocio
- **Dependency Injection**: Configuración mediante config.py
- **Factory Pattern**: Inicialización de app

## Implementaciones Técnicas

### JWT (JSON Web Tokens)
```python
# Estructura del payload
{
    'iat': datetime.datetime.now(tz=cls.tz),
    'exp': datetime.datetime.now(tz=cls.tz) + datetime.timedelta(minutes=10),
    'username': user.username,
    'fullname': user.fullname,
    'roles': ['Administrator', 'Editor']
}
```

- 🔑 Algoritmo: HS256
- ⏱️ Tiempo de expiración: 10 minutos
- 🛡️ Roles: Administrator, Editor
- 🔒 Verificación mediante decorator en rutas protegidas

### Sistema de Logs
```python
# Formato de logs
%(asctime)s | %(levelname)s | %(message)s
```

#### Eventos Registrados
- 🚫 Errores de aplicación
- 🚪 Accesos a endpoints
- 💾 Operaciones de base de datos
- 🔐 Intentos de autenticación

### Variables de Entorno (.env)
```plaintext
# Configuración requerida
SECRET_KEY=tu_clave_secreta
MYSQL_HOST=localhost
MYSQL_USER=usuario_db
MYSQL_PASSWORD=password_db
MYSQL_DB=flask_jwt_logs
JWT_KEY=clave_jwt
```

### Blueprints (Modularidad)
```python
# Registro de rutas
app.register_blueprint(IndexRoutes.main, url_prefix='/')
app.register_blueprint(AuthRoutes.main, url_prefix='/auth')
app.register_blueprint(LanguageRoutes.main, url_prefix='/languages')
```

## Seguridad

### Medidas Implementadas
- 🛡️ Autenticación JWT
- 🔒 Variables sensibles en .env
- 📝 Logs de seguridad
- 🧹 Sanitización de entradas
- ⚠️ Manejo global de excepciones

## Requisitos y Configuración

### Dependencias Principales
- Python 3.x
- Flask 2.3.2
- MySQL/MariaDB
- PyJWT 2.7.0
- python-decouple 3.8
- Gunicorn 20.1.0

### Instalación
```bash
# Instalar dependencias
pip install -r requirements.txt

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus configuraciones

# Iniciar servidor
python index.py
```

### Base de Datos
```sql
# Importar estructura inicial
mysql -u usuario -p flask_jwt_logs < scripts/flask_jwt_log_backup.sql
```

## Dockerización 🐳

### Características Docker
- 🚀 Imagen base ligera: python:3.13-slim
- 📦 Multistage building para optimización
- ⚙️ Configuración via variables de entorno
- 🌐 Gunicorn como servidor WSGI de producción
- 🔌 Exposición del puerto 5000

### Construcción de la Imagen
```bash
# Construir imagen
docker build -t flask-api:latest .

# Construir sin caché (desarrollo)
docker build --no-cache --force-rm -t flask-api:latest .
```

### Variables de Entorno en Docker
```bash
# Variables requeridas para el contenedor
SECRET_KEY=tu_clave_secreta
MYSQL_HOST=host.docker.internal  # Conexión a MySQL local
MYSQL_USER=usuario_db
MYSQL_PASSWORD=password_db
MYSQL_DB=flask_jwt_logs
JWT_KEY=tu_clave_jwt
```

### Ejecución del Contenedor
```powershell
docker run `
  --env SECRET_KEY='B!3w6*NAt2T^%kvhUI*S^_' `
  --env MYSQL_HOST='host.docker.internal' `
  --env MYSQL_USER='root' `
  --env MYSQL_PASSWORD='' `
  --env MYSQL_DB='flask_jwt_logs' `
  --env JWT_KEY='D8*F?_1?-d$f*5' `
  -p 5000:5000 -d --name flask-api flask-api:latest
```

### Características de Producción
- ⚡ 4 workers de Gunicorn para mejor rendimiento
- 📝 Logs de aplicación persistentes que se ven desde docker en files app/src/utils/log
- 🧹 Ignorar archivos innecesarios (.dockerignore)
- 🔄 Manejo de señales para graceful shutdown
- 🔒 Conexión segura a MySQL


### Cobertura
- ✅ Tests unitarios de servicios
- ✅ Validación de rutas protegidas
- ✅ Verificación de respuestas HTTP
- ✅ Casos de error

## Ejemplos de Uso

### Autenticación
```http
POST /auth/login
Content-Type: application/json

{
    "username": "lordMiguel",
    "password": "Migusprime"
}
```

### Endpoint Protegido
```http
GET /languages
Authorization: Bearer (el jwt q devuelve el route auth)
```

## Licencia
Distribuido bajo la Licencia MIT. Ver `LICENSE` para más información.