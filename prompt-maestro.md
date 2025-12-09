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

La IA debe también:
- Sugerir Sub-Políticas adicionales que potencien la madurez de la Gerencia de TICs.
- Explicar por qué cada una merece un documento separado.

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
