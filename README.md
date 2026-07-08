# Tienda Perritos - DevOps Project

Proyecto de automatización CI/CD para una aplicación de tienda de alimentos para perritos, desplegada en Amazon EKS con GitHub Actions.

## Integrantes
- Gonzalo Berrios
- Cristian Cerda
- Solange Valladares

## Arquitectura

```
Frontend (Nginx) → Backend (Node.js/Express) → MySQL 8
```

## Tecnologías
- **Frontend**: Nginx + HTML/CSS/JavaScript
- **Backend**: Node.js + Express + mysql2
- **Base de datos**: MySQL 8
- **Contenedores**: Docker, Docker Compose
- **Registro**: Amazon ECR
- **Orquestación**: Amazon EKS (Kubernetes)
- **CI/CD**: GitHub Actions
- **Cloud**: AWS (VPC, EKS, ECR, IAM, CloudWatch)

## Estructura del proyecto

```
tienda-perritos-EKS/
├── frontend/               # Frontend Nginx
│   ├── Dockerfile          # Multi-stage build
│   ├── .dockerignore
│   ├── default.conf
│   ├── index.html
│   └── app.js
├── backend/                # API Node.js
│   ├── Dockerfile          # Multi-stage build
│   ├── .dockerignore
│   ├── package.json
│   └── server.js
├── db/                     # Base de datos
│   ├── Dockerfile
│   ├── .dockerignore
│   └── init.sql
├── k8s/                    # Manifiestos Kubernetes
│   ├── namespace.yaml
│   ├── mysql-secret.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── backend-hpa.yaml
│   └── frontend-hpa.yaml
├── .github/workflows/
│   └── deploy-eks.yml      # Pipeline CI/CD
├── docker-compose.yml       # Entorno desarrollo local
└── README.md
```

## Desarrollo Local

### Prerrequisitos
- Docker y Docker Compose

### Levantar entorno local
```bash
docker-compose up --build
```

### Endpoints
- Frontend: http://localhost:80
- API Productos: http://localhost:3001/api/productos
- API Salud: http://localhost:3001/api/health

## CI/CD Pipeline

El pipeline en GitHub Actions automatiza:
1. Build de imágenes Docker para frontend, backend y db
2. Push a Amazon ECR con tags (commit SHA + latest)
3. Despliegue en Amazon EKS
4. Verificación de estado

### Ejecución del pipeline (Run #9)
Pipeline completado exitosamente en 1m 31s.

Ver pipeline: [Actions Run #9](https://github.com/Basty66/tienda-perritos-EKS/actions/runs/28138059031)

## Despliegue en Producción

**URL:** http://aaf6981a1eddc4d95883f10254500569-342821456.us-east-1.elb.amazonaws.com

**API:** http://aaf6981a1eddc4d95883f10254500569-342821456.us-east-1.elb.amazonaws.com/api/productos

### Componentes desplegados
- 5 pods en EKS (1 MySQL, 2 Backend, 2 Frontend)
- Auto-escalado horizontal (HPA: frontend 60%, backend 70%)
- Classic Load Balancer exponiendo frontend

## Monitoreo
- Logs del pipeline: GitHub Actions
- Métricas de clúster: AWS CloudWatch
- Health checks: Endpoint /api/health

## Seguridad
- Imágenes base Alpine (minimalistas)
- Usuario no-root en contenedores
- Credenciales en GitHub Secrets
- IAM con mínimo privilegio
- Puertos mínimos expuestos en Security Groups
