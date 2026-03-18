# 🎸 DJANGO DESDE CERO - GUÍA COMPLETA

**Django desde Cero** es un sitio educativo completo diseñado para enseñar Django desde los fundamentos hasta conceptos avanzados, con explicaciones claras, ejemplos prácticos y código listo para usar.

> *"Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design."*

---

## 🎯 ¿Qué es este Proyecto?

Este proyecto proporciona un recurso educativo gratuito para aprender Django, incluyendo:

- **Documentación completa** de cada tema
- **Ejemplos de código** listos para ejecutar
- **Ejercicios prácticos** para reforzar el aprendizaje
- **Sitio web educativo** con navegación intuitiva

---

## 📚 Contenido del Curso

### Módulo 1: Fundamentos

1. **Introducción**
   - ¿Qué es Django?
   - MTV Pattern (Model-Template-View)
   - Filosofía Django

2. **Instalación**
   - Python y pip
   - Virtual environments
   - django-admin
   - Estructura de proyecto

3. **Conceptos básicos**
   - Models
   - Views
   - Templates
   - URLs

### Módulo 2: Intermedio

4. **Ejemplos prácticos**
   - Django ORM
   - Forms y ModelForms
   - Autenticación
   - Admin personalizado

5. **Buenas prácticas**
   - Class-based views
   - Django REST Framework
   - Testing
   - Debugging

### Módulo 3: Avanzado

6. **Casos reales**
   - APIs REST
   - WebSockets con Channels
   - Celery para tareas
   - Deploy con Gunicorn/Nginx

7. **Proyecto final**
   - Aplicación completa
   - Deploy a producción
   - CI/CD pipeline

---

## 🗂️ Estructura del Proyecto

```
Practica-Suma-de-Matrices/
├── index.html          # Página principal
├── css/
│   └── styles.css      # Estilos del sitio
├── js/
│   └── main.js         # JavaScript del sitio
└── README.md
```

---

## 🚀 Cómo Usar este Proyecto

### Opción 1: Navegar el Sitio Web

1. Abre `index.html` en tu navegador
2. Navega por las secciones del curso
3. Haz clic en los temas para ver la documentación detallada

### Opción 2: Ejecutar los Ejemplos

1. Instala Python y Django
2. Crea proyecto Django
3. Ejecuta con `python manage.py runserver`

### Requisitos

- Python 3.8+
- pip y virtualenv
- Node.js (opcional para frontend)

---

## 📝 Ejemplos Rápidos

### Models

```python
from django.db import models
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    published = models.BooleanField(default=False)

    class Meta:
        ordering = ['-created_at']

    def __str__(self):
        return self.title
```

### Views

```python
from django.shortcuts import render, get_object_or_404, redirect
from django.http import HttpResponse, JsonResponse
from django.views.generic import ListView, DetailView, CreateView

from .models import Post
from .forms import PostForm

# Function-based view
def post_list(request):
    posts = Post.objects.filter(published=True)
    return render(request, 'posts/list.html', {'posts': posts})

# Class-based view
class PostListView(ListView):
    model = Post
    template_name = 'posts/list.html'
    context_object_name = 'posts'
    paginate_by = 10

# API view
def post_api(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return JsonResponse({
        'title': post.title,
        'content': post.content
    })
```

### URLs

```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('', views.PostListView.as_view(), name='post-list'),
    path('post/<int:pk>/', views.PostDetailView.as_view(), name='post-detail'),
    path('post/create/', views.PostCreateView.as_view(), name='post-create'),
    path('api/posts/', views.post_list_api, name='api-post-list'),
]
```

### Templates

```django
{# templates/posts/list.html #}

{% extends 'base.html' %}

{% block title %}Posts{% endblock %}

{% block content %}
<div class="container">
    <h1>Posts</h1>

    {% if user.is_authenticated %}
        <a href="{% url 'post-create' %}">Nuevo Post</a>
    {% endif %}

    {% for post in posts %}
        <article>
            <h2>{{ post.title }}</h2>
            <p>{{ post.content|truncatewords:30 }}</p>
            <small>Por {{ post.author }} el {{ post.created_at }}</small>
            <a href="{% url 'post-detail' post.pk %}">Leer más</a>
        </article>
    {% empty %}
        <p>No hay posts disponibles.</p>
    {% endfor %}

    {% if is_paginated %}
        <div class="pagination">
            {% if page_obj.has_previous %}
                <a href="?page={{ page_obj.previous_page_number }}">Anterior</a>
            {% endif %}

            <span>Página {{ page_obj.number }} de {{ page_obj.paginator.num_pages }}</span>

            {% if page_obj.has_next %}
                <a href="?page={{ page_obj.next_page_number }}">Siguiente</a>
            {% endif %}
        </div>
    {% endif %}
</div>
{% endblock %}
```

### Forms

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content', 'published']
        widgets = {
            'title': forms.TextInput(attrs={'class': 'form-control'}),
            'content': forms.Textarea(attrs={'class': 'form-control', 'rows': 10}),
        }

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)

    def clean_email(self):
        email = self.cleaned_data.get('email')
        if not email.endswith('@gmail.com'):
            raise forms.ValidationError("Debe ser un email de Gmail")
        return email
```

---

## 🎓 Metodología de Aprendizaje

### 1. Leer la Teoría
Cada tema comienza con una explicación clara del concepto.

### 2. Ver Ejemplos
Los ejemplos de código muestran la aplicación práctica.

### 3. Practicar
Los ejercicios te permiten aplicar lo aprendido.

### 4. Experimentar
Modifica los ejemplos para entender cómo funcionan.

---

## 🔧 Comandos Esenciales

### Django CLI

```bash
# Instalar Django
pip install django

# Crear proyecto
django-admin startproject mi_proyecto

# Crear app
python manage.py startapp mi_app

# Servidor de desarrollo
python manage.py runserver

# Migraciones
python manage.py makemigrations
python manage.py migrate

# Crear superusuario
python manage.py createsuperuser

# Shell de Django
python manage.py shell

# Tests
python manage.py test
```

---

## 📖 Recursos Adicionales

### Documentación Oficial

- [Django Documentation](https://docs.djangoproject.com/)
- [Django Tutorial](https://docs.djangoproject.com/en/stable/intro/tutorial01/)
- [Django Packages](https://djangopackages.org/)

### Herramientas Recomendadas

- **Django Debug Toolbar** - Debugging
- **Django REST Framework** - APIs
- **pytest-django** - Testing

### Comunidades

- [Django Community](https://www.djangoproject.com/community/)
- [Stack Overflow - Django](https://stackoverflow.com/questions/tagged/django)
- [Reddit r/django](https://www.reddit.com/r/django/)

---

## 💡 Consejos para Principiantes

1. **Sigue el tutorial oficial**: Es excelente.
2. **Usa el admin**: Es una de las mejores features.
3. **Aprende el ORM**: Es poderoso y elegante.
4. **Lee el código fuente**: Django es open source.
5. **Explora Django Packages**: Hay miles de paquetes.

---

## ⚠️ Mejores Prácticas

### Seguridad

- Usa CSRF tokens
- Valida todos los inputs
- Usa HTTPS en producción

### Código Limpio

- Sigue PEP 8
- Usa class-based views cuando sea apropiado
- Mantén views delgadas

### Rendimiento

- Usa select_related y prefetch_related
- Implementa caching
- Usa indexes en la base de datos

---

## 🧪 Ejercicios Prácticos

### Nivel Básico

1. Blog personal
2. Sistema de encuestas
3. CRUD de contactos

### Nivel Intermedio

1. API REST con DRF
2. Sistema de e-commerce
3. Red social básica

### Nivel Avanzado

1. Plataforma SaaS
2. Sistema en tiempo real con Channels
3. Microservicios con Django

---

## 👨‍💻 Desarrollado por Isaac Esteban Haro Torres

**Ingeniero en Sistemas · Full Stack · Automatización · Data**

- 📧 Email: zackharo1@gmail.com
- 📱 WhatsApp: 098805517
- 💻 GitHub: https://github.com/ieharo1
- 🌐 Portafolio: https://ieharo1.github.io/portafolio-isaac.haro/

---

© 2026 Isaac Esteban Haro Torres - Todos los derechos reservados.
