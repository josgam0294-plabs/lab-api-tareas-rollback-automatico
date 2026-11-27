# API de tareas

Aplicación de laboratorio del curso SOFT-756 (Implementación de CI/CD y Prácticas DevOps). Es una
API pequeña en Python (Flask) que guarda tareas en memoria. El código de la aplicación casi no
cambia durante el curso: lo que crece es el **pipeline** que la construye, la prueba, la empaqueta
y la despliega.

## Endpoints

| Método | Ruta | Respuesta |
|---|---|---|
| GET | `/health` | `{"estado": "ok"}` si la API está viva |
| GET | `/version` | Versión desplegada y ambiente (`APP_VERSION`, `APP_ENV`) |
| GET | `/tareas` | Lista de tareas |
| GET | `/tareas/<id>` | Una tarea, o 404 |
| POST | `/tareas` | Crea una tarea con `{"titulo": "..."}`; 201, o 400 si el título no es válido |

Si la variable `API_TOKEN` tiene valor, `POST /tareas` exige el encabezado
`Authorization: Bearer <token>`. Las lecturas no lo exigen.

## Configuración

| Variable | Para qué sirve | Valor por defecto |
|---|---|---|
| `APP_ENV` | Nombre del ambiente (desarrollo, staging, produccion) | `desarrollo` |
| `APP_VERSION` | Versión que reporta `/version` | `dev` |
| `API_TOKEN` | Token que protege `POST /tareas` | sin valor (no se exige) |

## Correr en local

Requisitos: Python 3.10 o posterior, Git y una terminal Bash (en Windows, **Git Bash**).

```bash
python -m venv .venv
source .venv/bin/activate          # en Windows (Git Bash): source .venv/Scripts/activate
python -m pip install -r requirements-dev.txt
bash scripts/pruebas.sh            # corre las pruebas
python -m flask --app app run --port 8000
```

Con la API en marcha, en otra terminal:

```bash
bash scripts/smoke.sh http://localhost:8000
```

Resultado esperado:

```text
Smoke test contra http://localhost:8000
OK /health
OK /version -> {"ambiente":"desarrollo","version":"dev"}
OK /tareas
Smoke test superado
```

## Estructura

```text
app/            código de la API (config.py lee las variables de entorno)
tests/          pruebas con pytest
scripts/        pruebas.sh y smoke.sh: los mismos comandos en local y en el pipeline
docs/           documentación del flujo de entrega
requirements.txt       dependencias de la aplicación, con versión fija
requirements-dev.txt   dependencias para desarrollar y probar
```

## Parte 3 

V3.12-slim
IMAGE                                                                          ID             DISK USAGE   CONTENT SIZE   EXTRA
ghcr.io/josgam0294-plabs/api-tareas:7acde46b6ddb32d101680f28c3412ee5324e3145   3806e162aea4        188MB         45.5MB        

IMAGE ID DISK USAGE CONTENT SIZE EXTRA

ghcr.io/josgam0294-plabs/api-tareas:89d4b4445535dadef86529ec8e7aa6996da06ba7 acb0e3e4ad91 1.61GB 414MB

### Docker Ignore
.git = Evitar incluir configuraciones de git en el paquete
.github = Evitar incluir configuraciones de github en el paquete
.venv = Para no incluir las configuraciones del ambiente virtual de python
venv = En caso de haber nombrado venv al ambiente virtual mismo principio del .venv
**/__pycache__ = Para no incluir archivos temporales de la ejecución del ambiente de python
**/*.pyc = Para no incluir archivos temporales de la ejecución del ambiente de python
.pytest_cache = Para no incluir archivos temporales de la ejecución del ambiente de pruebas de python
.ruff_cache = Para evitar que build de docker requiera reconstruir la imagen por conflictos con el cache de la imagen
.coverage = Este archivo es autogenerado por los paquetes de testing de Python
htmlcov = Este archivo es autogenerado por los paquetes de testing de Python
reportes = No es necesario para construir la imagen en Docker
tests = No es necesario para construir la imagen en Docker
docs = No es necesario para construir la imagen en Docker
.env = En caso de haber nombrado venv al ambiente virtual mismo principio del .venv