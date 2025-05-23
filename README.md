# CI/CD Pipeline con Buildah y SAST

Este pipeline define un flujo de trabajo automatizado utilizando Buildah como herramienta de construcción de imágenes sin demonio y análisis de seguridad con SAST (Static Application Security Testing).

## Etapas del Pipeline

### 1. `setup`
- Define la etiqueta (`TAG`) según la rama (`develop`, `qa`, `master` o cualquier otra).
- Guarda la etiqueta en el archivo `.env` para ser usada en otras etapas.

### 2. `build`
> *(Actualmente no definido en el script provisto, se espera que se agregue la lógica para construir contenedores con Buildah aquí).*
- Debería incluir la construcción de la imagen usando Buildah.

### 3. `deploy`
> *(Actualmente no definido en el script provisto, se espera que se agregue la lógica de despliegue).*
- Posible uso de scripts personalizados para desplegar la imagen construida.

### 4. `sast`
- Contiene configuraciones de análisis estático del código fuente.
- Utiliza plantillas e imágenes de analizador GitLab como `gitlab-advanced-sast` y `semgrep`.
- Subida de reportes a S3 si está configurado.

## Variables de Entorno

| Variable | Descripción |
|---------|-------------|
| `VERSION` | Versión del artefacto generada dinámicamente. |
| `STORAGE_DRIVER` | Define el controlador de almacenamiento de Buildah. |
| `BUILDAH_ISOLATION` | Modo de aislamiento de Buildah (por ejemplo, `chroot`). |
| `SECURE_ANALYZERS_PREFIX` | Ruta base para las imágenes de analizadores de seguridad. |
| `SAST_IMAGE_SUFFIX` | Sufijo para las imágenes de analizadores SAST. |
| `SAST_EXCLUDED_ANALYZERS` | Lista de analizadores SAST a excluir. |
| `DEFAULT_SAST_EXCLUDED_PATHS` | Rutas por defecto que no se analizan. |
| `SCAN_KUBERNETES_MANIFESTS` | Indica si se escanean manifiestos de Kubernetes. |

## Reglas por Rama

| Rama | TAG |
|------|-----|
| `develop` | develop |
| `qa` | qa |
| `master` | versión basada en variables `VERSION` |
| Cualquier otra | latest |

## Análisis Avanzado con GitLab

- Se incluye el analizador `gitlab-advanced-sast`, habilitado solo si las condiciones de la rama y tipo de archivo lo permiten.
- Se emplea la imagen configurada por `SAST_ANALYZER_IMAGE`.

## Reportes de Seguridad

- El reporte `gl-sast-report.json` es generado en la etapa de `sast`.
- Es subido a un bucket S3 si las variables AWS están configuradas.

## Requisitos para Ejecutar el Pipeline

- Runner con permisos de Buildah y `docker:dind` si se desea integración con Docker.
- Acceso al registro de contenedores (`CI_REGISTRY_USER` y `CI_REGISTRY_PASSWORD`).
- Permisos AWS configurados si se suben reportes a S3 (`DBP_S3_AWS_ACCESS_KEY`, `DBP_S3_AWS_SECRET_KEY`).

## Seguridad

- El pipeline incluye medidas de análisis estático de código para evitar introducir vulnerabilidades de seguridad desde etapas tempranas.
- Admite personalización de exclusión de analizadores y rutas.

---

> **Nota:** Las etapas de `build` y `deploy` están definidas como parte del flujo pero no tienen configuración detallada en este archivo. Se recomienda implementar dichas etapas según el caso de uso del proyecto.
