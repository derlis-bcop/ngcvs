# SP-17 — POLÍTICA DE MÉTRICAS DE PRODUCTO Y TELEMETRÍA  
**Sub-Política Técnica — Nivel 1**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Dependiente de: NGCVS — Norma General de Ciclo de Vida de Software

---

# 1. Objetivo
Establecer las directrices normativas obligatorias para la captura, procesamiento, publicación, almacenamiento y uso de métricas de producto y telemetría, con el fin de garantizar la observabilidad, mejora continua, toma de decisiones basada en datos, trazabilidad y la operación segura de los sistemas desarrollados por la Gerencia de TICs.

---

# 2. Alcance
Esta Sub-Política aplica a:

- Todos los sistemas, APIs, microservicios, contenedores, aplicaciones móviles, web o backend.  
- Sistemas nuevos, evolutivos, legados y adquiridos.  
- Ambientes DEV, QA, UAT, PRE-PROD y PROD.  
- Equipos de Desarrollo, QA, Arquitectura, Seguridad TICs y Producción/Infraestructura.  

Incluye tanto **métricas técnicas** como **métricas de producto**.

---

# 3. Carácter Normativo
La presente Sub-Política es de **cumplimiento obligatorio** dentro de la Gerencia de TICs.  
Ningún artefacto podrá ser promovido a ambientes superiores si incumple los requisitos aquí definidos o los de su Manual Técnico asociado (MT-17).

Producción e Infraestructura mantienen **poder de veto** sobre cualquier despliegue que no provea métricas o telemetría adecuadas.

---

# 4. Definiciones

### Métricas de Producto  
Indicadores que miden comportamiento funcional, uso, experiencia de usuario, tiempos de operación, volúmenes procesados y desempeño del producto digital.

### Métricas Técnicas  
Indicadores que miden rendimiento, capacidad, errores, latencias, recursos, disponibilidad y estabilidad operativa.

### Telemetría  
Conjunto de señales emitidas por el servicio en tiempo real o diferido, destinadas al monitoreo, diagnóstico y auditoría.

### Instrumentación  
Proceso de agregar métricas, logs estructurados, trazas o eventos técnicos a un sistema.

### Sistema de Observabilidad  
Plataforma institucional donde se almacenan y visualizan métricas, logs, trazas y eventos.

---

# 5. Principios Normativos

## 5.1 Observabilidad obligatoria
Todo sistema debe ser observable por diseño y desde su primera versión.

## 5.2 Métricas como artefacto crítico
La ausencia de métricas o telemetría invalida un despliegue.

## 5.3 Telemetría sin impacto negativo
La recolección de métricas no debe generar degradación del servicio.

## 5.4 Datos no sensibles
La telemetría jamás debe incluir datos personales, confidenciales o sensibles.

## 5.5 Homogeneidad
Todas las métricas deben seguir esquemas de nombres, formatos y convenciones institucionales.

## 5.6 Inmutabilidad y auditabilidad
Las métricas deben conservar histórico suficiente para auditorías operativas y regulatorias.

---

# 6. Políticas Obligatorias

## 6.1 Métricas Técnicas
Todo sistema debe exponer métricas como mínimo:

1. Disponibilidad del servicio.  
2. Latencia promedio, p95, p99.  
3. Throughput y número de peticiones por segundo.  
4. Errores (4xx, 5xx) y tasas de fallos.  
5. Uso de CPU, memoria y disco (si aplica).  
6. Tiempos de procesamiento de operaciones clave.  
7. Métricas de colas, jobs o batch.  
8. Retry, timeouts y circuit breakers.

---

## 6.2 Métricas de Producto
Los sistemas deben capturar métricas específicas del dominio funcional, tales como:

- Eventos relevantes en el flujo del usuario.  
- Volumen de transacciones procesadas.  
- Uso por tipo de operación.  
- Cantidad de usuarios activos.  
- KPI definidos por el Product Owner.  

Todas deben cumplir requisitos de privacidad y anonimización.

---

## 6.3 Exposición de métricas
Toda aplicación debe:

- Exponer un endpoint `/metrics` o equivalente.  
- Exponer métricas en formato institucional (basado en Prometheus o equivalente).  
- Ofrecer métricas legibles por agentes de observabilidad.  
- No requerir autenticación para métricas técnicas básicas (según normativa de infraestructura).  
- Requerir autenticación para métricas internas o sensibles del producto.  

---

## 6.4 Telemetría y Trazabilidad
Los sistemas deben registrar:

- Eventos de negocio relevantes.  
- Trazas de requests distribuidas.  
- Identificadores únicos por transacción.  
- Eventos críticos de operación (errores, degradaciones, timeouts).  

La telemetría debe cumplir las políticas:

- SP-01 (Logging)  
- SP-02 (Monitoreo)  
- SP-06 (Seguridad del CI/CD)  
- SP-07 (Secrets Management)

---

## 6.5 Datos Prohibidos en Métricas
Está estrictamente prohibido incluir:

- Nombres, documentos, correos, teléfonos u otros datos personales.  
- Datos confidenciales o financieros.  
- Tokens, claves, hashes, firmas o parámetros de autenticación.  
- Características técnicas internas que puedan comprometer seguridad.

---

## 6.6 Ingesta y Almacenamiento
Toda métrica debe ser:

- Enviada a la plataforma institucional de observabilidad.  
- Almacenada con políticas de retención definidas (mín. 12 meses).  
- Registrada con timestamp, nombre normalizado
