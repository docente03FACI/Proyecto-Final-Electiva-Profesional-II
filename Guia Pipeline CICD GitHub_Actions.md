# Pipeline CI/CD con GitHub Actions
### Preparación para la Entrega 5 — Despliegue Automatizado

Este documento diseña un pipeline de **Integración Continua y Despliegue Continuo (CI/CD)** para el proyecto de aula, usando **GitHub Actions**. Se integra como actividad de la **Semana 11** (Integración final, ajustes y despliegue), de modo que en la **Entrega 5** el equipo demuestre no solo una aplicación funcional, sino un flujo de trabajo profesional de despliegue automatizado.

---

## 1. ¿Por qué un pipeline en este proyecto?

Hasta la Entrega 4, cada equipo ha estado construyendo y probando manualmente. Un pipeline CI/CD introduce una práctica central de la industria: **cada cambio subido al repositorio se valida y despliega automáticamente**, sin intervención manual. Esto refuerza:

- La disciplina de pruebas automatizadas (ya trabajada con Postman/Newman desde la Entrega 3)
- La gestión segura de credenciales (ya trabajada con variables de entorno en la guía de AWS RDS)
- La entrega continua como estándar profesional en desarrollo fullstack

---

## 2. Arquitectura del pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│                        GitHub Repository                        │
│   /backend (Spring Boot)          /frontend (Angular)           │
└───────────────┬───────────────────────────┬─────────────────────┘
                │ push a main                │ push a main
                ▼                            ▼
   ┌─────────────────────────┐   ┌──────────────────────────┐
   │  Workflow: backend-ci-cd │   │ Workflow: frontend-ci-cd │
   │  1. Build (Maven)        │   │ 1. Build (npm/Angular)   │
   │  2. Tests unitarios      │   │ 2. Lint + tests (Karma)  │
   │  3. Empaquetar JAR       │   │ 3. Build producción      │
   │  4. Deploy → AWS EB      │   │ 4. Deploy → AWS S3 +     │
   │                          │   │    invalidar CloudFront  │
   └───────────┬──────────────┘   └────────────┬──────────────┘
               ▼                                ▼
     AWS Elastic Beanstalk              AWS S3 + CloudFront
     (Backend Spring Boot)              (Frontend Angular)
               │
               ▼
        AWS RDS (MySQL)
```

Cada workflow se ejecuta de forma **independiente**, activado solo cuando hay cambios en su respectiva carpeta (`/backend` o `/frontend`), evitando builds innecesarios.

> 🎓 **Alternativa simplificada (Opción B):** si el equipo tiene poco tiempo o dificultades con IAM en AWS, se puede sustituir el despliegue así: Backend → **Render** (usando su GitHub Action oficial o *deploy hook*); Frontend → **Netlify** o **Vercel** (integración nativa con GitHub, sin necesidad de configurar credenciales AWS). La lógica de build y tests del pipeline es la misma; solo cambia el paso de despliegue. Recomendado si el curso prioriza el aprendizaje de CI/CD sobre el ecosistema AWS específico.

---

## 3. Prerrequisitos antes de construir el pipeline

- [ ] Repositorio en GitHub con estructura de monorepo: `/backend` y `/frontend` (o dos repos separados — la guía funciona igual, ajustando los `paths`)
- [ ] Backend con pruebas unitarias mínimas (JUnit) ya implementadas
- [ ] Frontend con pruebas básicas (Karma/Jasmine) ya implementadas
- [ ] Instancia de AWS RDS ya funcionando (Entrega 2)
- [ ] Cuenta de AWS con permisos para crear: Elastic Beanstalk, S3, CloudFront, e IAM (usuario para GitHub Actions)

---

## 4. Preparar los recursos en AWS

### 4.1 Backend → AWS Elastic Beanstalk

1. Consola AWS → **Elastic Beanstalk** → **Create application**
2. Nombre de la aplicación (ej. `proyecto-aula-backend`)
3. Plataforma: **Java** (versión Corretto acorde a tu proyecto, ej. Java 17)
4. Crear un **environment** de tipo *Web server environment*
5. En **Configuration → Software**, agregar las variables de entorno que el backend necesita para conectarse a RDS (las mismas de la guía anterior: `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USERNAME`, `DB_PASSWORD`)
6. Verificar que el Security Group de RDS permita conexión desde el Security Group de Elastic Beanstalk (no solo desde la IP local)

### 4.2 Frontend → AWS S3 + CloudFront

1. Consola AWS → **S3** → **Create bucket** (ej. `proyecto-aula-frontend`)
2. Habilitar **Static website hosting** en las propiedades del bucket
3. Consola AWS → **CloudFront** → **Create distribution**, apuntando como origen al bucket S3 (permite HTTPS y mejor rendimiento que S3 solo)
4. Anotar el **Distribution ID** de CloudFront (se usará en el workflow)

### 4.3 Usuario IAM para GitHub Actions

1. Consola AWS → **IAM** → **Users** → **Create user** (ej. `github-actions-deploy`)
2. Asignar permisos mínimos necesarios (principio de menor privilegio):
   - `AWSElasticBeanstalkFullAccess` (o una política personalizada más restringida)
   - `AmazonS3FullAccess` (o restringida al bucket específico)
   - `CloudFrontFullAccess` (o restringida a la distribución específica)
3. Generar **Access Key ID** y **Secret Access Key**

> 🎓 **Nota pedagógica:** este es un buen momento para hablar del **principio de menor privilegio** — en un entorno profesional real, nunca se usarían permisos "FullAccess"; se crearían políticas IAM personalizadas y acotadas exactamente a los recursos necesarios.

---

## 5. Configurar los Secrets en GitHub

En el repositorio: **Settings → Secrets and variables → Actions → New repository secret**

| Secret | Valor |
|---|---|
| `AWS_ACCESS_KEY_ID` | Access Key del usuario IAM creado |
| `AWS_SECRET_ACCESS_KEY` | Secret Key del usuario IAM creado |
| `AWS_REGION` | ej. `us-east-1` |
| `EB_APPLICATION_NAME` | Nombre de la app en Elastic Beanstalk |
| `EB_ENVIRONMENT_NAME` | Nombre del environment en Elastic Beanstalk |
| `S3_BUCKET_NAME` | Nombre del bucket del frontend |
| `CLOUDFRONT_DISTRIBUTION_ID` | ID de la distribución CloudFront |

> ⚠️ Estos secrets **nunca** deben escribirse en el código ni en los archivos `.yml` — GitHub Actions los inyecta de forma segura en tiempo de ejecución.

---

## 6. Workflow del Backend (`backend-ci-cd.yml`)

Guardar en `.github/workflows/backend-ci-cd.yml`. Ver el archivo completo adjunto a esta guía.

**Resumen de las etapas:**

| Job | Qué hace |
|---|---|
| `build-and-test` | Compila el proyecto con Maven, ejecuta pruebas unitarias JUnit |
| `deploy` | Empaqueta el JAR y lo despliega a AWS Elastic Beanstalk (solo si `build-and-test` fue exitoso y el push fue a `main`) |

---

## 7. Workflow del Frontend (`frontend-ci-cd.yml`)

Guardar en `.github/workflows/frontend-ci-cd.yml`. Ver el archivo completo adjunto a esta guía.

**Resumen de las etapas:**

| Job | Qué hace |
|---|---|
| `build-and-test` | Instala dependencias, ejecuta lint y pruebas Karma en modo headless, genera build de producción |
| `deploy` | Sube los archivos del build a S3 e invalida la caché de CloudFront (solo si `build-and-test` fue exitoso y el push fue a `main`) |

---

## 8. Flujo de trabajo recomendado para los equipos

1. Cada integrante trabaja en una **rama propia** (`feature/nombre-funcionalidad`)
2. Al terminar, abre un **Pull Request** hacia `main`
3. El pipeline **se ejecuta automáticamente en el Pull Request** (solo los jobs de `build-and-test`, sin desplegar) — esto sirve como validación antes de fusionar
4. Al aprobar y fusionar (*merge*) a `main`, el pipeline completo se ejecuta, incluyendo el **despliegue automático**
5. El equipo revisa la pestaña **Actions** de GitHub para ver el estado de cada ejecución

> 🎓 Este flujo introduce a los estudiantes al concepto de **trunk-based development / feature branches**, ampliamente usado en la industria, y refuerza por qué las pruebas automatizadas (Entrega 3) son la base de un pipeline confiable.

---

## 9. Errores comunes

| Error | Causa probable | Solución |
|---|---|---|
| El workflow no se activa | Los `paths` del trigger no coinciden con la carpeta modificada | Verificar la sección `on: push: paths:` en el `.yml` |
| `Error: Credentials could not be loaded` | Secrets mal nombrados o no configurados | Verificar que los nombres en `secrets.NOMBRE` coincidan exactamente con los definidos en GitHub |
| Deploy exitoso pero la app no responde | Variables de entorno de BD no configuradas en Elastic Beanstalk | Revisar Configuration → Software en la consola de EB |
| Frontend desplegado pero muestra versión antigua | Caché de CloudFront no invalidada | Verificar que el paso de `create-invalidation` se ejecutó correctamente |
| Tests fallan solo en el pipeline, no en local | Diferencias de entorno (versión de Java/Node, variables de entorno faltantes) | Fijar versiones exactas en el workflow (`java-version`, `node-version`) |

---

## 10. Checklist final de la actividad

- [ ] Recursos de AWS creados (Elastic Beanstalk, S3, CloudFront, usuario IAM)
- [ ] Secrets configurados en GitHub
- [ ] Workflow de backend ejecuta build + tests correctamente en cada Pull Request
- [ ] Workflow de frontend ejecuta build + tests correctamente en cada Pull Request
- [ ] Al fusionar a `main`, el backend se despliega automáticamente y responde en su URL pública de Elastic Beanstalk
- [ ] Al fusionar a `main`, el frontend se despliega automáticamente y es accesible vía la URL de CloudFront
- [ ] El equipo puede explicar en la sustentación (Entrega 5) qué hace cada etapa del pipeline y por qué

---

## Archivos incluidos

- `backend-ci-cd.yml` — workflow listo para copiar a `.github/workflows/`
- `frontend-ci-cd.yml` — workflow listo para copiar a `.github/workflows/`

Ambos requieren únicamente ajustar los `paths` según la estructura real del repositorio del equipo (monorepo vs. repos separados).
