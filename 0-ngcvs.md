# NORMA GENERAL DE CICLO DE VIDA DE SOFTWARE (NGCVS)  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**  
**Nivel Normativo 0**

## 1. Objetivo
La presente Norma General de Ciclo de Vida de Software (NGCVS) establece el marco normativo obligatorio que regula todas las actividades de diseño, desarrollo, aseguramiento de calidad, integración, despliegue, operación, monitoreo y mantenimiento de software dentro de la Gerencia de TICs.  
Su objetivo es garantizar la calidad, seguridad, trazabilidad, confiabilidad y gobernanza técnica de todo artefacto de software, independientemente de su origen, complejidad o criticidad.

## 2. Alcance
Esta norma aplica a:
- Desarrollos internos, externos, contratados, evolutivos, correctivos y preventivos.
- Componentes backend, frontend, móviles, integraciones, bases de datos, microservicios, automatizaciones, pipelines y cualquier artefacto tecnológico que se ejecute, interactúe o dependa de infraestructura de la Gerencia TICs.
- Todo el personal de Desarrollo, QA, Arquitectura, Seguridad, Producción/Infraestructura y proveedores externos.

La NGCVS posee **carácter normativo interno dentro de la Gerencia de TICs** y es de cumplimiento obligatorio.

## 3. Definiciones
- **Artefacto:** Cualquier producto generado en el ciclo de vida del software (código, binarios, scripts, pipelines, configuraciones, contenedores, etc.).
- **Ciclo de Vida de Software:** Conjunto de procesos desde la concepción de una idea hasta su retiro definitivo.
- **Ambientes:** DEV, QA, UAT, PRE-PROD y PROD, definidos formalmente y con reglas de admisión.
- **Sub-Política:** Documento normativo técnico de Nivel 1 que desarrolla una dimensión específica del ciclo de vida.
- **Manual Técnico:** Documento operativo de Nivel 2 con instrucciones, configuraciones y estándares mínimos.
- **Checklist:** Herramienta de auditoría operativa (Nivel 3) que determina admisibilidad y cumplimiento.
- **Evidencia:** Registro verificable que prueba la correcta ejecución de procesos exigidos por la NGCVS.

## 4. Jerarquía Normativa
1. **Nivel 0 – NGCVS** (documento rector)
2. **Nivel 1 – Sub-Políticas Técnicas** (normativas y obligatorias)
3. **Nivel 2 – Manuales Técnicos** (operativos y detallados)
4. **Nivel 3 – Checklists** (verificación y auditoría)

Ningún documento de nivel inferior puede contradecir ni modificar uno de nivel superior.

## 5. Responsabilidades

### 5.1 Gerencia de TICs
- Aprobar, modificar y revocar la NGCVS, Sub-Políticas y Manuales.
- Resolver excepciones de cumplimiento cuando corresponda.
- Mantener un sistema de auditoría anual.

### 5.2 Desarrollo
- Cumplir la NGCVS y todas las Sub-Políticas aplicables.
- Generar código versionado, trazable y con estándares técnicos.
- Gestionar evidencias en CI/CD.
- Asegurar calidad técnica previa a entregar a QA.

### 5.3 QA
- Validar cumplimiento normativo antes de promover artefactos a UAT.
- Ejecutar pruebas funcionales, no funcionales y de regresión.
- Verificar evidencias almacenadas en los pipelines CI/CD.

### 5.4 Producción/Infraestructura
- Administrar ambientes y autorizar o vetar accesos a PRE-PROD y PROD.
- Evaluar elegibilidad técnica mediante el Checklist CETI.
- Verificar configuración de despliegues, monitoreo, health checks y rollback.
- Garantizar monitoreo 24/7.

### 5.5 Arquitectura y Seguridad Informática
- Definir estándares, patrones y lineamientos técnicos obligatorios.
- Evaluar riesgos de seguridad, dependencias y vulnerabilidades.
- Aprobar modelos de despliegue, integraciones y uso de tecnologías.

## 6. Proceso Formal de Gestión Normativa
### 6.1 Repositorio Único
Todos los documentos normativos deben almacenarse en un repositorio central administrado por la Gerencia TICs.

### 6.2 Versionado Formal
Toda modificación debe:
- Poseer número de versión semántico.
- Contener registro de cambios.
- Mantener historial auditable.

### 6.3 Ciclo de Vida Documental
- **Alta:** creación y aprobación formal.
- **Modificación:** cambios gestionados mediante solicitud de actualización.
- **Baja:** revocación o reemplazo mediante documento superior.

### 6.4 Auditoría Anual Obligatoria
La Gerencia TICs auditará:
- Uso correcto de políticas y manuales.
- Cumplimiento de checklists.
- Evidencias de CI/CD.

## 7. Régimen de Cumplimiento y Admisión a Ambientes Productivos

### 7.1 Principio General
Ningún artefacto podrá avanzar de ambiente sin cumplir:
- La NGCVS.
- Las Sub-Políticas Técnicas obligatorias.
- Los Manuales Técnicos vinculados.
- Los Checklists correspondientes.

### 7.2 Poder de Veto
El equipo de Producción/Infraestructura puede **denegar la admisión a PRE-PROD o PROD** si:
- Falta cumplimiento normativo.
- No existen evidencias técnicas en CI/CD.
- Existen riesgos operativos, de disponibilidad o seguridad.

### 7.3 Evidencias obligatorias
Deben quedar registradas en CI/CD:
- Resultados de pruebas.
- Análisis de seguridad (SAST, dependencias).
- Validaciones de linting y estándares.
- Artefactos generados y su hash de integridad.
- Resultados de health checks previos.
- Verificación de rollback.

## 8. Metodología de Alineación Temprana con Producción e Infraestructura

### 8.1 Participación desde la Fase de Idea
Todo proyecto debe iniciar con:
- Revisión temprana por Producción.
- Análisis de viabilidad operativa.
- Checklist CETI (Checklist de Elegibilidad Técnica Inicial).

### 8.2 Validaciones Continuas
Producción participa en:
- Diseño de arquitectura.
- Estrategia de despliegue y rollback.
- Plan de monitoreo, alertas y health checks.
- Estrategias de contingencia.

### 8.3 Pre-Aprobación de Despliegue
Previo al primer despliegue en PRE-PROD:
- Debe existir pre-aprobación firmada.
- Debe validarse que el pipeline esté completo y auditable.

## 9. Requisitos Técnicos Obligatorios

### 9.1 Control de Versiones
- Uso obligatorio de **SVN** como repositorio corporativo.
- Estructura estándar, branching model obligatorio.
- Prohibición de ambientes paralelos no versionados.

### 9.2 CI/CD Obligatorio
Todo software **debe**:
- Contar con pipeline CI/CD.
- Registrar evidencias.
- Ejecutar pruebas, validadores internos, análisis de seguridad y despliegues automatizados.

### 9.3 Validador In-House
Uso obligatorio para:
- Linting.
- Estandarización de código.
- Formato de commits.
- Verificación estructural del proyecto.

### 9.4 Control de Dependencias y Vulnerabilidades
- Escaneo obligatorio por cada build.
- Prohibición de dependencias obsoletas o de alto riesgo.

### 9.5 Health Checks
- Cada artefacto debe exponer endpoint o mecanismo de health check.
- Debe incluir readiness y liveness probes.

### 9.6 Logging Estructurado
- Obligatorio para todos los sistemas.
- Formato JSON cuando aplique.

### 9.7 Monitoreo Centralizado
- Integración obligatoria con plataforma institucional.
- Alertas automáticas basadas en SLIs y SLOs.

### 9.8 Ambientes Segregados
Los ambientes DEV, QA, UAT, PRE-PROD y PROD deben:
- Ser físicamente y lógicamente separados.
- Contar con reglas de acceso diferenciadas.

### 9.9 Rollback Formal
- Plan de rollback obligatorio antes de cada despliegue.
- Evidencia del procedimiento probada en PRE-PROD.

### 9.10 Validaciones de Seguridad
En cada pipeline deben ejecutarse:
- SAST.
- Análisis de dependencias.
- Peer review obligatorio.

### 9.11 Versiones Mínimas
Cada proyecto deberá declarar explícitamente:
- Versión mínima de lenguaje.
- Versión mínima de servidor de aplicaciones.
- Versión mínima de sistema operativo.
- Versión mínima soportada de Oracle.

## 10. Recomendación Estratégica: Contenedores Docker
La Gerencia de TICs recomienda, sin carácter obligatorio:
- La contenedorización mediante Docker.
- La adopción progresiva de prácticas alineadas a Kubernetes.
- Estandarización de despliegues, monitoreo y escalabilidad.

## 11. Anexos
Los anexos incluyen:
- Glosario.
- Modelos de evidencias.
- Formatos para excepciones normativas.
- Templates de arquitectura.

## 12. Metodología de Alineación Temprana con Producción e Infraestructura

La presente metodología define el modelo obligatorio de interacción anticipada entre los equipos de Desarrollo, QA, Arquitectura, Seguridad TICs y Producción/Infraestructura, aplicable a todo proyecto tecnológico administrado por la Gerencia de TICs.  
Su incumplimiento constituye causal suficiente para impedir la continuación del proyecto o bloquear su promoción a ambientes superiores.

---

### 12.1 Principio General

Todo proyecto, iniciativa, evolución o corrección mayor debe integrar, desde su concepción, la participación formal de Producción e Infraestructura con el fin de garantizar:

- Operabilidad  
- Mantenibilidad  
- Observabilidad  
- Seguridad  
- Integración correcta a la plataforma institucional  
- Cumplimiento normativo y de ciclo de vida  

Esta participación **no es delegable** y debe quedar documentada.

---

## 12.2 Participación desde la Fase de Idea (Obligatorio)

Todo proyecto deberá convocar a Producción/Infraestructura **antes de redactar requerimientos funcionales o diseños técnicos**, para evaluar:

1. **Viabilidad operativa**  
2. **Compatibilidad con estándares institucionales**  
3. **Impacto en infraestructuras actuales**  
4. **Necesidad de nuevos recursos (CPU, RAM, storage, redes, balanceadores)**  
5. **Requerimientos de despliegue, monitoreo y recuperación**  
6. **Dependencias externas críticas**  

La Gerencia de TICs no aprobará ningún proyecto que no cuente con esta validación temprana.

---

## 12.3 Validaciones Continuas durante el Ciclo de Vida

Producción e Infraestructura deben revisar y aprobar, en todas las etapas del ciclo de vida:

### Fase de Diseño Técnico
- Arquitectura de despliegue  
- Flujos de red (entrante / saliente)  
- Configuración de ambientes  
- Modelo de logging, monitoreo y métricas  
- Requirements de health checks  
- Política de rollback  

### Fase de Desarrollo
- Contenedores e imágenes (si aplica)  
- Scripts de despliegue  
- Pipelines CI/CD  
- Versionado del artefacto  
- Dependencias y riesgos detectados  

### Fase de QA / UAT
- Validaciones operativas  
- Cargas esperadas  
- Escalabilidad  
- Telemetría  
- Alertas y dashboards  

### Fase de PRE-PROD
- Pruebas de smoke operativas  
- Validación de readiness de monitoreo  
- Aprobación de seguridad técnica  

---

## 12.4 Pre-aprobación de Despliegue (Gate Operativo Obligatorio)

Antes de iniciar cualquier despliegue hacia PRE-PROD o PROD, Producción debe emitir una **Pre-Aprobación de Despliegue**, la cual valida:

1. Conformidad técnica del artefacto  
2. Evidencias completas en CI/CD  
3. Cumplimiento de Sub-Políticas de Nivel 1  
4. Cumplimiento de Manuales Técnicos de Nivel 2  
5. Checklists de Nivel 3 aprobados  
6. Ausencia de riesgos operativos no mitigados  
7. Correcta configuración de health checks  
8. Correcta configuración de monitoreo, métricas y alertas  
9. Correcto procedimiento de rollback  

### Sin esta pre-aprobación:
**El despliegue queda automáticamente bloqueado** independientemente de aprobaciones funcionales.

---

## 12.5 Participación de Producción en Diseño de Despliegue, Monitoreo, Health Checks y Rollback

### Producción debe aprobar obligatoriamente:
- Esquema de despliegue (blue/green, rolling, canary, pipeline institucional)  
- Parámetros de release: ventanas, lotes, horarios y responsables  
- Estrategias de recuperación  
- Health checks (`/live`, `/ready`, `/health`)  
- Estándares de logging técnico  
- Dashboards obligatorios (latencia, disponibilidad, errores, métricas de producto)  
- Alertas críticas  

### Producción puede vetar el despliegue si:
- Se detectan fallas en instrumentación  
- No se cumple seguridad operativa  
- El sistema no es observable  
- No se garantiza rollback en menos de X minutos (definido institucionalmente)  
- No existe documentación clara de despliegue y recuperación  

---

# 12.6 Checklist CETI — Checklist de Elegibilidad Técnica Inicial (Obligatorio)

El CETI debe completarse antes de iniciar cualquier proyecto y constituye el **primer gate técnico obligatorio**.

Un proyecto **no podrá iniciar** si el CETI no está aprobado por:

- Arquitectura  
- Seguridad TICs  
- Producción/Infraestructura  
- Equipo de Desarrollo  

El CETI valida:

### A) Gobernanza y Cumplimiento
- [ ] Proyecto registrado formalmente  
- [ ] Roles definidos  
- [ ] Artefactos serán versionados en SVN  
- [ ] CI/CD será utilizado obligatoriamente  
- [ ] Se aplicarán Sub-Políticas y Manuales institucionales  

### B) Operabilidad mínima
- [ ] Se definieron requerimientos de capacidad  
- [ ] Se identificaron dependencias críticas  
- [ ] Se definió estrategia preliminar de despliegue  
- [ ] Se definió estrategia preliminar de rollback  
- [ ] Se definió estrategia de monitoreo  
- [ ] Se definió estrategia de health checks  
- [ ] Se definió estrategia de métricas y telemetría  

### C) Seguridad
- [ ] Aplicación cumple lineamientos de acceso, autenticación y permisos  
- [ ] Se identifican flujos sensibles  
- [ ] Requiere revisión de datos personales o confidenciales  
- [ ] Se definió estrategia de Secrets Management  

### D) Infraestructura
- [ ] Evaluación de soporte y compatibilidad  
- [ ] Requerimientos de red (entradas/salidas)  
- [ ] Requerimientos de contenedores o VMs  
- [ ] Requerimientos de ambientes  

### E) Riesgos iniciales
- [ ] Riesgos técnicos documentados  
- [ ] Medidas de mitigación definidas  
- [ ] Impacto en servicios existentes evaluado  

---

## 12.7 Resultado del CETI

El CETI solo puede tener tres estados:

1. **APROBADO** — El proyecto puede iniciar formalmente.  
2. **APROBADO CON OBSERVACIONES** — El proyecto inicia, pero debe corregir puntos antes del diseño técnico final.  
3. **NO APROBADO** — El proyecto queda detenido hasta corregir todas las deficiencias.

---

## 12.8 Relación con la Jerarquía Normativa

El CETI se integra y articula con:

- **NGCVS** (Nivel 0)  
- **Sub-Políticas Técnicas** (Nivel 1)  
- **Manuales Técnicos** (Nivel 2)  
- **Checklists Operativos** (Nivel 3)  

Ningún proyecto se considera “válido” sin CETI aprobado.

---

# FIN DE LA SECCIÓN 12 — METODOLOGÍA DE ALINEACIÓN TEMPRANA CON PRODUCCIÓN E INFRAESTRUCTURA
