# SP-03 — POLÍTICA DE DEPENDENCIAS Y LIBRERÍAS  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos obligatorios para la gestión, uso, control, auditoría y actualización de dependencias, librerías, frameworks y paquetes utilizados en el desarrollo de software dentro de la Gerencia de TICs.  
Su propósito es minimizar riesgos de seguridad, garantizar estabilidad, asegurar trazabilidad y mantener versiones compatibles con los lineamientos corporativos.

## 2. Alcance
Aplica a:
- Todo desarrollo interno o externo.  
- Servicios backend, frontend, móviles, APIs, microservicios, pipelines, automatizaciones, ETLs y aplicaciones web.  
- Dependencias nativas o de terceros, públicas o privadas.  

Es de **cumplimiento obligatorio** dentro de la Gerencia de TICs.

## 3. Definiciones
- **Dependencia:** Cualquier librería o paquete utilizado por un sistema.  
- **Gestor de dependencias:** Herramienta para instalar y versionar paquetes (npm, pip, Maven, Gradle, etc.).  
- **Vulnerabilidad:** Debilidad detectada en una dependencia, calificada mediante CVSS.  
- **SBOM (Software Bill of Materials):** Inventario formal de dependencias del proyecto.

## 4. Principios Normativos
1. **Toda dependencia utilizada debe ser conocida, aprobada y auditada.**  
2. **Está prohibido usar librerías sin mantenimiento, sin soporte o con vulnerabilidades críticas.**  
3. **Las versiones deben ser explícitas, fijas y controladas.**  
4. **Se debe generar un SBOM obligatorio por cada build.**  
5. **Las actualizaciones deben realizarse de forma controlada y documentada.**

## 5. Requisitos Técnicos Obligatorios

### 5.1 Uso de Gestores de Dependencias Oficiales
Cada lenguaje debe utilizar su gestor formal:
- Python → `pip`, `pipenv`, `poetry`  
- Java → Maven o Gradle  
- JavaScript/Node → `npm` o `yarn`  
- Otros lenguajes → gestores oficiales según la tecnología

Prohibido administrar dependencias manualmente.

### 5.2 Versiones Fijas (“pinning”)
Todas las dependencias deben:
- Declarar una versión exacta.  
- Evitar rangos amplios (`>=`, `^`, `~`) salvo justificación técnica formal.  
- Mantener archivo de lock (package-lock.json, Pipfile.lock, etc.).

### 5.3 Auditoría Automatizada de Vulnerabilidades
Cada pipeline CI/CD debe ejecutar:
- Escaneo de vulnerabilidades  
- Reporte automático en CI/CD  
- Verificación de CVSS

No puede avanzarse de ambiente si existe:
- Vulnerabilidad **Crítica (CVSS ≥ 9.0)**  
- Vulnerabilidad **Alta (CVSS ≥ 7.0)** sin justificación formal

### 5.4 Repositorios Autorizados
Las dependencias deben provenir de:
- Repositorios oficiales  
- Repositorios privados corporativos  
- Artefactories internos autorizados  

Se prohíbe el uso de repositorios desconocidos o sin firma.

### 5.5 Dependencias Obsoletas o EOL
- Deben ser reemplazadas o actualizadas.  
- No se permite su uso en nuevos proyectos.  
- Su permanencia requiere excepción aprobada por Arquitectura + Seguridad.

### 5.6 Generación de SBOM Obligatorio
Cada build debe generar automáticamente:
- Inventario de dependencias  
- Versiones exactas  
- Hash de integridad  
- Identificación de vulnerabilidades

El SBOM debe almacenarse como evidencia en CI/CD.

### 5.7 Firma e Integridad
Las dependencias deben ser:
- Auténticas (firma digital o hash verificado)  
- Íntegras (sin modificaciones forzadas)  
- Registradas en el pipeline CI/CD

### 5.8 Revisión de Dependencias Transitivas
Toda dependencia debe revisarse incluyendo sus subdependencias.  
No se admite desconocimiento de librerías indirectas.

## 6. Lineamientos Operativos

### 6.1 En la Fase de Desarrollo
- Documentar dependencias nuevas.  
- Evitar el uso excesivo de librerías externas.  
- Prever ciclos de actualización.  
- Aplicar pruebas unitarias al actualizar versiones.

### 6.2 En la Fase de QA
- Validar compatibilidad de nuevas versiones.  
- Ejecutar regresiones completas.  
- Confirmar análisis de vulnerabilidades.

### 6.3 En la Fase de Producción
- Mantener repositorios internos actualizados.  
- Programar ventanas de actualización de dependencias.  
- Emitir reportes mensuales de vulnerabilidades abiertas.

## 7. Responsabilidades

### 7.1 Desarrollo
- Mantener dependencias actualizadas.  
- Documentar incorporación o eliminación de librerías.  
- Ejecutar auditoría de vulnerabilidades en CI/CD.  
- Garantizar compatibilidad de nuevas versiones.

### 7.2 QA
- Validar impactos de actualizaciones.  
- Garantizar que las evidencias estén en CI/CD.  
- Exigir corrección de versiones inseguras.

### 7.3 Arquitectura
- Definir frameworks autorizados.  
- Revisar solicitudes de excepciones.  
- Emitir lineamientos de versiones mínimas.

### 7.4 Seguridad Informática
- Evaluar riesgos de vulnerabilidades.  
- Mantener reglas de aceptación basadas en CVSS.  
- Monitorear cumplimiento normativo.

### 7.5 Producción/Infraestructura
- Configurar repositorios privados corporativos.  
- Mantener seguridad y disponibilidad de artefactories.  
- Emitir alertas sobre dependencias obsoletas.

## 8. Controles Normativos
- Ningún artefacto puede avanzar a QA si:
  - No tiene dependencias definidas  
  - No existe lock file  
  - No se generó SBOM
- Ningún componente puede avanzar a PRE-PROD o PROD si:
  - Posee vulnerabilidades críticas  
  - Usa librerías prohibidas  
  - Utiliza repositorios no autorizados

## 9. Anexos
- Anexo A: Lista de gestores de dependencias autorizados  
- Anexo B: Convención de nombres y versiones  
- Anexo C: Ejemplo de SBOM requerido  
