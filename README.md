# Evaluación Final Transversal - AUY1104

# Operación Resiliencia en TechMarket

## Tabla de Contenidos

-   Descripción
-   Objetivos
-   Arquitectura de la Solución
-   Estructura del Proyecto
-   Función de los Archivos Principales
-   Herramientas Utilizadas
-   Plantillas Reutilizables
-   Pipeline CI
-   Pipeline CD
-   Estrategia de Despliegue
-   Comparación entre Estrategias
-   Justificación de Blue-Green
-   Mecanismo de Remediación Automática
-   Escenarios de Error
-   Beneficios para el Negocio
-   Conclusión
-   Declaración de Uso de IA
-   Referencias
-   Integrante

------------------------------------------------------------------------

# Descripción

Este repositorio corresponde a la Evaluación Final Transversal (EFT) de
la asignatura **Ciclo de Vida del Software II (AUY1104)**.

El proyecto implementa un proceso de Integración Continua (CI) y
Despliegue Continuo (CD) utilizando GitHub Actions, Docker Hub y
Kubernetes, aplicando una estrategia **Blue-Green Deployment** y
mecanismos de recuperación automática para aumentar la disponibilidad
del servicio.

> Para esta evaluación se utiliza la infraestructura trabajada durante
> el semestre. Tal como fue indicado por el docente, el rol descrito
> para Amazon EKS se resuelve mediante **K3s sobre EC2**, mientras que
> el rol de Amazon ECR se resuelve utilizando **Docker Hub**,
> manteniendo la misma lógica de despliegue.

# Objetivos

## Objetivo General

Implementar un pipeline CI/CD reutilizable que automatice la
construcción, validación y despliegue del microservicio, incorporando
una estrategia Blue-Green con rollback automático.

## Objetivos Específicos

-   Automatizar la construcción de imágenes Docker.
-   Publicar imágenes en Docker Hub.
-   Reutilizar workflows mediante GitHub Actions.
-   Implementar Blue-Green Deployment.
-   Automatizar rollback.
-   Reducir el riesgo durante despliegues.

# Arquitectura de la Solución

``` mermaid
flowchart TD
A[Desarrollador] --> B[GitHub]
B --> C[GitHub Actions]
C --> D[Lint]
C --> E[Security Audit]
C --> F[Tests]
F --> G[Build Docker]
G --> H[Docker Hub]
H --> I[Kubernetes K3s sobre EC2]
I --> J[Blue]
I --> K[Green]
K --> L[Health Check]
L --> M{¿Correcto?}
M -->|Sí| N[Cambiar tráfico]
M -->|No| O[Rollback]
```

# Estructura del Proyecto

``` text
SharedClient/
 ├── .github/workflows
 ├── k8s
 ├── Dockerfile
 └── package.json

SharedWorkflows/
 └── .github/workflows
```

# Función de los Archivos Principales

  Archivo               Función
  --------------------- -------------------------------
  client.yaml           Ejecuta el pipeline principal
  deploy-api.yaml       Build y Push Docker Hub
  deploy-k8s-api.yaml   Despliegue Kubernetes
  cd-pipeline.yaml      Blue-Green y rollback
  hotfix.yml            Despliegue Hotfix
  test.yml              Lint, Audit y Tests
  blue-green.yaml       Estrategia Blue-Green
  canary.yaml           Estrategia Canary
  rolling-update.yaml   Estrategia Rolling Update

# Herramientas Utilizadas

  Herramienta        Función
  ------------------ ----------------------
  Git                Control de versiones
  GitHub             Repositorio
  GitHub Actions     CI/CD
  Docker             Contenedores
  Docker Hub         Registro
  Kubernetes (K3s)   Orquestación
  Node.js            Aplicación

# Plantillas Reutilizables

Se implementan workflows reutilizables mediante `workflow_call` para
desacoplar la lógica de construcción y despliegue, favoreciendo la
reutilización y el mantenimiento.

# Pipeline CI

-   Instalación de dependencias.
-   ESLint.
-   npm audit.
-   Tests.
-   Build Docker.
-   Publicación en Docker Hub.

# Pipeline CD

1.  Checkout.
2.  Configuración del entorno.
3.  Despliegue Blue-Green.
4.  Health Check.
5.  Cambio de tráfico.
6.  Rollback automático si existe una falla.

# Estrategia de Despliegue

Se seleccionó **Blue-Green Deployment** para minimizar el downtime y
permitir una recuperación inmediata mediante el cambio del Service entre
la versión estable (Blue) y la nueva versión (Green).

# Comparación entre Estrategias

  Estrategia       Disponibilidad   Riesgo     Rollback
  ---------------- ---------------- ---------- ------------
  All-in-Once      Baja             Alto       Difícil
  Rolling Update   Media            Medio      Medio
  Canary           Alta             Bajo       Muy rápido
  Blue-Green       Muy alta         Muy bajo   Inmediato

# Justificación

Blue-Green fue elegida por ofrecer alta disponibilidad, menor riesgo,
validación previa al cambio de tráfico y recuperación rápida.

# Mecanismo de Remediación Automática

``` mermaid
flowchart TD
A[Deploy Green] --> B[Health Check]
B --> C{Resultado}
C -->|OK| D[Cambiar Service]
C -->|Error| E[Rollback]
E --> F[Volver a Blue]
```

# Escenarios de Error

-   CrashLoopBackOff
-   ImagePullBackOff
-   Error HTTP 500
-   Readiness Probe fallida
-   Liveness Probe fallida
-   Error de configuración
-   Imagen inexistente

# Beneficios para el Negocio

-   Mayor disponibilidad.
-   Reducción del MTTR.
-   Menor intervención manual.
-   Automatización del despliegue.
-   Mayor confiabilidad.

# Conclusión

La solución integra prácticas DevOps modernas mediante GitHub Actions,
Docker Hub y Kubernetes, utilizando Blue-Green Deployment y rollback
automático para reducir riesgos y mejorar la continuidad operacional.

# Declaración de Uso de Inteligencia Artificial

Se utilizó Inteligencia Artificial como apoyo para revisión técnica y
redacción. La implementación, validación y adaptación del proyecto
fueron realizadas y verificadas por la autora.

# Referencias

-   GitHub Actions Documentation
-   Docker Documentation
-   Docker Hub Documentation
-   Kubernetes Documentation
-   Node.js Documentation

# Integrante

**Valentina Paz Astudillo Martinez**

------------------------------------------------------------------------

**Asignatura:** Ciclo de Vida del Software II (AUY1104)

**Evaluación:** Evaluación Final Transversal (EFT)

**Institución:** Duoc UC
