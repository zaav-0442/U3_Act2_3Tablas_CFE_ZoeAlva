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

¿Quieres que ahora te genere los **archivos HTML completos** (base.html, navbar.html, inicio.html y los de sucursal) con los estilos y menús descritos?
