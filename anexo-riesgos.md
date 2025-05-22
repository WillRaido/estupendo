# 📋 Análisis Crítico de Riesgos y Omisiones - Proyecto Estupendo 2.0

Este documento identifica aspectos técnicos y estratégicos que no están claramente definidos en el anexo técnico y podrían representar riesgos serios para el éxito del proyecto.

---

## 1. Riesgos en la Descomposición de Microservicios y Acoplamiento de Datos

### 🔶 Riesgo del "Monolito Distribuido"
El anexo propone una descomposición modular viva, sin un plan predefinido. Esto es riesgoso dada la complejidad de los controladores PHP existentes. Sin delimitaciones claras, los microservicios podrían quedar acoplados, compartiendo datos y anulando ventajas como escalabilidad y despliegue independiente.

### 🔶 Acoplamiento de Base de Datos
Se mantiene una única instancia MongoDB compartida. En microservicios, se recomienda una base por servicio. La falta de independencia en los datos compromete la autonomía de los servicios y su evolución.

---

## 2. Ambigüedad y Fragmentación en la Estrategia de CI/CD

### 🔶 Doble Herramienta de CI/CD
El uso conjunto de CodeSuite y GitHub Actions sin una estrategia clara puede generar inconsistencias en los despliegues y aumentar la complejidad operativa.

### 🔶 Falta de Detalle en Rollbacks
No se especifican estrategias de rollback ante fallos. En microservicios, revertir despliegues problemáticos es esencial. CloudFormation ayuda con infraestructura, pero no cubre el rollback de aplicaciones.

---

## 3. Observabilidad Insuficiente para Microservicios

### 🔶 Monitoreo Básico
Se mencionan métricas básicas con CloudWatch. Sin embargo, los sistemas distribuidos necesitan rastreo transaccional completo para detectar cuellos de botella y problemas de integración.

### 🔶 Omisión de Rastreo Distribuido y APM
No se considera X-Ray u OpenTelemetry. Estas herramientas son clave para trazabilidad, diagnóstico de latencia y resolución eficiente de errores en entornos complejos.

---

## 4. Detalles de Seguridad Incompletos

### 🔶 Cifrado de Datos Sensibles
Se menciona "cifrado en tránsito y en reposo", pero sin detallar algoritmos, cifrado en Mongo/S3/EBS ni uso de AWS KMS. La falta de precisión en seguridad es riesgosa frente a normativas.

### 🔶 Rate Limiting y Protección DDoS
El límite fijo de 1000 req/min es inflexible y puede ser inadecuado. No se integra con una estrategia DDoS robusta (WAF, Shield). Esto deja vulnerable una plataforma crítica.

---

## 5. Complejidad de la Capa Backend for Frontends (BFF)

El uso de BFF es correcto, pero se omiten sus responsabilidades exactas y cómo se evitará que se convierta en un cuello de botella. No se detalla si se implementa como microservicio separado o integrado en el Gateway.

---

## 6. Reescritura Completa de la Lógica de Negocio

Migrar de PHP a Python/Node implica reescribir toda la lógica. No es refactorización, es reconstrucción. Sin estrategia de pruebas y paridad funcional, es alto el riesgo de errores, omisiones y retrasos.

---

## 7. Falta de Detalle en Pruebas de Rendimiento y Carga

Se mencionan metas como 2M de documentos mensuales y disponibilidad en <5s, pero no se detallan pruebas de rendimiento, estrés o validaciones que aseguren el cumplimiento en producción.

---

## 8. Gestión de Costos y Optimización

Aunque se menciona auto-scaling y reservas, no hay estrategia de control proactivo de costos ni un modelo de análisis por servicio, región o uso. Esto puede hacer que los costos escalen sin visibilidad.

---

## 9. Plan de Desactivación del Sistema Legado

No se menciona cómo será la transición del sistema PHP. ¿Coexistirá un tiempo? ¿Cómo se migran o acceden datos históricos? La falta de planificación puede llevar a inconsistencias y duplicidades operativas.

---

## ✅ Conclusión

La propuesta de Estupendo 2.0 va en buena dirección, pero debe reforzarse en detalles críticos para evitar riesgos serios. Una hoja de ruta clara, pruebas exhaustivas, seguridad avanzada y un modelo de costos realista son claves para el éxito.

