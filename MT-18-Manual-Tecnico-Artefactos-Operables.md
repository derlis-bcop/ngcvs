# MT-18 — MANUAL TÉCNICO DE ARTEFACTOS OPERABLES  
**Manual Técnico — Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-18 — Política de No Admisión de Artefactos No Operables

---

# 1. Propósito Operativo

Este manual establece los requisitos técnicos, configuraciones mínimas, patrones de diseño y lineamientos obligatorios para garantizar que todos los artefactos desarrollados o administrados por la Gerencia de TICs sean **operables**, **supervisables**, **resilientes** y **autorreparables**, eliminando cualquier dependencia de ejecución manual o interacción humana para iniciar, mantener o recuperar procesos.

Se operacionaliza la SP-18, definiendo los estándares técnicos que permiten:

- Zero-touch recovery  
- Autorrestart garantizado  
- Observabilidad completa (logs, métricas, telemetría)  
- Despliegue reproducible e independiente de manualidades  
- Integración con la infraestructura institucional  
- Supervisión 24/7  
- Cumplimiento estricto de CI/CD  

---

# 2. Alcance

Este manual aplica a:

- APIs, microservicios, backends, workers, Daemons, batch processors.  
- ETL, integradores, colas, procesadores asíncronos.  
- Contenedores OCI, VMs y servicios SOA.  
- Herramientas complementarias de misión crítica.  
- Todos los ambientes de la Gerencia de TICs.

Quedan excluidos únicamente artefactos autorizados bajo excepción formal (ver SP-18).

---

# 3. Requisitos Técnicos Obligatorias de Operabilidad

## 3.1 Prohibición de ejecución manual

Ningún artefacto puede requerir:

- Ejecución manual vía CLI.  
- Comandos interactivos (`python script.py`, `java -jar`, etc.).  
- Envío al background con `&`, `nohup`, `screen`, `tmux`.  
- Lanzamiento desde shell script sin supervisor.  
- Reinicio manual en caso de fallas.  

Cualquier dependencia de operador humano → artefacto **rechazado**.

---

## 3.2 Ejecución como servicio persistente

Todo artefacto debe ejecutarse como servicio supervisado mediante:

- `systemd` (Linux)  
- `supervisord`  
- Contenedor OCI con restart policies  
- Orquestadores institucionales  

Debe cumplir:

- Restart automático ante fallos  
- Inicio automático al boot del host / contenedor  
- Integración con monitoreo institucional  
- Exposición de health checks  
- Trazabilidad mediante logs estándar  

---

## 3.3 Ejecución en contenedores OCI (cuando aplique)

Los contenedores deben cumplir:

### a) Configuración mínima del Dockerfile
- Imagen base oficial y aprobada  
- Usuario no root  
- Dependencias fijadas  
- Health checks declarados  
- Señales de sistema correctamente gestionadas (SIGTERM, SIGINT)  
- No contener secretos  

### b) Políticas de restart
- `restart: always`  
- O `on-failure` con backoff  

### c) Observabilidad
- Logs estructurados  
- Métricas técnicas y de producto expuestas  
- Trace ID y correlación distribuida  

### d) Resiliencia
El contenedor debe recuperarse automáticamente ante:

- errores de conexión a la BD  
- pérdida de red  
- timeouts  
- fallas internas no fatales  

### e) Despliegue reproducible
El contenedor debe construirse exclusivamente desde CI/CD.

---

## 3.4 Requisitos de Health Checks

Todo servicio o contenedor debe exponer al menos:

- `/health` → estado general  
- `/ready` → listo para tráfico  
- `/live` → proceso en ejecución  

El pipeline debe validar:

- Tiempo de respuesta  
- Formato esperado  
- No exponer errores internos o stack traces  

---

## 3.5 Zero-Touch Recovery (Recuperación Automática)

El artefacto debe recuperarse sin intervención humana ante:

- reinicios inesperados  
- desconexión temporal a la BD  
- timeouts de red  
- fallos en dependencias externas  
- caídas temporales de infraestructura  

Debe implementar:

- Retrys con backoff exponencial  
- Circuit breakers  
- Reconexión automática  
- Validación de estado interno  
- Idempotencia en tareas repetibles  

---

## 3.6 Observabilidad Obligatoria

El artefacto debe contar con:

### Logs
- Formato estructurado  
- Nivelado correctamente (INFO, WARN, ERROR)  
- Trazabilidad (`trace_id`, `correlation_id`)  
- Sin datos sensibles  

### Métricas
- Latencia  
- Errores  
- Disponibilidad  
- Throughput  
- Dependencias  

### Telemetría
- Trazas distribuidas  
- Eventos críticos de operación  

Monitoreo debe poder reconstruir incidentes sin acceder al servidor.

---

## 3.7 Supervisión Automática

El artefacto debe permitir:

- Detección inmediata de fallas  
- Alertas integradas a monitoreo central  
- Registro en dashboards institucionales  
- Consulta de métricas en tiempo real  

---

# 4. Requisitos de Despliegue y Operación

## 4.1 Integración con CI/CD
Cada artefacto debe:

- Compilarse únicamente mediante CI/CD  
- Validarse con controles automáticos  
- Ejecutar pruebas automatizadas  
- Registrar evidencia de despliegue  
- Estar vinculado a un commit y versión específica  

## 4.2 Procedimientos de despliegue
Cada artefacto debe incluir:

- Manifest del servicio / contenedor  
- Parámetros configurables  
- Plan de despliegue  
- Plan de rollback  

Cualquier artefacto sin documentación → rechazado.

---

# 5. Requisitos para Procesos Batch

Los batch o workers deben:

- Ejecutarse como servicio o contenedor  
- Registrar inicio, fin, errores y cantidad procesada  
- Tener control de concurrencia  
- Establecer retries  
- Estar integrados a monitoreo  
- Ser reproducibles y auditables  

Queda prohibida su ejecución manual en cron local del servidor.

---

# 6. Anti-Patrones Prohibidos

- Scripts ejecutados manualmente.  
- Procesos dependientes de supervisión humana.  
- Servicios sin health checks.  
- Contenedores sin restart policy.  
- Procesos que “mueren en silencio”.  
- Batch sin logs o telemetría.  
- Servicios que requieren acceso SSH para reiniciarse.  
- Artefactos que dependen del PATH del usuario o variables no declaradas.  
- Programas que no manejan errores de infraestructura.  

---

# 7. Roles y Responsabilidades

## Desarrollo
- Entregar artefactos operables y supervisables.  
- Cumplir plantillas oficiales.  
- No entregar scripts manuales.  

## QA
- Validar resiliencia, restart automático y health checks.  

## Producción
- Supervisar operabilidad real.  
- Configurar monitoring y restart.  
- Emitir veto si el artefacto no cumple.  

## Arquitectura
- Definir plantillas de servicios y contenedores.  
- Asegurar compatibilidad futura.  

## Seguridad TICs
- Validar ausencia de secretos y datos sensibles.  

---

# 8. Validaciones en CI/CD

El pipeline debe rechazar artefactos que:

- No se ejecuten como servicio o contenedor.  
- Carezcan de health checks.  
- No tengan restart automático.  
- No expongan logs estructurados.  
- No expongan métricas.  
- Contengan secretos embebidos.  

---

# 9. Relación con Documentos Normativos

- NGCVS — Nivel 0  
- SP-18 — Política principal  
- SP-01 Logging  
- SP-02 Monitoreo  
- SP-05 Health Checks  
- SP-15 Contenedores OCI  
- SP-17 Métricas  
- Checklist SP-18 — Nivel 3  

---

# FIN DEL MANUAL TÉCNICO MT-18
