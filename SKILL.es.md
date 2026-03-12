version: 1.0.0
description: Herramienta de automatización de infraestructura impulsada por IA para crear, desplegar y gestionar infraestructura nativa en la nube como código utilizando plantillas y mejores prácticas impulsadas por IA.
author: OpenClaw Team
tags: infrastructure, ai, automation, cloud-native, IaC
dependencies:
  - terraform >= 1.5.0
  - ansible >= 2.15.0
  - kubectl >= 1.28.0
  - docker >= 24.0.0
  - python >= 3.9.0
  - awscli >= 2.13.0
  - az >= 2.50.0
  - gcloud >= 450.0.0
environment_variables:
  PRIME_CRAFTER_API_KEY: Clave API requerida para acceso al modelo de IA
  PRIME_CRAFTER_MODEL: Opcional, por defecto gpt-4o; soporta gpt-3.5-turbo, claude-3-sonnet
  PRIME_CRAFTER_REGION: Opcional, por defecto us-east-1 para despliegues AWS
  PRIME_CRAFTER_LOG_LEVEL: Opcional, por defecto INFO; opciones: DEBUG, INFO, WARN, ERROR
requirements:
  - Cuenta activa de proveedor de nube (AWS, Azure, GCP) con acceso programático
  - Permisos IAM suficientes para aprovisionamiento de infraestructura
  - Conectividad de red a APIs de nube
  - Al menos 8GB RAM para procesamiento de IA
  - SO soportado: Linux, macOS, Windows (WSL2)
---

# Habilidad Prime Crafter

## Propósito

Prime Crafter es una habilidad de automatización de infraestructura impulsada por IA diseñada para generar, validar y desplegar plantillas de infraestructura como código (IaC) listas para producción para aplicaciones nativas en la nube. Utiliza modelos de IA avanzados para crear módulos Terraform, playbooks Ansible, manifiestos Kubernetes y configuraciones Docker basadas en requisitos especificados por el usuario, asegurando cumplimiento de seguridad, escalabilidad y mejores prácticas.

### Casos de Uso Reales

1. **Despliegue Rápido de Prototipo**: Genera una configuración completa de VPC con instancias EC2, bases de datos RDS y balanceadores de carga para una aplicación web en minutos, reduciendo el tiempo de scripting manual de horas a segundos.
2. **Migración Multi-Nube**: Automatiza la creación de recursos de infraestructura equivalentes en AWS, Azure y GCP, incluyendo peering de red, grupos de seguridad y pipelines CI/CD para migraciones sin tiempo de inactividad.
3. **IaC Impulsado por Cumplimiento**: Crea pipelines de datos de salud HIPAA-compliant con almacenamiento encriptado, logging de auditoría y backups automatizados, asegurando adherencia regulatoria sin configuración manual de políticas.
4. **Orquestación de Microservicios**: Genera despliegues Kubernetes con service mesh Istio, monitoreo Prometheus y tracing Jaeger para una plataforma de e-commerce containerizada, optimizando para alta disponibilidad y auto-scaling.
5. **Configuración de Recuperación de Desastres**: Crea infraestructura geo-redundante con routing de failover Route 53, tablas globales DynamoDB y automatización de backup basada en Lambda para sistemas de trading financiero.

## Alcance

Prime Crafter opera dentro del dominio de infraestructura, enfocándose en generación y despliegue de IaC. No maneja código de aplicación, diseño de esquema de base de datos o desarrollo frontend.

### Comandos Específicos

- `prime-crafter generate --template vpc --provider aws --region us-west-2 --security-level high --output ./infrastructure/vpc.tf`: Genera una plantilla VPC con configuración de alta seguridad.
- `prime-crafter validate --input ./infrastructure/ --provider azure --check compliance --standard soc2`: Valida IaC contra estándares de cumplimiento SOC2.
- `prime-crafter deploy --input ./infrastructure/ --provider gcp --dry-run --auto-approve`: Realiza un despliegue de dry-run a Google Cloud.
- `prime-crafter monitor --resource eks-cluster --provider aws --metrics cpu,memory --alert-threshold 80`: Monitorea recursos de clúster EKS con alertas.
- `prime-crafter optimize --input ./infrastructure/ --cost-target 1000 --region eu-central-1`: Optimiza infraestructura para eficiencia de costo apuntando a $1000/mes en EU Central.
- `prime-crafter audit --input ./infrastructure/ --provider multi --report json`: Genera un reporte de auditoría de seguridad multi-nube en formato JSON.

## Proceso de Trabajo

Prime Crafter sigue un flujo de trabajo estructurado guiado por IA para asegurar creación de infraestructura confiable:

1. **Análisis de Requisitos**: Analiza la entrada del usuario para proveedor de nube, tipos de recursos, requisitos de seguridad y necesidades de escalado utilizando procesamiento de lenguaje natural.
2. **Selección de Plantilla**: Coincide requisitos con plantillas pre-entrenadas (e.g., ECS Fargate para serverless, EKS para Kubernetes) con recomendaciones de IA.
3. **Generación de Código**: Utiliza IA para generar código IaC, incorporando mejores prácticas como tagging, encriptación y IAM de menor privilegio.
4. **Fase de Validación**: Ejecuta verificaciones automatizadas para errores de sintaxis, vulnerabilidades de seguridad (vía herramientas como Checkov) y estimaciones de costo.
5. **Simulación**: Ejecuta despliegues dry-run en un entorno sandbox para predecir uso de recursos y conflictos potenciales.
6. **Despliegue**: Aplica cambios con auto-aprobación para recursos no críticos, requiriendo confirmación manual para entornos de producción.
7. **Integración de Monitoreo**: Integra dashboards de monitoreo y alertas utilizando CloudWatch, Cloud Monitoring o Azure Monitor.
8. **Documentación**: Auto-genera archivos README, diagramas de arquitectura y runbooks para la infraestructura creada.

### Pasos de Verificación

- **Verificación de Sintaxis**: Ejecuta `terraform validate` o equivalente para asegurar no hay errores de parsing.
- **Escaneo de Seguridad**: Ejecuta `checkov -f ./infrastructure/` para identificar vulnerabilidades.
- **Estimación de Costo**: Utiliza `infracost breakdown --path ./infrastructure/` para verificar cumplimiento de presupuesto.
- **Éxito de Dry-Run**: Confirma `terraform plan` muestra no cambios destructivos sin `--auto-approve`.
- **Salud de Recursos**: Post-despliegue, consulta APIs (e.g., `aws ec2 describe-instances`) para confirmar instancias están ejecutándose.

## Reglas de Oro

1. **Nunca Desplegar a Producción Sin Revisión**: Siempre requiere aprobación humana para despliegues prod; usa `--dry-run` para pruebas iniciales.
2. **Encriptar Secretos**: Automáticamente genera y usa AWS KMS, Azure Key Vault o GCP Secret Manager para todos los datos sensibles.
3. **Seguir Menor Privilegio**: Roles IAM generados por IA incluyen solo permisos necesarios; evita políticas wildcard.
4. **Control de Versiones IaC**: Commitea todo código generado a Git con mensajes descriptivos; etiqueta releases.
5. **Monitorear Costos**: Integra alertas de costo y establece presupuestos; optimiza automáticamente si se exceden umbrales.
6. **Backup de Datos Críticos**: Incluye snapshots automatizados y replicación cross-region para recursos stateful.
7. **Probar Failover**: Genera scripts de chaos engineering para simular fallos y validar recuperación.
8. **Documentar Dependencias**: Lista todos los servicios externos, APIs y requisitos de red en READMEs generados.

## Ejemplos

### Ejemplo 1: Generar una VPC Segura para una App Web

**Input Prompt**: "Crea una VPC con subnets públicas y privadas, NAT gateway, security groups para HTTP/HTTPS, y una instancia RDS PostgreSQL para mi sitio de e-commerce en AWS us-east-1."

**Generated Output**:
```hcl
# vpc.tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "ecommerce-vpc"
    Environment = "production"
  }
}

resource "aws_subnet" "public" {
  count = 2
  vpc_id = aws_vpc.main.id
  cidr_block = cidrsubnet(aws_vpc.main.cidr_block, 8, count.index)
  availability_zone = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
}

# Recursos adicionales para NAT, security groups, y RDS...
```

**CLI Execution**: `prime-crafter generate --prompt "Crea una VPC con subnets públicas y privadas, NAT gateway, security groups para HTTP/HTTPS, y una instancia RDS PostgreSQL para mi sitio de e-commerce en AWS us-east-1." --output ./infrastructure/`

### Ejemplo 2: Desplegar un Clúster Kubernetes con Monitoreo

**Input Prompt**: "Despliega un clúster EKS con 3 nodos, monitoreo Prometheus, y dashboard Grafana en AWS."

**Generated Output**:
- Archivos Terraform para configuración EKS
- Charts Helm para Prometheus/Grafana
- Overlays Kustomize para personalización

**CLI Execution**: `prime-crafter deploy --input ./k8s-cluster/ --provider aws --monitor --dashboard grafana`

### Ejemplo 3: Optimizar Infraestructura Existente para Costo

**Input Prompt**: "Reduce costos para mi granja de VMs Azure en un 30% sin downtime."

**Generated Output**: Migración a instancias spot, recomendaciones de instancias reservadas y políticas de autoscaling.

**CLI Execution**: `prime-crafter optimize --input ./azure-vms/ --cost-reduction 30 --no-downtime`

## Comandos de Rollback

- `prime-crafter rollback --deployment-id 12345 --provider aws --confirm`: Revierte el último despliegue a AWS, destruyendo recursos creados.
- `prime-crafter undo --input ./infrastructure/ --version v1.2.3 --provider gcp`: Revierte a una versión previa de IaC en GCP.
- `prime-crafter destroy --resource rds-instance --provider azure --backup-first`: Destruye una instancia RDS después de crear un backup en Azure.
- `prime-crafter revert-changes --dry-run --input ./infrastructure/`: Muestra qué se revertiría sin aplicar.

### Solución de Problemas Comunes

1. **Límites de Tasa de API**: Si los despliegues fallan debido a límites de tasa, usa `--retry-delay 30` para añadir backoff exponencial.
2. **Conflictos de Dependencias**: Ejecuta `prime-crafter validate --check dependencies` para identificar proveedores o módulos faltantes.
3. **Violaciones de Seguridad**: Aborda errores de Checkov regenerando con `--security-level strict`.
4. **Excesos de Costo**: Habilita `--cost-alert` para pausar despliegues que excedan presupuestos.
5. **Tiempos de Espera de Red**: Incrementa `--timeout 600` para despliegues grandes en redes restringidas.