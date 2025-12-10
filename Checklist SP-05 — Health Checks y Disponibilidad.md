# CHECKLIST SP-05 — HEALTH CHECKS Y DISPONIBILIDAD  
**Nivel 3 — Checklist Técnico Normativo**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)

Este checklist debe completarse **antes de promover cualquier artefacto hacia QA, UAT, PRE-PROD o PROD**.  
El incumplimiento de cualquier punto obligatorio impide avanzar entre ambientes.

---

# 1. ENDPOINTS OBLIGATORIOS

## 1.1 Existencia
- [ ] `/live` implementado correctamente  
- [ ] `/ready` implementado correctamente  
- [ ] `/health` (o `/healthz`) implementado correctamente  

## 1.2 Validación técnica
- [ ] `/live` devuelve estado inmediato del proceso  
- [ ] `/ready` valida disponibilidad real del servicio  
- [ ] `/health` devuelve:
  - [ ] Estado UP/DOWN/DEGRADED  
  - [ ] Versión del servicio  
  - [ ] Timestamp  
  - [ ] Dependencias verificadas  

---

# 2. ESTADOS DE SALUD

- [ ] El servicio reporta `UP` únicamente si todas las dependencias críticas están operativas  
- [ ] Reporta `DEGRADED` si una dependencia no crítica está caída  
- [ ] Reporta `DOWN` si una dependencia crítica falla  
- [ ] Latencias altas generan estados `DEGRADED` (si corresponde)  

---

# 3. VALIDACIÓN DE DEPENDENCIAS

## 3.1 Base de datos
- [ ] Se ejecuta consulta mínima para validar conexión  
- [ ] Timeout ≤ 100ms en QA/PROD  
- [ ] Errores capturados sin exponer detalles sensibles  

## 3.2 Cache (Redis u otro)
- [ ] Validación de ping  
- [ ] Verificación opcional de claves críticas  

## 3.3 Servicios externos
- [ ] Validación HTTP liviana (HEAD o GET corto)  
- [ ] Timeout ≤ 1 segundo  
- [ ] Manejo seguro de errores  

## 3.4 Filesystem
- [ ] Validación de espacio disponible  
- [ ] Verificación de permisos de escritura  

---

# 4. PRUEBAS DE QA SOBRE HEALTH CHECKS

## 4.1 Condiciones normales
- [ ] `/health` responde con latencia < 200 ms  
- [ ] `/ready` responde con estado UP  
- [ ] `/live` responde sin errores  

## 4.2 Simulación de fallos
- [ ] Base de datos caída → `/health` detecta y reporta correctamente  
- [ ] Cache caída → `/health` reporta `DEGRADED` o `DOWN` según criticidad  
- [ ] Servicio externo caído → estado reflejado  
- [ ] Filesystem lleno → estado reflejado  

## 4.3 Verificación post-despliegue
- [ ] `/ready` evalúa readiness correctamente  
- [ ] `/health` refleja cambios en dependencias  
- [ ] Logs muestran validaciones realizadas  

---

# 5. INTEGRACIÓN CON ORQUESTADORES (Opcional pero recomendado)

## 5.1 Configuración de probes (si aplica)
- [ ] Liveness probe configurado  
- [ ] Readiness probe configurado  
- [ ] Probes con valores adecuados (timeouts, delays, retries)  

### 5.2 Validaciones recomendadas
- [ ] Rolling updates no exponen tráfico a pods no listos  
- [ ] Blue/Green verifica `/health` antes del switch  
- [ ] Canary testing monitorea degradación  

---

# 6. INTEGRACIÓN CON PLATAFORMA DE MONITOREO

- [ ] Health checks se reportan en la plataforma de observabilidad  
- [ ] Alertas creadas para:
  - [ ] Estado DOWN  
  - [ ] Estado DEGRADED  
  - [ ] Timeouts  
- [ ] Observabilidad incluye métricas asociadas  

---

# 7. CI/CD — VALIDACIONES AUTOMÁTICAS

- [ ] Pipeline invoca `/ready` antes de finalizar despliegue  
- [ ] Pipeline invoca `/health` en smoke test inicial  
- [ ] CI/CD falla si:
  - [ ] `/health` no responde  
  - [ ] `/ready` no está UP  
  - [ ] El formato del JSON es inválido  
- [ ] Evidencia archivada con resultados  

---

# 8. PRODUCCIÓN / OPERACIÓN

- [ ] Monitoreo 24/7 recibe alertas basadas en health checks  
- [ ] Health checks permiten prever degradación  
- [ ] Documentación operativa actualizada con dependencias y criterios  

---

# 9. AUDITORÍA

- [ ] Auditoría puede consultar histórico de health checks  
- [ ] Logs contienen trazabilidad suficiente  
- [ ] Estados DEGRADED tienen explicación registrada  
- [ ] Versiones del servicio están reflejadas en `/health`  

---

# 10. ERRORES QUE IMPIDEN PROMOVER AMBIENTE

- [ ] No existe `/health`  
- [ ] No existe `/ready`  
- [ ] No existe `/live`  
- [ ] Health check no valida dependencias  
- [ ] Respuesta no es JSON válido  
- [ ] Estados no cumplen la norma (UP/DOWN/DEGRADED)  
- [ ] Tiempo de respuesta excesivo  
- [ ] CI/CD no ejecuta validaciones de salud  

---

# 11. VEREDICTO FINAL

- [ ] **APROBADO PARA PROMOCIÓN DE AMBIENTE**  
- [ ] **NO APROBADO — INCUMPLIMIENTOS DETECTADOS**

Lista de incumplimientos:
- _______________________________________________
- _______________________________________________
- _______________________________________________

---

# FIN DEL CHECKLIST SP-05 — HEALTH CHECKS Y DISPONIBILIDAD
