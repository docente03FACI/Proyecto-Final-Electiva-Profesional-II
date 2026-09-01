# Cronograma de Proyecto de Aula — Desarrollo FullStack
### Angular · Spring Boot · MySQL · APIs REST · AWS RDS · Postman

**Duración:** 12 semanas
**Modalidad de trabajo:** Equipos de 4-5 estudiantes
**Número de entregas:** 5 entregas evaluables

---

## 1. Visión general del proyecto

Los equipos deberán diseñar, construir y desplegar una aplicación web fullstack completa que resuelva un problema real o simulado (gestión académica, inventarios, reservas, salud, e-commerce, etc.), aplicando:

- **Frontend:** Angular (componentes, servicios, formularios reactivos, routing, consumo de API REST)
- **Backend:** Spring Boot (arquitectura en capas, Spring Data JPA, Spring Security, manejo de excepciones)
- **Base de datos:** MySQL modelado y desplegado en **AWS RDS**
- **Documentación y pruebas de API:** Postman (colecciones, variables de entorno, tests automatizados)

---

## 2. Mapa general de entregas

| Entrega | Semana de entrega | Foco principal | % Sugerido |
|---|---|---|---|
| Entrega 1 | Semana 3 | Requerimientos y Arquitectura | 10% |
| Entrega 2 | Semana 5 | Modelado de BD + Backend inicial (CRUD base) | 20% |
| Entrega 3 | Semana 8 | Backend completo + Seguridad + Pruebas Postman | 20% |
| Entrega 4 | Semana 10 | Frontend Angular integrado al Backend | 20% |
| Entrega 5 | Semana 12 | Proyecto integrado, desplegado y sustentado | 30% |

---

## 3. Cronograma semana a semana

### Semana 1 — Formación de equipos y definición de idea de proyecto
- Conformación de equipos (4-5 estudiantes)
- Lluvia de ideas y selección del problema a resolver
- Definición del alcance preliminar (MVP)
- Asignación de roles sugeridos: Líder técnico, Encargado BD/AWS, Encargado Backend, Encargado Frontend, Encargado de documentación/QA
- **Entregable de control:** Ficha de idea de proyecto (1 página)

### Semana 2 — Levantamiento de requerimientos
- Técnicas de elicitación de requerimientos (entrevistas simuladas, historias de usuario)
- Redacción de requerimientos funcionales y no funcionales
- Definición de actores/roles del sistema
- Inicio del diseño de arquitectura (diagrama de componentes preliminar)

### Semana 3 — 📌 ENTREGA 1: Documento de Requerimientos y Arquitectura
*(Ver detalle completo en sección 4)*

### Semana 4 — Modelado de base de datos
- Diagrama entidad-relación (DER)
- Normalización (hasta 3FN)
- Diccionario de datos
- Creación de cuenta/instancia en AWS RDS (capa gratuita)
- Configuración de seguridad de red (Security Groups) para acceso a RDS

### Semana 5 — 📌 ENTREGA 2: Modelado de BD + Backend Inicial
*(Ver detalle completo en sección 4)*

### Semana 6 — Desarrollo backend: lógica de negocio
- Implementación de capa de servicios (Service Layer)
- Relaciones entre entidades en JPA (@OneToMany, @ManyToOne, etc.)
- Manejo centralizado de excepciones (@ControllerAdvice)
- Validaciones con Bean Validation (@Valid, @NotNull, etc.)

### Semana 7 — Seguridad y pruebas de API
- Implementación de Spring Security (autenticación básica o JWT)
- Control de acceso por roles
- Construcción de colección Postman con todos los endpoints
- Pruebas automatizadas en Postman (tests con scripts)

### Semana 8 — 📌 ENTREGA 3: Backend Completo + Seguridad + Postman
*(Ver detalle completo en sección 4)*

### Semana 9 — Desarrollo frontend: estructura y consumo de API
- Estructura de módulos y componentes Angular
- Servicios Angular con HttpClient
- Formularios reactivos y validaciones
- Routing y guards básicos
- Consumo de endpoints del backend (integración inicial)

### Semana 10 — 📌 ENTREGA 4: Frontend Angular Integrado
*(Ver detalle completo en sección 4)*

### Semana 11 — Integración final, ajustes y despliegue
- Corrección de bugs de integración
- Interceptores HTTP (manejo de tokens, errores globales)
- Pruebas de extremo a extremo (end-to-end manual)
- Preparación de material de sustentación

### Semana 12 — 📌 ENTREGA 5: Proyecto Integrado, Desplegado y Sustentado
*(Ver detalle completo en sección 4)*

---

## 4. Detalle de entregables y tareas

### 🟦 ENTREGA 1 — Documento de Requerimientos y Arquitectura (Semana 3)

**Objetivo:** Formalizar el problema a resolver y proponer una arquitectura técnica coherente antes de escribir código.

**Tareas del equipo:**
1. Redactar el **planteamiento del problema** y justificación del proyecto
2. Definir **objetivo general y objetivos específicos**
3. Listar **requerimientos funcionales** (mínimo 10, con formato de historia de usuario: *"Como [rol], quiero [acción], para [beneficio]"*)
4. Listar **requerimientos no funcionales** (seguridad, rendimiento, usabilidad, escalabilidad)
5. Identificar **actores/roles** del sistema y sus permisos
6. Elaborar **diagrama de arquitectura propuesta** (cliente-servidor, capas: Angular ↔ API REST Spring Boot ↔ MySQL en AWS RDS)
7. Elaborar **diagrama de casos de uso** general
8. Proponer un **modelo entidad-relación preliminar** (borrador, sin normalizar aún)
9. Definir **stack tecnológico** y justificar cada elección
10. Establecer **cronograma interno del equipo** (distribución de tareas por integrante)

**Estructura sugerida del documento:**
```
1. Portada
2. Introducción y planteamiento del problema
3. Objetivos (general y específicos)
4. Alcance y limitaciones
5. Requerimientos funcionales (tabla numerada)
6. Requerimientos no funcionales
7. Actores del sistema
8. Arquitectura propuesta (diagrama + explicación)
9. Modelo ER preliminar
10. Stack tecnológico y justificación
11. Cronograma interno del equipo
12. Conclusiones preliminares
```

**Formato de entrega:** PDF, máx. 15 páginas + anexos
**Criterios de evaluación sugeridos:**
- Claridad y coherencia de requerimientos (25%)
- Calidad y viabilidad técnica de la arquitectura (30%)
- Completitud del modelo ER preliminar (20%)
- Redacción, formato y sustentación oral breve (25%)

---

### 🟦 ENTREGA 2 — Modelado de BD + Backend Inicial (Semana 5)

**Objetivo:** Tener la base de datos desplegada en AWS RDS y un backend Spring Boot con operaciones CRUD básicas conectado a ella.

**Tareas del equipo:**
1. DER final normalizado (mínimo 3FN) + diccionario de datos
2. Instancia MySQL activa y funcional en **AWS RDS**
3. Scripts de creación de base de datos (DDL) documentados
4. Proyecto Spring Boot inicializado (Spring Initializr) con dependencias: Web, JPA, MySQL Driver, Validation
5. Configuración de conexión a RDS en `application.properties`/`application.yml` (usando variables de entorno, **sin credenciales expuestas en el repositorio**)
6. Implementación de entidades JPA (@Entity) para al menos 3 tablas principales
7. Implementación de Repositorios (Spring Data JPA)
8. Implementación de Controladores REST con operaciones CRUD básicas (GET, POST, PUT, DELETE) para al menos 2 entidades
9. Prueba manual de endpoints en Postman (evidencia con capturas)
10. Repositorio en GitHub con README inicial

**Entregables:**
- Documento con DER final y diccionario de datos
- Enlace/evidencia de instancia AWS RDS activa
- Repositorio de código (link)
- Capturas de Postman probando endpoints CRUD

---

### 🟦 ENTREGA 3 — Backend Completo + Seguridad + Postman (Semana 8)

**Objetivo:** Backend robusto, seguro y completamente documentado/probado.

**Tareas del equipo:**
1. CRUD completo para **todas** las entidades del sistema
2. Relaciones entre entidades correctamente implementadas en JPA
3. Manejo centralizado de excepciones (respuestas de error estandarizadas)
4. Validaciones de entrada en todos los endpoints
5. Implementación de autenticación (Spring Security + JWT recomendado)
6. Control de acceso por roles (mínimo 2 roles distintos)
7. Colección de Postman completa y organizada por carpetas (por módulo/entidad)
8. Uso de **variables de entorno en Postman** (URL base, tokens)
9. Al menos 10 **tests automatizados** en Postman (scripts de verificación de status code, estructura de respuesta, etc.)
10. Documentación de la API (puede ser README, Postman Docs, o Swagger/OpenAPI)

**Entregables:**
- Repositorio actualizado
- Colección de Postman exportada (.json)
- Reporte de ejecución de tests (Postman Collection Runner o Newman)
- Documento breve de arquitectura de seguridad implementada

---

### 🟦 ENTREGA 4 — Frontend Angular Integrado (Semana 10)

**Objetivo:** Interfaz funcional en Angular que consuma el backend real (no datos mock).

**Tareas del equipo:**
1. Estructura modular del proyecto Angular (módulos por funcionalidad)
2. Componentes para todas las vistas principales (listar, crear, editar, eliminar)
3. Servicios Angular con `HttpClient` para consumir cada endpoint del backend
4. Formularios reactivos con validaciones (`ReactiveFormsModule`)
5. Enrutamiento (`Angular Router`) entre las distintas vistas
6. Implementación de login/autenticación desde el frontend (consumo del endpoint de seguridad)
7. Guards de ruta según rol de usuario
8. Interceptor HTTP para adjuntar token de autenticación
9. Manejo de errores y mensajes al usuario (feedback visual)
10. Diseño responsive básico (puede usar Angular Material, Bootstrap o CSS propio)

**Entregables:**
- Repositorio del frontend
- Video corto (3-5 min) mostrando el flujo funcional principal
- Evidencia de integración real con backend (no mocks)

---

### 🟦 ENTREGA 5 — Proyecto Integrado, Desplegado y Sustentado (Semana 12)

**Objetivo:** Entrega final del producto funcionando de extremo a extremo, con sustentación técnica.

**Tareas del equipo:**
1. Corrección de bugs identificados en integraciones anteriores
2. Backend y base de datos funcionando de forma estable contra AWS RDS
3. Frontend totalmente integrado y navegable
4. (Opcional/recomendado según el curso) Despliegue del backend en un servicio cloud (Render, Railway, AWS EC2/Elastic Beanstalk) y frontend en Vercel/Netlify/S3
5. Documento técnico final consolidado (une y actualiza las 4 entregas anteriores)
6. Manual de usuario básico
7. Colección Postman final actualizada
8. Preparación de sustentación: demo en vivo + defensa técnica de decisiones de arquitectura
9. Autoevaluación y coevaluación de trabajo en equipo

**Entregables:**
- Repositorio final (backend + frontend)
- Documento técnico consolidado (PDF)
- Colección Postman final
- Sustentación en vivo (15-20 min por equipo: demo + preguntas)
- Formato de auto/coevaluación diligenciado

**Criterios de evaluación sugeridos para la sustentación:**
- Funcionalidad completa end-to-end (35%)
- Calidad técnica del código y arquitectura (25%)
- Seguridad y manejo de errores (15%)
- Dominio conceptual demostrado en la defensa (15%)
- Trabajo en equipo y distribución de responsabilidades (10%)

---

## 5. Recomendaciones para el docente

- **Semanas sin entrega** (1, 2, 4, 6, 7, 9, 11) son ideales para asesorías técnicas en clase, revisiones de avance informales (checkpoints rápidos de 5-10 min por equipo) y resolución de dudas puntuales sobre AWS RDS, Postman o configuración de entornos.
- Se sugiere un **checkpoint de avance** a mitad de cada intervalo entre entregas para detectar equipos retrasados a tiempo.
- Para AWS RDS, recomendar a los estudiantes usar la **capa gratuita (Free Tier)** y recordarles **eliminar la instancia al finalizar el curso** para evitar cargos.
- Fomentar el uso de **GitHub con commits frecuentes** como evidencia de trabajo distribuido entre los integrantes del equipo (se puede usar como insumo para la coevaluación).
- Considerar una **rúbrica única consolidada** (puedo generarla si la necesitas) que se aplique de forma consistente en las 5 entregas.

---

## 6. Posibles siguientes pasos

¿Quieres que profundice en alguno de estos puntos?
- Rúbricas de evaluación detalladas (con niveles de desempeño) para cada entrega
- Plantilla del documento de Entrega 1 (requerimientos y arquitectura) lista para usar
- Guía técnica paso a paso para conectar Spring Boot con AWS RDS
- Formato de autoevaluación/coevaluación de trabajo en equipo
