# 🚀 Proyecto Final Introduccion a DevOps:

Este repositorio contiene la implementación completa de un flujo de trabajo **MLOps (Machine Learning Operations)**. El objetivo del proyecto es entrenar, evaluar y desplegar automáticamente un modelo de Machine Learning que predice si los ingresos de una persona superan los $50K/año, basándose en datos censales (Adult Dataset).

## 👥 Equipo de Trabajo
* **Marvin Bernal** 
* **Manuel Pérez** 
* **Jose Alexander** 
* **Enmanuel Alejandro** 

---

## 🏗️ Arquitectura y Flujo de Trabajo (CI/CD)

El proyecto utiliza **GitHub Actions** para la automatización y **Render** para el despliegue en la nube. El flujo se divide en tres pipelines automáticas:

1. **Integración (`integration.yml`):** Se dispara en las Pull Requests hacia `main`. Verifica la calidad del código, instala las dependencias y ejecuta los test unitarios. Bloquea el PR si algo falla.

2. **Build (`build.yml`):** Se dispara al hacer merge en `main`. Descarga el dataset, entrena el modelo (`src/main.py`), ejecuta los tests del modelo (`model_test/test_model.py`) asegurando un *accuracy* superior al 80%, y finalmente empaqueta el modelo y sus transformadores como un "Release" en GitHub.

3. **Deploy (`deploy.yml`):** Se dispara automáticamente si el *Build* es exitoso. Envía un Webhook a Render para iniciar el despliegue de la nueva versión de la API.

---

## 📁 Estructura del Repositorio

El proyecto está organizado de la siguiente manera:

```PONTIA-MLOPS
├── .github/workflows/       # Pipelines de CI/CD 
│   ├── build.yml            # Pipeline de entrenamiento y empaquetado
│   ├── deploy.yml           # Pipeline de despliegue a Render
│   └── integration.yml      # Pipeline de tests y calidad de código
├── data/raw/                # Carpeta para almacenar el dataset (ignorado en git)
│   └── .gitkeep             
├── deployment/              # Código para el servidor web (Producción)
│   ├── app/                 
│   │   ├── __init__.py
│   │   └── main.py          # API en FastAPI que sirve el modelo
│   └── requirements.txt     # Dependencias exclusivas para la API
├── model_test/              # Pruebas automatizadas de rendimiento del modelo
│   ├── __init__.py
│   └── test_model.py
├── models/                  # Artefactos del modelo generados localmente
│   └── .gitkeep
├── src/                     # Código fuente de Machine Learning
│   ├── __init__.py
│   ├── data_loader.py       # Carga y limpieza de datos
│   ├── evaluate.py          # Métricas de evaluación
│   ├── main.py              # Script principal de entrenamiento
│   └── model.py             # Definición del modelo
├── unit_tests/              # Pruebas unitarias de las funciones de ML
│   ├── __init__.py
│   ├── test_data_loader.py
│   ├── test_evaluate.py
│   └── test_model.py
├── .gitignore               # Archivos y carpetas ignorados por git (ej. venv)
├── pytest.ini               # Configuración del framework de testing
├── README.md                # Documentación principal del proyecto
├── render.yaml              # Infraestructura como Código (Blueprint de Render)
└── requirements.txt         # Dependencias globales del proyecto

