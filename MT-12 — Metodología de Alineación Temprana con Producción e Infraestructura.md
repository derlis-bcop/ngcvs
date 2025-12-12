# MT-12 — MANUAL TÉCNICO DE ALINEACIÓN TEMPRANA CON PRODUCCIÓN E INFRAESTRUCTURA
**Manual Técnico — Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: NGCVS — Sección 12

---

# 1. Propósito Operativo
Definir el proceso técnico obligatorio que deben seguir todos los proyectos tecnológicos para alinearse desde etapas tempranas con Producción e Infraestructura, garantizando:

- Operabilidad y estabilidad del servicio  
- Cumplimiento normativo  
- Despliegues seguros y auditables  
- Observabilidad completa desde la primera versión  
- Prevención de riesgos técnicos y operativos  

Este manual operacionaliza la Sección 12 de la NGCVS.

---

# 2. Alcance
Este manual aplica a:

- Proyectos nuevos  
- Evolutivos relevantes  
- Reingenierías  
- Sustitución de plataformas  
- Integraciones con sistemas críticos  
- Migraciones tecnológicas  

Incluye todos los artefactos: backend, frontend, APIs, contenedores, batch, pipelines, bases de datos y servicios externos.

---

# 3. Proceso General de Alineación Temprana

## 3.1 Fase 1 — Idea (Kickoff Técnico Inicial)
El equipo debe convocar obligatoriamente a:

- Producción / Infraestructura  
- Arquitectura  
- Seguridad TICs  
- Desarrollo  
- QA  

### Actividades:
- Evaluación preliminar de viabilidad técnica  
- Validación preliminar de necesidades de infraestructura  
- Identificación temprana de riesgos  
- Definición inicial de ambientes necesarios  
- Primer borrador del modelo de despliegue  

Documentación generada:
- CETI (Checklist de Elegibilidad Técnica Inicial)  
- Registro de riesgos  
- Aprobación de continuidad del proyecto  

---

## 3.2 Fase 2 — Diseño Técnico
Producción participa obligatoriamente en:

- Diseño de arquitectura  
- Definición de contenedores, imágenes y recursos  
- Diseño de despliegue (blue/green, rolling, canary)  
- Configuración de redes, puertos, DNS, balanceadores  
- Estrategias de escalamiento y resiliencia  
- Estándares de logging, métricas, health checks y telemetría  

Salidas obligatorias:
- Documento de Arquitectura Técnica aprobado  
- Validación operativa de infraestructura  
- Diseño formal de despliegue y rollback  

---

## 3.3 Fase 3 — Desarrollo
El equipo de Desarrollo debe interactuar continuamente con Producción para validar:

- Dockerfiles / imágenes OCI  
- Pipelines CI/CD  
- Uso del registry corporativo  
- Instrumentación de health checks  
- Instrumentación de métricas y telemetría  
- Manejo de Secrets Management  
- Control de dependencias y vulnerabilidades  

Salidas obligatorias:
- Pipeline validado  
- Artefacto reproducible  
- Evidencias técnicas almacenadas  

---

## 3.4 Fase 4 — QA / UAT
Producción valida:

- Monitoreo funcional y técnico  
- Dashboards y alertas  
- Telemetría operativa  
- Performance y consumo estimado  
- Estrategia de recuperación ante fallos  

Salidas:
- Validación operativa QA/UAT  
- Checklist técnico intermedio  

---

## 3.5 Fase 5 — Pre-Aprobación de Despliegue
Requisito **obligatorio** para PRE-PROD y PROD.

Producción/Infraestructura revisa:

- Evidencias CI/CD completas  
- Resultados de pruebas  
- Cumplimiento de Sub-Políticas  
- Estrategias de rollback y monitoreo  
- Contenedores y artefactos finales  
- Configuración de redes y políticas de seguridad  

Si algo falla → **veto automático del despliegue**.

---

## 3.6 Fase 6 — Despliegue Operativo (PRE-PROD / PROD)
Producción participa en:

- Procedimiento técnico de despliegue  
- Validación de readiness y liveness  
- Validación de monitoreo activo  
- Validación de telemetría  
- Validación de seguridad operativa  

Registro obligatorio post-despliegue:
- Estado final  
- Anomalías  
- Métricas de estabilización  
- Validación del rollback  

---

# 4. Requisitos Técnicos Obligatoros

## 4.1 Artefactos
- Reproducibles  
- Versionados  
- Sin secretos  
- Analizados (SAST / dependencias)  
- Con SBOM  

## 4.2 CI/CD
- Pipelines aprobados por Producción y Seguridad  
- Validaciones de health checks  
- Validaciones de contenedores  
- Evidencias archivadas  

## 4.3 Observabilidad
Todo proyecto debe incluir:

- Métricas técnicas  
- Métricas de producto  
- Logs estructurados  
- Trazabilidad distribuida  
- Alertas críticas  
- Dashboards mínimos  

## 4.4 Operabilidad
Debe existir:

- Procedimiento de despliegue aprobado  
- Procedimiento de rollback  
- Mapa de dependencias  
- Flujos de red autorizados  
- Documentación operativa  

---

# 5. Anti-patrones Operativos (Prohibido)

- Contactar a Producción solo al final del proyecto  
- Desarrollar sin atender restricciones operativas  
- Desplegar sin observabilidad  
- Realizar releases sin pre-aprobación  
- Utilizar contenedores sin validación técnica  
- Cambiar flujos de red sin autorización  
- Realizar bypass del pipeline  

---

# 6. Vigencia y Revisiones
Este manual debe revisarse **anualmente** por:

- Arquitectura TICs  
- Producción / Infraestructura  
- Seguridad TICs  

---

# FIN DEL MANUAL TÉCNICO MT-12
