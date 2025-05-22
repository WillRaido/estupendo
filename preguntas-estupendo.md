# 🧠 Análisis Técnico Avanzado - Plataforma Esdinamico SAS v2.0

Este documento contiene **30+ observaciones técnicas y estratégicas** formuladas desde tres enfoques:  
- Arquitectura de Software  
- Arquitectura en AWS  
- DevOps & Observabilidad

---

## 🔍 1–10: Preguntas desde la Arquitectura de Software

1. **¿Está claramente definido el Bounded Context de cada microservicio?**  
   → Para evitar dependencias implícitas y duplicación lógica.

2. **¿Se aplicará DDD (Domain Driven Design) o una variación pragmática?**  
   → Especialmente útil para dominios como nómina y emisión que son complejos.

3. **¿Qué patrón de resiliencia se implementará? (Retry, Circuit Breaker, Fallback)**  
   → Para controlar fallos de servicios como la DIAN.

4. **¿Cómo se manejará la trazabilidad completa de un documento desde UI hasta DIAN?**  
   → Esencial para auditoría y soporte.

5. **¿Habrá una capa de BFF (Backend for Frontend) específica o lógica de agregación compartida?**  
   → Importante para desacoplar el frontend del dominio real.

6. **¿Cuál será la política de versionado de APIs?**  
   → REST: `/v1`, gRPC: semántico con tags proto.

7. **¿Qué estrategia se aplicará para el schema evolution en MongoDB?**  
   → Si es necesario usar Mongoose o validadores JSON Schema.

8. **¿Se utilizará algún patrón de comunicación asincrónica? (Event-driven, Outbox)**  
   → Especialmente entre microservicios críticos como nómina y reportes.

9. **¿Cómo se mantendrá la consistencia entre bases de datos distribuidas (si aplica)?**  
   → ¿Habrá eventual consistency y uso de patrones Saga?

10. **¿Qué criterios definen cuándo usar gRPC vs REST?**  
    → ¿REST para integraciones externas, gRPC para internos?

---

## ☁️ 11–20: Preguntas desde Arquitectura AWS

11. **¿Se usarán ECS Fargate o EC2?**  
    → ¿Costos y necesidades de escalabilidad justifican Fargate?

12. **¿El ALB manejará tráfico HTTP/2 con gRPC habilitado?**  
    → Importante para performance y multiplexado.

13. **¿Cuáles son las zonas de disponibilidad utilizadas y su cobertura geográfica?**  
    → ¿Está todo en us-east-1 o se contempla multi-región?

14. **¿Se han definido correctamente las políticas de IAM para cada tarea ECS?**  
    → Mínimos privilegios, roles por servicio.

15. **¿Hay NAT Gateway redundante para salidas seguras a internet desde VPC privadas?**  
    → Costoso pero necesario para resiliencia.

16. **¿Secrets Manager tiene rotación automática habilitada?**  
    → Validar caducidad y alertamiento.

17. **¿Existe integración con Amazon Cognito o federación con otro IdP?**  
    → Autenticación moderna y control granular.

18. **¿Están habilitados los registros VPC Flow Logs?**  
    → Esenciales para auditoría y diagnóstico de red.

19. **¿Se utilizará CloudTrail + Config para auditoría de infraestructura?**  
    → Para trazabilidad y cumplimiento normativo.

20. **¿Qué métricas personalizadas se exportarán a CloudWatch?**  
    → ¿Hay dashboards por microservicio, alerta por SLIs?

---

## ⚙️ 21–30: Preguntas desde DevOps, Observabilidad y CI/CD

21. **¿Qué herramientas de CI/CD se usarán y cómo se dividirán los pipelines?**  
    → ¿Separación por servicio o monorepo?

22. **¿Cada push a master genera despliegue automático en qué entorno? (Dev, QA, Prod)**  
    → ¿Hay validación manual para producción?

23. **¿Se usan pruebas de contrato automatizadas para los servicios gRPC?**  
    → Protoculture, Buf o similar.

24. **¿Qué herramientas de escaneo se integrarán? (Snyk, SonarQube, Checkov)**  
    → Seguridad y calidad del código.

25. **¿Qué estrategia de rollback se usará en ECS?**  
    → ¿Revisión por health checks, blue/green, canary?

26. **¿Se utilizan etiquetas y naming convention por entorno y tipo de recurso?**  
    → Fundamental para gobernanza y billing.

27. **¿Existe un proceso de aprobación de infraestructura como código?**  
    → Pull Request + revisión manual en Terraform/CloudFormation.

28. **¿OpenTelemetry está habilitado para trazabilidad distribuida entre servicios?**  
    → Correlación completa con trace ID.

29. **¿Hay métricas SLO por servicio (ej. latencia < 300ms, uptime > 99.9%)?**  
    → Necesario para alertamiento proactivo.

30. **¿Se está utilizando alguna solución de error tracking centralizada? (Sentry, Rollbar, etc.)**  
    → Reducción MTTR y mejora continua.

---

_Generado como base para cuestionamiento técnico profundo previo a ejecución de Estupendo 2.0._
