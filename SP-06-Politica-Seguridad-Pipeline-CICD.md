# SP-06 — POLÍTICA DE SEGURIDAD DEL PIPELINE CI/CD  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos obligatorios para proteger la integridad, confidencialidad y disponibilidad de los pipelines CI/CD, garantizando que la construcción, análisis, empaquetado, firma, promoción y despliegue de artefactos se realice mediante procesos seguros, auditables y libres de manipulación manual.

## 2. Alcance
Aplica a:
- Todos los pipelines CI/CD de la Gerencia de TICs.  
- Proyectos internos, externos, evolutivos, correctivos y de terceros.  
- Infraestructura de CI/CD, servidores de build, runners y agentes.  
- Despliegues automatizados hacia DEV, QA, UAT, PRE-PROD y PROD.

Posee **carácter normativo interno** dentro de la Gerencia de TICs.

## 3. Definiciones
- **Pipeline Seguro:** Proceso estructurado, automatizado y libre de acciones manuales.  
- **Movimiento Lateral:** Acción mediante la cual un actor no autorizado escala privilegios dentro del pipeline.  
- **Integridad del Artefacto:** Garantía de que el artefacto generado no fue modificado posteriormente.  
- **Firma/Código de Integridad:** Hash o firma digital utilizada para validar artefactos.

## 4. Principios Normativos
1. **Todo pipeline debe funcionar sin intervención manual.**  
2. **Todos los pasos críticos deben estar firmados o registrados como evidencia.**  
3. **Las credenciales deben manejarse exclusivamente mediante un gestor de secretos.**  
4. **El pipeline debe ser reproducible, inmutable y auditable.**  
5. **Toda acción manual fuera de CI/CD está prohibida para ambientes superiores.**

## 5. Requisitos Técnicos Obligatorios

### 5.1 Seguridad de Runners y Agentes
- Deben operar en entornos segregados.  
- Deben estar actualizados y parchados.  
- Deben contar con permisos mínimos necesarios (principio de menor privilegio).  
- Está prohibido ejecutar pipelines en dispositivos personales.

### 5.2 Control de Acceso
- Autenticación obligatoria mediante LDAP/SSO o mecanismo corporativo.  
- Autorizaciones diferenciadas por rol (dev, QA, infraestructura, seguridad).  
- Prohibido compartir credenciales.  
- Prohibido tener usuarios genéricos o cuentas sin dueño.

### 5.3 Manejo de Secretos
Todo secreto debe estar:
- Almacenado en gestor de secretos aprobado (Vault o equivalente).  
- Fuera del repositorio de código.  
- Rotado periódicamente.  
- Encriptado en tránsito y reposo.

El pipeline **jamás** debe imprimir secretos en logs.

### 5.4 Análisis de Seguridad Obligatorios
Cada pipeline debe ejecutar:
- SAST  
- Análisis de dependencias  
- Linting de seguridad  
- Escaneo de imágenes (si aplica)  
- Verificación del validador in-house  

Artefactos con vulnerabilidades severas no pueden avanzar.

### 5.5 Integridad del Pipeline
Debe registrarse evidencia de:
- Hash del commit generado  
- Hash del artefacto  
- Fecha y hora de cada etapa  
- Identidad del solicitante del despliegue  
- Logs de todas las ejecuciones

Está prohibido:
- Modificar artefactos manualmente  
- Subir binarios creados fuera del pipeline  
- Alterar el pipeline en tiempo de ejecución

### 5.6 Protección contra Manipulación
El pipeline debe:
- Definir firmas digitales para artefactos.  
- Bloquear promoción de artefactos mutados.  
- Usar almacenamiento seguro para logs y evidencias.  
- Tener auditoría automática de cambios en la configuración.

### 5.7 Aprobaciones y Auditoría
- Deploys a PRE-PROD y PROD requieren aprobación dual.  
- Todo pipeline debe registrar quién aprobó y cuándo.  
- Auditoría trimestral obligatoria por Seguridad + Producción.

### 5.8 Restricción de Conectividad
- Los pipelines no pueden conectarse a internet salvo repositorios autorizados.  
- No pueden acceder a servicios internos sin justificación y permisos formales.

## 6. Lineamientos Operativos

### 6.1 Desarrollo
- Mantener pipelines actualizados.  
- Minimizar uso de workflows personalizados.  
- Evitar comandos shell no auditables.

### 6.2 QA
- Validar que los análisis de seguridad estén activos.  
- Confirmar que no existan credenciales expuestas.  
- Registrar evidencias en CI/CD.

### 6.3 Producción/Infraestructura
- Mantener servidores y runners seguros.  
- Configurar aprobaciones y restricciones por ambiente.  
- Revisar integridad de logs y auditorías.

### 6.4 Seguridad Informática
- Definir reglas de SAST y análisis de dependencias.  
- Detectar abuso o riesgo en pipelines.  
- Aprobar o rechazar configuraciones no seguras.

## 7. Controles Normativos
Ningún proyecto podrá promover artefactos a PRE-PROD o PROD si:
- El pipeline contiene pasos manuales  
- No genera evidencias completas  
- No ejecuta análisis de seguridad  
- Usa secretos sin un gestor aprobado  
- Carece de aprobaciones formales  
- No asegura integridad del artefacto

Producción y Seguridad poseen poder de **veto normativo**.

## 8. Anexos
- Anexo A: Estructura mínima de un pipeline seguro  
- Anexo B: Matriz de permisos por rol en CI/CD  
- Anexo C: Lista de secretos prohibidos en repositorios  
