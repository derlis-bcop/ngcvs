# SP-02 — POLÍTICA DE MONITOREO Y OBSERVABILIDAD  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos normativos y técnicos obligatorios que garanticen que todos los servicios, aplicaciones, integraciones, bases de datos y componentes tecnológicos cuenten con monitoreo integral, continuo y centralizado.  
El propósito es asegurar disponibilidad, detección temprana de incidentes, diagnóstico eficiente, cumplimiento de SLAs y trazabilidad operacional en todos los ambientes.

## 2. Alcance
Aplica obligatoriamente a:
- Sistemas internos y externos desplegados en DEV, QA, UAT, PRE-PROD y PROD.  
- Microservicios, APIs, aplicaciones web, móviles, batch, ETLs, colas de mensajería y procesos automatizados.  
- Componentes de infraestructura asociados al despliegue.  
- Todo desarrollo evolutivo o correctivo.

Posee **carácter normativo interno** dentro de la Gerencia de TICs.

## 3. Definiciones
- **Monitoreo:** Recolección de métricas, logs, traces y eventos para supervisar el funcionamiento de sistemas.  
- **Observabilidad:** Capacidad de inferir el estado interno de un sistema a partir de sus salidas (logs, métricas, trazas).  
- **SLO (Service Level Objective):** Nivel objetivo de calidad del servicio.  
- **SLI (Service Level Indicator):** Métrica medible utilizada para evaluar cumplimiento del SLO.  
- **Alerta:** Notificación automática sobre condiciones anómalas o degradación.

## 4. Principios Normativos
1. **Todo sistema debe ser observable.**  
2. **El monitoreo debe ser centralizado, unificado y estandarizado.**  
3. **Debe existir telemetría mínima obligatoria (logs, métricas, trazas).**  
4. **Las alertas deben basarse en SLIs y umbrales formales.**  
5. **La observabilidad debe permitir diagnóstico autónomo y correlación de eventos.**  
6. **La falta de monitoreo constituye causal de veto para PRE-PROD y PROD.**

## 5. Requisitos Técnicos Obligatorios

### 5.1 Telemetría Mínima
Todos los sistemas deben exponer al menos:
- **Logs estructurados** (según SP-01).  
- **Métricas técnicas**, incluyendo:
  - Uso de CPU, memoria, disco, IO, conexiones.  
  - Latencia de endpoints.  
  - Tiempo de respuesta.  
  - Cantidad de errores por tipo.
- **Traces distribuidos** para ecosistemas basados en microservicios.

### 5.2 Integración con Plataforma Centralizada
Los sistemas deben integrarse obligatoriamente con:
- SIEM o plataforma de observabilidad corporativa.  
- Dashboards institucionales predefinidos.  
- Agentes o SDK aprobados por Producción/Infraestructura.

### 5.3 Monitoreo de Estados de Salud
Todo componente debe exponer:
- **Liveness Probe**: determina si la aplicación está viva.  
- **Readiness Probe**: determina si está en condiciones de servir tráfico.  
- **Endpoint de Health Check** estandarizado y accesible para monitoreo 24/7.

### 5.4 Reglas de Alertamiento
Toda aplicación debe contar con alertas:
- **Críticas:** caída del servicio, errores masivos, pérdida de conectividad.  
- **Altas:** degradación significativa de performance.  
- **Medias:** patrones inusuales de uso o comportamiento.  
- **Bajas:** eventos de mantenimiento o warning.

Alertas críticas deben enviarse a los canales operativos definidos por Producción de manera automática.

### 5.5 Dashboards Obligatorios
Para cada servicio deben existir dashboards que presenten:
- Latencia y throughput.  
- Disponibilidad (SLO vs real).  
- Fallos por endpoint.  
- Uso de recursos.

### 5.6 Monitoreo 24/7
Producción debe mantener vigilancia continua sobre:
- Disponibilidad del servicio.  
- Alertas críticas.  
- Comportamientos anómalos detectados por AI/ML o reglas del SIEM.

El incumplimiento de configuración de alertas puede impedir despliegues.

### 5.7 Instrumentación Estándar
Debe utilizarse instrumentación aprobada por la Gerencia de TICs:
- OpenTelemetry o agente equivalente.  
- Exportadores oficiales o compatibles.  
- Librerías autorizadas en Manual Técnico de Observabilidad.

## 6. Lineamientos Operativos

### 6.1 Fase de Desarrollo
- Instrumentar métricas y traces desde la primera iteración del proyecto.  
- Asegurar endpoints de health check.  
- Validar integración mínima con la plataforma en QA.

### 6.2 Fase de QA
- Validar SLIs básicos: latencia, tasa de éxito, disponibilidad.  
- Verificar que las alertas se disparen correctamente.  
- Confirmar que logs, métricas y trazas se visualizan correctamente en la plataforma.

### 6.3 Fase de Producción
- Configurar retención adecuada de métricas.  
- Mantener dashboards institucionales.  
- Reentrenar alertas en función de comportamiento histórico.

## 7. Responsabilidades

### 7.1 Desarrollo
- Implementar instrumentación.  
- Generar métricas, logs y trazas según estándar.  
- Documentar SLIs y health checks.

### 7.2 QA
- Validar monitoreo y alertas antes de admitir a UAT.  
- Documentar evidencias en CI/CD.

### 7.3 Producción/Infraestructura
- Garantizar monitoreo 24/7.  
- Mantener conectores, agentes y colectores.  
- Emitir reportes mensuales de disponibilidad.

### 7.4 Seguridad Informática
- Analizar eventos de seguridad.  
- Monitorear comportamientos anómalos.  
- Definir reglas de correlación.

## 8. Controles Normativos
- Ningún servicio puede avanzar a PRE-PROD sin:
  - Métricas expuestas  
  - Health checks activos  
  - Integración con plataforma de m
