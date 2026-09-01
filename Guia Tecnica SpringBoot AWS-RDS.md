# Guía Técnica — Conectar Spring Boot con AWS RDS (MySQL)
### Guía paso a paso para estudiantes

Esta guía cubre desde la creación de la instancia de base de datos en AWS RDS hasta la conexión funcional desde una aplicación Spring Boot, incluyendo buenas prácticas de seguridad y errores comunes.

**Prerrequisitos:**
- Cuenta de AWS activa (usar capa gratuita / *Free Tier*)
- Proyecto Spring Boot con Java 17+ y Maven/Gradle
- Cliente MySQL para pruebas (MySQL Workbench, DBeaver, o línea de comandos)
- Postman instalado

---

## Parte 1 — Crear la instancia de base de datos en AWS RDS

### Paso 1: Acceder a la consola de RDS
1. Ingresar a [AWS Console](https://console.aws.amazon.com)
2. Buscar el servicio **RDS** en la barra de búsqueda
3. Clic en **"Create database"** (Crear base de datos)

### Paso 2: Configurar el motor de base de datos
1. Método de creación: **Standard create**
2. Motor: **MySQL**
3. Versión: elegir una versión estable (ej. MySQL 8.0.x)
4. Plantilla: **Free tier** (para no generar costos durante el curso)

### Paso 3: Configuración de la instancia
1. **DB instance identifier:** nombre identificador (ej. `proyecto-aula-db`)
2. **Master username:** usuario administrador (ej. `admin`)
3. **Master password:** contraseña segura — **guardarla en un lugar seguro**, no se puede recuperar después, solo restablecer

> ⚠️ **Importante:** esta contraseña nunca debe escribirse directamente en el código ni subirse a GitHub. Se usará mediante variables de entorno (ver Parte 3).

### Paso 4: Tipo de instancia y almacenamiento
1. Clase de instancia: `db.t3.micro` o `db.t2.micro` (elegibles para Free Tier)
2. Almacenamiento: 20 GB (suficiente para un proyecto académico)
3. Desactivar almacenamiento autoescalable si se quiere evitar cualquier costo adicional

### Paso 5: Conectividad — el paso más importante
1. **Virtual Private Cloud (VPC):** dejar la VPC por defecto
2. **Acceso público (Public access): `Yes`**
   Esto es indispensable para que el equipo pueda conectarse desde su máquina local y desde el backend durante el desarrollo.
3. **Grupo de seguridad de VPC (Security group):** crear uno nuevo, ej. `sg-proyecto-aula`
4. **Zona de disponibilidad:** dejar por defecto

### Paso 6: Configuración adicional
1. **Initial database name:** definir el nombre de la base de datos inicial (ej. `proyecto_aula_db`) — este campo es clave, si se deja vacío hay que crear la base de datos manualmente después
2. Backups automáticos: se pueden dejar activos (no generan costo adicional en Free Tier dentro de los límites)
3. Clic en **"Create database"**

La creación de la instancia puede tardar entre 5 y 10 minutos.

---

## Parte 2 — Configurar el Security Group (acceso a la base de datos)

Este es el paso donde más fallan los estudiantes. Si no se configura correctamente, la conexión dará **timeout**.

### Paso 1: Ubicar el Security Group
1. En la consola de RDS, clic en la instancia creada
2. En la pestaña **"Connectivity & security"**, ubicar la sección **"Security"**
3. Clic en el enlace del Security Group (esto abre la consola de EC2)

### Paso 2: Editar las reglas de entrada (Inbound rules)
1. En la consola de EC2, ir a la pestaña **"Inbound rules"**
2. Clic en **"Edit inbound rules"**
3. Clic en **"Add rule"** con esta configuración:

| Campo | Valor |
|---|---|
| Type | MySQL/Aurora |
| Protocol | TCP |
| Port range | 3306 |
| Source | Ver opciones abajo |

**Opciones para el campo Source (elegir según el contexto):**

- **Para uso individual/pruebas locales:** `My IP` (AWS detecta automáticamente la IP pública del estudiante). Esta opción es la más segura pero requiere actualizar la regla cada vez que cambie la IP (por ejemplo, al cambiar de red WiFi).
- **Para trabajo en equipo (recomendado durante el curso):** `0.0.0.0/0` (permite conexión desde cualquier IP). **Esto es aceptable únicamente en un entorno académico/de pruebas**, nunca en un proyecto en producción con datos reales.

> 🎓 **Nota pedagógica:** este es un excelente momento para explicar a los estudiantes la diferencia entre buenas prácticas de seguridad en desarrollo académico vs. producción real, y por qué `0.0.0.0/0` sería inaceptable en un sistema con datos reales de usuarios.

4. Guardar los cambios (**Save rules**)

---

## Parte 3 — Obtener los datos de conexión

1. Volver a la consola de RDS → clic en la instancia
2. En la pestaña **"Connectivity & security"**, copiar:
   - **Endpoint** (ej. `proyecto-aula-db.c9akciq32xyz.us-east-1.rds.amazonaws.com`)
   - **Port** (por defecto `3306`)
3. Con estos datos, la URL de conexión JDBC será:

```
jdbc:mysql://<ENDPOINT>:<PORT>/<NOMBRE_BASE_DATOS>?useSSL=false&serverTimezone=UTC
```

### Verificación previa con cliente MySQL (recomendado antes de tocar Spring Boot)

Antes de configurar el backend, se recomienda **verificar que la conexión funciona** con un cliente externo:

```bash
mysql -h <ENDPOINT> -P 3306 -u admin -p
```

Si pide la contraseña y logra conectar, la instancia y el Security Group están correctamente configurados. Si no conecta, revisar la Parte 2 antes de continuar.

También se puede verificar con **MySQL Workbench**: New Connection → Hostname = endpoint, Port = 3306, Username = admin.

---

## Parte 4 — Configurar el proyecto Spring Boot

### Paso 1: Dependencias necesarias (`pom.xml`)

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
```

### Paso 2: Configurar `application.properties` (⚠️ NO usar credenciales directas)

**❌ Forma incorrecta (nunca hacer esto, menos aún subirlo a GitHub):**
```properties
spring.datasource.url=jdbc:mysql://proyecto-aula-db.xyz.rds.amazonaws.com:3306/proyecto_aula_db
spring.datasource.username=admin
spring.datasource.password=MiContraseñaSecreta123
```

**✅ Forma correcta — usando variables de entorno:**
```properties
spring.datasource.url=jdbc:mysql://${DB_HOST}:${DB_PORT}/${DB_NAME}?useSSL=false&serverTimezone=UTC
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

### Paso 3: Definir las variables de entorno

**Opción A — Variables de entorno del sistema operativo (recomendada para trabajo individual):**

En Linux/Mac:
```bash
export DB_HOST=proyecto-aula-db.xyz.rds.amazonaws.com
export DB_PORT=3306
export DB_NAME=proyecto_aula_db
export DB_USERNAME=admin
export DB_PASSWORD=MiContraseñaSecreta123
```

En Windows (PowerShell):
```powershell
$env:DB_HOST="proyecto-aula-db.xyz.rds.amazonaws.com"
$env:DB_PORT="3306"
$env:DB_NAME="proyecto_aula_db"
$env:DB_USERNAME="admin"
$env:DB_PASSWORD="MiContraseñaSecreta123"
```

**Opción B — Archivo `.env` o `application-local.properties` (recomendada para trabajo en equipo):**

1. Crear un archivo `application-local.properties` con los valores reales
2. **Agregarlo al `.gitignore`** para que nunca se suba al repositorio:

```
# .gitignore
application-local.properties
.env
```

3. Cada integrante del equipo crea su propia copia local con las mismas credenciales (compartidas por un canal seguro, no por GitHub)
4. Ejecutar la aplicación con el perfil activo:
```bash
mvn spring-boot:run -Dspring-boot.run.profiles=local
```

> 🎓 **Nota pedagógica:** este es el momento ideal para introducir el concepto de **gestión segura de secretos**, relevante tanto en el entorno académico como en la industria real (variables de entorno, `.env`, AWS Secrets Manager, etc.).

### Paso 4: Ejecutar y verificar

1. Ejecutar la aplicación:
```bash
mvn spring-boot:run
```

2. Si la conexión es exitosa, en la consola debe aparecer algo similar a:
```
HikariPool-1 - Starting...
HikariPool-1 - Start completed.
Tomcat started on port(s): 8080
```

3. Si se definió `spring.jpa.hibernate.ddl-auto=update`, las tablas correspondientes a las entidades `@Entity` se crearán automáticamente en la base de datos de RDS.

---

## Parte 5 — Verificación final con Postman

1. Crear una petición `GET` a un endpoint ya implementado, ej.: `http://localhost:8080/api/estudiantes`
2. Si retorna una respuesta (`200 OK`, aunque sea un arreglo vacío `[]`), la conexión end-to-end **Spring Boot → AWS RDS** está funcionando correctamente
3. Probar un `POST` para insertar un registro y luego verificar en MySQL Workbench que el dato llegó a la base de datos en RDS

---

## Errores comunes y cómo resolverlos

| Error | Causa probable | Solución |
|---|---|---|
| `Communications link failure` / timeout | Security Group mal configurado | Verificar Parte 2: la regla de entrada debe permitir el puerto 3306 desde la IP correcta |
| `Access denied for user` | Usuario o contraseña incorrectos | Verificar las variables de entorno; revisar que no haya espacios extra |
| `Unknown database 'xxx'` | El nombre de la base de datos no coincide | Verificar el "Initial database name" configurado en RDS, o crear la base de datos manualmente |
| La app conecta pero tarda mucho | Instancia en región lejana o clase de instancia muy pequeña | Verificar región de AWS más cercana; normal en Free Tier tener algo de latencia |
| Funciona en un computador pero no en otro del mismo equipo | Security Group configurado con `My IP` en vez de `0.0.0.0/0` | Cambiar el Source de la regla de entrada (ver nota de seguridad en Parte 2) |
| `Public Key Retrieval is not allowed` | Configuración de SSL/autenticación de MySQL 8 | Agregar `allowPublicKeyRetrieval=true&useSSL=false` a la URL JDBC (solo en entorno académico) |
| El repositorio en GitHub expone la contraseña | Credenciales escritas directamente en `application.properties` | Rotar la contraseña en RDS inmediatamente y migrar a variables de entorno (Parte 4, Paso 3) |

---

## Checklist final para los equipos

- [ ] Instancia RDS creada en Free Tier
- [ ] Security Group permite conexión en el puerto 3306
- [ ] Conexión verificada con cliente MySQL externo (Workbench o CLI) antes de tocar Spring Boot
- [ ] Credenciales gestionadas mediante variables de entorno (nunca hardcodeadas)
- [ ] `.gitignore` actualizado para excluir archivos con credenciales
- [ ] Aplicación Spring Boot conecta exitosamente (logs de Hikari sin errores)
- [ ] Endpoint probado en Postman con respuesta exitosa
- [ ] Dato de prueba insertado y verificado directamente en la base de datos de RDS

---

## ⚠️ Recordatorio importante para el cierre del curso

Al finalizar el proyecto (Entrega 5), cada equipo debe **eliminar su instancia de AWS RDS** desde la consola (RDS → Databases → seleccionar instancia → Actions → Delete) para evitar cargos una vez expire la capa gratuita o se superen sus límites. Se recomienda que el docente incluya este paso como parte del checklist de cierre del proyecto.
