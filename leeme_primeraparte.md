Perfecto ✅ — te dejo **la guía paso a paso completa** para realizar **la primera parte del proyecto “CFE (Comisión Federal de Electricidad)”** con **Django en VS Code**, siguiendo exactamente tu descripción y con todos los comandos necesarios.

---

## 🧩 PRIMERA PARTE — PROYECTO “CFE”

### 🔹 1. Crear carpeta del proyecto

```bash
mkdir UIII_CFE_0442
cd UIII_CFE_0442
```

---

### 🔹 2. Abrir VS Code sobre la carpeta `UIII_CFE_0442`

En terminal:

```bash
code .
```

O desde el explorador de archivos:
📂 Clic derecho sobre la carpeta → “**Abrir con VS Code**”.

---

### 🔹 3. Abrir terminal en VS Code

En la barra superior de VS Code:

```
Ver → Terminal
```

O usa el atajo:

```
Ctrl + `
```

---

### 🔹 4. Crear carpeta de entorno virtual `.venv` desde la terminal

```bash
python -m venv .venv
```

---

### 🔹 5. Activar el entorno virtual

#### En Windows:

```bash
.venv\Scripts\activate
```

#### En macOS / Linux:

```bash
source .venv/bin/activate
```

Deberás ver algo como:

```
(.venv) C:\Users\Zoé\UIII_CFE_0442>
```

---

### 🔹 6. Activar intérprete de Python en VS Code

1. Presiona **Ctrl + Shift + P**
2. Escribe **Python: Select Interpreter**
3. Elige la ruta de `.venv`.

---

### 🔹 7. Instalar Django

```bash
pip install django
```

Puedes verificar la instalación:

```bash
django-admin --version
```

---

### 🔹 8. Crear proyecto `backend_CFE` sin duplicar carpeta

Asegúrate de estar dentro de `UIII_CFE_0442`:

```bash
django-admin startproject backend_CFE .
```

*(El punto final evita crear una carpeta adicional.)*

---

### 🔹 9. Ejecutar servidor en el puerto **8042**

```bash
python manage.py runserver 8042
```

---

### 🔹 10. Copiar y pegar el link en el navegador

Abre en tu navegador:

```
http://127.0.0.1:8042/
```

---

### 🔹 11. Crear aplicación `app_CFE`

```bash
python manage.py startapp app_CFE
```

---

### 🔹 12. Crear el modelo `models.py` en `app_CFE/models.py`

```python
from django.db import models

# ==========================================
# MODELO: SUCURSAL
# ==========================================
class Sucursal(models.Model):
    nombre = models.CharField(max_length=100)
    clave = models.CharField(max_length=50)
    direccion = models.CharField(max_length=200)
    telefono = models.CharField(max_length=20)
    ciudad = models.CharField(max_length=100)
    estado = models.CharField(max_length=100)
    fecha_apertura = models.DateField()

    def __str__(self):
        return self.nombre


# ==========================================
# MODELO: EMPLEADO
# ==========================================
class Empleado(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    puesto = models.CharField(max_length=100)
    rfc = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    fecha_contratacion = models.DateField()
    empleadorio = models.DecimalField(max_digits=10, decimal_places=2)
    sucursal = models.ForeignKey(Sucursal, on_delete=models.CASCADE, related_name="empleados")

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: DOMICILIO
# ==========================================
class Domicilio(models.Model):
    calle = models.CharField(max_length=100)
    numero = models.CharField(max_length=10)
    colonia = models.CharField(max_length=100)
    ciudad = models.CharField(max_length=100)
    codigo_postal = models.CharField(max_length=10)
    titular = models.CharField(max_length=100)
    fecha_registro = models.DateField()
    empleados_asignados = models.ForeignKey(Empleado, on_delete=models.CASCADE, related_name="domicilios")

    def __str__(self):
        return f"{self.calle} #{self.numero}"
```

---

### 🔹 12.5. Realizar las migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 🔹 13. Trabajar primero con el modelo **Sucursal**

---

### 🔹 14. En `app_CFE/views.py`, agregar las funciones CRUD para Sucursal

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Sucursal

def inicio_cfe(request):
    return render(request, 'inicio.html')

def agregar_sucursal(request):
    if request.method == 'POST':
        Sucursal.objects.create(
            nombre=request.POST['nombre'],
            clave=request.POST['clave'],
            direccion=request.POST['direccion'],
            telefono=request.POST['telefono'],
            ciudad=request.POST['ciudad'],
            estado=request.POST['estado'],
            fecha_apertura=request.POST['fecha_apertura']
        )
        return redirect('ver_sucursales')
    return render(request, 'sucursal/agregar_sucursal.html')

def ver_sucursales(request):
    sucursales = Sucursal.objects.all()
    return render(request, 'sucursal/ver_sucursales.html', {'sucursales': sucursales})

def actualizar_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    return render(request, 'sucursal/actualizar_sucursal.html', {'sucursal': sucursal})

def realizar_actualizacion_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    if request.method == 'POST':
        sucursal.nombre = request.POST['nombre']
        sucursal.clave = request.POST['clave']
        sucursal.direccion = request.POST['direccion']
        sucursal.telefono = request.POST['telefono']
        sucursal.ciudad = request.POST['ciudad']
        sucursal.estado = request.POST['estado']
        sucursal.fecha_apertura = request.POST['fecha_apertura']
        sucursal.save()
        return redirect('ver_sucursales')

def borrar_sucursal(request, id):
    sucursal = get_object_or_404(Sucursal, id=id)
    sucursal.delete()
    return redirect('ver_sucursales')
```

---

### 🔹 15–22. Crear carpeta y archivos HTML

**Estructura:**

```
app_CFE/
 └── templates/
      ├── base.html
      ├── header.html
      ├── navbar.html
      ├── footer.html
      ├── inicio.html
      └── sucursal/
           ├── agregar_sucursal.html
           ├── ver_sucursales.html
           ├── actualizar_sucursal.html
           └── borrar_sucursal.html
```

Todos con estilos suaves y modernos usando **Bootstrap 5**.
*(Puedo generarte los HTML si lo deseas en la siguiente parte).*

---

### 🔹 24. Crear `urls.py` en `app_CFE`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_cfe, name='inicio_cfe'),
    path('sucursales/', views.ver_sucursales, name='ver_sucursales'),
    path('sucursales/agregar/', views.agregar_sucursal, name='agregar_sucursal'),
    path('sucursales/actualizar/<int:id>/', views.actualizar_sucursal, name='actualizar_sucursal'),
    path('sucursales/realizar_actualizacion/<int:id>/', views.realizar_actualizacion_sucursal, name='realizar_actualizacion_sucursal'),
    path('sucursales/borrar/<int:id>/', views.borrar_sucursal, name='borrar_sucursal'),
]
```

---

### 🔹 25. Registrar app en `settings.py` de `backend_CFE`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_CFE',  # ← Agregar aquí
]
```

---

### 🔹 26. Configurar `urls.py` principal de `backend_CFE`

`backend_CFE/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_CFE.urls')),
]
```

---

### 🔹 27. Registrar modelos en `admin.py`

```python
from django.contrib import admin
from .models import Sucursal, Empleado, Domicilio

admin.site.register(Sucursal)
admin.site.register(Empleado)
admin.site.register(Domicilio)
```

Volver a migrar:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 🔹 28–33. Trabajar solo con **Sucursal**, aplicar colores suaves y modernos en HTML, y ejecutar servidor:

```bash
python manage.py runserver 8042
```

Abrir:

```
http://127.0.0.1:8042/
```

---

A continuación te dejo **todos los archivos HTML completos** con diseño moderno, colores suaves y usando **Bootstrap 5**.
Incluyen la estructura de navegación, pie de página, submenús y las páginas CRUD para **Sucursal**.
*(No hay validación de datos, como pediste.)*

---

## 📁 Estructura de carpetas

```
app_CFE/
 └── templates/
      ├── base.html
      ├── header.html
      ├── navbar.html
      ├── footer.html
      ├── inicio.html
      └── sucursal/
           ├── agregar_sucursal.html
           ├── ver_sucursales.html
           ├── actualizar_sucursal.html
           └── borrar_sucursal.html
```

---

## 🧱 base.html

```html
{% include 'header.html' %}
<body>
    {% include 'navbar.html' %}
    <main class="container mt-4 mb-5">
        {% block content %}
        {% endblock %}
    </main>
    {% include 'footer.html' %}
</body>
</html>
```

---

## 🧩 header.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Administración CFE</title>
    <!-- Bootstrap 5 -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <!-- Íconos de Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8fafc;
            color: #212529;
            font-family: 'Segoe UI', sans-serif;
        }
        footer {
            background-color: #004d40;
            color: white;
            position: fixed;
            bottom: 0;
            width: 100%;
            padding: 10px 0;
            text-align: center;
            font-size: 0.9rem;
        }
        nav.navbar {
            background-color: #00695c;
        }
        nav a.nav-link, nav .navbar-brand {
            color: white !important;
        }
        nav a.nav-link:hover {
            background-color: #004d40;
        }
        .btn-custom {
            background-color: #26a69a;
            color: white;
        }
        .btn-custom:hover {
            background-color: #00897b;
        }
        .card {
            border-radius: 1rem;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
    </style>
</head>
```

---

## 🧭 navbar.html

```html
<nav class="navbar navbar-expand-lg">
  <div class="container-fluid">
    <a class="navbar-brand fw-bold" href="{% url 'inicio_cfe' %}">
      <i class="bi bi-lightning-charge-fill"></i> Sistema de Administración CFE
    </a>
    <button class="navbar-toggler text-white" type="button" data-bs-toggle="collapse" data-bs-target="#navbarCFE">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarCFE">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio_cfe' %}"><i class="bi bi-house-door-fill"></i> Inicio</a></li>
        <!-- Menú Sucursales -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="sucursalesDropdown" role="button" data-bs-toggle="dropdown">
            <i class="bi bi-building"></i> Sucursales
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_sucursal' %}">Agregar Sucursal</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_sucursales' %}">Ver Sucursales</a></li>
          </ul>
        </li>
        <!-- Menú Empleados -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="empleadosDropdown" role="button" data-bs-toggle="dropdown">
            <i class="bi bi-person-badge"></i> Empleados
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
            <li><a class="dropdown-item" href="#">Ver Empleados</a></li>
            <li><a class="dropdown-item" href="#">Actualizar Empleado</a></li>
            <li><a class="dropdown-item" href="#">Borrar Empleado</a></li>
          </ul>
        </li>
        <!-- Menú Domicilios -->
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="domiciliosDropdown" role="button" data-bs-toggle="dropdown">
            <i class="bi bi-geo-alt-fill"></i> Domicilios
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Domicilio</a></li>
            <li><a class="dropdown-item" href="#">Ver Domicilios</a></li>
            <li><a class="dropdown-item" href="#">Actualizar Domicilio</a></li>
            <li><a class="dropdown-item" href="#">Borrar Domicilio</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

---

## 🦶 footer.html

```html
<footer>
  © {{ now|date:"Y" }} Comisión Federal de Electricidad — Creado por Zoé Alva, CBTIS 128
</footer>
```

---

## 🏠 inicio.html

```html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
    <h1 class="fw-bold text-success">Bienvenido al Sistema de Administración CFE</h1>
    <p class="lead mt-3">Administra sucursales, empleados y domicilios de la Comisión Federal de Electricidad.</p>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Logo_de_la_CFE.svg" alt="CFE" class="img-fluid mt-4" style="max-height:200px;">
</div>
{% endblock %}
```

---

## 🏢 sucursal/agregar_sucursal.html

```html
{% extends 'base.html' %}
{% block content %}
<div class="card p-4">
  <h3 class="text-center text-success mb-3">Agregar Nueva Sucursal</h3>
  <form method="POST">
    {% csrf_token %}
    <div class="row g-3">
      <div class="col-md-6"><input type="text" name="nombre" class="form-control" placeholder="Nombre"></div>
      <div class="col-md-6"><input type="text" name="clave" class="form-control" placeholder="Clave"></div>
      <div class="col-md-12"><input type="text" name="direccion" class="form-control" placeholder="Dirección"></div>
      <div class="col-md-6"><input type="text" name="telefono" class="form-control" placeholder="Teléfono"></div>
      <div class="col-md-6"><input type="text" name="ciudad" class="form-control" placeholder="Ciudad"></div>
      <div class="col-md-6"><input type="text" name="estado" class="form-control" placeholder="Estado"></div>
      <div class="col-md-6"><input type="date" name="fecha_apertura" class="form-control"></div>
    </div>
    <div class="text-center mt-4">
      <button class="btn btn-custom px-5">Guardar</button>
    </div>
  </form>
</div>
{% endblock %}
```

---

## 📋 sucursal/ver_sucursales.html

```html
{% extends 'base.html' %}
{% block content %}
<h3 class="text-success text-center mb-4">Listado de Sucursales</h3>
<table class="table table-striped table-hover">
  <thead class="table-success">
    <tr>
      <th>ID</th>
      <th>Nombre</th>
      <th>Clave</th>
      <th>Ciudad</th>
      <th>Estado</th>
      <th>Teléfono</th>
      <th>Fecha Apertura</th>
      <th>Acciones</th>
    </tr>
  </thead>
  <tbody>
    {% for s in sucursales %}
    <tr>
      <td>{{ s.id }}</td>
      <td>{{ s.nombre }}</td>
      <td>{{ s.clave }}</td>
      <td>{{ s.ciudad }}</td>
      <td>{{ s.estado }}</td>
      <td>{{ s.telefono }}</td>
      <td>{{ s.fecha_apertura }}</td>
      <td>
        <a href="{% url 'actualizar_sucursal' s.id %}" class="btn btn-sm btn-warning"><i class="bi bi-pencil-square"></i></a>
        <a href="{% url 'borrar_sucursal' s.id %}" class="btn btn-sm btn-danger"><i class="bi bi-trash"></i></a>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="8" class="text-center">No hay sucursales registradas.</td></tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

---

## ✏️ sucursal/actualizar_sucursal.html

```html
{% extends 'base.html' %}
{% block content %}
<div class="card p-4">
  <h3 class="text-center text-success mb-3">Actualizar Sucursal</h3>
  <form method="POST" action="{% url 'realizar_actualizacion_sucursal' sucursal.id %}">
    {% csrf_token %}
    <div class="row g-3">
      <div class="col-md-6"><input type="text" name="nombre" class="form-control" value="{{ sucursal.nombre }}"></div>
      <div class="col-md-6"><input type="text" name="clave" class="form-control" value="{{ sucursal.clave }}"></div>
      <div class="col-md-12"><input type="text" name="direccion" class="form-control" value="{{ sucursal.direccion }}"></div>
      <div class="col-md-6"><input type="text" name="telefono" class="form-control" value="{{ sucursal.telefono }}"></div>
      <div class="col-md-6"><input type="text" name="ciudad" class="form-control" value="{{ sucursal.ciudad }}"></div>
      <div class="col-md-6"><input type="text" name="estado" class="form-control" value="{{ sucursal.estado }}"></div>
      <div class="col-md-6"><input type="date" name="fecha_apertura" class="form-control" value="{{ sucursal.fecha_apertura|date:'Y-m-d' }}"></div>
    </div>
    <div class="text-center mt-4">
      <button class="btn btn-custom px-5">Actualizar</button>
    </div>
  </form>
</div>
{% endblock %}
```

---

## 🗑️ sucursal/borrar_sucursal.html

```html
{% extends 'base.html' %}
{% block content %}
<div class="text-center">
  <h3 class="text-danger">¿Seguro que deseas eliminar esta sucursal?</h3>
  <p><strong>{{ sucursal.nombre }}</strong></p>
  <a href="{% url 'ver_sucursales' %}" class="btn btn-secondary">Cancelar</a>
  <a href="{% url 'borrar_sucursal' sucursal.id %}" class="btn btn-danger">Eliminar</a>
</div>
{% endblock %}
```

---

💡 **Listo:** con estos archivos y tu `views.py`, `urls.py` y configuraciones, tu sistema CFE ya está **funcional para CRUD de Sucursales** en el puerto `8042`.

---

