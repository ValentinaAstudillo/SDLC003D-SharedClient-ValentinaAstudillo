# SDLC003D - Shared Client CI/CD

Aplicación de ejemplo desarrollada en **Node.js** y **Express**, utilizada para la implementación y validación de una arquitectura **CI/CD basada en GitHub Actions**, Docker y workflows reutilizables.

## Objetivo

Este repositorio corresponde al componente **Cliente** de la solución CI/CD. Su responsabilidad es:

* Ejecutar validaciones automáticas en ramas de desarrollo.
* Consumir workflows reutilizables definidos en un repositorio central.
* Construir y publicar imágenes Docker mediante tags versionados.
* Mantener separación entre procesos de validación y release.

## Arquitectura

La solución está compuesta por dos repositorios:

### Repositorio Cliente

`SDLC003D-SharedClient-ValentinaAstudillo`

Contiene:

* Código fuente de la aplicación.
* Pruebas automatizadas.
* Dockerfile.
* Workflow de validación (`test.yml`).
* Workflow consumidor (`client.yaml`).

### Repositorio Central

`SDLC003D-SharedWorkflows-ValentinaAstudillo`

Contiene:

* Workflow reutilizable (`deploy-api.yaml`).
* Lógica centralizada para pruebas, construcción y publicación Docker.

## Pipelines Implementados

### Pipeline de Validación

Archivo:

```text
.github/workflows/test.yml
```

Se ejecuta cuando existe un push en ramas distintas de `main`.

Acciones realizadas:

1. Checkout del repositorio.
2. Instalación de dependencias.
3. Ejecución de pruebas automatizadas.
4. Validación de construcción Docker mediante:

```bash
docker build -t validacion-local:${{ github.sha }} .
```

Este pipeline no publica imágenes y permite validar cambios antes del release.

### Pipeline de Release

Archivo:

```text
.github/workflows/client.yaml
```

Se ejecuta únicamente mediante tags versionados:

```text
v*.*.*
```

Acciones realizadas:

1. Invocación de workflow reutilizable central.
2. Instalación de dependencias.
3. Ejecución de pruebas.
4. Construcción de imagen Docker.
5. Publicación en Docker Hub.

## Ejecución Local

Instalar dependencias:

```bash
npm install
```

Ejecutar aplicación:

```bash
npm start
```

La aplicación queda disponible en:

```text
http://localhost:3000
```

## Docker

Construir imagen:

```bash
docker build -t demo-api .
```

Ejecutar contenedor:

```bash
docker run -p 3000:3000 demo-api
```

## Endpoints

| Método | Endpoint    | Descripción                      |
| ------ | ----------- | -------------------------------- |
| GET    | /health     | Verifica estado de la aplicación |
| GET    | /api/saludo | Devuelve saludo en formato JSON  |
| POST   | /api/echo   | Retorna el contenido enviado     |

## Versiones

Tags utilizados:

* v1.0.0
* v1.0.1
* v1.0.2
* v1.0.3

Release actual:

**v1.0.3**

## Tecnologías

* Node.js
* Express
* Docker
* GitHub Actions
* Docker Hub

## Autor

Valentina Astudillo

Evaluación - CI/CD Pipeline & Validación
