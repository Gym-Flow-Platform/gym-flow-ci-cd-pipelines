# GymFlow CI/CD Pipelines

Este repositorio contiene flujos de trabajo (workflows) reutilizables de GitHub Actions diseñados para estandarizar el proceso de Integración Continua y Despliegue Continuo (CI/CD) en todos los servicios de **GymFlow**.

## 🚀 Workflows Disponibles

### 1. PR Validation (`pr-validation.yml`)
Se encarga de validar el código cuando se realiza un Pull Request. Realiza las siguientes tareas:
- Instalación de dependencias (`npm install`).
- Ejecución de pruebas unitarias (`npm test`).
- Análisis estático de código con **SonarQube Scan**.

#### Uso
```yaml
jobs:
  validate:
    uses: GymFlowProyect/gym-flow-ci-cd-pipelines/.github/workflows/pr-validation.yml@main
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

### 2. Docker Build (`docker-build.yml`)
Automatiza la construcción, escaneo y publicación de imágenes Docker en DockerHub.
- Construcción de la imagen Docker.
- Escaneo de vulnerabilidades con **Trivy** (falla si encuentra severidades CRITICAL o HIGH).
- Push de la imagen a DockerHub.

#### Inputs
| Parámetro | Descripción | Requerido |
| :--- | :--- | :---: |
| `image_name` | Nombre de la imagen en DockerHub (ej: `user/gymflow-service`) | Sí |

#### Uso
```yaml
jobs:
  build:
    uses: GymFlowProyect/gym-flow-ci-cd-pipelines/.github/workflows/docker-build.yml@main
    with:
      image_name: "mi-usuario/mi-repositorio"
    secrets:
      DOCKERHUB_USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}
      DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}
```

---

## 🔐 Secretos Requeridos

Para que estos workflows funcionen correctamente, los repositorios que los llaman deben definir los siguientes secretos:

| Secreto | Workflow | Descripción |
| :--- | :--- | :--- |
| `SONAR_TOKEN` | PR Validation | Token de autenticación para SonarQube. |
| `DOCKERHUB_USERNAME` | Docker Build | Usuario de DockerHub. |
| `DOCKERHUB_TOKEN` | Docker Build | Access Token o contraseña de DockerHub. |

---

## 🛠️ Tecnologías

- **GitHub Actions**: Orquestación de pipelines.
- **Docker**: Containerización.
- **Trivy**: Escaneo de seguridad de contenedores.
- **SonarQube**: Calidad y seguridad del código.
- **npm**: Gestión de dependencias y testing.

---

> [!NOTE]
> Todos los workflows están diseñados para ser invocados mediante `workflow_call`.
