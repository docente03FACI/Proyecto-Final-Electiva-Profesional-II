# Rúbricas de Evaluación — Proyecto de Aula FullStack
### Angular · Spring Boot · MySQL · APIs REST · AWS RDS · Postman

**Escala de niveles de desempeño (usada en todas las rúbricas):**

| Nivel | Rango | Descripción general |
|---|---|---|
| **Excelente (E)** | 90-100% del puntaje del criterio | Cumple totalmente, con calidad profesional y detalles adicionales de valor |
| **Bueno (B)** | 75-89% | Cumple lo esencial, con detalles menores por mejorar |
| **Aceptable (A)** | 60-74% | Cumple parcialmente, con vacíos que afectan la calidad o completitud |
| **Insuficiente (I)** | 0-59% | No cumple o presenta errores graves/ausencia del criterio |

---

## RÚBRICA — ENTREGA 1: Documento de Requerimientos y Arquitectura

| Criterio | Peso | Excelente (E) | Bueno (B) | Aceptable (A) | Insuficiente (I) |
|---|---|---|---|---|---|
| **Planteamiento del problema y objetivos** | 10 pts | Problema claro, relevante y bien justificado; objetivos SMART y alineados al problema | Problema claro pero justificación superficial; objetivos coherentes con detalles menores | Problema identificado pero poco justificado; objetivos genéricos o ambiguos | Problema confuso o no justificado; objetivos ausentes o desalineados |
| **Requerimientos funcionales** | 20 pts | Mínimo 10 RF, redactados como historias de usuario, priorizados, sin ambigüedad, cubren todo el alcance | 8-10 RF claros con formato correcto, priorización parcial | 5-7 RF, formato inconsistente o ambigüedades menores | Menos de 5 RF, mal redactados o no reflejan el alcance real |
| **Requerimientos no funcionales** | 10 pts | Cubren seguridad, rendimiento, usabilidad y escalabilidad con criterios medibles | Cubren al menos 3 categorías con criterios razonables | Cubren 1-2 categorías, poco medibles | Ausentes o genéricos sin relación al proyecto |
| **Actores y roles del sistema** | 10 pts | Actores bien definidos con permisos y responsabilidades claramente diferenciados | Actores identificados con permisos generales definidos | Actores identificados pero permisos poco claros | Actores no identificados o incoherentes con los RF |
| **Arquitectura propuesta** | 25 pts | Diagrama claro y correcto (cliente-servidor, capas), explica flujo de datos Angular↔API↔MySQL/RDS, justifica decisiones técnicas | Diagrama correcto con explicación adecuada, justificación parcial | Diagrama presente pero con errores conceptuales menores o explicación superficial | Diagrama ausente, incorrecto o no corresponde al stack solicitado |
| **Modelo entidad-relación preliminar** | 15 pts | Entidades, atributos y relaciones coherentes con los RF; notación correcta | Modelo coherente con errores menores de notación o cardinalidad | Modelo incompleto o con relaciones mal definidas | Modelo ausente o totalmente incoherente con los requerimientos |
| **Redacción, formato y sustentación oral** | 10 pts | Documento profesional, bien estructurado, cero errores ortográficos; sustentación clara y segura | Buena redacción con errores menores; sustentación adecuada | Redacción aceptable con varios errores; sustentación con vacíos | Documento desordenado o mal redactado; sustentación deficiente o ausente |

**Observación pedagógica:** Esta entrega es la base de todo el proyecto — se recomienda **no permitir avanzar a la Entrega 2** sin retroalimentación explícita sobre la arquitectura y el modelo ER, dado que errores aquí se heredan a todas las entregas siguientes.

---

## RÚBRICA — ENTREGA 2: Modelado de BD + Backend Inicial

| Criterio | Peso | Excelente (E) | Bueno (B) | Aceptable (A) | Insuficiente (I) |
|---|---|---|---|---|---|
| **Modelo ER final y normalización** | 15 pts | DER completo, normalizado hasta 3FN, con diccionario de datos detallado (tipo, restricciones, descripción) | DER normalizado con diccionario de datos con vacíos menores | DER presente pero con problemas de normalización o diccionario incompleto | DER ausente, mal formado o sin normalizar |
| **Despliegue en AWS RDS** | 20 pts | Instancia RDS activa, configurada correctamente (security groups, acceso), evidencia clara de funcionamiento | Instancia activa y funcional con configuración básica correcta | Instancia creada pero con problemas de configuración o acceso intermitente | Instancia no creada, no funcional, o usan BD local en su lugar |
| **Scripts DDL y estructura de BD** | 10 pts | Scripts documentados, ejecutables sin error, reflejan exactamente el modelo ER | Scripts funcionales con documentación mínima | Scripts con errores menores que requieren ajuste manual | Scripts ausentes o no ejecutables |
| **Configuración del proyecto Spring Boot** | 10 pts | Proyecto bien estructurado (paquetes por capa), dependencias correctas, conexión a RDS mediante variables de entorno (sin credenciales expuestas) | Proyecto estructurado correctamente, conexión funcional, buenas prácticas parciales | Proyecto funcional pero con estructura desordenada o credenciales hardcodeadas | Proyecto no compila, no conecta a la BD, o estructura inexistente |
| **Entidades JPA y Repositorios** | 15 pts | Entidades correctamente anotadas, relaciones básicas mapeadas, repositorios con métodos personalizados donde aplica | Entidades y repositorios funcionales sin relaciones complejas aún | Entidades con errores de mapeo o repositorios incompletos | Entidades ausentes o no corresponden al modelo ER |
| **Endpoints CRUD funcionales** | 20 pts | CRUD completo y probado para al menos 2 entidades, códigos de estado HTTP correctos, respuestas bien estructuradas | CRUD funcional con manejo básico de códigos de estado | CRUD parcial (faltan operaciones) o respuestas inconsistentes | CRUD no funcional o ausente |
| **Evidencia de pruebas en Postman** | 5 pts | Capturas claras de todos los endpoints probados exitosamente, incluyendo casos de error | Capturas de los endpoints principales funcionando | Evidencia parcial o poco clara | Sin evidencia de pruebas |
| **Repositorio GitHub y documentación** | 5 pts | Historial de commits distribuido entre integrantes, README claro con instrucciones de ejecución | Repositorio organizado con README básico | Repositorio con commits concentrados en 1-2 personas | Repositorio inexistente, vacío o sin uso real de control de versiones |

---

## RÚBRICA — ENTREGA 3: Backend Completo + Seguridad + Postman

| Criterio | Peso | Excelente (E) | Bueno (B) | Aceptable (A) | Insuficiente (I) |
|---|---|---|---|---|---|
| **CRUD completo de todas las entidades** | 20 pts | Todas las entidades tienen CRUD completo, consistente y probado | Casi todas las entidades (80%+) tienen CRUD completo | CRUD implementado solo para entidades principales (50-79%) | Menos del 50% de las entidades tienen CRUD funcional |
| **Relaciones entre entidades** | 10 pts | Relaciones JPA correctamente implementadas (evitando problemas como referencias circulares o N+1) | Relaciones funcionales con optimización parcial | Relaciones implementadas con errores menores de comportamiento | Relaciones ausentes o mal implementadas, generan errores |
| **Manejo de excepciones y validaciones** | 15 pts | Manejo centralizado (@ControllerAdvice), mensajes de error claros y estandarizados, validaciones en todos los endpoints | Manejo centralizado presente con cobertura parcial de validaciones | Manejo de excepciones básico (try-catch local) sin estandarizar | Sin manejo de excepciones; errores expuestos como stack traces |
| **Autenticación (Spring Security / JWT)** | 20 pts | Autenticación robusta y funcional, tokens generados y validados correctamente, expiración configurada | Autenticación funcional con configuración básica correcta | Autenticación parcialmente funcional o con vulnerabilidades menores | Autenticación ausente o no funcional |
| **Autorización por roles** | 10 pts | Mínimo 2 roles correctamente diferenciados, endpoints protegidos según permisos, probado exhaustivamente | Roles implementados y funcionales con cobertura parcial de endpoints | Roles definidos pero control de acceso inconsistente | Sin diferenciación de roles o control de acceso inexistente |
| **Colección Postman organizada** | 10 pts | Colección completa, organizada por carpetas/módulos, usa variables de entorno de forma consistente | Colección organizada con uso parcial de variables de entorno | Colección funcional pero desorganizada | Colección ausente o incompleta |
| **Tests automatizados en Postman** | 10 pts | 10+ tests bien diseñados (status code, estructura, valores), evidencian pensamiento de QA | 6-9 tests funcionales y relevantes | 3-5 tests básicos | Menos de 3 tests o ausentes |
| **Documentación de la API** | 5 pts | Documentación clara y completa (README/Swagger/Postman Docs) de todos los endpoints | Documentación de los endpoints principales | Documentación mínima o desactualizada | Sin documentación de la API |

---

## RÚBRICA — ENTREGA 4: Frontend Angular Integrado

| Criterio | Peso | Excelente (E) | Bueno (B) | Aceptable (A) | Insuficiente (I) |
|---|---|---|---|---|---|
| **Estructura y modularidad del proyecto** | 10 pts | Proyecto organizado en módulos/features, separación clara de componentes, servicios y modelos | Buena organización con separación parcial de responsabilidades | Estructura funcional pero desordenada (todo en un módulo) | Sin estructura clara, código difícil de mantener |
| **Componentes y vistas funcionales** | 15 pts | Todas las vistas CRUD funcionan correctamente y de forma fluida (listar, crear, editar, eliminar) | La mayoría de las vistas (80%+) funcionan correctamente | Vistas principales funcionan, otras presentan errores | Menos del 50% de las vistas son funcionales |
| **Consumo real de la API (no mocks)** | 20 pts | Todos los servicios consumen el backend real vía HttpClient, manejo correcto de Observables | La mayoría de los servicios consumen el backend real | Consumo parcial de la API, algunos datos aún simulados | Uso de datos mock/estáticos en lugar de la API real |
| **Formularios reactivos y validaciones** | 10 pts | Formularios reactivos bien implementados, validaciones claras con feedback visual al usuario | Formularios reactivos funcionales con validaciones básicas | Formularios funcionales pero con validaciones mínimas o mal implementadas (template-driven cuando se pidió reactivo) | Formularios no funcionales o sin validación alguna |
| **Enrutamiento (Routing)** | 10 pts | Rutas bien definidas, navegación fluida, lazy loading donde aplica, manejo de rutas no encontradas | Rutas funcionales y navegación correcta | Rutas básicas funcionando con inconsistencias menores | Enrutamiento ausente o no funcional |
| **Autenticación desde el frontend** | 15 pts | Login funcional, token almacenado y gestionado correctamente, interceptor HTTP adjunta token automáticamente | Login funcional con gestión básica del token | Login funcional pero sin interceptor (token manejado manualmente) | Login no funcional o ausente |
| **Guards de ruta según rol** | 10 pts | Guards correctamente implementados, restringen acceso según rol de forma consistente | Guards implementados con cobertura parcial | Guards presentes pero con fallos de protección | Sin guards; cualquier usuario accede a cualquier ruta |
| **Manejo de errores y UX** | 5 pts | Mensajes de error claros y amigables, estados de carga (loading), feedback visual consistente | Manejo de errores presente con UX aceptable | Manejo de errores mínimo, poco amigable | Sin manejo de errores visible para el usuario |
| **Diseño responsive** | 5 pts | Interfaz adaptable a distintos tamaños de pantalla, uso coherente de un sistema de diseño (Material/Bootstrap/CSS propio) | Diseño adaptable con inconsistencias menores | Diseño funcional solo en escritorio | Sin consideración de responsividad, diseño roto |

---

## RÚBRICA — ENTREGA 5: Proyecto Integrado, Desplegado y Sustentado
**Peso sugerido: 40% de la nota final | Puntaje total: 100 pts**

| Criterio | Peso | Excelente (E) | Bueno (B) | Aceptable (A) | Insuficiente (I) |
|---|---|---|---|---|---|
| **Funcionalidad end-to-end** | 25 pts | Flujo completo funciona sin errores: frontend↔backend↔BD en RDS, todas las funcionalidades del alcance operativas | Flujo funciona con errores menores no bloqueantes | Flujo funciona parcialmente, algunas funcionalidades fallan | Flujo no funciona de extremo a extremo o falla en pasos críticos |
| **Calidad técnica del código y arquitectura** | 15 pts | Código limpio, buenas prácticas (SOLID, nomenclatura, separación de capas), arquitectura coherente con lo propuesto en Entrega 1 | Código organizado con buenas prácticas parciales | Código funcional pero con deuda técnica visible (duplicación, acoplamiento alto) | Código desordenado, sin buenas prácticas, arquitectura no corresponde a lo propuesto |
| **Seguridad y manejo de errores integral** | 15 pts | Seguridad robusta en todo el flujo (backend y frontend), manejo de errores consistente en toda la aplicación | Seguridad y manejo de errores adecuados con vacíos menores | Seguridad básica presente, manejo de errores inconsistente | Seguridad ausente o con vulnerabilidades evidentes |
| **Despliegue en la nube (si aplica según el curso)** | 10 pts | Backend y frontend desplegados y accesibles públicamente, funcionando de forma estable | Desplegado con intermitencias menores | Despliegue parcial (solo uno de los dos componentes) | Sin despliegue, solo ejecución local |
| **Documento técnico consolidado** | 10 pts | Documento integra y actualiza las 4 entregas anteriores, coherente y profesional, incluye manual de usuario | Documento consolidado con actualizaciones parciales | Documento presente pero desactualizado respecto al producto final | Documento ausente o simple copia de entregas anteriores sin consolidar |
| **Dominio conceptual en la sustentación** | 15 pts | Todos los integrantes explican con propiedad decisiones técnicas y responden preguntas con seguridad | La mayoría domina el proyecto; 1 integrante con vacíos menores | Dominio desigual entre integrantes, respuestas superficiales | Equipo no puede explicar decisiones técnicas propias |
| **Trabajo en equipo y distribución de responsabilidades** | 10 pts | Evidencia clara (commits, roles) de participación equilibrada de todos los integrantes | Participación mayormente equilibrada con 1 integrante con menor aporte | Participación desigual, 1-2 integrantes concentran el trabajo | Evidencia de que 1 solo integrante desarrolló el proyecto |

---

## 7. Tabla resumen de ponderación global

| Entrega | Peso en la nota final | Puntaje máximo interno |
|---|---|---|
| Entrega 1 — Requerimientos y Arquitectura | 10% | 100 pts |
| Entrega 2 — BD + Backend Inicial | 20% | 100 pts |
| Entrega 3 — Backend Completo + Seguridad | 20% | 100 pts |
| Entrega 4 — Frontend Integrado | 20% | 100 pts |
| Entrega 5 — Proyecto Final Integrado | 30% | 100 pts |
| **Total** | **100%** | — |

---
