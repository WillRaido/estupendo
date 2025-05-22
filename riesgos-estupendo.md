# ⚠️ Riesgos y Omisiones Técnicas Detectadas - Estupendo 2.0

Este documento resume los principales riesgos técnicos, omisiones y preguntas críticas que no están claramente abordadas en el anexo técnico de modernización de la plataforma Esdinamico SAS.

Cada punto incluye el riesgo detectado, su impacto potencial, y preguntas sugeridas para el equipo técnico o de arquitectura.

---

## 1. Riesgo: Monolito Distribuido
**Impacto:** Servicios acoplados sin independencia real.  
**Preguntas:**
- ¿Se han definido los bounded contexts con claridad?
- ¿Qué mecanismos evitarán que servicios accedan a datos ajenos?
- ¿Se comparte el mismo cluster de MongoDB entre múltiples microservicios?

## 2. Acoplamiento en la Base de Datos
**Impacto:** Dificultad para evolucionar el esquema sin romper otros servicios.  
**Preguntas:**
- ¿Se implementará una estrategia de "database per service"?
- ¿Qué servicios serán dueños de qué colecciones?
- ¿Cómo se manejarán migraciones de schema?

## 3. Ambigüedad en CI/CD
**Impacto:** Complejidad operativa y errores en despliegue.  
**Preguntas:**
- ¿Cuál es la herramienta única y oficial para CI/CD?
- ¿Se integrarán GitHub Actions con AWS CodePipeline o se excluyen mutuamente?
- ¿Quién es responsable del pipeline por entorno?

## 4. Falta de Rollbacks Claros
**Impacto:** Alto MTTR ante errores en producción.  
**Preguntas:**
- ¿Qué mecanismos de rollback existen por servicio?
- ¿Se usan estrategias blue/green o canary?

## 5. Observabilidad Limitada
**Impacto:** Dificultad para diagnosticar errores y medir experiencia.  
**Preguntas:**
- ¿Se implementará OpenTelemetry?
- ¿Se usarán herramientas APM como X-Ray o Datadog?
- ¿Existen dashboards específicos por servicio?

## 6. Sin Rastreo Distribuido
**Impacto:** Ceguera operativa en sistemas distribuidos.  
**Preguntas:**
- ¿Se genera trace ID por petición?
- ¿Se conectan logs, métricas y trazas?

## 7. Seguridad Ambigua
**Impacto:** Riesgo de incumplimiento o fuga de datos.  
**Preguntas:**
- ¿Qué algoritmos se usarán para cifrado en Mongo/S3?
- ¿Se utilizará AWS KMS con rotación automática?
- ¿Hay protección contra inyección de dependencias?

## 8. Rate Limiting Incompleto
**Impacto:** Vulnerabilidad ante abuso o DDoS.  
**Preguntas:**
- ¿Cómo se personaliza el rate limit por cliente?
- ¿Existe protección DDoS por capa de red y aplicación?

## 9. Complejidad en Backend for Frontends (BFF)
**Impacto:** Aumento de puntos de fallo.  
**Preguntas:**
- ¿Qué servicios tendrán BFF dedicados?
- ¿Quién es responsable del mantenimiento de los BFFs?
- ¿Qué lógica vive en el BFF y cuál no?

## 10. Reescritura Total del Backend
**Impacto:** Alta probabilidad de errores y pérdida de lógica.  
**Preguntas:**
- ¿Cómo se asegura la paridad funcional con el sistema legacy?
- ¿Qué lógica no documentada puede perderse?
- ¿Qué criterios definen "listo para producción"?

## 11. Pruebas de Carga y Rendimiento
**Impacto:** Riesgo de degradación bajo tráfico real.  
**Preguntas:**
- ¿Qué herramientas se usarán para pruebas de estrés?
- ¿Cuál es el SLA esperado por operación?
- ¿Cómo se validará el objetivo de 2M documentos mensuales?

## 12. Estrategia de Costos Insuficiente
**Impacto:** Escalamiento inesperado de costos.  
**Preguntas:**
- ¿Hay monitoreo activo de costos por servicio?
- ¿Qué alertas existen por exceso de gasto?

## 13. Plan de Desactivación del Legacy
**Impacto:** Coexistencia conflictiva y duplicidad operativa.  
**Preguntas:**
- ¿Qué servicios se migran primero?
- ¿Cómo se migran los datos?
- ¿Qué se hará con la plataforma legacy?

## 14. Sin Plan de Migración de Datos
**Impacto:** Inconsistencias y errores en el nuevo sistema.  
**Preguntas:**
- ¿Cómo se transforman los datos legacy?
- ¿Se usará un pipeline de migración o acceso compartido?

## 15. Riesgo de Acoplamiento por API Gateway
**Impacto:** Un punto de fallo o cuello de botella.  
**Preguntas:**
- ¿Se usarán múltiples Gateways por dominio?
- ¿Qué ocurre si el gateway cae?

---

_Es crucial que cada uno de estos puntos sea abordado con claridad y justificación técnica en el plan de implementación para asegurar una modernización exitosa._
