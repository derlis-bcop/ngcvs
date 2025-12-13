# CHECKLIST SP-18 — ARTEFACTOS OPERABLES  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-18 y MT-18

Este checklist es obligatorio para auditar cualquier artefacto antes de su promoción entre ambientes (DEV → QA → UAT → PRE-PROD → PROD).  
El incumplimiento de cualquiera de los puntos obligatorios implica rechazo automático del artefacto.

---

# 1. PROHIBICIONES — VERIFICACIÓN INICIAL

## 1.1 Ejecución manual (prohibido)
- [ ] El artefacto NO requiere ejecutar comandos manuales para iniciar.  
- [ ] El artefacto NO requiere interacción CLI.  
- [ ] NO utiliza `nohup`, `screen`, `tmux`, `&` ni técnicas de background manual.  
- [ ] NO requiere acceso SSH para arrancar.  
- [ ] NO depende de scripts shell no supervisados.  

## 1.2 Dependencia de supervisión humana (prohibido)
- [ ] No requiere reinicio manual ante fallas.  
- [ ] No depende de monitoreo manual.  
- [ ] No requiere validaciones humanas para continuar procesos.  

**Si alguno falla → ARTEFACTO RECHAZADO.**

---

# 2. EJECUCIÓN COMO SERVICIO PERSISTENTE (OBLIGATORIO)

- [ ] Existe archivo de definición de servicio (`systemd`, `supervisord`, equivalente institucional).  
- [ ] El servicio inicia automáticamente al boot del host.  
- [ ] El servicio tiene restart policy activa.  
- [ ] El servicio puede reiniciarse sin intervención humana.  
- [ ] Logs del servicio están unificados y estructurados.  

---

# 3. EJECUCIÓN EN CONTENEDORES OCI (SI APLICA)

## 3.1 Dockerfile
- [ ] Usa imagen base aprobada institucionalmente.  
- [ ] Usa usuario NO root.  
- [ ] Dependencias fijadas.  
- [ ] No contiene secretos.  

## 3.2 Runtime del contenedor
- [ ] Tiene `restart: always` o `on-failure`.  
- [ ] Expone health checks obligatorios.  
- [ ] Maneja correctamente señales del sistema (SIGTERM, SIGINT).  
- [ ] Se recupera automáticamente ante desconexión de BD.  
- [ ] Se recupera ante errores de red.  

## 3.3 Observabilidad
- [ ] Logs estructurados dentro del contenedor.  
- [ ] Métricas técnicas expuestas.  
- [ ] Trazas distribuidas activas.  

---

# 4. HEALTH CHECKS (OBLIGATORIO)

- [ ] Existe `/health` (estado general).  
- [ ] Existe `/live` (el proceso está vivo).  
- [ ] Existe `/ready` (puede recibir tráfico).  
- [ ] Los health checks devuelven formato estándar.  
- [ ] Los health checks no exponen detalles internos o stack traces.  
- [ ] El pipeline CI/CD valida el correcto funcionamiento de cada endpoint.  

---

# 5. RECUPERACIÓN AUTOMÁTICA (ZERO-TOUCH RECOVERY)

- [ ] El artefacto tiene retry automático ante fallos de BD.  
- [ ] Posee backoff exponencial (si aplica).  
- [ ] Posee reconexión automática de red.  
- [ ] Tareas idempotentes o reintentables.  
- [ ] No requiere intervención humana para recuperarse.  
- [ ] El servicio se reinicia automáticamente al fallar.  
- [ ] El servicio se estabiliza solo después del restart.  

**Si el artefacto requiere intervención humana → RECHAZADO.**

---

# 6. OBSERVABILIDAD

## 6.1 Logs
- [ ] Logs estructurados (JSON u otro formato institucional).  
- [ ] Incluyen `trace_id` y `correlation_id`.  
- [ ] No incluyen datos sensibles.  
- [ ] Son legibles por la plataforma de monitoreo.  

## 6.2 Métricas
- [ ] Expone métricas técnicas obligatorias (latencia, errores, disponibilidad).  
- [ ] Expone métricas de producto (si aplica).  
- [ ] Formato compatible con monitoreo institucional.  

## 6.3 Trazabilidad
- [ ] Implementa tracing distribuido.  
- [ ] Cada request incluye identificadores únicos.  

---

# 7. SUPERVISIÓN Y MONITOREO

- [ ] El servicio aparece registrado en monitoreo institucional.  
- [ ] El estado del servicio puede consultarse sin SSH.  
- [ ] Existen alertas configuradas.  
- [ ] Dashboards operativos disponibles.  
- [ ] El servicio registra eventos críticos.  

---

# 8. CI/CD — VALIDACIONES AUTOMÁTICAS

- [ ] El pipeline verifica restart automático.  
- [ ] El pipeline verifica health checks.  
- [ ] El pipeline verifica ausencia de ejecución manual.  
- [ ] El pipeline valida despliegue reproducible.  
- [ ] El pipeline valida ausencia de secretos.  
- [ ] El pipeline vincula el artefacto a un commit, versión y SBOM.  

**Si el pipeline falla → ARTEFACTO RECHAZADO.**

---

# 9. DOCUMENTACIÓN OBLIGATORIA

- [ ] Documento de despliegue.  
- [ ] Documento de rollback.  
- [ ] Especificación del servicio o contenedor.  
- [ ] Requisitos de infraestructura.  
- [ ] Lista de dependencias.  
- [ ] Consideraciones de resiliencia.  

---

# 10. PROCESOS BATCH (SI APLICA)

- [ ] Se ejecutan como servicio o contenedor.  
- [ ] Registro de inicio y fin.  
- [ ] Registro de cantidad procesada.  
- [ ] Retry automático.  
- [ ] Control de concurrencia.  
- [ ] Integrados a monitoreo.  

Queda prohibido ejecutar batch desde cron local del servidor sin supervisión.

---

# 11. ANTI-PATRONES (DETECCIÓN OBLIGATORIA)

- [ ] No existen scripts manuales.  
- [ ] No existen procesos que mueren en silencio.  
- [ ] No existen dependencias de logs locales no centralizados.  
- [ ] No existen dependencias del PATH del usuario.  
- [ ] No existen procesos sin supervisión.  
- [ ] No existen artefactos que requieran validaciones humanas.  

---

# 12. RESULTADO FINAL DE AUDITORÍA

- [ ] **APROBADO** — El artefacto es operable y cumple SP-18.  
- [ ] **NO APROBADO** — Incumplimientos detectados.  
- [ ] **NO APTO PARA PROMOCIÓN DE AMBIENTE**.

Observaciones:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-18  
