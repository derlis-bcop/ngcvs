# MT-14 — MANUAL TÉCNICO DE PRUEBAS AUTOMATIZADAS  
**Manual Técnico — Nivel 2**  
Gerencia de Tecnologías de la Información y Comunicaciones (TICs)  
Vinculado a: SP-14 — Sub-Política de Pruebas Automatizadas

---

# 1. Propósito Operativo
Este manual establece los lineamientos técnicos obligatorios para diseñar, escribir, ejecutar, validar, reportar y mantener pruebas automatizadas en los sistemas desarrollados por la Gerencia de TICs.

Define herramientas, estándares, configuraciones, patrones, restricciones, ejemplos, buenas prácticas y validaciones operativas necesarias para garantizar:

- Calidad del software  
- Confiabilidad  
- Seguridad  
- Estabilidad  
- Reproducibilidad  
- Cumplimiento normativo  

---

# 2. Categorías de Pruebas Automatizadas

## 2.1 Pruebas Unitarias (obligatorias)
Validan funciones, métodos, clases o pequeños componentes, aislados de dependencias externas.

Características:
- Rápidas (< 100 ms)  
- Determinísticas  
- No acceden a BD real  
- No dependen de red  

Ejemplo Python (pytest):

\```
def test_sum():
    assert suma(1, 2) == 3
\```

---

## 2.2 Pruebas de Integración (obligatorias)
Validan interacción entre componentes del sistema.

Características:
- Pueden incluir BD, colas, APIs  
- Se ejecutan en ambientes controlados  
- Verifican compatibilidad entre módulos  

Ejemplo Java (Spring Boot):

\```
@RunWith(SpringRunner.class)
@SpringBootTest
public class ClienteIT {
  @Autowired ClienteService service;

  @Test
  public void testCrearCliente() {
      Cliente c = service.crear("Juan", "123");
      assertNotNull(c.getId());
  }
}
\```

---

## 2.3 Pruebas Funcionales Automatizadas (cuando aplica)

Utilizadas para validar flujos de negocio completos.

Ejemplo (Selenium):

\```
driver.get("/login");
driver.findElement("#usuario").sendKeys("test");
driver.findElement("#clave").sendKeys("123");
driver.findElement("#btnLogin").click();
assertTrue(driver.getPageSource().contains("Bienvenido"));
\```

---

## 2.4 Pruebas de Regresión Automatizadas (obligatorias)

- Detectan impactos colaterales luego de cambios  
- Se ejecutan en QA/UAT  
- Deben cubrir funcionalidades críticas  

---

## 2.5 Pruebas de Performance (cuando aplica)

Herramientas sugeridas:
- JMeter  
- k6  
- Locust  

Tipos:
- Stress  
- Load  
- Spike  
- Soak  

---

## 2.6 Pruebas de Seguridad Automatizadas

Incluyen:
- SAST  
- Análisis de dependencias  
- Validaciones anti-secreto  
- OWASP baseline scans  

---

# 3. Estructura de Proyecto Requerida

Toda aplicación debe respetar un estándar de archivos. Ejemplos:

## Python
\```
/app
/tests
    /unit
    /integration
pytest.ini
\```

## Java (Maven)
\```
src/main/java
src/test/java
src/integrationTest/java
pom.xml
\```

## Node.js
\```
/src
/tests/unit
/tests/integration
jest.config.js
\```

---

# 4. Cobertura de Pruebas

## 4.1 Requisito obligatorio
- La cobertura mínima es **≥ 80%**  
- El pipeline CI/CD debe fallar si no se cumple  

## 4.2 Herramientas sugeridas
- Java → JaCoCo  
- Python → Coverage.py  
- JavaScript → Istanbul/NYC  
- .NET → Coverlet  

## 4.3 Evidencias
El pipeline debe almacenar:

- Reportes HTML  
- Reportes XML/JSON  
- Porcentaje total y por módulo  

---

# 5. Datos de Prueba

Reglas obligatorias:
- No utilizar datos reales o personales  
- Datos deben ser determinísticos  
- Datos deben residir en fixtures o constructores controlados  
- No depender de servicios externos reales  

Ejemplo de fixture:

\```
@pytest.fixture
def cliente_valido():
    return {"nombre": "Juan", "documento": "123"}
\```

---

# 6. Mocking y Stubbing

Debe utilizarse cuando:
- Se necesita aislar dependencias  
- Un servicio externo es lento o inestable  
- No es posible acceder a BD real  

Ejemplos:

Python:
\```
@patch("servicio.externo.get")
def test_api(mock):
    mock.return_value.json.return_value = {"ok": true}
\```

Node.js:
\```
sinon.stub(db, "query").returns([{ id: 1 }]);
\```

---

# 7. Integración con CI/CD

## 7.1 Antes de compilar
- Verificación sintáctica  
- Lint  
- Validación de estructura  

## 7.2 Durante el build
- Ejecución de pruebas unitarias  
- Análisis de cobertura  
- Evidencias generadas  

## 7.3 Antes de despliegue
- Pruebas de integración  
- Pruebas funcionales críticas  
- Regresión automatizada  
- Validación de performance (si aplica)  

Cada falla → **pipeline fallido**.

---

# 8. Auditoría y Trazabilidad

Requisitos:

- Mantener reportes como parte del artefacto  
- Registrar versión de código asociada  
- Registrar fecha/hora y responsable  
- Almacenar resultados ≥ 12 meses  
- Permitir reconstrucción histórica de cobertura por versión  

---

# 9. Buenas Prácticas Obligatorias

- Cada bug corregido debe incluir una prueba que reproduzca el error  
- Las pruebas deben ser autoexplicativas  
- Un test = un caso específico  
- Evitar sleeps innecesarios  
- Evitar dependencias de tiempo o concurrencia  
- Mantener suites rápidas (< 2 minutos en dev)  

---

# 10. Anti-Patrones (Prohibido)

- Usar BD real de producción  
- Pruebas que dependen del orden de ejecución  
- Pruebas que dependen de conectividad externa real  
- Pruebas intermitentes (“flaky tests”)  
- Pruebas que modifican datos compartidos  
- Pruebas que requieren intervención humana  
- No versionar las pruebas  

---

# 11. Reglas de Aprobación entre Ambientes

## DEV → QA
- Pruebas unitarias completas  
- Cobertura ≥ 80%  
- Sin fallos en integración mínima  

## QA → UAT
- Regresión automatizada completa  
- Pruebas funcionales listas  

## UAT → PRE-PROD
- Performance validada si aplica  
- Pruebas críticas automatizadas sin fallas  

## PRE-PROD → PROD
- Seguridad aprueba resultados  
- Producción verifica estabilidad  
- No existen pruebas fallando  
- Se presenta evidencia completa del pipeline CI/CD  

---

# 12. Frameworks Homologados (Ejemplos)

### Python
- PyTest  
- unittest + coverage  

### Java
- JUnit 5  
- Mockito  
- Spring Test  

### Node.js
- Jest  
- Mocha/Chai  
- Supertest  

### .NET
- xUnit  
- MSTest  
- NUnit  

---

# 13. Ejemplo de Pipeline Seguro de Pruebas

\```
stages:
  - build
  - test
  - coverage
  - integration
  - package

test:
  script:
    - pytest --disable-warnings -q
    - coverage run -m pytest
    - coverage xml
  artifacts:
    paths:
      - coverage.xml
    when: always
  allow_failure: false

coverage:
  script:
    - coverage report
    - python validate_coverage.py --min=80
\```

---

# 14. Criterios de Rechazo (Bloqueo)

- Cobertura < 80%  
- Reportes de pruebas incompletos  
- Pruebas fallando  
- Pruebas marcadas como skip sin justificación  
- Datos reales utilizados  
- Dependencias directas a servicios externos sin mocks  
- Ausencia de pruebas de regresión  
- Falla en pruebas de seguridad automatizadas  

---

# FIN DEL MANUAL TÉCNICO MT-14
