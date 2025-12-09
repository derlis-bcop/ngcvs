# SP-01 — POLÍTICA DE LOGGING  
**Sub-Política Técnica de Nivel 1**  
**Gerencia de Tecnologías de la Información y Comunicaciones (TICs)**

## 1. Objetivo
Establecer los lineamientos obligatorios para la generación, estructuración, almacenamiento, transmisión y resguardo de logs en todos los sistemas desarrollados, mantenidos o integrados por la Gerencia de TICs.  
Busca garantizar trazabilidad, auditoría técnica, diagnóstico eficiente, seguridad operacional y uniformidad institucional.

## 2. Alcance
Esta política aplica a:
- Todos los desarrollos internos, externos, evolutivos o correctivos.  
- APIs, servicios, microservicios, backend, frontend, aplicaciones móviles, procesos batch y automatizaciones.  
- Infraestructura de monitoreo y plataformas de observabilidad.

Es de **cumplimiento obligatorio** y posee carácter normativo dentro de la Gerencia TICs.

## 3. Definiciones
- **Log estructurado:** Registro en formato clave-valor o JSON que permite análisis automatizado.  
- **Correlación:** Inclusión de identificadores de seguimiento (trace-id / correlation-id) para unir eventos de distintos componentes.  
- **Nivel de severidad:** Categoría formal del evento (INFO, WARN, ERROR, FATAL, DEBUG).  
- **PII:** Información personal identificable, con restricciones estrictas de registro.

## 4. Principios Normativos
1. **Todo sistema debe generar logs estructurados.**  
2. **Debe existir trazabilidad completa por medio de correlation-id obligatorio.**  
3. **Está prohibido almacenar información sensible o PII**, salvo autorización formal de Seguridad.  
4. **Los logs deben integrarse a la plataforma de monitoreo centralizada.**  
5. **El logging debe ser consistente entre ambientes** y mantener estándares de formato.  
6. **Los logs son evidencia técnica obligatoria** para auditorías y CI/CD.

## 5. Requisitos Técnicos Obligatorios

### 5.1 Formato de Logs
- El formato **JSON** es obligatorio para sistemas backend y microservicios.  
- Los campos mínimos son:
  - `timestamp`
  - `level`
  - `service`
  - `environment`
  - `correlation_id`
  - `message`
  - `exception` (si aplica)
  - `user` (si aplica)
  - `operation`
  - `duration_ms`
- Prohibido el uso de texto libre no estructurado, salvo INFO complementaria.

### 5.2 Niveles de Severidad
- **DEBUG:** Solo en ambientes DEV y QA.  
- **INFO:** Eventos estándar operativos.  
- **WARN:** Incidentes recuperables.  
- **ERROR:** Fallos funcionales o técnicos.  
- **FATAL:** Caída del servicio o corrupción de datos.

### 5.3 Correlation-ID
Todo sistema **debe generar o propagar** un `correlation_id` en cada request.  
Debe conservarse en:
- Logs de backend  
- Logs de frontend (cuando corresponda)  
- Integraciones inter-servicios  
- Mensajería asíncrona  

### 5.4 Resguardo de Logs
- Los logs deben enviarse de forma centralizada mediante:
  - Agentes de recolección
  - APIs de ingestión
  - Sockets seguros
- Está prohibido guardar logs en disco sin rotación ni cifrado.

### 5.5 Seguridad
- Prohibido registrar contraseñas, tokens, claves privadas, secretos o contenido de sesiones.  
- Los logs deben transmitirse mediante TLS en tránsito y almacenarse en repositorios protegidos.  
- Las políticas de retención se ajustarán al área de Seguridad y Matriz de Riesgo.

### 5.6 Integración con Plataforma de Observabilidad
Todo artefacto debe:
- Integrar logs con el sistema corporativo (SIEM, APM o equivalente).  
- Mantener índices separados por servicio y ambiente.  
- Emitir métricas clave derivadas del logging.

## 6. Lineamientos Operativos

### 6.1 Logging de Entradas y Salidas
Debe registrarse:
- Inicio y fin de cada operación relevante.  
- Duración de requests y procesos batch.  
- Resumen de payloads (pero nunca datos sensibles).

### 6.2 Logging de Errores
Cada error debe:
- Incluir stacktrace.  
- Indicar operación, usuario (si aplica), entorno y servicio.  
- Propagarse hasta la capa de observabilidad.

### 6.3 Logging para Seguridad
Registrar:
- Intentos fallidos de autenticación.  
- Accesos no autorizados.  
- Errores relacionados a integridad de datos.  
- Comportamientos anómalos detectados.

### 6.4 Logging para Auditoría
Debe existir capacidad de reconstruir:
- Secuencia de eventos.  
- Operaciones críticas.  
- Cambios de estado.  
- Acciones administrativas.

## 7. Responsabilidades

### 7.1 Desarrollo
- Implementar el framework de logging estándar.  
- Asegurar estructura, niveles y correlation-id.  
- Proveer documentación técnica por servicio.

### 7.2 QA
- Validar que los logs se generen correctamente.  
- Confirmar estructura JSON y niveles adecuados.  
- Documentar hallazgos en CI/CD.

### 7.3 Producción/Infraestructura
- Mantener la plataforma centralizada de ingestión.  
- Garantizar retención, rotación y capacidad.  
- Emitir alertas operativas basadas en logs.

### 7.4 Seguridad Informática
- Definir restricciones de PII y datos sensibles.  
- Revisar patrones de logging para cumplimiento.  
- Ejecutar auditorías periódicas.

## 8. Controles Normativos
- Ningún artefacto avanza a QA sin logging habilitado.  
- Ningún componente avanza a PRE-PROD sin integración a monitoreo centralizado.  
- Producción puede **vetar** despliegues si el logging:
  - No es estructurado  
  - Contiene PII indebida  
  - No cumple formato estándar  

## 9. Anexos
- Anexo A: Campos estándar de logging.  
- Anexo B: Política de retención y rotación.  
- Anexo C: Ejemplos de logs válidos/ inválidos.  
