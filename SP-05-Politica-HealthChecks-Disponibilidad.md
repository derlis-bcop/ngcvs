# SP-05 — POLÍTICA DE HEALTH CHECKS Y DISPONIBILIDAD  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos normativos y técnicos que aseguran que todos los sistemas, servicios, microservicios, aplicaciones y procesos tecnológicos cuenten con mecanismos de verificación de estado (health checks) que permitan monitoreo continuo, diagnóstico temprano y cumplimiento de objetivos de disponibilidad definidos por la Gerencia de TICs.

## 2. Alcance
Aplica obligatoriamente a:
- APIs, microservicios, aplicaciones web, móviles y servicios backend.  
- Procesos batch, automatizaciones, colas de mensajería y orquestadores.  
- Integraciones internas y externas que dependan de infraestructura administrada por TICs.  

Posee **carácter normativo interno** dentro de la Gerencia de TICs.

## 3. Definiciones
- **Health Check:** Mecanismo que reporta el estado operativo actual de un servicio.  
- **Liveness Probe:** Indica si el proceso está vivo o ejecutándose correctamente.  
- **Readiness Probe:** Indica si el servicio está en condiciones de recibir tráfico.  
- **Degradación:** Estado en el cual el servicio está funcional, pero con rendimiento inferior al esperado.  
- **SLA:** Acuerdo de nivel de servicio obligatorio respecto a disponibilidad y respuesta.

## 4. Principios Normativos
1. **Todo sistema debe exponer health checks estandarizados y accesibles.**  
2. **Toda aplicación debe contar con Liveness y Readiness obligatorios.**  
3. **El monitoreo 24/7 debe estar conectado a estos mecanismos.**  
4. **La falta de health checks constituye causal de veto para PRE-PROD y PROD.**  
5. **Los health checks deben ser confiables, determinísticos y sin lógica de negocio.**  
6. **Los despliegues deben validar readiness antes de tomar tráfico.**

## 5. Requisitos Técnicos Obligatorios

### 5.1 Endpoints Estándar
Todo servicio debe exponer:
- `/health` o `/healthz`  
- `/live`  
- `/ready`

Cada endpoint debe responder en formato JSON con:
- `status` (UP, DOWN, DEGRADED)  
- `timestamp`  
- `version`  
- `checks` (detalle por dependencia o recurso)  

### 5.2 Validaciones Internas del Health Check
Los health checks deben evaluar:
- Conectividad a base de datos  
- Conexión a API externas críticas  
- Configuración esencial cargada  
- Accesibilidad de colas de mensajería  
- Estado del filesystem o almacenamiento temporal  
- Recursos mínimos disponibles (memoria, CPU, file descriptors)

### 5.3 Integración con la Plataforma de Monitoreo
Debe enviarse el resultado de health checks a:
- SIEM corporativo  
- Dashboards operativos  
- Motor de alertas institucional  

Alertas deben dispararse ante:
- Estado `DOWN`  
- Estado `DEGRADED` con impacto operacional  
- Fallos en sucesivas verificaciones

### 5.4 Validación de Disponibilidad
Cada sistema debe definir:
- SLO mínimo aceptado  
- Umbral de alertas  
- Métricas de latencia y tasa de éxito

### 5.5 Pruebas de Health Check en CI/CD
Cada pipeline debe ejecutar:
- Health checks previos al build  
- Health checks posteriores al deploy  
- Validaciones de readiness antes de marcar despliegue como exitoso  

Si un health check falla:
- El despliegue debe revertirse automáticamente.  
- Debe registrarse evidencia del fallo.

### 5.6 Uso en Estrategias de Despliegue
Todo despliegue automatizado debe usar health checks para:
- Blue/Green deployment  
- Rolling updates  
- Canary releases  

Readiness determina cuándo una nueva versión puede recibir tráfico.

### 5.7 Disponibilidad Operativa (24/7)
Todo servicio crítico debe:
- Mantener monitoreo continuo  
- Poseer alertas automáticas  
- Registrar incidentes y recuperaciones  

La falta de health checks deshabilita monitoreo crítico y se considera incumplimiento grave.

## 6. Lineamientos Operativos

### 6.1 Fase de Desarrollo
- Implementar health checks desde la primera versión.  
- Mantener bajo acoplamiento entre health checks y lógica de negocio.  
- Documentar dependencias críticas monitoreadas.

### 6.2 Fase de QA
- Validar endpoints y formatos de respuesta.  
- Confirmar detección de estados DOWN y DEGRADED.  
- Probar readiness durante despliegues.  
- Verificar integración con dashboards.

### 6.3 Fase de Producción
- Configurar frecuencia de verificación.  
- Mantener thresholds operativos.  
- Emitir reportes de disponibilidad mensual.  

Producción puede ajustar periodicidad según la criticidad del servicio.

## 7. Responsabilidades

### 7.1 Desarrollo
- Implementar endpoints obligatorios.  
- Mantener documentación actualizada.  
- Garantizar exactitud del estado reportado.  

### 7.2 QA
- Validar funcionamiento y respuesta estructurada.  
- Confirmar integración con monitoreo.  
- Registrar hallazgos en CI/CD.  

### 7.3 Producción/Infraestructura
- Monitorear servicios 24/7.  
- Configurar alertas críticas y operativas.  
- Ejecutar rollback automático cuando corresponda.  

### 7.4 Seguridad Informática
- Analizar health checks expuestos públicamente.  
- Validar controles de acceso si corresponde.  
- Auditar cumplimiento de disponibilidad.

## 8. Controles Normativos
Ningún sistema puede avanzar a PRE-PROD o PROD si:
- No expone `/health`, `/live` y `/ready`  
- Sus health checks no son determinísticos  
- No existe integración con monitoreo  
- No se validó readiness en despliegue  
- Su disponibilidad histórica incumple SLO definido  

## 9. Anexos
- Anexo A: Formato estándar de health checks  
- Anexo B: Tabla de estados y respuestas requeridas  
- Anexo C: Ejemplos de implementación por tecnología  
