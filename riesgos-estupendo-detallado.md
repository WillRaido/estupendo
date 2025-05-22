# Riesgos Técnicos Identificados

## 1. Riesgos en la Descomposición de Microservicios y Acoplamiento de Datos

### Riesgo del "Monolito Distribuido"
El anexo menciona una "Descomposición modular en microservicios Anexo de listado de microservicios que será un archivo vivo en el proceso de desarrollo".  
Si bien la flexibilidad es buena, la ausencia de un plan detallado y predefinido de descomposición en esta etapa —especialmente dada la complejidad de los *controladores gordos* en PHP existentes— representa un riesgo arquitectónico crítico.

Sin límites de microservicios claramente definidos, basados en capacidades de negocio y contextos delimitados, existe una alta probabilidad de crear un **"monolito distribuido"**: servicios físicamente separados pero fuertemente acoplados, compartiendo datos inapropiadamente e interdependientes, lo que anula los beneficios clave de los microservicios (despliegue independiente, escalado, heterogeneidad tecnológica).

### Acoplamiento de Base de Datos
Se especifica que *"se mantiene la configuración de Mongo Atlas"* y que los microservicios interactuarán con ella.  
En una verdadera arquitectura de microservicios, se recomienda el patrón de **"una base de datos por servicio"** para garantizar la independencia.  
Una única instancia de MongoDB compartida entre múltiples microservicios, sin límites claros de propiedad de datos, puede llevar a una capa de datos fuertemente acoplada.  
Esto limita la independencia, complica la evolución del esquema y pone en riesgo la consistencia. No se detalla cómo se garantizará la independencia ni cómo se evitará el acceso cruzado entre microservicios.

---

## 2. Ambigüedad y Fragmentación en la Estrategia de CI/CD

### Doble Herramienta de CI/CD
El documento menciona tanto el uso de AWS CodeSuite (CodePipeline, CodeCommit, CodeBuild, CloudFormation) como "Github y action" para el nuevo proyecto.  
Esta ambigüedad, o una posible estrategia dual, puede introducir una complejidad significativa:

- Inconsistencias en pipelines.
- Mayor carga operativa.
- Requisitos más altos de habilidades del equipo.

Una estrategia unificada suele simplificar la operación y mejorar la consistencia.

### Falta de Detalle en Rollbacks
Aunque se menciona la automatización de despliegues, no se detallan los procedimientos de rollback.  
En un entorno de microservicios, **la reversión rápida ante errores en producción es crítica**.  
La Infraestructura como Código (CloudFormation) es valiosa para recuperación de desastres, pero no reemplaza una estrategia de rollback bien definida a nivel de aplicación.

---

## 3. Observabilidad Insuficiente para Microservicios

### Monitoreo Básico
Amazon CloudWatch es adecuado para métricas básicas (CPU, memoria, latencia del ALB) y logs, pero **la complejidad de microservicios requiere una observabilidad más profunda**.  
Los logs centralizados y métricas básicas son insuficientes para comprender flujos de transacción de extremo a extremo.

### Omisión de Rastreo Distribuido y APM
No se mencionan planes para rastreo distribuido (e.g., AWS X-Ray, OpenTelemetry) ni soluciones de APM.  
Estas herramientas son esenciales para:

- Ver cómo se propagan las solicitudes.
- Detectar cuellos de botella.
- Identificar causas raíz rápidamente.

Su ausencia complica la resolución de problemas y el mantenimiento del rendimiento en sistemas distribuidos como *Estupendo 2.0*.

---

## 4. Detalles de Seguridad Incompletos

### Cifrado de Datos Sensibles
Aunque se menciona el cifrado "en tránsito y en reposo", faltan detalles clave:

- Algoritmos de cifrado usados (MongoDB Atlas, S3, EBS).
- Estrategia de gestión de claves (ej. AWS KMS).

En una plataforma que maneja información fiscal y de nómina bajo regulación de la DIAN, **la falta de detalles puede ocultar vulnerabilidades o riesgos de cumplimiento**.

### Rate Limiting y Protección DDoS
Se propone un límite de 1000 solicitudes por minuto por cliente para las APIs, pero:

- ¿Es suficiente o demasiado rígido?
- No se aborda como parte de una estrategia integral contra DDoS.

**Un límite fijo es insuficiente** para proteger una plataforma crítica. Se requiere una estrategia defensiva más completa y dinámica.

---

## 5. Complejidad de la Capa Backend for Frontends (BFF)

### Implementación y Gestión de BFFs
Se menciona la inclusión de una capa BFF (Backend for Frontends), pero no se profundiza en:

- Su implementación técnica.
- La gestión de versiones.
- Cómo se aislará la lógica de presentación.

Dada la función crítica del BFF en la orquestación entre frontend y microservicios, una planificación incompleta puede generar cuellos de botella, redundancia o aumento de latencia.

---
