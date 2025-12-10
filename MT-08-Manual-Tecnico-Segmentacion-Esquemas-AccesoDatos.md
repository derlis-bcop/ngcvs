# MT-08 — MANUAL TÉCNICO DE SEGMENTACIÓN DE ESQUEMAS Y ACCESO A DATOS  
**Manual Técnico de Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: **SP-08 Sub-Política de Segmentación de Esquemas y Acceso a Datos**

---

# 1. Propósito Operativo

Este manual establece los lineamientos técnicos, configuraciones mínimas, ejemplos prácticos, flujos de permisos, diseño de objetos de interfaz y validaciones necesarias para implementar **segmentación estricta de esquemas** en Oracle:

- CORE  
- INTERFAZ  
- SATÉLITE  

El objetivo es garantizar:

- Aislamiento lógico del sistema CORE  
- Acceso seguro y controlado  
- Auditoría completa  
- Evitar exposición indebida de datos  
- Cumplimiento de normas internas de TICs  

---

# 2. Arquitectura Base del Modelo de Segmentación

## 2.1 Esquema CORE (`CORE_<sistema>`)
Contiene:
- Tablas maestras  
- Tablas transaccionales  
- Vistas internas  
- Paquetes internos  
- Funciones internas  
- Triggers  
- Tipos de datos  

**Prohibido** crear allí:
- Objetos de aplicaciones satélite  
- Sinónimos públicos  
- Tablas temporales utilizadas por satélites  

---

## 2.2 Esquema INTERFAZ (`INT_<sistema>`)
Contiene:
- Vistas autorizadas  
- Sinónimos controlados  
- Paquetes WRAPPER que exponen lógica del CORE  
- Funciones controladas  
- Validaciones de parámetros de entrada  

Debe contener **únicamente objetos de exposición**, nunca tablas.

---

## 2.3 Esquemas SATÉLITE (`SAT_<app>`)
Contienen:
- Tablas propias  
- Paquetes propios  
- Procedimientos de negocio específicos de la aplicación  
- Sinónimos privados hacia objetos de interfaz  

**Prohibido**, sin excepciones:
- Acceso directo al CORE  
- Tablas duplicadas del CORE  
- Exposición de objetos a otros satélites  

---

# 3. Usuarios y Credenciales por Ambiente

## 3.1 Usuario por aplicación satélite
Cada aplicación debe poseer:

| Ambiente | Usuario |
|----------|---------|
| DEV | SAT_APP1_DEV |
| QA | SAT_APP1_QA |
| UAT | SAT_APP1_UAT |
| PRE-PROD | SAT_APP1_PRE |
| PROD | SAT_APP1_PROD |

Cada uno:
- Tiene permisos mínimos  
- Nunca comparte contraseña  
- Utiliza credenciales almacenadas en gestor de secretos (SP-07)  

---

# 4. Flujos de Acceso Permitidos

El único flujo permitido es:

SATÉLITE → INTERFAZ → CORE

Representación simple:

\```
SAT_APP  --->  INT_CORE  --->  CORE
\```

Cualquier acceso directo:

\```
SAT_APP  --->  CORE   ❌ PROHIBIDO
\```

---

# 5. GRANTs Permitidos y Prohibidos

## 5.1 GRANTs permitidos

### (A) Acceso a vistas de interfaz
\```
GRANT SELECT ON INT_CORE.VW_CLIENTES TO SAT_APP1_PROD;
\```

### (B) Acceso a paquetes de interfaz
\```
GRANT EXECUTE ON INT_CORE.PKG_OPERACIONES TO SAT_APP1_PROD;
\```

---

## 5.2 GRANTs estrictamente prohibidos

### (A) Acceso directo al CORE
\```
GRANT SELECT ON CORE.TB_CLIENTES TO SAT_APP1_PROD; -- PROHIBIDO
\```

### (B) Acceso a tablas internas del CORE
\```
GRANT INSERT ON CORE.TB_OPERACIONES TO SAT_APP1_PROD; -- PROHIBIDO
\```

---

# 6. Diseño de Objetos de INTERFAZ

## 6.1 Vistas controladas
Ejemplo de vista segura:

\```
CREATE OR REPLACE VIEW INT_CORE.VW_CLIENTES AS
SELECT
    c.id_cliente,
    c.nombre,
    c.documento,
    CASE 
        WHEN c.tipo = 'VIP' THEN 'RESTRINGIDO'
        ELSE c.estado
    END AS estado
FROM CORE.CLIENTES c;
\```

### Normas:
- No exponer columnas sensibles  
- Aplicar filtros relevantes  
- Enmascarar datos según SP de Seguridad  

---

## 6.2 Paquetes wrapper

Ejemplo:

\```
CREATE OR REPLACE PACKAGE INT_CORE.PKG_OPERACIONES AS
  PROCEDURE registrar_operacion(
     p_cliente_id NUMBER,
     p_monto NUMBER,
     p_res OUT VARCHAR2
  );
END PKG_OPERACIONES;
\```

Los wrappers:
- Validan parámetros  
- Aplican permisos  
- No exponen tablas del CORE  
- Documentan restricciones  

---

# 7. Control de Cambios y Flujo de Aprobación

## 7.1 Solicitud de Nuevos Objetos de Interfaz
Pasos:

1. Desarrollador presenta requerimiento formal.  
2. Arquitectura analiza impacto.  
3. Seguridad revisa exposición.  
4. DBA implementa el objeto.  
5. QA valida funcionalidad.  
6. Producción autoriza su uso en ambientes superiores.  

---

# 8. Validaciones en CI/CD

El pipeline debe:

- Revisar que las cadenas de conexión NO apunten al CORE.  
- Validar que solo se conecte con usuario satélite.  
- Verificar que no existan queries directas al CORE:

Ejemplo de regla estática:

\```
grep -R "FROM CORE\." -n src/ && exit 1
\```

- Validar que los GRANTs no se otorguen a esquemas incorrectos.  

---

# 9. Auditoría y Monitoreo de Accesos

Los DBAs deben auditar:

- SELECT, INSERT, UPDATE, DELETE ejecutados contra INTERFAZ  
- Cualquier intento de acceso al CORE  
- Cambios en permisos  
- Frecuencia de acceso por usuario  

Registros deben guardarse mínimo 12 meses.

---

# 10. Patrones Recomendados

## 10.1 Vistas de proyección
Exponer sólo lo necesario.

## 10.2 Wrappers con validación
Centralizar la lógica de negocio en CORE, pero exponer de forma limitada.

## 10.3 Minimizar cardinalidad
No permitir acceso a tablas completas.

## 10.4 Evitar joins complejos en interfaz
Los joins internos deben permanecer en el CORE.

---

# 11. Restricciones Importantes

- ❌ No se puede otorgar `SELECT ANY TABLE`  
- ❌ No se puede utilizar sinónimos públicos  
- ❌ No se pueden usar usuarios compartidos entre aplicaciones  
- ❌ No se pueden almacenar secretos en cadenas de conexión  

---

# 12. Ejemplo de Estructura de Carpetas en SVN

\```
/database
   /core
      tables/
      packages/
   /interface
      views/
      wrappers/
   /satellite
      app1/
      app2/
\```

---

# 13. Checklist de Validación Técnica

(Complemento del checklist normativo)

- [ ] Esquemas creados correctamente  
- [ ] Usuarios creados por ambiente  
- [ ] No existen GRANTs directos al CORE  
- [ ] Todas las consultas pasan por INTERFAZ  
- [ ] Vista o paquete validado por Arquitectura y Seguridad  
- [ ] Cadenas de conexión correctas  
- [ ] Auditoría de acceso habilitada  
- [ ] Evidencias cargadas en CI/CD  

---

# 14. Esquema Visual (Mermaid)

\```mermaid
flowchart TB
    SAT[Esquema SATÉLITE] -->|SELECT / EXECUTE| INT[Esquema INTERFAZ]
    INT -->|Vistas / Paquetes| CORE[Esquema CORE]

    classDef core fill:#ffcccc,stroke:#990000;
    classDef interfaz fill:#ccffcc,stroke:#006600;
    classDef sat fill:#ccccff,stroke:#000099;

    class CORE core
    class INT interfaz
    class SAT sat
\```

---

# FIN DEL MANUAL MT-08
