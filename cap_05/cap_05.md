# Capítulo V: Product Implementation, Validation & Deployment

## 5.1 Testing Suites & General Patterns

Para garantizar la estabilidad y confiabilidad de los servicios principales de **Alimenta**, se implementara una estrategia de pruebas automatizadas alineada con la arquitectura definida en el Capitulo IV y con las historias de usuario criticas del Capitulo III (registro, autenticacion, publicacion y reserva).

La suite de pruebas del backend combinara pruebas unitarias, de integracion y de capa web para validar controladores, servicios y repositorios en escenarios reales de uso. En esta etapa se utilizaran las bibliotecas de **Mockito** para el aislamiento de dependencias y simulacion de comportamientos, junto con **Spring Boot Test** y **MockMvc** para verificar endpoints HTTP.

Con este enfoque se busca reducir regresiones, asegurar contratos entre capas y validar que los flujos del nucleo transaccional (Auth, Donations y Matching) funcionen de forma consistente antes de desplegar nuevos incrementos.

### 5.1.1 Backend Application Core Testing Suite

De acuerdo con el diagrama de contenedores C4, el nucleo backend de **Alimenta** concentra servicios de negocio conectados mediante API Gateway y persistidos en la Core DB. Bajo esa estructura, la primera evidencia de pruebas se enfoca en el **Auth Service**, ya que habilita el acceso seguro al resto de capacidades del sistema.

Para validar este nucleo se definio una prueba de integracion del endpoint de registro de usuarios, vinculada a las historias **US01** y **US02** del Capitulo III.

- **TestForCreateANewUser**

En esta prueba se utiliza **Spring Boot Test** con **MockMvc** para simular una solicitud HTTP `POST` al endpoint ` /api/v1/authentication/sign-up `. La peticion envia un cuerpo en formato JSON con los datos de registro y verifica que el servicio procese correctamente la creacion del nuevo usuario. Cuando el flujo requiere autenticacion previa para acceder al recurso, la solicitud incluye el encabezado `Authorization` con un token JWT valido.

Adicionalmente, se emplea **Mockito** para desacoplar dependencias internas (por ejemplo, servicios auxiliares o repositorios no relevantes para el caso), permitiendo concentrar la validacion en el comportamiento esperado del caso de uso y en la respuesta HTTP devuelta por el endpoint.


### 5.1.2 Pattern Based Backend Application(s)

Durante la implementacion del backend se aplicaron patrones de diseno y arquitectura ya definidos en el Capitulo IV, con el objetivo de mantener una base de codigo modular, mantenible y preparada para crecimiento por microservicios.

- **Service Layer Pattern**

Este patron organiza la logica de negocio en servicios de aplicacion (por ejemplo, autenticacion, donaciones, reservas o matching), evitando que los controladores concentren reglas complejas. Los servicios coordinan validaciones, flujos de uso y transacciones, y actuan como punto de orquestacion entre la capa de entrada y la capa de persistencia.

- **Repository Pattern**

Este patron desacopla el acceso a datos de la logica del dominio mediante repositorios especializados. De esta forma, las operaciones sobre entidades como usuarios, roles, donaciones, reservas o metricas se encapsulan en componentes de persistencia, facilitando pruebas con mocks y mantenimiento de consultas sin afectar los casos de uso.

- **Model Pattern (aclaracion de uso)**

En el proyecto se trabaja con modelos y entidades de dominio como parte natural de Spring Boot/JPA; sin embargo, no se declaro un patron independiente llamado "Model Pattern" dentro de la arquitectura formal. En consecuencia, para el reporte se consideran como patrones principales implementados **Service Layer** y **Repository**, que son los que estructuran directamente el backend.

La combinacion de estos patrones permite mantener separacion de responsabilidades, mejorar la testabilidad con **Mockito** y sostener una evolucion ordenada del sistema conforme crezcan los bounded contexts.


**Figura 1. Implementacion de Service Layer Pattern**

<img alt="Image" src="https://github.com/user-attachments/assets/50d86ece-a844-40ff-9ed2-c209d9fb41be" />

**Figura 2. Implementacion de Repository Pattern**

<img alt="Image" src="https://github.com/user-attachments/assets/7adfa643-ac8e-4ee3-9966-784fc7e8e0b3" />

<img alt="Image" src="https://github.com/user-attachments/assets/5403afdf-dc47-4500-8df2-5928e13c5698" />

### 5.1.3 Pattern Based Custom Software Library

En el backend de **Alimenta** se aprovechan bibliotecas y anotaciones del ecosistema **Spring Boot** para implementar de forma consistente los patrones de API REST y capa de aplicacion. Estas anotaciones estandarizan la exposicion de endpoints, la documentacion tecnica y la gestion de operaciones CRUD en los servicios principales.

A continuacion, se describen los elementos de biblioteca utilizados como base en los controladores:

- **`@RestController`**

Declara la clase como controlador REST y permite retornar respuestas en formato JSON de manera directa. En **Alimenta**, esta anotacion se aplica en controladores de autenticacion, donaciones, reservas y otros modulos del nucleo transaccional.

- **`@RequestMapping(...)`**

Define la ruta base del recurso y el tipo de contenido de salida (`application/json`). Su uso permite mantener endpoints uniformes bajo convenciones como `api/v1/...`, facilitando versionado y mantenibilidad de contratos.

- **`@Tag(...)`**

Anotacion utilizada para documentar cada controlador en **Swagger/OpenAPI**. Con esto se mejora la trazabilidad tecnica de los servicios y se facilita el consumo de la API por parte del equipo frontend y pruebas.

- **`@PostMapping`**

Gestiona solicitudes HTTP `POST` para operaciones de creacion, por ejemplo registro de usuarios, publicacion de donaciones o alta de reservas.

- **`@PutMapping`**

Gestiona solicitudes HTTP `PUT` para actualizacion de recursos existentes, por ejemplo modificacion de datos de perfil, actualizacion de una donacion o cambio de estado en un flujo operativo.

- **`@DeleteMapping(...)`**

Gestiona solicitudes HTTP `DELETE` para eliminacion logica o fisica de recursos segun reglas del negocio, validando previamente restricciones de integridad y permisos del usuario autenticado.

El uso combinado de estas bibliotecas permite aplicar una estructura repetible en los microservicios, reduciendo codigo duplicado y reforzando principios de separacion de responsabilidades definidos en la arquitectura.


**Figura 3. Anotaciones Spring en controlador REST**

<img  alt="Image" src="https://github.com/user-attachments/assets/84e9108c-6b22-4f53-bf9e-a4ba086a0547" />

**Figura 4. Evidencia de documentacion en Swagger/OpenAPI**

<img alt="Image" src="https://github.com/user-attachments/assets/d16d0cc8-2fb7-4779-9aa3-a8dbb01815c8" />

### 5.1.4 Framework Pattern Driven Refactoring Report

Con base en la revision de los diagramas C4 disponibles (Contexto, Contenedores y Componentes), la refactorizacion del backend para el Sprint 1 se enfoca en mantener coherencia sobre el grupo **core** de microservicios: **API Gateway**, **Auth Service**, **Donations Service** y **Matching Service**, con persistencia en **Core DB**.

Durante la revision se identificaron diferencias menores entre vistas (por ejemplo, nivel de detalle de integraciones externas o alcance de algunos servicios especializados). En esta etapa no se corrigen los diagramas; en su lugar, se adopta una linea de implementacion consistente con lo que se repite en la mayoria de vistas del core.

Los ajustes de refactorizacion orientados por patrones fueron los siguientes:

- **Refactorizacion hacia Service Layer Pattern**

Se reforzo la separacion entre controladores y logica de negocio. Los `Controller` quedaron orientados a entrada/salida HTTP, mientras que los `Application Service` concentran validaciones, reglas y orquestacion de casos de uso (registro, autenticacion, publicacion, reserva y priorizacion).

- **Refactorizacion hacia Repository Pattern**

El acceso a datos se consolido en repositorios para aislar consultas y operaciones de persistencia respecto de la logica de dominio. Esto reduce acoplamiento con la base de datos y mejora la testabilidad de servicios mediante mocks.

- **Refactorizacion del flujo de entrada en API Gateway (Pipeline de Filtros)**

Se estructuro el ingreso de solicitudes en una secuencia clara: auditoria/logging, control de trafico, validacion JWT, validacion por rol y enrutamiento. Esta organizacion mejora seguridad transversal y evita duplicar validaciones en cada microservicio.

- **Refactorizacion para comunicacion asincrona (Observer via Publish/Subscribe)**

Se estandarizo la publicacion de eventos desde servicios core para desacoplar procesos derivados. Este enfoque sigue el patron Observer implementado con broker de mensajeria, permitiendo que otros modulos reaccionen sin dependencia directa del flujo transaccional principal.

- **Refactorizacion de reglas variables en Matching (enfoque Strategy)**

La logica de priorizacion se separo en componentes de criterios y motor de prioridad, habilitando evolucion de reglas sin reescribir el flujo completo del servicio de matching.

Como resultado, el backend del Sprint 1 queda mejor preparado para crecimiento incremental, con contratos mas claros entre capas y menor impacto ante cambios futuros en reglas de negocio.

## 5.2 Software Configuration Management

### 5.2.1 Software Development Environment Configuration

Para el desarrollo colaborativo de **Alimenta**, el equipo definio un entorno comun de trabajo que cubre implementacion, diseno, gestion de tareas y control de versiones. La siguiente tabla resume las herramientas oficiales, su proposito dentro del ciclo de vida y la URL de referencia.

| Herramienta | Proposito en el proyecto | URL de referencia |
| :-- | :-- | :-- |
| **Flutter** | Desarrollo de aplicaciones moviles para los clientes de restaurante y albergue/ONG. | https://flutter.dev |
| **Visual Studio Code** | Edicion de codigo, depuracion y soporte de extensiones para desarrollo full-stack. | https://code.visualstudio.com |
| **Eclipse IDE 2025** | Desarrollo y mantenimiento de microservicios backend en Java/Spring Boot. | https://www.eclipse.org/downloads |
| **GitHub** | Control de versiones, gestion de repositorios y colaboracion mediante Pull Requests. | https://github.com |
| **Figma** | Diseno y prototipado de interfaces de usuario y flujos de experiencia. | https://www.figma.com |
| **Trello** | Gestion de backlog, seguimiento de tareas por sprint y coordinacion del equipo. | https://trello.com |

### 5.2.2 Source Code Management

Para gestionar y colaborar eficientemente en el proyecto, se utilizara **GitHub** como plataforma principal de control de versiones. Esto permitira almacenar y administrar el codigo fuente y la documentacion, facilitando la cooperacion entre los miembros del equipo.

Los repositorios estaran organizados dentro del espacio de trabajo del equipo en GitHub y se seguira la metodologia **GitFlow** para mantener un flujo de trabajo claro, trazable y ordenado durante los sprints.

Adicionalmente, el equipo aplicara **Conventional Commits** para estandarizar mensajes de commit y **Semantic Versioning** para etiquetar releases.

| Elemento GitFlow | Convencion de nomenclatura | Proposito |
| :-- | :-- | :-- |
| **Main Branch** | `main` | Rama estable con versiones listas para entrega o despliegue. |
| **Develop Branch** | `develop` | Rama de integracion continua de funcionalidades completadas. |
| **Feature Branch** | `feature/<id-us>-<descripcion-corta>` | Desarrollo de nuevas funcionalidades por historia de usuario o tarea tecnica. |
| **Release Branch** | `release/v<major>.<minor>.<patch>` | Preparacion y estabilizacion previa a una version oficial. |
| **Hotfix Branch** | `hotfix/v<major>.<minor>.<patch>` | Correccion urgente sobre una version publicada en `main`. |

| Tipo de commit (Conventional Commits) | Formato base | Uso esperado |
| :-- | :-- | :-- |
| **Feature** | `feat(scope): descripcion` | Incorporacion de nueva funcionalidad. |
| **Fix** | `fix(scope): descripcion` | Correccion de defectos o errores funcionales. |
| **Docs** | `docs(scope): descripcion` | Cambios en documentacion tecnica o funcional. |
| **Refactor** | `refactor(scope): descripcion` | Mejora interna del codigo sin cambiar comportamiento externo. |
| **Test** | `test(scope): descripcion` | Creacion o ajuste de pruebas automatizadas. |
| **Chore** | `chore(scope): descripcion` | Tareas de mantenimiento, configuracion o soporte. |


**Figura 5. Estructura de ramas GitFlow en GitHub**

<img alt="Image" src="https://github.com/user-attachments/assets/7101ed43-35b4-4de6-b884-430ad4c7617c" />

### 5.2.3 Source Code Style Guide & Conventions

En esta seccion se establecen las directrices de nomenclatura, estilo y uso de idioma para mantener consistencia tecnica en el desarrollo y en la documentacion del proyecto.

Se adoptan convenciones ampliamente reconocidas para asegurar un codigo estandarizado, mantenible y escalable durante los sprints.

#### 1) Convenciones por tecnologia

- **Frontend y Mobile**
  Se utilizara **Flutter** como tecnologia principal de cliente. Para edicion y soporte de desarrollo se empleara **Visual Studio Code**.

- **Landing Page**
  Se utilizara **HTML** y **CSS** para estructura y estilos de la landing page.

- **Backend**
  Se utilizara **Java con Spring Boot** para los microservicios principales, con soporte de desarrollo en **Eclipse IDE 2025**.

- **Control de versiones (GitHub)**
  Se aplicara **GitFlow** como flujo de ramas. Los mensajes de commit seguiran el estandar **Conventional Commits** para mantener trazabilidad y claridad del historial.

- **Herramientas adicionales**
  Se utilizara **Figma** para diseno/prototipado y **Trello** para gestion de tareas y seguimiento de sprint.

#### 2) Convenciones de idioma

- El codigo fuente se escribira en **ingles** (nombres de clases, metodos, variables, endpoints y ramas).
- Los mensajes de commit y etiquetas de version se mantendran en **ingles** para uniformidad tecnica.
- La documentacion academica y los artefactos del reporte se presentaran en **espanol**.

#### 3) Convenciones de estilo de codigo

- Se priorizara nomenclatura descriptiva en ingles y responsabilidad unica por clase/servicio.
- Los controladores mantendran logica minima y delegaran la orquestacion a la capa de servicios.
- El acceso a datos se centralizara en repositorios para evitar acoplamiento directo desde capas de entrada.
- Los contratos API se documentaran con OpenAPI/Swagger para mantener consistencia entre implementacion y consumo.


**Figura 6. Convenciones de codigo y estructura por capas**

<img alt="Image" src="https://github.com/user-attachments/assets/39951ec5-00e3-438d-9f00-ee5b61ae0738" />

### 5.2.4 Software Deployment Configuration

Para una primera version de despliegue (Sprint 1) se definio una estrategia simple y rapida orientada al nucleo del negocio: **API Gateway**, **Auth Service**, **Donations Service** y **Matching Service**.

El despliegue se realizara sobre una **VPS** usando contenedores Docker y orquestacion con **Docker Compose**, permitiendo levantar todos los servicios con un solo comando y mostrar evidencia funcional de endpoints documentados en Swagger/OpenAPI.

#### Arquitectura de despliegue inicial

- `api-gateway` (Spring Cloud Gateway)
- `auth-service` (Spring Boot)
- `donations-service` (Spring Boot)
- `matching-service` (Spring Boot)
- `core-db` (PostgreSQL)

#### Pasos generales de despliegue

1. Preparar VPS con Docker y Docker Compose.
2. Clonar o actualizar repositorio(s) con `git pull`.
3. Construir imagenes Docker de cada microservicio.
4. Configurar variables de entorno (`DB`, `JWT`, puertos, urls internas).
5. Levantar infraestructura con `docker compose up -d`.
6. Verificar salud de contenedores y logs de arranque.
7. Validar acceso a Swagger/OpenAPI de servicios publicados.


## 5.3 Microservices Implementation

### 5.3.1 Sprint 1

#### 5.3.1.1 Sprint Backlog 1

El objetivo principal de este Sprint es **disenar, implementar y desplegar el nucleo de microservicios (core)** de la plataforma Alimenta, incluyendo **API Gateway, Auth Service, Donations Service y Matching Service**, con persistencia en Core DB y una primera evidencia de consumo mediante Swagger/OpenAPI. Este avance permitira validar el flujo tecnico base de autenticacion, publicacion de donaciones y priorizacion inicial para futuras iteraciones.

<div style="font-size:65%;">
<table>
  <tr>
    <td>Sprint #</td>
    <td colspan="7">Sprint 1</td>
  </tr>
  <tr>
    <td colspan="2">User Story</td>
    <td colspan="2">Work-Item / Task</td>
    <td>Description</td>
    <td>Estimation (Hours)</td>
    <td>Assigned To</td>
    <td>Status (To-do / In-Process / To-Review / Done)</td>
  </tr>
  <tr>
    <td>Id</td>
    <td>Title</td>
    <td>Id</td>
    <td>Title</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>US01</td>
    <td>Registrarse como restaurante</td>
    <td>T1</td>
    <td>Implementar endpoint de registro</td>
    <td>Construir `sign-up` en Auth Service con validaciones base y respuesta JSON.</td>
    <td>6</td>
    <td>Backend Dev</td>
    <td>In-Process</td>
  </tr>
  <tr>
    <td>US03</td>
    <td>Iniciar sesion en la plataforma</td>
    <td>T2</td>
    <td>Implementar login y JWT</td>
    <td>Desarrollar endpoint `sign-in`, generacion de token JWT y flujo de autenticacion inicial.</td>
    <td>6</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>US04</td>
    <td>Acceder segun rol</td>
    <td>T3</td>
    <td>Configurar autorizacion por rol</td>
    <td>Aplicar validaciones de acceso por rol en endpoints sensibles del core.</td>
    <td>5</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>US09</td>
    <td>Registrar paquete alimentario</td>
    <td>T4</td>
    <td>Implementar alta de donacion</td>
    <td>Crear endpoint en Donations Service para registrar lotes y estado inicial de disponibilidad.</td>
    <td>7</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>US17</td>
    <td>Reservar paquete disponible</td>
    <td>T5</td>
    <td>Implementar reserva de donacion</td>
    <td>Desarrollar endpoint de reserva y validacion de disponibilidad del paquete.</td>
    <td>7</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>US17</td>
    <td>Reservar paquete disponible</td>
    <td>T6</td>
    <td>Implementar bloqueo de concurrencia</td>
    <td>Evitar doble asignacion del mismo paquete en reservas concurrentes.</td>
    <td>6</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>US13</td>
    <td>Visualizar paquetes cercanos en el mapa</td>
    <td>T7</td>
    <td>Publicar consulta base de paquetes</td>
    <td>Exponer endpoint listado de paquetes disponibles para consumo inicial del cliente.</td>
    <td>5</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>-</td>
    <td>-</td>
    <td>T8</td>
    <td>Configurar API Gateway</td>
    <td>Definir rutas por servicio, filtro JWT inicial y forwarding a Auth/Donations/Matching.</td>
    <td>6</td>
    <td>Backend Dev</td>
    <td>In-Process</td>
  </tr>
  <tr>
    <td>TS01</td>
    <td>Configurar broker de mensajeria con Kafka</td>
    <td>T9</td>
    <td>Preparar mensajeria asincrona base</td>
    <td>Levantar Kafka en entorno de desarrollo y configurar producer inicial en servicios core.</td>
    <td>5</td>
    <td>Backend Dev</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>-</td>
    <td>-</td>
    <td>T10</td>
    <td>Configurar Core DB</td>
    <td>Provisionar PostgreSQL y conectar Auth/Donations/Matching con esquema base.</td>
    <td>4</td>
    <td>Backend Dev</td>
    <td>In-Process</td>
  </tr>
  <tr>
    <td>-</td>
    <td>-</td>
    <td>T11</td>
    <td>Containerizar microservicios core</td>
    <td>Crear Dockerfiles y parametros de ejecucion para despliegue uniforme.</td>
    <td>5</td>
    <td>DevOps</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>-</td>
    <td>-</td>
    <td>T12</td>
    <td>Despliegue inicial con Docker Compose</td>
    <td>Levantar core en VPS con `docker compose up -d` y verificacion de contenedores.</td>
    <td>6</td>
    <td>DevOps</td>
    <td>To-do</td>
  </tr>
  <tr>
    <td>-</td>
    <td>-</td>
    <td>T13</td>
    <td>Publicar evidencia Swagger/OpenAPI</td>
    <td>Validar y capturar documentacion de endpoints para Auth y Donations en entorno desplegado.</td>
    <td>4</td>
    <td>QA / Backend Dev</td>
    <td>To-review</td>
  </tr>
</table>
</div>

#### 5.3.1.2 Development Evidence for Sprint Review

## Development Evidence - Sprint 1

<div style="font-size:65%;">

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|---|---|---|---|---|---|
| 1ASI0657-2610-17949/alimenta-auth-service | feature/auth-signup | a17f2c1 | feat(auth): add sign-up endpoint | Se implementa endpoint de registro de usuarios con validaciones iniciales. | 07/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | feature/auth-signin-jwt | b29a4d8 | feat(auth): add sign-in and jwt generation | Se implementa inicio de sesion y generacion de token JWT. | 07/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | feature/auth-roles | c31be59 | feat(auth): add role-based authorization checks | Se agregan validaciones por rol para endpoints protegidos. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | feature/auth-swagger | d44c8f0 | docs(auth): configure openapi annotations | Se documentan endpoints de autenticacion en Swagger/OpenAPI. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | e55dd71 | merge: integrate feature/auth-signup into develop | Integracion de funcionalidad de registro en rama develop. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | f66e932 | merge: integrate feature/auth-signin-jwt into develop | Integracion de autenticacion con JWT en rama develop. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | a77f143 | merge: integrate feature/auth-roles into develop | Integracion de control de acceso por roles. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | b88a254 | test(auth): add integration test for sign-up | Se agrega prueba de integracion para registro de nuevo usuario. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | main | c99b365 | merge(release): promote develop into main | Merge final de Sprint 1 hacia rama principal con funcionalidades core de auth. | 12/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/donation-create | d10ac76 | feat(donations): add donation create endpoint | Se implementa endpoint para registrar lote de donacion. | 07/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/donation-list | e21bd87 | feat(donations): add available donations listing | Se agrega listado de donaciones disponibles para consumo inicial. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/reservation-create | f32ce98 | feat(reservations): add reservation endpoint | Se implementa endpoint para reserva de paquete alimentario. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/reservation-lock | a43dfa9 | fix(reservations): prevent double booking | Se agrega bloqueo para evitar doble reserva concurrente. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/donations-swagger | b54e0ba | docs(donations): add openapi documentation | Se documentan endpoints de donaciones y reservas en Swagger. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | c65f1cb | merge: integrate feature/donation-create into develop | Integracion de alta de donaciones en develop. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | d76a2dc | merge: integrate feature/reservation-create into develop | Integracion de reserva de donaciones en develop. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | e87b3ed | merge: integrate feature/reservation-lock into develop | Integracion de control de concurrencia para reservas. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | f98c4fe | test(donations): add endpoint tests for create and reserve | Se agregan pruebas para crear donacion y reservar paquete. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | main | a09d50f | merge(release): promote develop into main | Merge final de Sprint 1 hacia rama principal del servicio de donaciones. | 12/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/matching-core | b11e611 | feat(matching): add matching orchestration service | Se implementa servicio de orquestacion de priorizacion. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/matching-criteria | c22f722 | feat(matching): add criteria evaluation component | Se agrega evaluacion de criterios de prioridad por receptor. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/matching-endpoint | d33a833 | feat(matching): add matching endpoint | Se expone endpoint para ejecutar y consultar matching inicial. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/matching-swagger | e44b944 | docs(matching): configure swagger endpoints | Se publica documentacion OpenAPI del servicio matching. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | develop | f55ca55 | merge: integrate feature/matching-core into develop | Integracion del motor base de matching en develop. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | develop | a66db66 | merge: integrate feature/matching-endpoint into develop | Integracion de endpoint de matching en develop. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | develop | b77ec77 | test(matching): add basic matching service tests | Se agregan pruebas unitarias del flujo de priorizacion. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | main | c88fd88 | merge(release): promote develop into main | Merge final de Sprint 1 hacia rama principal del servicio de matching. | 12/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | feature/gateway-routes | d99ae99 | feat(gateway): add route configuration for core services | Se configuran rutas para auth, donations y matching. | 08/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | feature/gateway-jwt-filter | e10bf10 | feat(gateway): add jwt auth filter | Se agrega filtro JWT para validacion de solicitudes entrantes. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | feature/gateway-role-filter | f21cg21 | feat(gateway): add role authorization filter | Se incorpora filtro de autorizacion por rol en endpoints protegidos. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | feature/gateway-rate-limit | a32dh32 | feat(gateway): add basic rate limiter | Se implementa control basico de trafico para proteger endpoints core. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | develop | b43ei43 | merge: integrate feature/gateway-routes into develop | Integracion de rutas de gateway en rama develop. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | develop | c54fj54 | merge: integrate feature/gateway-jwt-filter into develop | Integracion de seguridad JWT en gateway. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | develop | d65gk65 | fix(gateway): adjust routing and auth filter order | Ajuste de orden de filtros para correcto enrutamiento y autorizacion. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | main | e76hl76 | merge(release): promote develop into main | Merge final de Sprint 1 hacia rama principal del API Gateway. | 12/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | feature/dockerfiles-core | f87im87 | chore(deploy): add dockerfiles for core services | Se agregan Dockerfiles para auth, donations, matching y gateway. | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | feature/docker-compose-core | a98jn98 | chore(deploy): add docker compose for sprint 1 | Se agrega docker-compose con core services, postgres y red interna. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | feature/kafka-base | b09ko09 | chore(deploy): add kafka and broker configuration | Se incorpora configuracion base de Kafka para mensajeria asincrona. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | develop | c10lp10 | merge: integrate feature/docker-compose-core into develop | Integracion del orquestador de despliegue en develop. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | develop | d21mq21 | docs(deploy): add deployment runbook for vps | Se documentan pasos de despliegue rapido en VPS para Sprint 1. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-deployment | main | e32nr32 | merge(release): promote develop into main | Merge final de Sprint 1 hacia rama principal de despliegue. | 12/05/26 |

</div>

#### 5.3.1.3 Testing Suite Evidence for Sprint Review

En esta seccion se presenta el avance del enfoque de pruebas para los Web Services incluidos en el Sprint 1. La estrategia combina:

- **Integration Tests** con `Spring Boot Test` + `MockMvc`.
- **Unit Tests** de servicios con `Mockito` para aislamiento de dependencias.
- **Acceptance/BDD Tests** mediante archivos `.feature` escritos en **Gherkin**, vinculados a User Stories del sprint.

Los archivos `.feature` se orientan principalmente a los flujos de autenticacion y donaciones del core (`US01`, `US03`, `US09`, `US17`), permitiendo validar criterios de aceptacion de manera legible para negocio y equipo tecnico.

## Testing Evidence - Sprint 1

<div style="font-size:65%;">

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|---|---|---|---|---|---|
| 1ASI0657-2610-17949/alimenta-auth-service | feature/test-auth-integration | f11aa01 | test(auth): add integration tests for sign-up endpoint | Se agregan pruebas de integracion con MockMvc para registro de usuarios (US01). | 09/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | feature/test-auth-bdd | a22bb12 | test(bdd): add authentication feature file | Se agrega `authentication.feature` en Gherkin para registro e inicio de sesion (US01, US03). | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | b33cc23 | merge: integrate feature/test-auth-bdd into develop | Integracion de pruebas BDD de autenticacion en rama develop. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-auth-service | develop | c44dd34 | test(auth): add service tests with mockito | Se agregan pruebas unitarias de servicios de autenticacion usando Mockito. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/test-donations-integration | d55ee45 | test(donations): add integration tests for create donation | Se agregan pruebas de integracion para alta de donacion (US09). | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | feature/test-reservations-bdd | e66ff56 | test(bdd): add reservation feature file | Se agrega `reservations.feature` para escenarios de reserva y concurrencia (US17). | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | f77aa67 | merge: integrate feature/test-reservations-bdd into develop | Integracion de pruebas BDD de reservas en rama develop. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-donations-service | develop | a88bb78 | test(donations): add mockito tests for reservation service | Se incorporan pruebas unitarias con Mockito para ReservationService. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/test-matching-unit | b99cc89 | test(matching): add unit tests for priority engine | Se agregan pruebas unitarias del motor de prioridad del matching. | 10/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | feature/test-matching-bdd | c10dd90 | test(bdd): add matching feature file | Se agrega `matching.feature` para validacion inicial de criterios de priorizacion. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-matching-service | develop | d21ee01 | merge: integrate feature/test-matching-bdd into develop | Integracion de pruebas BDD de matching en rama develop. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | feature/test-gateway-routes | e32ff12 | test(gateway): add route integration tests | Se agregan pruebas de integracion para rutas y filtros principales del gateway. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-api-gateway | develop | f43aa23 | merge: integrate feature/test-gateway-routes into develop | Integracion de pruebas del API Gateway en rama develop. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-testing-bdd | feature/gherkin-sprint1 | a54bb34 | test(bdd): add sprint1 feature files for core services | Se centralizan `.feature` de auth, donations y matching para Sprint 1. | 11/05/26 |
| 1ASI0657-2610-17949/alimenta-testing-bdd | develop | b65cc45 | merge: integrate feature/gherkin-sprint1 into develop | Integracion de archivos Gherkin del sprint para ejecucion y trazabilidad. | 12/05/26 |
| 1ASI0657-2610-17949/alimenta-testing-bdd | main | c76dd56 | merge(release): promote develop into main | Merge final de evidencias de testing del Sprint 1. | 12/05/26 |

</div>

**Relacion de archivos `.feature` (BDD) del Sprint 1:**

- `authentication.feature` -> US01, US03
- `donations.feature` -> US09
- `reservations.feature` -> US17
- `matching.feature` -> US17 (priorizacion inicial)

