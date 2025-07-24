# API_BASE - API REST para Gestión de Usuarios y Sucursales

## 📋 Descripción

API_BASE es una aplicación REST desarrollada en Flask que proporciona endpoints para la gestión de usuarios, autenticación, sucursales y aplicaciones. La API utiliza JWT para la autenticación y está diseñada para trabajar con una base de datos MySQL.

## 🚀 Características

- **Autenticación JWT**: Sistema de autenticación seguro con tokens de acceso y refresh
- **Gestión de Usuarios**: Registro, login, actualización de información personal
- **Gestión de Sucursales**: Asignación y cambio de sucursales activas
- **Gestión de Aplicaciones**: Control de acceso a diferentes aplicaciones
- **CORS habilitado**: Soporte para peticiones desde diferentes orígenes
- **Validación de datos**: Validación robusta de entradas
- **Encriptación de contraseñas**: Uso de bcrypt para seguridad

## 🛠️ Tecnologías Utilizadas

- **Python 3.12**
- **Flask**: Framework web
- **Flask-JWT-Extended**: Autenticación JWT
- **Flask-CORS**: Soporte para CORS
- **MySQL Connector**: Conexión a base de datos
- **bcrypt**: Encriptación de contraseñas

## 📦 Instalación

### Prerrequisitos

- Python 3.12 o superior
- MySQL Server
- pip (gestor de paquetes de Python)

### Pasos de instalación

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/mbravot/API_BASE.git
   cd API_BASE
   ```

2. **Crear entorno virtual**
   ```bash
   python -m venv venv
   ```

3. **Activar entorno virtual**
   ```bash
   # Windows
   venv\Scripts\activate
   
   # Linux/Mac
   source venv/bin/activate
   ```

4. **Instalar dependencias**
   ```bash
   pip install -r requirements.txt
   ```

5. **Configurar variables de entorno**
   
   Crear un archivo `.env` en la raíz del proyecto:
   ```env
   DEBUG=True
   DATABASE_URL=mysql+pymysql://usuario:password@localhost/nombre_base_datos
   DB_USER=usuario
   DB_PASSWORD=password
   DB_NAME=nombre_base_datos
   ```

6. **Configurar la base de datos**
   
   Asegúrate de que la base de datos MySQL esté configurada con las tablas necesarias.

## 🏃‍♂️ Ejecución

### Desarrollo local

```bash
python app.py
```

La API estará disponible en `http://localhost:5000`

### Producción

```bash
# Usando gunicorn (recomendado para producción)
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

## 📚 Endpoints

### Autenticación (`/api/auth`)

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/login` | Iniciar sesión |
| POST | `/refresh` | Refrescar token |
| POST | `/cambiar-clave` | Cambiar contraseña |
| POST | `/cambiar-sucursal` | Cambiar sucursal activa |
| GET | `/me` | Obtener información del usuario |
| PUT | `/me` | Actualizar información del usuario |

### Usuarios (`/api/usuarios`)

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/sucursal` | Obtener sucursal del usuario |
| GET | `/sucursal-activa` | Obtener sucursal activa |
| POST | `/sucursal-activa` | Actualizar sucursal activa |
| GET | `/sucursales` | Obtener todas las sucursales |
| GET | `/<usuario_id>/sucursales-permitidas` | Obtener sucursales permitidas |
| POST | `/<usuario_id>/sucursales-permitidas` | Asignar sucursales permitidas |
| DELETE | `/<usuario_id>/sucursales-permitidas` | Eliminar sucursales permitidas |
| GET | `/apps` | Obtener todas las aplicaciones |
| GET | `/<usuario_id>/apps-permitidas` | Obtener aplicaciones permitidas |
| POST | `/<usuario_id>/apps-permitidas` | Asignar aplicaciones permitidas |
| DELETE | `/<usuario_id>/apps-permitidas` | Eliminar aplicaciones permitidas |

### Opciones (`/api/opciones`)

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/` | Obtener opciones generales |
| GET | `/sucursales` | Obtener sucursales del usuario |

## 🔐 Autenticación

La API utiliza JWT (JSON Web Tokens) para la autenticación. Para acceder a endpoints protegidos, incluye el token en el header:

```
Authorization: Bearer <tu_token_jwt>
```

### Ejemplo de login

```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "usuario": "usuario_ejemplo",
    "clave": "password123"
  }'
```

**Respuesta:**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "usuario": "usuario_ejemplo",
  "nombre": "Juan",
  "id_sucursal": 1,
  "sucursal_nombre": "Sucursal Principal",
  "id_rol": 3,
  "id_perfil": 1
}
```

## 📊 Estructura de la Base de Datos

### Tabla `general_dim_usuario`

```sql
CREATE TABLE `general_dim_usuario` (
  `id` varchar(45) NOT NULL,
  `id_sucursalactiva` int NOT NULL,
  `usuario` varchar(45) NOT NULL,
  `nombre` varchar(45) NOT NULL,
  `apellido_paterno` varchar(45) NOT NULL,
  `apellido_materno` varchar(45) DEFAULT NULL,
  `clave` varchar(255) NOT NULL,
  `fecha_creacion` date NOT NULL,
  `id_estado` int NOT NULL DEFAULT '1',
  `correo` varchar(100) NOT NULL,
  `id_rol` int NOT NULL DEFAULT '3',
  `id_perfil` int NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`),
  UNIQUE KEY `id_UNIQUE` (`id`),
  UNIQUE KEY `usuario_UNIQUE` (`usuario`)
);
```

## 🔧 Configuración

### Variables de entorno

- `DEBUG`: Modo debug (True/False)
- `DATABASE_URL`: URL de conexión a la base de datos
- `DB_USER`: Usuario de la base de datos
- `DB_PASSWORD`: Contraseña de la base de datos
- `DB_NAME`: Nombre de la base de datos
- `JWT_SECRET_KEY`: Clave secreta para JWT

### CORS

La API está configurada para aceptar peticiones desde:
- `http://localhost:*`
- `http://127.0.0.1:*`
- `http://192.168.1.52:*`
- `http://192.168.1.208:*`
- `http://192.168.1.60:*`

## 🧪 Testing

Para ejecutar las pruebas:

```bash
python -m pytest tests/
```

## 📝 Ejemplos de Uso

### Obtener información del usuario

```bash
curl -X GET http://localhost:5000/api/auth/me \
  -H "Authorization: Bearer <tu_token_jwt>"
```

## 🤝 Contribución

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## 👥 Autor

**Miguel Bravo**
- GitHub: [@mbravot](https://github.com/mbravot)

## 🆘 Soporte

Si tienes alguna pregunta o necesitas ayuda, por favor abre un issue en el repositorio. 