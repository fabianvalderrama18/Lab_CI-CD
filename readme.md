# Greeting App

API REST desarrollada con FastAPI para el laboratorio de CI/CD con GitHub Actions y Amazon ECS.

## Requisitos

- Python 3.12
- Poetry
- Docker (para construir y ejecutar la imagen de contenedor)
- Una cuenta de AWS con permisos para Amazon ECR y Amazon ECS

## Instalación local

1. Instala las dependencias:

   ```bash
   poetry install --no-root
   ```

2. Ejecuta la aplicación:

   ```bash
   poetry run uvicorn main:app --app-dir src --host 0.0.0.0 --port 8000
   ```

3. Abre la documentación interactiva de FastAPI en:

   http://localhost:8000/docs

## Endpoints

### `GET /`

Devuelve un saludo personalizado mediante los parámetros `first_name` y `last_name`.

Ejemplo:

```bash
curl "http://localhost:8000/?first_name=Alvaro&last_name=Valderrama"
```

Respuesta esperada:

```json
{"message":"Hello, Alvaro Valderrama!"}
```

### `GET /health`

Endpoint de comprobación de salud de la aplicación.

## Pipeline CI/CD

El workflow de GitHub Actions está definido en `.github/workflows/ci.yml` e incluye:

1. **Lint:** análisis estático con Pylint.
2. **Tests:** ejecución de pruebas automatizadas con Pytest.
3. **Build & Push:** construcción de la imagen Docker y publicación en Amazon ECR.
4. **Deploy:** despliegue de la aplicación en Amazon ECS sobre instancias EC2.

## Variables y secretos de GitHub

Configura en **Settings → Secrets and variables → Actions** los siguientes valores:

### Repository secrets

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

### Repository variables

- `STUDENT_SUFFIX`
- `AWS_REGION` (por ejemplo, `us-east-1`)

## Estructura esperada del repositorio

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── main.py
├── tests/
├── scripts/
│   └── deploy.sh
├── Dockerfile
├── pyproject.toml
├── poetry.lock
└── README.md
```

## Notas

- Ejecuta los comandos desde la raíz del repositorio.
- El nombre del archivo debe ser exactamente `README.md` (respetando mayúsculas y minúsculas), ya que los runners de GitHub Actions usan Linux.
- Si Poetry continúa reportando que no encuentra el archivo, verifica que `README.md` esté en la raíz del repositorio, que haya sido agregado al commit y que el cambio se haya enviado a la rama que ejecuta el workflow.
