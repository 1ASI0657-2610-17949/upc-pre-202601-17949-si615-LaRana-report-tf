# Capítulo IV: Product Architecture Design

## 4.1 Desing Concepts, ViewPoints & ER Diagrams

### 4.1.1 Principles Statements

De la vision de negocio y de la vision arquitectonica de **Alimenta** se desprenden los siguientes principios de diseno. Estos principios orientan las decisiones tecnicas del sistema para asegurar escalabilidad, trazabilidad, seguridad y evolucion progresiva de la plataforma.

- **Priorizar comunicacion asincrona para procesos desacoplados.** Las interacciones criticas de negocio pueden iniciar de forma sincrona, pero los eventos derivados como notificaciones, matching, actualizacion de metricas, trazabilidad o reconocimiento deben ejecutarse de forma asincrona para reducir acoplamiento y mejorar la escalabilidad.

- **Separar responsabilidades por capacidad de negocio.** Cada servicio o microservicio debe concentrarse en una responsabilidad clara, como autenticacion, donaciones, seguridad alimentaria, logistica o impacto social, evitando mezclar reglas de negocio no relacionadas en un mismo componente.

- **Compartir datos solo donde el acoplamiento este justificado.** Los servicios centrales pueden apoyarse en una base compartida cuando forman parte de un nucleo operativo comun, mientras que los dominios especializados como seguridad alimentaria, logistica e impacto deben mantener bases separadas para preservar autonomia y evolucion independiente.

- **Trazabilidad como capacidad transversal.** Toda donacion debe dejar evidencia verificable desde su publicacion hasta su entrega, incluyendo estados, responsables, validaciones, ubicaciones y evidencias multimedia, de modo que el sistema pueda auditar cada intercambio.

- **Seguridad y permisos desde el diseno.** El acceso a funcionalidades y datos debe controlarse desde la arquitectura mediante autenticacion, autorizacion por roles y validacion de permisos para restaurante, albergue, recolector y administrador.

- **Validar primero, ejecutar despues.** Ninguna donacion debe avanzar a etapas logisticas o de entrega sin haber superado las validaciones funcionales y, cuando aplique, las validaciones de seguridad alimentaria correspondientes.

- **Diseno orientado a disponibilidad operativa.** Las funciones principales del flujo de donacion, reserva, evaluacion, recojo y entrega deben mantenerse operativas aun cuando servicios secundarios presenten latencia o fallos temporales.

- **Modelar para evolucionar.** La arquitectura debe permitir incorporar nuevos criterios de priorizacion, nuevos tipos de evidencia, reglas sanitarias o mecanismos de reconocimiento sin requerir redisenos completos del nucleo transaccional.

- **Usar tecnologias y bibliotecas maduras.** Se priorizaran frameworks, librerias y herramientas con documentacion estable, adopcion amplia y soporte comercial o comunitario solido, con el fin de reducir riesgo tecnico y facilitar mantenimiento.

- **Diseñar interfaces simples y explicitas.** Cada servicio debe exponer contratos claros, consistentes y orientados al dominio, minimizando dependencias ocultas y facilitando pruebas, integracion y mantenimiento.

### 4.1.2 Approaches Statements Architectural Styles & Patterns

Para el desarrollo de **Alimenta** se adopta como enfoque principal **Domain Driven Design (DDD)**, debido a que el problema de negocio presenta capacidades diferenciadas que evolucionan con reglas propias, como autenticacion, donaciones, priorizacion, seguridad alimentaria, logistica e impacto social. Este enfoque permite organizar el sistema alrededor del dominio y no solo de componentes tecnicos, facilitando la separacion de responsabilidades y la evolucion progresiva de la solucion.

Desde DDD, la arquitectura se organiza en **bounded contexts** que representan areas funcionales con lenguaje y reglas propias. En este proyecto, los contextos mas claros son: **Usuarios/Auth**, **Donaciones y Reservas**, **Matching y Priorizacion**, **Seguridad Alimentaria**, **Logistica y Evidencia de Entrega** e **Impacto Social y Reconocimiento**. Esta particion permite que los servicios centrales compartan un nucleo operativo comun cuando se justifica, mientras que los microservicios especializados mantengan autonomia tecnica y de datos.

El estilo arquitectonico principal considerado es una **arquitectura de microservicios con enfoque hibrido de persistencia**. Se plantea un nucleo central con servicios que comparten una base de datos operativa para reducir complejidad en procesos altamente acoplados del flujo transaccional principal, y microservicios especializados con bases separadas para dominios con reglas mas autonomas. Esta combinacion busca un equilibrio entre simplicidad inicial, escalabilidad selectiva y bajo acoplamiento en capacidades avanzadas.

Como complemento, se considera un estilo **event-driven** para la comunicacion entre servicios en procesos desacoplados. Las operaciones centrales, como autenticacion, registro de donaciones o reservas, pueden resolverse mediante llamadas sincronas cuando la respuesta inmediata es necesaria. Sin embargo, procesos derivados como priorizacion, notificaciones, actualizacion de metricas, generacion de evidencias o reconocimientos deben apoyarse en eventos para evitar dependencias directas entre componentes.

Sobre estos estilos se aplican los siguientes patrones arquitectonicos y de diseno:

- **API Gateway.** Centraliza el ingreso de clientes web o moviles, simplifica el acceso a los servicios y concentra preocupaciones transversales como autenticacion, autorizacion y routing.

- **Database per Service** en microservicios especializados. Se aplica en Seguridad Alimentaria, Logistica e Impacto para preservar autonomia, despliegue independiente y control de su propio modelo de datos.

- **Shared Database** en el nucleo operativo. Se utiliza de manera controlada en Usuarios/Auth, Donaciones y Reservas, y Matching y Priorizacion, debido a la fuerte relacion transaccional entre estas capacidades durante la etapa principal del flujo de negocio.

- **Publish/Subscribe.** Permite que eventos como donacion publicada, donacion validada o entrega confirmada activen procesos en otros servicios sin generar acoplamiento temporal fuerte.

- **Saga basica u orquestacion por estados.** Ayuda a coordinar flujos de varias etapas, por ejemplo desde la publicacion de una donacion hasta su validacion, asignacion de recolector, entrega y registro de impacto, manteniendo consistencia operativa entre servicios.

- **Repository Pattern.** Separa la logica de dominio del acceso a datos y facilita que cada servicio gestione su persistencia sin contaminar la capa de negocio.

- **DTO y contratos explicitos.** Permiten intercambiar datos entre servicios y clientes de forma controlada, reduciendo dependencia directa de modelos internos.

- **Strategy Pattern.** Resulta adecuado para encapsular diferentes reglas de priorizacion, clasificacion sanitaria o criterios de reconocimiento sin modificar el flujo principal del sistema.

- **Adapter Pattern.** Facilita la integracion con servicios externos o componentes de infraestructura, como motores de IA para analisis de alimentos, almacenamiento de evidencias o servicios de geolocalizacion.

En conjunto, estos enfoques permiten que **Alimenta** mantenga un nucleo funcional simple para el flujo principal de donaciones, mientras reserva mayor independencia arquitectonica para capacidades avanzadas que requieren escalamiento, evolucion y reglas especializadas.

### 4.1.3 Context Diagram

<img alt="Image" src="https://github.com/user-attachments/assets/04d74eaa-0c56-430a-b6af-fd6f72f3de02" />

### 4.1.4 Approach driven ViewPoints Diagrams

#### Diagrama de Contexto

<img alt="Image" src="https://github.com/user-attachments/assets/48b9c1d2-5d9f-49a8-9ad7-5ff609dac702" />

#### Diagrama de Contenedores

<img alt="Image" src="https://github.com/user-attachments/assets/2ce7194d-787a-4598-8af2-1eeba571933c" />

#### Diagrama de Componentes

##### API Gateway

<img alt="Image" src="https://github.com/user-attachments/assets/ebff2be4-5892-4c02-b457-c6ccefafbe13" />

##### Auth Service

<img alt="Image" src="https://github.com/user-attachments/assets/ebff2be4-5892-4c02-b457-c6ccefafbe13" />

##### Social Impact Service

<img alt="Image" src="https://github.com/user-attachments/assets/54c2c7fd-377e-44c0-8469-26b4b4a83177" />

##### Donations Service

<img alt="Image" src="https://github.com/user-attachments/assets/0010526c-09f8-452a-b9cd-37a336dfa319" />

##### Food Safety Service

<img alt="Image" src="https://github.com/user-attachments/assets/49e51378-2d24-4d33-b3b1-4ead5f0d00d5" />

##### Logistics Service

<img  alt="Image" src="https://github.com/user-attachments/assets/08a14c5e-13b7-4bfa-8dd1-42d9532fc3e6" />

##### Matching Service

<img alt="Image" src="https://github.com/user-attachments/assets/c4c9109e-dc68-49e9-95f6-35d7571b78ac" />

### Diagrama de Actividades UML - Flujo Principal de Donacion

<img alt="Image" src="https://github.com/user-attachments/assets/345a8d8c-570b-4a23-a90b-6db14f90c8cc" />

#### Diagrama de Estados UML

<img alt="Image" src="https://github.com/user-attachments/assets/06ac5532-af5e-458e-8610-67a97ee3e810" />

#### Diagrama de UML clases

##### Auth Service

<img alt="Image" src="https://github.com/user-attachments/assets/70997363-7aa4-46d1-953d-95c88781d754" />

##### Donations Service

<img alt="Image" src="https://github.com/user-attachments/assets/740fc384-6b35-4d65-a44b-9ae7e932abe2" />

#### Matching Service

<img alt="Image" src="https://github.com/user-attachments/assets/27485190-2acb-4dc9-85c1-c724e985f1d8" />

#### Social Impact Service

<img alt="Image" src="https://github.com/user-attachments/assets/bdca4b40-8df8-4809-87d4-f285fdff9572" />

#### Food Safety Service

<img alt="Image" src="https://github.com/user-attachments/assets/4799c402-dbf7-470e-a641-6871fe726221" />

#### Logistics Service

<img alt="Image" src="https://github.com/user-attachments/assets/05255168-ae1a-4839-926e-9e7054cc47db" />

### 4.1.5 Relational/Non Relational Database Diagram

Para **Alimenta** se ha definido el uso exclusivo de **bases de datos relacionales**. Esta decision responde a que el dominio principal del sistema requiere integridad referencial, control de estados, trazabilidad de transacciones, consistencia entre relaciones y facilidad para modelar constraints como llaves primarias, llaves foraneas, unicidad y validaciones de negocio. En consecuencia, tanto la **Core DB** como las bases de datos especializadas de **Food Safety**, **Logistics** e **Impact** se modelan bajo un enfoque relacional.

Las entidades clave identificadas para la persistencia del sistema son las siguientes:

- `users`: almacena la identidad base de cada usuario registrado en la plataforma.
- `user_profiles`: contiene los datos de perfil y contacto asociados al usuario.
- `roles`: define los tipos de rol disponibles dentro del sistema.
- `permissions`: define permisos granulares para controlar acceso a funcionalidades.
- `role_permissions`: relaciona roles con permisos.
- `user_roles`: relaciona usuarios con sus roles asignados.
- `donation_lots`: registra cada lote de alimentos publicado por un restaurante.
- `donation_locations`: almacena la ubicacion de recojo asociada a una donacion.
- `reservations`: registra la reserva de una donacion por parte de un albergue.
- `matching_processes`: almacena cada ejecucion del proceso de priorizacion.
- `candidate_receivers`: registra los receptores candidatos evaluados en cada matching.
- `priority_rules`: define las reglas y pesos usados para priorizar receptores.
- `safety_evaluations`: almacena la evaluacion sanitaria de una donacion.
- `checklist_items`: registra el detalle del checklist aplicado en seguridad alimentaria.
- `food_evidences`: almacena referencias a evidencias fotografias usadas en la validacion sanitaria.
- `ai_analysis_results`: registra resultados devueltos por el analisis asistido por IA.
- `delivery_processes`: registra el flujo logistico de recojo y entrega de una donacion.
- `tracking_points`: almacena puntos GPS reportados durante el traslado.
- `delivery_evidences`: almacena archivos de evidencia asociados a la entrega.
- `delivery_locations`: registra el destino logístico de la entrega.
- `impact_records`: consolida el impacto generado por una donacion completada.
- `impact_metrics`: almacena indicadores cuantitativos derivados de las donaciones.
- `recognitions`: registra certificados o insignias otorgadas a restaurantes.
- `shelter_publications`: almacena publicaciones realizadas por albergues sobre donaciones recibidas.

El siguiente diagrama muestra la propuesta de modelo relacional del sistema, incluyendo tablas, atributos principales, llaves primarias, llaves foraneas y relaciones entre bounded contexts.

<img alt="Image" src="https://github.com/user-attachments/assets/d8f928c0-9d2c-44b1-8ec2-71a3f7fda46a" />

click para ver completo [enlace](https://github.com/user-attachments/assets/d8f928c0-9d2c-44b1-8ec2-71a3f7fda46a)

### 4.1.6 Design Patterns

- **Repository Pattern**

  **Objetivo:**  
  Abstraer y encapsular el acceso a la base de datos, separando la logica de persistencia de las entidades y agregados del dominio.

  **Aplicacion:**  
  Se implementa en servicios como **Auth**, **Donations**, **Matching**, **Food Safety**, **Logistics** y **Social Impact**, mediante repositorios como `UserRepository`, `RoleRepository`, `DonationRepository`, `ReservationRepository`, `MatchingRepository`, `FoodRepository`, `LogisticsRepository`, `TrackingRepository`, `MetricsRepository` y `RecognitionRepository`. Estos repositorios concentran las operaciones de lectura y escritura sobre las tablas del modelo relacional.

  Como beneficio, este patron facilita desacoplar la logica de negocio de la tecnologia de persistencia y permite mantener, extender o sustituir la capa de acceso a datos sin afectar el comportamiento central del dominio.

- **Service Layer Pattern**

  **Objetivo:**  
  Centralizar la logica de aplicacion en servicios que coordinan casos de uso, validaciones, reglas del dominio e interacciones entre repositorios y adaptadores externos.

  **Aplicacion:**  
  Se utiliza en componentes como `AuthenticationService`, `UserManagementService`, `DonationService`, `ReservationService`, `MatchingService`, `ClassificationService`, `TrackingService`, `ImpactMetricsService` y `RecognitionService`. Cada uno organiza operaciones del negocio como autenticacion, publicacion de donaciones, priorizacion, validacion sanitaria, tracking GPS y generacion de reconocimientos.

  Como beneficio, este patron mejora la organizacion del codigo, evita duplicidad de logica en controllers y favorece una separacion clara entre capa de entrada, capa de aplicacion y persistencia.

- **Observer Pattern**

  **Objetivo:**  
  Permitir que multiples componentes reaccionen automaticamente ante cambios de estado o eventos del sistema sin depender de invocaciones directas y fuertemente acopladas.

  **Aplicacion:**  
  Se aplica en la arquitectura orientada a eventos mediante **Kafka**, donde servicios como **Donations**, **Food Safety** y **Logistics** publican eventos, y servicios como **Social Impact** o mecanismos de notificacion reaccionan a ellos. Este patron resulta especialmente apropiado para procesos como actualizacion del estado de una donacion, tracking del traslado, confirmacion de entrega y calculo posterior de metricas o reconocimientos.

  Como beneficio, este patron favorece el desacoplamiento entre productores y consumidores de eventos, mejora la escalabilidad y permite extender nuevas reacciones del sistema sin modificar el flujo principal.

- **Strategy Pattern**

  **Objetivo:**  
  Encapsular algoritmos o reglas variables en componentes intercambiables, permitiendo modificar criterios de decision sin alterar el flujo principal del servicio.

  **Aplicacion:**  
  Se considera especialmente adecuado en el **Matching Service** para manejar distintas estrategias de priorizacion segun cercania, urgencia, capacidad o disponibilidad del albergue. Tambien puede aplicarse en **Food Safety** para evaluar distintos criterios de clasificacion sanitaria o en **Social Impact** para definir reglas de generacion de insignias y certificados.

  Como beneficio, este patron permite evolucionar reglas del negocio de forma controlada, facilita pruebas sobre algoritmos de decision y reduce el impacto de cambios futuros en criterios operativos.

- **Adapter Pattern**

  **Objetivo:**  
  Integrar servicios externos a traves de interfaces internas consistentes, aislando detalles tecnicos de proveedores, protocolos o SDKs especificos.

  **Aplicacion:**  
  Se aplica en adaptadores como `AIAdapter`, `S3Adapter` y `FCMAdapter`, que encapsulan la integracion con el servicio de analisis de imagenes, el almacenamiento en Amazon S3 y el envio de notificaciones push mediante Firebase Cloud Messaging. De esta forma, la logica de negocio no depende directamente de las APIs externas.

  Como beneficio, este patron reduce el acoplamiento con infraestructura externa, facilita mantenimiento y mejora la posibilidad de reemplazar proveedores sin reescribir los servicios de dominio.

### 4.1.7 Tactics

#### 1. Tacticas de Rendimiento (Performance)

- **Caching de consultas frecuentes:**  
  Se considera el uso de cache para consultas de alta frecuencia, como perfiles de usuario, roles, permisos, donaciones disponibles, resultados de matching recientes y metricas de impacto, reduciendo la carga sobre las bases de datos relacionales.

- **Procesamiento asincrono con Kafka:**  
  Las operaciones derivadas como notificaciones, actualizacion de metricas, registro de impacto y procesamiento de eventos logisticos se ejecutan de manera asincrona mediante Kafka, evitando bloquear el flujo principal de publicacion, reserva o entrega.

- **Paginacion y filtrado de resultados:**  
  Para listados como donaciones, historial, publicaciones o tracking, se emplea paginacion y filtros por estado, fecha o usuario, con el fin de reducir volumen de datos transferidos y mejorar tiempos de respuesta.

#### 2. Tacticas de Disponibilidad (Availability)

- **Aislamiento por servicios y bases de datos:**  
  La separacion entre Core DB y las bases independientes de Food Safety, Logistics e Impact permite que una falla en un servicio especializado no detenga completamente la operacion principal del sistema.

- **Degradacion controlada de funcionalidades secundarias:**  
  Si servicios como impacto social, reconocimiento o analisis asistido por IA presentan fallas temporales, el sistema debe permitir que el flujo principal de donacion y reserva continue operando con normalidad.

- **Reintentos en integraciones externas:**  
  Las integraciones con Firebase Cloud Messaging, Amazon S3 o servicios externos de analisis pueden aplicar reintentos controlados para reducir el efecto de errores temporales de red o indisponibilidad parcial.

#### 3. Tacticas de Seguridad (Security)

- **Autenticacion basada en JWT:**  
  El acceso a los servicios se protege mediante tokens JWT validados en el API Gateway y en el Auth Service, garantizando que solo usuarios autenticados interactuen con la plataforma.

- **Autorizacion por roles y permisos:**  
  Las operaciones se restringen segun el rol del usuario, diferenciando acciones de restaurante, albergue y administracion interna, con validaciones explicitas sobre cada funcionalidad sensible.

- **Proteccion de evidencias y datos sensibles:**  
  Los archivos de evidencia almacenados en Amazon S3 y la informacion sensible del sistema deben gestionarse con acceso controlado, evitando exposicion no autorizada de fotos, videos, ubicaciones o credenciales.

#### 4. Tacticas de Modificabilidad (Modifiability)

- **Separacion por bounded contexts:**  
  La division del sistema en Auth, Donations, Matching, Food Safety, Logistics e Impact permite modificar reglas de negocio especializadas sin afectar todo el sistema de forma transversal.

- **Uso de patrones Repository y Service Layer:**  
  La separacion entre logica de aplicacion, dominio y persistencia facilita incorporar cambios en reglas, entidades o repositorios con menor impacto sobre otras capas.

- **Encapsulamiento de reglas variables con Strategy:**  
  Reglas como priorizacion de receptores, clasificacion sanitaria o asignacion de reconocimientos pueden evolucionar mediante estrategias intercambiables sin redisenar los servicios completos.

#### 5. Tacticas de Trazabilidad y Auditabilidad (Traceability)

- **Registro de estados y eventos del flujo:**  
  Cada donacion mantiene un seguimiento de estados desde su publicacion hasta su entrega o cancelacion, permitiendo reconstruir el historial operativo completo.

- **Persistencia de tracking y evidencias:**  
  El servicio de Logistics conserva puntos GPS, evidencias multimedia y confirmaciones de entrega para respaldar la ejecucion real del proceso de traslado.

- **Asociacion entre donacion, validacion e impacto:**  
  La arquitectura relacional vincula las evaluaciones sanitarias, entregas e indicadores de impacto con una donacion concreta, permitiendo auditoria funcional y analisis posterior del ciclo completo.

## 4.2. Architectural Drivers
### 4.2.1. Design Purpose
El propósito fundamental del diseño arquitectónico para Alimenta radica en la creación de una plataforma digital ágil, escalable y eficiente que aborde la ineficiencia logística en la redistribución de excedentes alimentarios de "último minuto". La aplicación busca simplificar y automatizar la conexión en tiempo real entre los restaurantes y los albergues u organizaciones sociales (ONGs), eliminando las barreras operativas y de tiempo tradicionales. A través de este diseño, se pretende garantizar una alta disponibilidad y rapidez en el sistema para facilitar la publicación instantánea, la geolocalización, la reserva concurrente sin conflictos y la trazabilidad segura de las donaciones, reduciendo así drásticamente el desperdicio masivo de alimentos aptos para el consumo humano.
### 4.2.2. Primary Functionality (Primary User Stories)

| ID User Story | User Story |
| :--- | :--- |
| **US09** | Como representante de un restaurante, quiero publicar un paquete alimentario con su tipo, cantidad y horario límite de recojo, para ponerlo a disposición de ONGs cercanas antes de que se pierda. |
| **US13** | Como representante de una ONG, quiero visualizar en un mapa los paquetes alimentarios disponibles cerca de mi ubicación, para identificar rápidamente oportunidades de recojo. |
| **US15** | Como representante de una ONG, quiero recibir una notificación automática cuando se publique un paquete cercano, para reaccionar rápidamente y aumentar las posibilidades de recojo. |
| **US17** | Como representante de una ONG, quiero reservar un paquete alimentario disponible, para asegurar su asignación a mi organización antes de que otra ONG lo tome. |
| **US27** | Como usuario participante en el intercambio, quiero confirmar digitalmente que la donación fue entregada, para cerrar el proceso de manera segura y verificable. |
| **US34** | Como sistema, quiero cancelar automáticamente un paquete alimentario cuando se alcance su horario límite de recojo, para evitar que paquetes vencidos sigan disponibles en la plataforma. |
| **US35** | Como sistema, quiero liberar un paquete reservado si no es recogido dentro del tiempo establecido, para permitir que otras ONGs puedan acceder a él antes de que expire. |
### 4.2.3. Quality Attribute Scenarios

A continuación, se muestran los principales *quality attribute scenarios*, abarcando aspectos críticos como rendimiento, disponibilidad, consistencia de datos y seguridad. Estos atributos garantizan que la aplicación Alimenta no solo funcione correctamente, sino que proporcione la inmediatez logística, la tolerancia a fallos y la confiabilidad requerida para operar eficientemente durante los cierres de turno y coordinar rescates de último minuto.

| ID | Atributo de Calidad | Escenarios | Historia de Usuario Asociada | Métrica |
| :--- | :--- | :--- | :--- | :--- |
| **AC001** | Rendimiento (Performance) | El sistema debe procesar la publicación de donaciones y el envío de alertas de manera casi instantánea para no interrumpir el flujo del personal de cocina. | US09, US15 | Tiempo de registro de paquete ≤ 2 segundos; Tiempo de envío de notificación push ≤ 5 segundos. |
| **AC002** | Consistencia de Datos (Integrity) | El sistema debe prevenir conflictos de concurrencia, asegurando que un paquete disponible no pueda ser reservado por más de una ONG al mismo tiempo. | US17 | 0% de ocurrencia de reservas duplicadas o conflictos de asignación. |
| **AC003** | Disponibilidad (Availability) | La plataforma debe estar operativa y accesible constantemente para garantizar que ninguna donación se pierda por caídas del sistema en horarios nocturnos. | US13, US17, US34 | Porcentaje de *uptime* (tiempo de actividad) mínimo del 99.5% mensual. |
| **AC004** | Escalabilidad (Scalability) | El sistema debe soportar picos de alto tráfico (ej. horario de cierre simultáneo de restaurantes) sin que el rendimiento y los tiempos de carga se degraden. | US09, US13 | Capacidad de soportar 1,000 usuarios concurrentes sin que el tiempo de respuesta supere los 3 segundos. |
| **AC005** | Tolerancia a Fallos (Reliability) | El sistema debe permitir que procesos automáticos (como la reasignación de paquetes) operen correctamente, evitando inconsistencias o bloqueos si ocurren fallos aislados en otros servicios. | US35 | Tasa de transacciones de donación y reasignaciones automáticas completadas ≥ 99% durante ventanas de fallos parciales. |
| **AC006** | Seguridad (Security) | El sistema debe asegurar que las confirmaciones y validaciones de los intercambios (como el escaneo del código QR) se realicen de forma segura e inmutable. | US27 | 100% de las acciones críticas de entrega requieren validación digital legítima y un token de sesión válido. |

### 4.2.4. Constraints
| ID | Constraints |
| :--- | :--- |
| **CON001** | La arquitectura del sistema tendrá un enfoque estrictamente basado en microservicios, para permitir el escalamiento independiente de módulos como inventario, geolocalización y notificaciones. |
| **CON002** | El backend de los microservicios será desarrollado utilizando el lenguaje de programación Java junto con el framework Spring Boot. |
| **CON003** | Se utilizará obligatoriamente **RabbitMQ** como *broker* de mensajería central para garantizar la comunicación asíncrona y la tolerancia a fallos entre los distintos microservicios. |
| **CON004** | El almacenamiento de archivos estáticos, específicamente los mapas vectoriales (`.pmtiles`) y las evidencias fotográficas de los recojos, deberá realizarse utilizando un *bucket* de **AWS S3**[cite: 1]. |
| **CON005** | Se usará **GitHub** como plataforma centralizada para el control de versiones y la gestión del código fuente de todo el proyecto. |
| **CON006** | El sistema debe implementar estándares de seguridad robustos, requiriendo el uso de **JWT (JSON Web Tokens)** para la autenticación de usuarios y comunicación cifrada mediante **HTTPS** en todos los *endpoints*. |
| **CON007** | El flujo de cierre de donaciones estará restringido al uso de tecnologías de validación en sitio, requiriendo la generación y lectura de códigos **QR** para confirmar las entregas de manera inmutable. |
| **CON008** | La gestión del proyecto, seguimiento de tareas y diseño de interfaces se apoyará en herramientas colaborativas estándar de la industria (como Jira, Trello y Figma). |

### 4.2.5. Architectural Concerns

| ID | Preocupación Arquitectónica | Concern |
| :--- | :--- | :--- |
| ARC-1 | Escalabilidad ante Picos de Tráfico | La arquitectura debe ser capaz de escalar dinámicamente para soportar la alta concurrencia generada en los horarios pico (cierres de turno de almuerzo y cena), cuando múltiples restaurantes publican paquetes simultáneamente. |
| ARC-2 | Consistencia de Datos Transaccional | Es crítico manejar adecuadamente la concurrencia para asegurar que un mismo paquete alimentario no pueda ser reservado por más de una ONG al mismo tiempo, evitando conflictos logísticos. |
| ARC-3 | Alta Disponibilidad y Tolerancia a Fallos | El sistema debe mantener su operatividad central (publicar y reservar) incluso si servicios secundarios, como el envío de notificaciones push, experimentan caídas o intermitencias. |
| ARC-4 | Desacoplamiento y Mensajería Asíncrona | Es fundamental diseñar una comunicación eficiente entre microservicios (Inventory, Geo, Notification, Tracking) utilizando RabbitMQ, para evitar cuellos de botella y llamadas bloqueantes en la red. |
| ARC-5 | Seguridad, Autenticación y Trazabilidad | Es vital proteger el acceso a los *endpoints* mediante JWT y asegurar que las entregas (validadas mediante QR) queden registradas de forma segura e inmutable para los reportes de impacto de los donantes. |
| ARC-6 | Mantenibilidad y Evolución | El diseño basado en microservicios debe permitir la incorporación ágil de futuras funcionalidades (como nuevos algoritmos de ruteo o canales de alerta) con un mínimo impacto en el entorno de producción actual. |
| ARC-7 | Rendimiento y Baja Latencia Geográfica | El cálculo de distancias (Geo Service) y la carga de mapas interactivos deben realizarse con la menor latencia posible para asegurar una respuesta inmediata frente a donaciones con ventanas de tiempo cortas. |


## 4.3 ADD Iterations

### 4.3.1 Iteración 1: Estructura Base del Sistema (Greenfield)

#### Paso 1: Review Inputs (Revisar Entradas)
El diseño arquitectónico de esta primera iteración parte del siguiente estado inicial de requerimientos, atributos de calidad, restricciones y preocupaciones.

| Categoría | Identificador | Descripción | Estado Actual |
| :--- | :--- | :--- | :--- |
| **Casos de Uso Principales** | US09<br>US13<br>US17 | Publicar un paquete alimentario.<br>Visualizar paquetes alimentarios en el mapa.<br>Reservar un paquete disponible. | No abordado |
| **Atributos de Calidad** | AC003<br>AC004<br>AC006 | Disponibilidad (Mantener plataforma operativa).<br>Escalabilidad (Soportar picos de alto tráfico).<br>Seguridad (Acciones críticas requieren JWT). | No abordado |
| **Restricciones** | CON001<br>CON002<br>CON006 | Enfoque basado en microservicios.<br>Desarrollo en Java con Spring Boot.<br>Uso de JWT para autenticación. | No abordado |
| **Preocupaciones (Concerns)** | ARC-1<br>ARC-3<br>ARC-5 | Escalabilidad ante Picos de Tráfico.<br>Alta Disponibilidad y Tolerancia a Fallos.<br>Seguridad, Autenticación y Trazabilidad. | No abordado |

**[Insertar Captura de Pantalla de Trello - Estado Inicial]**

#### Paso 2: Establish Iteration Goal by Selecting Drivers (Establecer Objetivo)

**Objetivo de la Iteración:** Diseñar la arquitectura base del sistema (greenfield) adoptando un enfoque de microservicios con Spring Boot, definiendo el núcleo para la publicación y reserva de donaciones, y asegurando la autenticación centralizada mediante JWT para soportar los requisitos iniciales de disponibilidad y escalabilidad.

**Drivers Seleccionados:**
* US09, US13, US17 (Publicación y Reserva)
* AC003, AC004, AC006 (Disponibilidad, Escalabilidad, Seguridad)
* CON001, CON002, CON006 (Microservicios, Spring Boot, JWT)
* ARC-1, ARC-3, ARC-5 (Escalabilidad, Disponibilidad, Seguridad)

#### Paso 3: Choose Elements to Refine (Elegir Elementos a Refinar)
* Al tratarse de la primera iteración para la construcción de un sistema nuevo, el elemento a refinar es **Todo el Sistema (Greenfield)**. El enfoque estará en establecer la topología base de los microservicios, el enrutamiento de peticiones y la persistencia central.

#### Paso 4: Choose Design Concepts (Elegir Conceptos de Diseño)

| Concepto de Diseño | Decisión Tomada | Justificación (Rationale) | Alternativas Descartadas |
| :--- | :--- | :--- | :--- |
| **Estilo Arquitectónico** | Arquitectura de Microservicios | Permite el escalamiento independiente (soporta AC004 y ARC-1) y cumple la restricción CON001. | Arquitectura Monolítica (descartada por no cumplir CON001 ni soportar escalabilidad modular). |
| **Tecnología / Framework** | Spring Boot (Java) | Cumple con la restricción de tecnología del proyecto (CON002). Ecosistema robusto para microservicios. | Node.js / Express (descartado por la restricción CON002). |
| **Patrón de Seguridad** | API Gateway con validación JWT | Centraliza el acceso, enruta las peticiones y verifica la autenticación, cumpliendo CON006, AC006 y ARC-5. | Verificación de token individual en cada microservicio (descartado por generar duplicidad de lógica). |
| **Patrón de Datos** | Base de Datos Relacional Compartida (Shared DB para el Core) | Garantiza la consistencia transaccional inmediata en el flujo de reservas y usuarios (mitiga parcialmente ARC-2). | Database per Service puro en esta etapa inicial (descartado por complejidad innecesaria para el núcleo fuertemente acoplado). |

#### Paso 5: Instantiate Elements and Allocate Responsibilities (Instanciar y Asignar Responsabilidades)

| Elemento (Componente/Módulo) | Responsabilidades | Interfaces (Conexiones) |
| :--- | :--- | :--- |
| **API Gateway** | Recibir peticiones de clientes, validar el token JWT y enrutar el tráfico al microservicio correspondiente. | Expone REST API (HTTPS). Conecta con Auth y Donations Service. |
| **Auth Service** | Gestionar identidades, roles, inicio de sesión y emisión de tokens JWT. | Recibe peticiones del Gateway. Conecta con la Core DB. |
| **Donations Service** | Ejecutar lógica de negocio para publicación (US09), visualización (US13) y reservas (US17). | Recibe peticiones del Gateway. Conecta con la Core DB. |
| **Core DB (PostgreSQL/MySQL)** | Almacenar datos críticos relacionales (`users`, `donation_lots`, `reservations`). | Conexión JDBC desde Auth Service y Donations Service. |

#### Paso 6: Sketch Views (Bocetar Vistas C4/UML)

**[Insertar Imagen del Diagrama C4 Nivel 2: Contenedores]**
> *Figura 1: Diagrama de Contenedores que refleja el estilo de microservicios con Spring Boot, la presencia del API Gateway para enrutamiento seguro y la capa de base de datos compartida para el Core transaccional.*

#### Paso 7: Analysis of Current Design and Review Iteration Goal (Análisis y Revisión)

| Driver Evaluado | Estado | Comentarios / Trabajo Pendiente |
| :--- | :--- | :--- |
| CON001, CON002 (Microservicios y Java) | **Completamente abordado** | Seleccionados como la tecnología y estilo base del sistema. |
| AC006, CON006, ARC-5 (Seguridad JWT) | **Completamente abordado** | El API Gateway centraliza la seguridad delegando la emisión al Auth Service. |
| US09, US13, US17 (Flujo Core) | **Parcialmente abordado** | Se definió el *Donations Service*, pero falta detallar la comunicación asíncrona de reservas (RabbitMQ) y geolocalización (Iteración 2). |
| AC003, AC004 (Disponibilidad y Escalabilidad) | **Parcialmente abordado** | La estructura distribuida base lo permite, pero faltan tácticas de mensajería asíncrona y despliegue en la nube (Iteraciones posteriores). |

**[Insertar Captura de Pantalla de Trello - Estado Final de la Iteración]**
> *Figura 2: Tablero Kanban actualizado mostrando los drivers de restricción en la columna 'Done' y los Casos de Uso principales movidos a 'In Progress'.*
