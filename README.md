# ☕ Cafetería - Sistema de Gestión de Pedidos

Sistema web completo para la gestión de pedidos, ventas y análisis en una cafetería. Desarrollado con Django y PostgreSQL.

## 📋 Características

- ✅ Gestión de productos y categorías
- ✅ Sistema de pedidos en tiempo real
- ✅ Múltiples métodos de pago (Efectivo, QR, Tarjeta)
- ✅ Generación de notas de venta
- ✅ Dashboard de vendedor con estadísticas
- ✅ Panel de encargado con control de inventario
- ✅ Sistema de autenticación seguro
- ✅ Auditoría de operaciones
- ✅ Reportes de ventas

## 🔧 Requisitos Previos

Antes de instalar, asegúrate de tener:

- **Python 3.8+** [Descargar](https://www.python.org/downloads/)
- **pip** (incluido con Python)
- **Git** [Descargar](https://git-scm.com/)
- **PostgreSQL 12+** (opcional, SQLite incluido por defecto)

## 📦 Instalación

### 1. Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/cafeteria.git
cd cafeteria
```

### 2. Crear Entorno Virtual

#### En Windows:
```bash
python -m venv venv
venv\Scripts\activate
```

#### En macOS/Linux:
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Instalar Dependencias

```bash
pip install -r requirements.txt
```

### 4. Configurar Variables de Entorno

Copia el archivo de ejemplo y configura tus credenciales:

```bash
cp .env.example .env
```

Edita el archivo `.env` con tus datos:

```env
# Django
SECRET_KEY=tu-clave-secreta-aqui
DEBUG=True

# Base de Datos (PostgreSQL)
DB_ENGINE=django.db.backends.postgresql
DB_NAME=cafeteria_dba
DB_USER=tu-usuario
DB_PASSWORD=tu-contraseña
DB_HOST=localhost
DB_PORT=5432

# Para desarrollo local (SQLite)
# DB_ENGINE=django.db.backends.sqlite3
# DB_NAME=db.sqlite3

# Hosts permitidos
ALLOWED_HOSTS=localhost,127.0.0.1
```

> ⚠️ **IMPORTANTE**: Nunca compartas el archivo `.env` en GitHub. Está incluido en `.gitignore`.

### 5. Ejecutar Migraciones

```bash
python manage.py migrate
```

### 6. Crear Superusuario (Admin)

```bash
python manage.py createsuperuser
```

Sigue las instrucciones en terminal. Ejemplo:
```
Username: admin
Email: admin@cafeteria.com
Password: ••••••••
```

### 7. Iniciar el Servidor

```bash
python manage.py runserver
```

Abre tu navegador en: **http://127.0.0.1:8000**

## 🏗️ Estructura del Proyecto

```
cafeteria/
├── Mysitecoffe/          # Configuración principal de Django
│   ├── settings.py       # Configuración del proyecto
│   ├── urls.py           # Rutas principales
│   └── wsgi.py           # WSGI para producción
├── personas/             # Gestión de usuarios (empleados, clientes)
├── operaciones/          # Gestión de pedidos y notas de venta
├── menu/                 # Catálogo de productos
├── categoria/            # Categorías de productos
├── auditoria/            # Registro de auditoría
├── templates/            # Plantillas HTML
├── static/               # Archivos estáticos (CSS, JS, imágenes)
├── requirements.txt      # Dependencias de Python
└── manage.py             # Interfaz de gestión de Django
```

## 🗄️ Configuración de Base de Datos

### Usar SQLite (Desarrollo local)

SQLite está configurado por defecto. El archivo `db.sqlite3` se crea automáticamente en la raíz del proyecto.

### Usar PostgreSQL (Recomendado para Producción)

1. **Instala PostgreSQL** desde [postgresql.org](https://www.postgresql.org/download/)

2. **Crea la base de datos:**

```bash
# Conectarse a PostgreSQL
psql -U postgres

# Crear base de datos
CREATE DATABASE cafeteria_dba;

# Crear usuario (optional)
CREATE USER cafeteria_user WITH PASSWORD 'tu_contraseña';

# Dar permisos
ALTER ROLE cafeteria_user SET client_encoding TO 'utf8';
ALTER ROLE cafeteria_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE cafeteria_user SET default_transaction_deferrable TO on;
GRANT ALL PRIVILEGES ON DATABASE cafeteria_dba TO cafeteria_user;
```

3. **Actualiza el `.env`:**

```env
DB_ENGINE=django.db.backends.postgresql
DB_NAME=cafeteria_dba
DB_USER=cafeteria_user
DB_PASSWORD=tu_contraseña
DB_HOST=localhost
DB_PORT=5432
```

4. **Ejecuta migraciones:**

```bash
python manage.py migrate
```

## 👥 Roles y Permisos

El sistema cuenta con los siguientes roles:

| Rol | Permisos |
|-----|----------|
| **Admin** | Acceso completo al panel de administración |
| **Encargado** | Gestión de inventario, reportes y empleados |
| **Vendedor** | Crear pedidos, registro de ventas, historial |
| **Cliente** | (Futuro) Realizar pedidos en línea |

## 🔒 Seguridad

- Las credenciales de base de datos están protegidas en `.env`
- El archivo `.env` **NO se sube a GitHub** (ver `.gitignore`)
- Se valida CSRF en todos los formularios
- Autenticación de sesión para usuario logueado

## 📊 Crear Datos de Prueba

Para crear datos de ejemplo:

```bash
python manage.py shell
```

```python
from personas.models import Cliente, Empleado
from menu.models import Producto
from categoria.models import Categoria

# Crear categoría
cat = Categoria.objects.create(nombre="Bebidas", descripcion="Bebidas calientes")

# Crear producto
Producto.objects.create(
    nombre="Café Americano",
    descripcion="Café espresso con agua caliente",
    precio=28.00,
    id_categoria=cat
)

# Crear cliente
Cliente.objects.create(
    nombre="Juan Pérez",
    ci_o_nit="12345678",
    email="juan@example.com"
)

exit()
```

## 🚀 Despliegue en Producción

Para desplegar en un servidor:

1. **Cambiar DEBUG a False en `.env`:**
   ```env
   DEBUG=False
   ```

2. **Usar Gunicorn como servidor WSGI:**
   ```bash
   pip install gunicorn
   gunicorn Mysitecoffe.wsgi:application
   ```

3. **Usar un web server (Nginx/Apache)** como proxy reverso

4. **Configurar HTTPS** con Let's Encrypt

## 📝 Comandos Útiles

```bash
# Crear migraciones de cambios en modelos
python manage.py makemigrations

# Aplicar migraciones
python manage.py migrate

# Crear superusuario
python manage.py createsuperuser

# Acceder a shell interactivo de Django
python manage.py shell

# Recolectar archivos estáticos
python manage.py collectstatic

# Ejecutar servidor con puerto específico
python manage.py runserver 0.0.0.0:8080

# Ver rutas disponibles
python manage.py show_urls
```

## 🐛 Solución de Problemas

### Error: `ModuleNotFoundError: No module named 'dotenv'`
```bash
pip install python-dotenv
```

### Error: `psycopg2` no encontrado
```bash
pip install psycopg2-binary
```

### Base de datos vacía
```bash
python manage.py migrate
python manage.py createsuperuser
```

### Permiso denegado en `/media/` o `/static/`
```bash
chmod -R 755 media/
chmod -R 755 static/
```

## 📞 Soporte

Para reportar bugs o sugerencias, abre un [issue](https://github.com/tu-usuario/cafeteria/issues).

## 📄 Licencia

Este proyecto está bajo licencia MIT. Ver archivo `LICENSE` para más detalles.

---

**Made with ☕ and Django**
