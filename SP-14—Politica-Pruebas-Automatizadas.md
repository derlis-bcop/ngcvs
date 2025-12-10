# SP-14 — POLÍTICA DE PRUEBAS AUTOMATIZADAS  
**Sub-Política Técnica – Nivel 1**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Dependiente de: Norma General de Ciclo de Vida de Software (NGCVS)

---

# 1. Objetivo
Establecer el marco normativo obligatorio para el diseño, implementación, ejecución, seguimiento y mantenimiento de pruebas automatizadas en todos los sistemas desarrollados, integrados, adquiridos o mantenidos por la Gerencia de Tecnologías de la Información y Comunicaciones (TICs).

Esta política garantiza que el software cumpla con los niveles mínimos requeridos de calidad, estabilidad, resiliencia y seguridad antes de su promoción hacia ambientes superiores (QA, UAT, PRE-PROD, PROD).

---

# 2. Alcance
La presente Sub-Política aplica a:

- Todo software interno, externo, evolucionado o corregido.  
- Sistemas legacy modernizados.  
- Backend, frontend, microservicios, APIs, funciones programadas, contenedores y pipelines.  
- Equipos de Desarrollo, QA, Arquitectura, Seguridad TICs y Producción/Infraestructura.  
- Todos los ambientes institucionales: DEV, QA, UAT, PRE-PROD, PROD.  

---

# 3. Carácter Normativo
La presente Sub-Política es de **cumplimiento obligatorio**.  
Ningún artefacto podrá avanzar de ambiente ni ser considerado candidato para producción si no cumple **todas** las exigencias aquí establecidas y en su Manual Técnico asociado (MT-14).

Producción y Seguridad TICs mantienen poder de **veto**.

---

# 4. Definiciones

### Pruebas automatizadas  
Conjunto de validaciones ejecutadas automáticamente para verificar comportamientos esperados del software.

### Cobertura de pruebas  
Porcentaje de líneas, funciones, ramas o módulos ejercitados por las pruebas automatizadas.

### Pruebas unitarias  
Validan unidades de software aisladas (funciones, métodos, clases).

### Pruebas de integración  
Validan comunicación entre componentes, servicios, módulos o capas.

### Pruebas funcionales automatizadas  
Validan flujos funcionales completos de la aplicación.

### Pruebas de regresión  
Se ejecutan para garantizar que cambios no introduzcan comportamientos no deseados.

---

# 5. Principios Normativos

## 5.1 Automatización como práctica obligatoria
Todo proyecto debe contar con pruebas automatizadas mínimas, sin excepción, desde el inicio de su ciclo de vida.

## 5.2 Un pipeline no puede aprobarse sin pruebas
La ejecución automática de pruebas en el pipeline CI/CD es mandatoria.

## 5.3 Cobertura mínima obligatoria
La cobertura mínima de pruebas unitarias e integración es de:

**≥ 80%**,  
según lineamientos solicitados en NGCVS.

## 5.4 Pruebas como documento vivo
Las pruebas deben mantenerse actualizadas ante cualquier cambio de código.

## 5.5 Trazabilidad
Cada prueba automatizada debe vincularse con los requerimientos funcionales y técnicos.

## 5.6 Fail-fast obligatorio
Cualquier falla en pruebas automatizadas detiene el pipeline inmediatamente.

## 5.7 Independencia de los ambientes
Las pruebas deben ser reproducibles y determinísticas, sin dependencia de datos mutables o no controlados.

---

# 6. Políticas Obligatorias

## 6.1 Inclusión de pruebas desde el inicio
- Cada nueva funcionalidad, corrección o refactorización debe incluir pruebas.  
- La ausencia de pruebas constituye un incumplimiento normativo.  

## 6.2 Tipos de pruebas obligatorias
Todo proyecto deberá incluir:

1. **Pruebas Unitarias** (obligatorias).  
2. **Pruebas de Integración** (obligatorias).  
3. **Pruebas de Regresión Automáticas** (obligatorias).  
4. **Pruebas Funcionales Automatizadas** (cuando aplique).  
5. **Pruebas de Carga/Performance Automatizadas** (para sistemas críticos).  
6. **Pruebas de Seguridad Automatizadas** (SAST, análisis de dependencias, validadores de políticas).

---

## 6.3 Controles de calidad en CI/CD
Los pipelines CI/CD deben:

- Ejecutar pruebas unitarias automáticamente.  
- Ejecutar pruebas de integración en ambientes controlados.  
- Ejecutar regresión automatizada en QA/UAT según criticidad.  
- Registrar resultados completos como evidencias.  
- Impedir despliegue cuando existan fallas o niveles de cobertura insuficientes.  

---

## 6.4 Cobertura de código
- La cobertura mínima es **≥ 80%** para aprobación del pipeline.  
- Sistemas críticos pueden requerir cobertura superior (definido por Arquitectura y Seguridad TICs).  
- La cobertura debe ser visible en reportes CI/CD y almacenada como evidencia.

---

## 6.5 Mantenibilidad
- Las pruebas deben ser legibles, mantenibles y estables.  
- No se aceptan pruebas frágiles o que dependan de tiempos, aleatoriedad o datos no controlados.  
- Las pruebas deben ejecutarse en tiempos razonables para no bloquear pipelines.

---

## 6.6 Datos de prueba
- Los datos utilizados deben ser controlados, seguros y reproducibles.  
- Está prohibido usar datos personales reales o información sensible.  

---

## 6.7 Responsabilidades

### Desarrollo
- Crear y mantener pruebas automatizadas de todos los módulos nuevos.  
- Garantizar cobertura mínima.  
- Asegurar estabilidad y reproducibilidad de pruebas.

### QA
- Validar cobertura, estabilidad y trazabilidad.  
- Crear pruebas funcionales automatizadas cuando aplique.  
- Verificar regresión automatizada antes de liberar a UAT.

### Arquitectura
- Definir marcos de pruebas autorizados para cada tecnología.  
- Revisar calidad técnica de suites de pruebas.

### Seguridad TICs
- Definir suites de pruebas de seguridad automatizadas obligatorias.  
- Revisar cumplimiento de validadores estáticos.

### Producción/Infraestructura
- Mantener agentes y entornos para ejecución de pruebas automatizadas.  
- Impedir promoción si los resultados del pipeline no cumplen la política.

---

# 7. Controles Mandatorios

### Dev → QA
- Pruebas unitarias ejecutadas  
- Cobertura ≥ 80%  
- Pruebas de integración exitosas  

### QA → UAT
- Regresión automatizada completa  
- Evidencias adjuntas  
- QA emite aprobación formal  

### UAT → PRE-PROD
- Flujo funcional crítico validado automáticamente  
- No existen pruebas fallando  

### PRE-PROD → PROD
- Producción y Seguridad confirman cumplimiento  
- No se permite excepción para cobertura < 80% sin documento aprobado por Seguridad y Arquitectura  

---

# 8. Auditoría

- Todos los resultados deben conservarse ≥ 12 meses.  
- Los reportes deben estar accesibles para auditorías internas/externas.  
- El estado de pruebas debe poder reconstruirse para cualquier versión histórica del software.  

---

# 9. Excepciones
Toda excepción debe:

1. Ser solicitada por Desarrollo  
2. Ser evaluada por Arquitectura  
3. Ser aprobada formalmente por Seguridad TICs  
4. Ser registrada junto con el artefacto en CI/CD  
5. Tener vigencia temporal y reevaluación periódica  

Las excepciones son **casos extraordinarios**, no una práctica permitida.

---

# 10. Relación con otros documentos

- NGCVS — Norma General  
- SP-04 — Artefactos, CI/CD y Versionamiento  
- SP-06 — Seguridad del Pipeline  
- SP-07 — Secrets Management  
- MT-14 — Manual Técnico de Pruebas Automatizadas  
- Checklists asociados  

---

# FIN DEL DOCUMENTO — SP-14
