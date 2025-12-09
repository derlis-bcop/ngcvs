# SP-04 — POLÍTICA DE ARTEFACTOS, CI/CD Y VERSIONAMIENTO  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer las reglas normativas y técnicas que rigen la construcción, empaquetado, versionamiento, almacenamiento, validación y despliegue automatizado de artefactos de software mediante pipelines CI/CD.  
Su propósito es asegurar trazabilidad, repetibilidad, integridad, seguridad y estandarización en todo el ciclo de vida del software.

## 2. Alcance
Esta política aplica obligatoriamente a:
- Todo desarrollo interno, contratado o tercerizado.
- Componentes backend, frontend, móviles, APIs, microservicios, ETLs, servicios batch, scripts y automatizaciones.
- Artefactos generados en pipelines CI/CD, independientemente del lenguaje o tecnología.

Posee **carácter normativo interno** dentro de la Gerencia de TICs.

## 3. Definiciones
- **Artefacto:** Resultado binario o empaquetado de un proceso de build (JAR, WAR, wheel, zip, docker image, etc.).  
- **Pipeline CI/CD:** Secuencia automatizada de build, test, validación, análisis, empaquetado y despliegue.  
- **Versionamiento Semántico (SEMVER):** Esquema estándar: MAJOR.MINOR.PATCH.  
- **Repositorio de Artefactos:** Sistema autorizado para almacenar builds aprobados.  
- **Hash de Integridad:** Firma digital o checksum que identifica unívocamente un artefacto.

## 4. Principios Normativos
1. **Todo artefacto debe ser generado exclusivamente mediante CI/CD.**  
2. **El versionamiento debe ser semántico y obligatorio.**  
3. **Las evidencias de CI/CD deben almacenarse de forma automática.**  
4. **Un artefacto solo puede considerarse válido si tiene hash de integridad.**  
5. **El despliegue manual está prohibido**, salvo excepciones autorizadas.  
6. **Producción puede vetar despliegues** que no cumplan esta política.

## 5. Requisitos Técnicos Obligatorios

### 5.1 Construcción de Artefactos
Todo artefacto debe ser:
- Construido automáticamente en CI/CD  
- Reproducible mediante script de build  
- Auditado por el validador in-house  
- generado a partir de fuentes almacenados en **SVN**

Artefactos construidos manualmente están prohibidos.

### 5.2 Versionamiento
Usar **SemVer** en todos los proyectos:
- **MAJOR:** cambios incompatibles  
- **MINOR:** nuevas funcionalidades compatibles  
- **PATCH:** correcciones y mejoras menores  

Requisitos:
- No se permite reutilizar versiones.  
- Cada build debe asociarse a un número de versión.  
- Debe existir un CHANGELOG obligatorio.

### 5.3 Repositorio de Artefactos
Todo artefacto debe almacenarse en:
- Repositorio privado corporativo  
- Artefactory interno autorizado  

Debe incluir:
- Versión  
- Fecha  
- Hash de integridad  
- Origen del commit (SVN revision)  
- Evidencias adjuntas

### 5.4 Integridad del Artefacto
Cada pipeline debe generar:
- **SHA-256 o equivalente**  
- Archivo de firma digital  
- Registro automático en las evidencias del pipeline  

Cualquier divergencia en hash anula la validez del artefacto.

### 5.5 Automatización del Despliegue
Todo despliegue debe ser:
- Totalmente automatizado  
- Reproducible  
- Sin intervención manual  
- Regido por aprobaciones formales en CI/CD  

Despliegues por consola, FTP o manipulación manual están prohibidos.

### 5.6 Evidencias de CI/CD
El pipeline debe almacenar:
- Resultados de pruebas unitarias, integración y regresión  
- Análisis SAST y de dependencias  
- Validaciones del validador in-house  
- Logs de compilación  
- Artefactos finales + hashes  
- Resultados de health checks previos al despliegue  
- Validación de rollback

### 5.7 Flujos de Promoción entre Ambientes
La promoción de un artefacto debe respetar:
DEV → QA → UAT → PRE-PROD → PROD

Reglas:
- Solo puede promoverse el artefacto existente en el repositorio, nunca uno reconstruido.  
- No pueden existir builds paralelos no autorizados.  
- No puede modificarse el artefacto entre ambientes.

### 5.8 Ramificación (Branching Model)
Se exige un esquema estándar:
- **trunk** (línea estable)  
- **branches/features**  
- **branches/releases**  
- **branches/hotfix**

Requisitos:
- Commit message estandarizado  
- Prohibición de commits directos a trunk (salvo roles autorizados)  
- Peer review obligatorio

## 6. Lineamientos Operativos

### 6.1 Fase de Desarrollo
- Configurar pipeline desde el inicio del proyecto.  
- Incorporar pruebas unitarias obligatorias.  
- Validar build reproducible.  
- Mantener CHANGELOG actualizado.

### 6.2 Fase de QA
- Validar si el artefacto respeta versionamiento.  
- Confirmar evidencias técnicas.  
- Verificar integridad del artefacto.  
- Validar automatización del despliegue en QA.

### 6.3 Fase de Producción
- Controlar el repositorio central de artefactos.  
- Auditar historial de builds.  
- Ejecutar despliegues únicamente desde CI/CD.  
- Validar rollback automatizado.

## 7. Responsabilidades

### 7.1 Desarrollo
- Implementar pipelines completos.  
- Mantener versionamiento correcto.  
- Garantizar integridad del artefacto.  
- Documentar procedimientos técnicos.

### 7.2 QA
- Verificar cumplimiento normativo.  
- Validar evidencias de CI/CD.  
- Registrar no conformidades.

### 7.3 Producción/Infraestructura
- Mantener infraestructura de CI/CD.  
- Asegurar funcionamiento de repositorios.  
- Vetar despliegues que incumplan requisitos.

### 7.4 Seguridad Informática
- Validar integridad, firmas y análisis de seguridad.  
- Asegurar trazabilidad en pipelines.  
- Revisar vulnerabilidades detectadas.

## 8. Controles Normativos
Ningún artefacto podrá avanzar a PRE-PROD o PROD si:
- No fue generado por CI/CD  
- No posee hash de integridad  
- No existe evidencia de pruebas  
- No cumple versionamiento SemVer  
- Usa un repositorio no autorizado  
- Presenta fallas de seguridad no resueltas

## 9. Anexos
- Anexo A: Estándar de estructura de pipeline  
- Anexo B: Formato de CHANGELOG obligatorio  
- Anexo C: Procedimiento para solicitar excepciones normativas  
