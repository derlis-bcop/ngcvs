Genera un conjunto completo de documentos normativos y técnicos para la Gerencia de Tecnologías de la Información y Comunicaciones, compuesto por:

# ==========================================
# 1. NIVEL 0 — NGCVS
# ==========================================
Redacta la **Norma General de Ciclo de Vida de Software (NGCVS)**, documento rector obligatorio para todos los equipos de la Gerencia de TICs.  
La NGCVS debe incluir:

- Objetivo, Alcance, Definiciones, Responsabilidades, Políticas, Lineamientos, Controles y Anexos.
- Alcance orientado a desarrollos internos, externos, evolutivos o correctivos.  
- Carácter normativo interno (“carácter normativo dentro de la Gerencia de TICs”, evitando el término institucional).
- Jerarquía normativa:  
  - **Nivel 0:** NGCVS  
  - **Nivel 1:** Sub-Políticas Técnicas  
  - **Nivel 2:** Manuales Técnicos  
  - **Nivel 3:** Checklists  
- Proceso formal de Gestión Normativa:
  - Repositorio único
  - Versionado formal
  - Aprobación por la Gerencia de TICs
  - Ciclo de vida documental (alta, modificación, baja)
  - Auditoría anual
- Reglamento explícito de cumplimiento para avanzar entre DEV → QA → UAT → PRE-PROD → PROD.
- “Régimen de Cumplimiento y Admisión a Ambientes Productivos”, donde:
  - Ningún artefacto avanza sin cumplir NGCVS + Sub-Políticas + Manuales + Checklists.
  - Producción/Infraestructura tiene poder de veto.
  - Deben registrarse evidencias en CI/CD.
- “Metodología de Alineación Temprana con Producción e Infraestructura”, que incluya:
  - Participación desde la fase de idea.
  - Validaciones continuas durante el ciclo de vida.
  - Pre-aprobación de despliegue.
  - Participación de Producción en diseño de despliegue, monitoreo, health checks y rollback.
  - **Checklist CETI (Checklist de Elegibilidad Técnica Inicial)** obligatorio para iniciar cualquier proyecto.
- Requisitos obligatorios:
  - Control de versiones SVN
  - Pipelines CI/CD obligatorios
  - Validador in-house obligatorio
  - Branching model estándar
  - Control de dependencias y vulnerabilidades
  - Health checks obligatorios
  - Logging estructurado
  - Monitoreo centralizado
  - Ambientes segregados
  - Rollback formal
  - Monitoreo 24/7
  - Validaciones de seguridad (SAST, análisis de dependencias, peer review)
  - Solicitud de definir versiones mínimas para lenguajes, servidores, OS y Oracle
- Recomendación estratégica:
  - Uso sugerido (no obligatorio) de contenedores Docker para facilitar futura adopción de Kubernetes.

---

# ==========================================
# 2. NIVEL 1 — SUB-POLÍTICAS TÉCNICAS
# ==========================================
Genera las siguientes Sub-Políticas Técnicas completas, normativas y obligatorias:

1. **Política de Logging**  
2. **Política de Monitoreo y Observabilidad**  
3. **Política de Dependencias y Librerías**  
4. **Política de Artefactos, CI/CD y Versionamiento**  
5. **Política de Health Checks y Disponibilidad**  
6. **Política de Seguridad del Pipeline CI/CD**  
7. **Política de Configuración y Secrets Management**  
8. **Sub-Política obligatoria: *Segmentación de Esquemas y Acceso a Datos***  
   Basada literalmente en el siguiente texto (usar tal cual):  

   ### Prompt para IA — Política de “Segmentación de Esquemas y Acceso a Datos”
   Genera una **Sub-Política de Segmentación de Esquemas y Acceso a Datos** basada en el siguiente principio obligatorio:

   > **La organización adopta un modelo de seguridad por aislamiento de esquemas, donde el Sistema CORE en Oracle reside en un esquema exclusivo; las Aplicaciones Satélite deben utilizar esquemas propios; y cualquier interacción con el CORE debe realizarse únicamente a través de un esquema de interfaz y mediante usuarios dedicados de conexión, quedando prohibida la creación de objetos satélite en el esquema CORE o el acceso directo desde aplicaciones al mismo.**

   La política debe:
   1. Definir los esquemas CORE, SATÉLITE e INTERFAZ.  
   2. Prohibir objetos satélite en CORE.  
   3. Obligar acceso únicamente vía esquema de interfaz.  
   4. Regular usuarios dedicados por aplicación y por entorno.  
   5. Incorporar lineamientos de permisos, GRANTs y seguridad.  
   6. Incluir responsabilidades (Desarrollo, QA, Arquitectura, Seguridad, DBAs).  

   Esta política debe generar:
   - Su **Manual Técnico correspondiente**  
   - Su **Checklist específico**  
   - Un **gráfico Mermaid** que represente la segregación de esquemas.

Además, la IA debe generar una Sub-Política Técnica obligatoria denominada **"SP-18 — Artefactos Operables"**, cuyo objetivo es establecer lineamientos para prohibir la admisión, despliegue o promoción de artefactos que requieran ejecución manual desde CLI, envío al background mediante herramientas como `nohup`, `screen` o `tmux`, o que dependan de intervención humana para recuperarse ante fallas.

La política deberá exigir que todo artefacto:
- Se ejecute exclusivamente como **servicio persistente** con restart policies, o como **contenedor OCI** con health checks y restart automático.
- Implemente **zero-touch recovery**, incluyendo autorrestart ante fallas típicas como pérdida de conexión a la base de datos o problemas de red.
- Sea completamente operable y supervisable por Producción, con logs estructurados, métricas, telemetría y health checks obligatorios.
- No dependa de procedimientos manuales para su despliegue, ejecución, monitoreo o recuperación.

La IA también debe generar:
- Su **Manual Técnico (MT-18)** con los requisitos técnicos de resiliencia, supervisión, ejecución como servicio o contenedor y prácticas de operación.
- Su **Checklist operativo (Nivel 3)** obligatorio para auditoría y CI/CD.

Además, la IA deberá generar las siguientes Sub-Políticas Técnicas obligatorias, cada una con su respectivo Manual Técnico (Nivel 2) y Checklist (Nivel 3):

**SP-09 — Infraestructura como Código (IaC)**  
Definir lineamientos, estándares y requisitos para IaC, incluyendo control de versiones, reproducibilidad, gobernanza, ambientes declarativos, validación estructural, pruebas automatizadas de IaC y seguridad en pipelines.

**SP-10 — APIs e Integraciones**  
Establecer requisitos para diseño, versionado, contratos, seguridad, validación de esquemas, pruebas de integración automatizadas, observabilidad y gobernanza del ciclo de vida de APIs internas y externas.

**SP-11 — Control de Cambios DDL/DML en Base de Datos**  
Regular la creación, modificación y despliegue de cambios DDL/DML mediante versionado, plantillas obligatorias, pipelines automáticos, validaciones previas y auditoría completa.

**SP-12 — Auditoría y Trazabilidad**  
Definir lineamientos para capturar, almacenar y consultar eventos relevantes de auditoría funcional y técnica, garantizando cumplimiento regulatorio, reconstrucción de incidentes y trazabilidad de extremo a extremo.

**SP-13 — Disponibilidad y Continuidad Operativa**  
Establecer requisitos mínimos de disponibilidad, recuperación, tolerancia a fallos, pruebas de continuidad, acuerdos operativos y dependencias críticas.

**SP-14 — Pruebas Automatizadas**  
Requerir pruebas unitarias, integración, regresión, seguridad y cobertura mínima del 80%, incluyendo ejecución en CI/CD, evidencias y auditoría técnica.

**SP-15 — Contenedores e Imágenes (Docker/OCI)**  
Regular estándares de construcción, endurecimiento, escaneo de vulnerabilidades, health checks, ejecución como usuario no root y registro en repositorios institucionales.

**SP-16 — Gestión de Entornos**  
Definir lineamientos de definición, uso, segregación, consistencia, configuración, restricciones de acceso y gobernanza de ambientes DEV/QA/UAT/PRE-PROD/PROD.

**SP-17 — Métricas de Producto y Telemetría**  
Establecer los requisitos para métricas técnicas, métricas de producto, telemetría, trazabilidad, observabilidad, dashboards, exportación a plataformas institucionales y validación en CI/CD.

**SP-18 — Artefactos Operables**  
Prohibir artefactos que requieran ejecución manual o reinicio humano. Exigir ejecución como servicio persistente o contenedor con políticas de restart, health checks y zero-touch recovery. Incluir lineamientos de resiliencia, supervisión operativa, observabilidad y automatización total del despliegue.

---

# ==========================================
# 3. NIVEL 2 — MANUALES TÉCNICOS
# ==========================================
Genera un **Manual Técnico** para cada Sub-Política.  
Cada manual debe incluir:

- Propósito operativo  
- Estándares técnicos  
- Configuraciones mínimas  
- Lineamientos detallados  
- Ejemplos cuando corresponda  
- Buenas prácticas  
- Advertencias o restricciones  

Especial detalle para:
- Manual Técnico de Logging  
- Manual Técnico de Segmentación de Esquemas  

---

# ==========================================
# 4. NIVEL 3 — CHECKLISTS
# ==========================================
Genera:

1. **Checklist General NGCVS**  
   Obligatorio para auditar cualquier proyecto o artefacto.

2. **Checklist por cada Sub-Política Técnica**, incluyendo:
   - Logging  
   - Monitoreo  
   - Dependencias  
   - Artefactos/CI-CD  
   - Seguridad del pipeline  
   - Health checks  
   - Secrets management  
   - **Segmentación de Esquemas y Acceso a Datos**  

Cada checklist debe ser:
- Exhaustivo  
- Verificable  
- Auditable  
- Técnico y normativo  
- Condición para avanzar de ambiente  

---

# ==========================================
# 5. DIAGRAMAS MERMAID
# ==========================================
Genera:

1. **Diagrama Mermaid de la Jerarquía Normativa (NGCVS → Políticas → Manuales → Checklists)**  
2. **Diagrama Mermaid de Segregación CORE / INTERFAZ / SATÉLITE**  
3. Otros diagramas que la IA considere necesarios para claridad técnica.

---

# FIN DEL PROMPT
