# API REST con Flask, JWT, test unitarios y Logs

Implementación de una API REST modular usando Flask, autenticación JWT, sistema de logs, test unitarios y variables de entorno.

## Características Principales

- ✨ Autenticación mediante JWT
- 📁 Arquitectura modular con Blueprints
- 📝 Sistema de logs detallado
- 🔐 Gestión segura de configuración (.env)
- 🗃️ Conexión a MySQL
- ✅ Tests unitarios

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

## Testing

### Ejecutar Tests
```bash
pytest src/tests/
```

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
    "username": "admin",
    "password": "admin123"
}
```

### Endpoint Protegido
```http
GET /languages
Authorization: Bearer eyJhbGciOiJIUzI1...
```

## Licencia
Distribuido bajo la Licencia MIT. Ver `LICENSE` para más información.