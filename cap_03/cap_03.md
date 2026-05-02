# Capítulo III: Requirements Specification

## 3.1. To-Be Scenario Mapping

##### To-Be Scenario Mapping - Gestión de Excedentes en Restaurante

![Image](https://github.com/user-attachments/assets/34c8cd73-a722-4c1c-9def-9164a52b40ac)

##### To-Be Scenario Mapping - Búsqueda de Donaciones en Albergue

![Image](https://github.com/user-attachments/assets/368fba62-15d4-43b6-8d41-13444936da02)

## 3.2. Epics

Las siguientes epicas sintetizan las capacidades principales que **Alimenta** debe ofrecer para conectar restaurantes con excedentes alimentarios y organizaciones sociales con necesidad de abastecimiento inmediato. Estas epicas se definieron a partir de los hallazgos del capitulo anterior, los escenarios To-Be y las necesidades criticas de ambos segmentos.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Epic ID</th>
      <th>Titulo</th>
      <th>Descripcion</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>EP01</td>
      <td>Registro y acceso de usuarios</td>
      <td>Restaurantes y ONGs pueden crear su cuenta e iniciar sesion segun su rol para operar en la plataforma.</td>
    </tr>
    <tr>
      <td>EP02</td>
      <td>Gestion de perfil institucional</td>
      <td>Cada actor configura su informacion basica: nombre, ubicacion y datos de contacto, lo minimo necesario para operar.</td>
    </tr>
    <tr>
      <td>EP03</td>
      <td>Publicacion de paquetes alimentarios</td>
      <td>El restaurante registra excedentes indicando tipo, cantidad y horario limite de recojo.</td>
    </tr>
    <tr>
      <td>EP04</td>
      <td>Busqueda y alerta de paquetes cercanos</td>
      <td>La ONG visualiza paquetes disponibles en el mapa y recibe una notificacion automatica cuando hay uno cerca.</td>
    </tr>
    <tr>
      <td>EP05</td>
      <td>Reserva y aceptacion de donacion</td>
      <td>La ONG reserva un paquete disponible, bloqueandolo para evitar asignacion doble.</td>
    </tr>
    <tr>
      <td>EP06</td>
      <td>Coordinacion del recojo</td>
      <td>Ambas partes acuerdan punto y hora de encuentro, con actualizacion de estado en tiempo real.</td>
    </tr>
    <tr>
      <td>EP07</td>
      <td>Validacion y trazabilidad de entrega</td>
      <td>La entrega se confirma mediante QR o registro digital, generando un registro inmutable del intercambio.</td>
    </tr>
    <tr>
      <td>EP08</td>
      <td>Registro de evidencias e impacto</td>
      <td>La ONG sube fotos del recojo y ambos actores pueden consultar su historial de donaciones realizadas.</td>
    </tr>
    <tr>
      <td>EP09</td>
      <td>Gestión automática de expiración</td>
      <td>El sistema cancela automaticamente paquetes no recogidos al alcanzar su horario limite.</td>
    </tr>
    <tr>
      <td>EP10</td>
      <td>Reasignación automática de paquetes</td>
      <td>Si una ONG no recoge a tiempo, el sistema libera el paquete y se notifica a otras ONGs disponibles.</td>
    </tr>
  </tbody>
</table>

## 3.3. User Stories

_(Esta seccion se completara a partir del desglose de las epicas definidas.)_

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>StoryID</th>
      <th>Título</th>
      <th>Descripción</th>
      <th>Criterios de aceptación</th>
      <th>Epic ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>US01</td>
      <td>Registrarse como restaurante</td>
      <td>Como representante de un restaurante<br>Quiero crear una cuenta en la plataforma<br>Para publicar excedentes de alimentos y gestionar donaciones.</td>
      <td><strong>E01 - Registro exitoso:</strong> Dado que el usuario se encuentra en el formulario de registro, cuando completa los campos obligatorios y selecciona el rol de restaurante, entonces el sistema crea su cuenta correctamente.<br><br><strong>E02 - Campos incompletos:</strong> Dado que el usuario intenta registrarse, cuando omite uno o mas campos obligatorios, entonces el sistema muestra mensajes de validacion y no permite continuar.</td>
      <td>EP01</td>
    </tr>
    <tr>
      <td>US02</td>
      <td>Registrarse como ONG</td>
      <td>Como representante de una ONG o albergue<br>Quiero crear una cuenta en la plataforma<br>Para acceder a paquetes disponibles y coordinar su recojo.</td>
      <td><strong>E01 - Registro exitoso:</strong> Dado que el usuario se encuentra en el formulario de registro, cuando completa los campos requeridos y selecciona el rol de ONG, entonces el sistema registra su cuenta satisfactoriamente.<br><br><strong>E02 - Datos invalidos:</strong> Dado que el usuario ingresa informacion incorrecta o incompleta, cuando intenta enviar el formulario, entonces el sistema informa el error y evita el registro.</td>
      <td>EP01</td>
    </tr>
    <tr>
      <td>US03</td>
      <td>Iniciar sesion en la plataforma</td>
      <td>Como usuario registrado<br>Quiero iniciar sesion con mis credenciales<br>Para acceder a las funcionalidades correspondientes a mi cuenta.</td>
      <td><strong>E01 - Acceso valido:</strong> Dado que el usuario ya tiene una cuenta registrada, cuando ingresa correo y contrasena correctos, entonces el sistema le permite acceder a la plataforma.<br><br><strong>E02 - Credenciales incorrectas:</strong> Dado que el usuario intenta iniciar sesion, cuando ingresa credenciales invalidas, entonces el sistema muestra un mensaje de error y deniega el acceso.</td>
      <td>EP01</td>
    </tr>
    <tr>
      <td>US04</td>
      <td>Acceder segun rol</td>
      <td>Como usuario autenticado<br>Quiero que la plataforma me dirija segun mi rol<br>Para utilizar solo las opciones correspondientes a restaurante o ONG.</td>
      <td><strong>E01 - Redireccion por rol:</strong> Dado que el usuario inicia sesion correctamente, cuando el sistema identifica su rol, entonces lo redirige a la vista correspondiente.<br><br><strong>E02 - Restriccion de funcionalidades:</strong> Dado que el usuario accede a la plataforma, cuando intenta ingresar a una opcion de otro rol, entonces el sistema restringe el acceso.</td>
      <td>EP01</td>
    </tr>
    <tr>
      <td>US05</td>
      <td>Completar perfil de restaurante</td>
      <td>Como representante de un restaurante<br>Quiero registrar el nombre del local, ubicacion y datos de contacto<br>Para operar en la plataforma con informacion basica visible.</td>
      <td><strong>E01 - Perfil guardado correctamente:</strong> Dado que el usuario restaurante se encuentra en la seccion de perfil, cuando completa los campos obligatorios y guarda los cambios, entonces el sistema registra la informacion correctamente.<br><br><strong>E02 - Datos obligatorios faltantes:</strong> Dado que el usuario intenta guardar su perfil, cuando deja vacio un campo obligatorio, entonces el sistema muestra una validacion y no guarda la informacion.</td>
      <td>EP02</td>
    </tr>
    <tr>
      <td>US06</td>
      <td>Completar perfil de ONG</td>
      <td>Como representante de una ONG o albergue<br>Quiero registrar el nombre de la organizacion, ubicacion y datos de contacto<br>Para estar habilitado para recibir y coordinar donaciones.</td>
      <td><strong>E01 - Perfil guardado correctamente:</strong> Dado que el usuario ONG se encuentra en la seccion de perfil, cuando completa la informacion minima requerida y guarda, entonces el sistema actualiza sus datos exitosamente.<br><br><strong>E02 - Informacion incompleta:</strong> Dado que el usuario intenta guardar el perfil, cuando faltan datos obligatorios, entonces el sistema informa el error y solicita completarlos.</td>
      <td>EP02</td>
    </tr>
    <tr>
      <td>US07</td>
      <td>Editar informacion institucional</td>
      <td>Como usuario registrado<br>Quiero actualizar la informacion basica de mi perfil institucional<br>Para mantener mis datos correctos y facilitar la coordinacion operativa.</td>
      <td><strong>E01 - Edicion exitosa:</strong> Dado que el usuario ya cuenta con un perfil registrado, cuando modifica sus datos y guarda los cambios, entonces el sistema actualiza la informacion correctamente.<br><br><strong>E02 - Persistencia de cambios:</strong> Dado que el usuario actualiza su perfil, cuando vuelve a ingresar a la seccion correspondiente, entonces visualiza la informacion modificada.</td>
      <td>EP02</td>
    </tr>
    <tr>
      <td>US08</td>
      <td>Visualizar perfil institucional en una coordinacion</td>
      <td>Como usuario con una donacion reservada o en coordinacion<br>Quiero visualizar la informacion basica del otro actor involucrado<br>Para contar con datos minimos de contacto y ubicacion al momento de coordinar una donacion.</td>
      <td><strong>E01 - Visualizacion de datos basicos:</strong> Dado que existe una donacion con reserva o coordinacion activa, cuando un usuario autorizado consulta el perfil del otro actor desde el detalle de la donacion, entonces el sistema muestra nombre, ubicacion y datos de contacto registrados.<br><br><strong>E02 - Informacion no disponible:</strong> Dado que el perfil del otro actor no ha sido completado totalmente, cuando el usuario lo consulta desde el detalle de la donacion, entonces el sistema solo muestra la informacion disponible o indica que faltan datos.</td>
      <td>EP02</td>
    </tr>
    <tr>
      <td>US09</td>
      <td>Registrar paquete alimentario</td>
      <td>Como representante de un restaurante<br>Quiero publicar un paquete alimentario con su tipo, cantidad y horario limite de recojo<br>Para ponerlo a disposicion de ONGs cercanas antes de que se pierda.</td>
      <td><strong>E01 - Publicacion exitosa:</strong> Dado que el restaurante se encuentra en el formulario de publicacion, cuando completa los datos requeridos y guarda el paquete, entonces el sistema registra la donacion como disponible.<br><br><strong>E02 - Campos obligatorios faltantes:</strong> Dado que el usuario intenta publicar un paquete, cuando no completa tipo, cantidad u horario limite, entonces el sistema no permite registrar la publicacion y muestra mensajes de validacion.</td>
      <td>EP03</td>
    </tr>
    <tr>
      <td>US10</td>
      <td>Definir horario limite de recojo</td>
      <td>Como representante de un restaurante<br>Quiero indicar el horario limite para recoger el paquete<br>Para asegurar que la donacion sea retirada dentro de la ventana de frescura.</td>
      <td><strong>E01 - Horario valido:</strong> Dado que el usuario registra un paquete, cuando selecciona un horario limite posterior a la hora actual, entonces el sistema lo guarda correctamente.<br><br><strong>E02 - Horario invalido:</strong> Dado que el usuario intenta definir un horario limite, cuando ingresa una hora invalida o vencida, entonces el sistema rechaza el dato y solicita corregirlo.</td>
      <td>EP03</td>
    </tr>
    <tr>
      <td>US11</td>
      <td>Editar publicacion de paquete</td>
      <td>Como representante de un restaurante<br>Quiero modificar la informacion de un paquete publicado<br>Para corregir datos antes de que sea reservado por una ONG.</td>
      <td><strong>E01 - Edicion permitida:</strong> Dado que el paquete aun no ha sido reservado, cuando el usuario modifica sus datos y guarda los cambios, entonces el sistema actualiza la publicacion correctamente.<br><br><strong>E02 - Edicion restringida:</strong> Dado que el paquete ya fue reservado o se encuentra en proceso de recojo, cuando el usuario intenta editarlo, entonces el sistema restringe la modificacion.</td>
      <td>EP03</td>
    </tr>
    <tr>
      <td>US12</td>
      <td>Cancelar publicacion de paquete</td>
      <td>Como representante de un restaurante<br>Quiero cancelar una publicacion de paquete disponible<br>Para evitar reservas cuando el alimento ya no pueda ser entregado.</td>
      <td><strong>E01 - Cancelacion exitosa:</strong> Dado que el paquete esta disponible y no ha sido reservado, cuando el usuario decide cancelarlo, entonces el sistema cambia su estado y deja de mostrarlo como disponible.<br><br><strong>E02 - Cancelacion no permitida:</strong> Dado que el paquete ya fue tomado por una ONG o esta en proceso de entrega, cuando el usuario intenta cancelarlo, entonces el sistema no permite la accion.</td>
      <td>EP03</td>
    </tr>
    <tr>
      <td>US13</td>
      <td>Visualizar paquetes cercanos en el mapa</td>
      <td>Como representante de una ONG<br>Quiero visualizar en un mapa los paquetes alimentarios disponibles cerca de mi ubicacion<br>Para identificar rapidamente oportunidades de recojo.</td>
      <td><strong>E01 - Mapa con resultados:</strong> Dado que existen paquetes disponibles cercanos, cuando la ONG accede al mapa, entonces el sistema muestra los paquetes con su ubicacion correspondiente.<br><br><strong>E02 - Sin paquetes disponibles:</strong> Dado que no existen paquetes cercanos disponibles, cuando la ONG consulta el mapa, entonces el sistema muestra un mensaje informativo indicando que no hay resultados.</td>
      <td>EP04</td>
    </tr>
    <tr>
      <td>US14</td>
      <td>Ver detalle de paquete disponible</td>
      <td>Como representante de una ONG<br>Quiero consultar el detalle de un paquete publicado<br>Para evaluar si mi organizacion puede recogerlo a tiempo.</td>
      <td><strong>E01 - Detalle visible:</strong> Dado que la ONG selecciona un paquete en el mapa o listado, cuando abre su informacion, entonces el sistema muestra tipo de alimento, cantidad, ubicacion y horario limite de recojo.<br><br><strong>E02 - Paquete no disponible:</strong> Dado que el paquete ya no esta disponible, cuando la ONG intenta ver su detalle, entonces el sistema informa que ya no puede ser tomado.</td>
      <td>EP04</td>
    </tr>
    <tr>
      <td>US15</td>
      <td>Recibir alerta de paquete cercano</td>
      <td>Como representante de una ONG<br>Quiero recibir una notificacion automatica cuando se publique un paquete cercano<br>Para reaccionar rapidamente y aumentar las posibilidades de recojo.</td>
      <td><strong>E01 - Notificacion enviada:</strong> Dado que se publica un paquete dentro del rango definido, cuando el sistema detecta cercania con una ONG, entonces envia una alerta automatica al usuario correspondiente.<br><br><strong>E02 - Notificacion fuera de rango:</strong> Dado que se publica un paquete fuera del rango de proximidad configurado, cuando el sistema procesa la publicacion, entonces no envia una alerta a esa ONG.</td>
      <td>EP04</td>
    </tr>
    <tr>
      <td>US16</td>
      <td>Actualizar disponibilidad de paquetes en tiempo real</td>
      <td>Como representante de una ONG<br>Quiero ver el estado actualizado de los paquetes disponibles<br>Para evitar intentar reservar donaciones que ya fueron tomadas o vencieron.</td>
      <td><strong>E01 - Estado actualizado:</strong> Dado que un paquete cambia de estado, cuando la ONG consulta el mapa o listado, entonces el sistema refleja si sigue disponible, reservado o vencido.<br><br><strong>E02 - Paquete vencido oculto:</strong> Dado que un paquete supero su horario limite, cuando el sistema actualiza la disponibilidad, entonces deja de mostrarlo como opcion activa para reserva.</td>
      <td>EP04</td>
    </tr>
    <tr>
      <td>US17</td>
      <td>Reservar paquete disponible</td>
      <td>Como representante de una ONG<br>Quiero reservar un paquete alimentario disponible<br>Para asegurar su asignacion a mi organizacion antes de que otra ONG lo tome.</td>
      <td><strong>E01 - Reserva exitosa:</strong> Dado que el paquete se encuentra disponible, cuando la ONG confirma la reserva, entonces el sistema lo asigna a su organizacion y actualiza su estado.<br><br><strong>E02 - Paquete ya no disponible:</strong> Dado que otra ONG reservo el paquete primero o este dejo de estar activo, cuando el usuario intenta reservarlo, entonces el sistema informa que ya no esta disponible.</td>
      <td>EP05</td>
    </tr>
    <tr>
      <td>US18</td>
      <td>Bloquear paquete tras reserva</td>
      <td>Como representante de una ONG<br>Quiero que al confirmar mi reserva el paquete quede bloqueado para otras organizaciones<br>Para evitar asignaciones dobles y asegurar el recojo para mi entidad.</td>
      <td><strong>E01 - Bloqueo inmediato:</strong> Dado que una ONG completa la reserva de un paquete, cuando confirma la accion, entonces el sistema cambia el estado del paquete a reservado y evita nuevas reservas.<br><br><strong>E02 - Intentos posteriores rechazados:</strong> Dado que el paquete ya esta reservado, cuando otra ONG intenta tomarlo, entonces el sistema impide la accion y muestra el estado actualizado.</td>
      <td>EP05</td>
    </tr>
    <tr>
      <td>US19</td>
      <td>Confirmar reserva realizada</td>
      <td>Como representante de una ONG<br>Quiero recibir una confirmacion de que la reserva fue exitosa<br>Para tener claridad sobre el paquete asignado y continuar con la coordinacion del recojo.</td>
      <td><strong>E01 - Confirmacion visible:</strong> Dado que la reserva fue procesada correctamente, cuando finaliza la accion, entonces el sistema muestra una confirmacion con los datos principales del paquete reservado.<br><br><strong>E02 - Error en la reserva:</strong> Dado que ocurre un problema al intentar reservar, cuando la operacion falla, entonces el sistema informa que la reserva no pudo completarse.</td>
      <td>EP05</td>
    </tr>
    <tr>
      <td>US20</td>
      <td>Visualizar paquetes reservados</td>
      <td>Como representante de una ONG<br>Quiero consultar los paquetes que mi organizacion ha reservado<br>Para dar seguimiento a las donaciones pendientes de recojo.</td>
      <td><strong>E01 - Lista de reservas:</strong> Dado que la ONG tiene una o mas reservas activas, cuando accede a su seccion de paquetes reservados, entonces el sistema muestra el listado correspondiente.<br><br><strong>E02 - Sin reservas activas:</strong> Dado que la ONG no tiene reservas registradas, cuando consulta la seccion, entonces el sistema informa que no existen paquetes reservados.</td>
      <td>EP05</td>
    </tr>
    <tr>
      <td>US21</td>
      <td>Definir punto de recojo</td>
      <td>Como usuario involucrado en una donacion<br>Quiero visualizar o acordar el punto de recojo del paquete<br>Para que ambas partes sepan donde se realizara la entrega.</td>
      <td><strong>E01 - Punto de recojo registrado:</strong> Dado que existe una reserva activa, cuando se define el punto de encuentro, entonces el sistema lo muestra en la donacion asociada.<br><br><strong>E02 - Punto no definido:</strong> Dado que aun no se ha establecido el lugar de recojo, cuando un usuario revisa la coordinacion, entonces el sistema indica que el dato sigue pendiente.</td>
      <td>EP06</td>
    </tr>
    <tr>
      <td>US22</td>
      <td>Establecer hora de recojo</td>
      <td>Como usuario involucrado en una donacion<br>Quiero acordar una hora de recojo para el paquete reservado<br>Para organizar el traslado dentro del tiempo limite disponible.</td>
      <td><strong>E01 - Hora registrada:</strong> Dado que la donacion ya fue reservada, cuando se define una hora valida de recojo, entonces el sistema la guarda y la muestra a ambas partes.<br><br><strong>E02 - Hora invalida:</strong> Dado que el usuario intenta registrar una hora posterior al limite de recojo o inconsistente, cuando guarda la coordinacion, entonces el sistema rechaza la accion.</td>
      <td>EP06</td>
    </tr>
    <tr>
      <td>US23</td>
      <td>Actualizar estado del recojo</td>
      <td>Como usuario de la plataforma<br>Quiero visualizar el estado actual del proceso de recojo<br>Para saber si la donacion esta reservada, en camino o entregada.</td>
      <td><strong>E01 - Cambio de estado visible:</strong> Dado que la donacion avanza en su proceso, cuando se actualiza el estado, entonces el sistema refleja el nuevo valor para ambas partes.<br><br><strong>E02 - Estados consistentes:</strong> Dado que un usuario consulta una donacion activa, cuando revisa su seguimiento, entonces el sistema muestra un estado valido y coherente con el flujo de recojo.</td>
      <td>EP06</td>
    </tr>
    <tr>
      <td>US24</td>
      <td>Consultar detalle de coordinacion</td>
      <td>Como participante de una donacion<br>Quiero revisar los datos de coordinacion del recojo<br>Para confirmar lugar, hora y estado antes del traslado.</td>
      <td><strong>E01 - Detalle disponible:</strong> Dado que existe una reserva activa con coordinacion registrada, cuando el usuario abre su detalle, entonces el sistema muestra punto, hora y estado del recojo.<br><br><strong>E02 - Informacion incompleta:</strong> Dado que aun faltan datos por coordinar, cuando el usuario revisa el detalle, entonces el sistema indica claramente que informacion sigue pendiente.</td>
      <td>EP06</td>
    </tr>
    <tr>
      <td>US25</td>
      <td>Recibir notificacion de coordinacion confirmada</td>
      <td>Como participante de una donacion<br>Quiero recibir una notificacion cuando se confirme el punto y la hora de recojo<br>Para saber que la coordinacion ya fue establecida y prepararme para la entrega o recojo.</td>
      <td><strong>E01 - Notificacion enviada:</strong> Dado que el punto y la hora de recojo ya fueron definidos, cuando la coordinacion queda registrada, entonces el sistema notifica a ambas partes con los datos acordados.<br><br><strong>E02 - Notificacion actualizada:</strong> Dado que la coordinacion cambia despues de haber sido definida, cuando se guardan nuevos datos, entonces el sistema envia una nueva notificacion con la informacion actualizada.</td>
      <td>EP06</td>
    </tr>
    <tr>
      <td>US26</td>
      <td>Generar codigo o validacion de entrega</td>
      <td>Como representante de un restaurante o una ONG<br>Quiero contar con un codigo QR o mecanismo digital asociado a la donacion<br>Para confirmar la entrega de manera formal al momento del intercambio.</td>
      <td><strong>E01 - Validacion disponible:</strong> Dado que una donacion se encuentra lista para ser entregada, cuando el usuario accede al detalle de la entrega, entonces el sistema muestra un medio de validacion asociado a esa operacion.<br><br><strong>E02 - Validacion unica:</strong> Dado que existe una donacion activa, cuando el usuario consulta su mecanismo de validacion, entonces este aparece vinculado unicamente a esa entrega.</td>
      <td>EP07</td>
    </tr>
    <tr>
      <td>US27</td>
      <td>Confirmar entrega de donacion</td>
      <td>Como usuario participante en el intercambio<br>Quiero confirmar digitalmente que la donacion fue entregada<br>Para cerrar el proceso de manera segura y verificable.</td>
      <td><strong>E01 - Entrega confirmada:</strong> Dado que el recojo ya se realizo, cuando el usuario valida la entrega mediante QR o confirmacion digital, entonces el sistema cambia el estado de la donacion a entregada.<br><br><strong>E02 - Confirmacion invalida:</strong> Dado que la validacion no corresponde a la donacion o presenta un error, cuando el usuario intenta confirmar, entonces el sistema rechaza la operacion.</td>
      <td>EP07</td>
    </tr>
    <tr>
      <td>US28</td>
      <td>Registrar trazabilidad del intercambio</td>
      <td>Como usuario participante en una donacion<br>Quiero que la entrega confirmada quede registrada en mi historial<br>Para contar con un seguimiento trazable del proceso de intercambio.</td>
      <td><strong>E01 - Registro almacenado:</strong> Dado que una entrega fue confirmada, cuando finaliza la validacion, entonces el sistema guarda el registro con los datos principales del intercambio en el historial correspondiente.<br><br><strong>E02 - Informacion consultable:</strong> Dado que existe una entrega validada, cuando un usuario autorizado consulta el historial, entonces el sistema muestra la trazabilidad correspondiente.</td>
      <td>EP07</td>
    </tr>
    <tr>
      <td>US29</td>
      <td>Evitar duplicidad de validacion</td>
      <td>Como participante de una donacion<br>Quiero que una entrega ya validada no pueda confirmarse nuevamente<br>Para preservar la integridad del registro de entrega.</td>
      <td><strong>E01 - Validacion unica aplicada:</strong> Dado que una donacion ya fue confirmada, cuando un usuario intenta validarla nuevamente, entonces el sistema bloquea la accion.<br><br><strong>E02 - Estado consistente tras validacion:</strong> Dado que la entrega ya fue registrada como completada, cuando se revisa su estado, entonces el sistema la mantiene como entregada sin generar duplicados.</td>
      <td>EP07</td>
    </tr>
    <tr>
      <td>US30</td>
      <td>Subir evidencia fotografica del recojo</td>
      <td>Como representante de una ONG<br>Quiero subir fotos del recojo realizado<br>Para dejar evidencia visual de que la donacion fue recibida.</td>
      <td><strong>E01 - Carga exitosa:</strong> Dado que la ONG se encuentra en una donacion entregada o en proceso de cierre, cuando adjunta una o mas fotos validas, entonces el sistema las almacena correctamente como evidencia.<br><br><strong>E02 - Archivo no valido:</strong> Dado que el usuario intenta subir un archivo no permitido o vacio, cuando confirma la accion, entonces el sistema rechaza la carga e informa el error.</td>
      <td>EP08</td>
    </tr>
    <tr>
      <td>US31</td>
      <td>Consultar historial de donaciones realizadas</td>
      <td>Como usuario de la plataforma<br>Quiero revisar el historial de donaciones en las que participe<br>Para llevar control de mis publicaciones, recepciones y entregas completadas.</td>
      <td><strong>E01 - Historial disponible:</strong> Dado que el usuario tiene donaciones registradas, cuando accede a su historial, entonces el sistema muestra la lista de operaciones asociadas a su cuenta.<br><br><strong>E02 - Historial vacio:</strong> Dado que el usuario aun no participa en donaciones, cuando consulta su historial, entonces el sistema informa que no existen registros.</td>
      <td>EP08</td>
    </tr>
    <tr>
      <td>US32</td>
      <td>Ver detalle de evidencias por donacion</td>
      <td>Como usuario de la plataforma<br>Quiero consultar las evidencias asociadas a una donacion especifica<br>Para revisar el respaldo visual y operativo de cada intercambio realizado.</td>
      <td><strong>E01 - Evidencias visibles:</strong> Dado que una donacion cuenta con fotos o registros asociados, cuando el usuario abre su detalle, entonces el sistema muestra las evidencias disponibles.<br><br><strong>E02 - Sin evidencias registradas:</strong> Dado que la donacion no tiene archivos adjuntos, cuando el usuario revisa el detalle, entonces el sistema indica que no existen evidencias cargadas.</td>
      <td>EP08</td>
    </tr>
    <tr>
      <td>US33</td>
      <td>Filtrar historial de donaciones</td>
      <td>Como restaurante u ONG<br>Quiero filtrar mi historial de donaciones por tipo de accion o estado<br>Para identificar rapidamente las donaciones publicadas, recibidas o completadas.</td>
      <td><strong>E01 - Filtro por tipo o estado:</strong> Dado que el usuario se encuentra en su historial, cuando selecciona un filtro como publicadas, recibidas, reservadas o completadas, entonces el sistema muestra solo los registros que coinciden con ese criterio.<br><br><strong>E02 - Sin resultados para el filtro:</strong> Dado que el usuario aplica un filtro sin coincidencias, cuando el sistema procesa la consulta, entonces informa que no existen registros para esa seleccion.</td>
      <td>EP08</td>
    </tr>
    <tr>
      <td>US34</td>
      <td>Expiración automática de paquete alimentario</td>
      <td>Como sistema, quiero cancelar automáticamente un paquete alimentario cuando se alcance su horario límite de recojo,
      para evitar que paquetes vencidos sigan disponibles en la plataforma.</td>
      <td><strong>E01 - Expiración automática exitosa:</strong> Dado que existe un paquete publicado con un horario límite definido, cuando el sistema detecta que la hora actual supera dicho horario, entonces el sistema cambia el estado del paquete a “expirado” y deja de mostrarlo como disponible.<br><br><strong>E02 - Paquete ya reservado o completado:</strong> Dado que un paquete ya ha sido recogido o se encuentra en estado “completado”, cuando se alcanza el horario límite, entonces el sistema no modifica su estado ni ejecuta la expiración.</td>
      <td>EP09</td>
    </tr>
    <tr>
      <td>US35</td>
      <td>Reasignación (notificación a otras ONGs)</td>
      <td>Como sistema, quiero liberar un paquete reservado si no es recogido dentro del tiempo establecido, para permitir que otras ONGs puedan acceder a él antes de que expire.</td>
      <td><strong>E01 - Liberación por incumplimiento de recojo:</strong> Dado que una ONG ha reservado un paquete, cuando no se realiza la confirmación de recojo dentro del tiempo límite definido, entonces el sistema cambia el estado del paquete a “disponible” y elimina la reserva actual.<br><br><strong>E02 - Paquete recogido correctamente:</strong> Dado que la ONG confirma el recojo dentro del tiempo establecido, cuando se valida la entrega del paquete, entonces el sistema mantiene el estado como “completado” y no ejecuta la liberación.</td>
      <td>EP10</td>
    </tr>
    <tr>
      <td>US36</td>
      <td>Notificación automática tras liberación de paquete</td>
      <td>Como sistema, quiero notificar a otras ONGs cercanas cuando un paquete vuelve a estar disponible, para incrementar las probabilidades de que sea recogido a tiempo.</td>
      <td><strong>E01 - Notificación a ONGs disponibles:</strong> Dado que un paquete ha sido liberado nuevamente, cuando el sistema actualiza su estado a “disponible”, entonces se envía una notificación a las ONGs cercanas registradas en la plataforma.<br><br><strong>E02 - Sin ONGs disponibles:</strong> Dado que no existen ONGs cercanas o activas en el momento de la liberación, cuando el sistema intenta enviar la notificación, entonces no se genera error y el paquete permanece disponible en la plataforma.</td>
      <td>EP10</td>
    </tr>
  </tbody>
</table>

## Requerimientos No Funcionales

Los siguientes requerimientos no funcionales definen las restricciones de calidad del sistema, considerando la naturaleza en tiempo real del rescate de alimentos y la necesidad de garantizar disponibilidad, rendimiento y confiabilidad.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>RNF</th>
      <th>Titulo</th>
      <th>Descripcion</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>RNF01</td>
      <td>Tiempo de respuesta</td>
      <td>El sistema debe permitir registrar un paquete alimentario en un tiempo menor o igual a 2 segundos.</td>
    </tr>
    <tr>
      <td>RNF02</td>
      <td>Tiempo de notificación</td>
      <td>El sistema debe enviar notificaciones a las ONGs en un tiempo menor a 5 segundos tras la publicación o liberación de un paquete.</td>
    </tr>
    <tr>
      <td>RNF03</td>
      <td>Disponibilidad del sistema</td>
      <td>El sistema debe garantizar una disponibilidad mínima del 99.5% mensual.</td>
    </tr>
    <tr>
      <td>RNF04</td>
      <td>Escalabilidad</td>
      <td>El sistema debe soportar al menos 1,000 usuarios concurrentes sin degradación significativa del rendimiento.</td>
    </tr>
    <tr>
      <td>RNF05</td>
      <td>Autenticación</td>
      <td>El sistema debe requerir autenticación para el inicio de sesion(Login).</td>
    </tr>
    <tr>
      <td>RNF06</td>
      <td>Consistencia de reservas</td>
      <td>El sistema debe garantizar que un paquete alimentario no pueda ser reservado por más de una ONG al mismo tiempo.</td>
    </tr>
    <tr>
      <td>RNF07</td>
      <td>Manejo de fallos</td>
      <td>El sistema debe continuar operando ante fallos parciales en servicios como notificaciones, evitando la pérdida de información.</td>
    </tr>
  </tbody>
</table>

## 3.4. Impact Mapping

##### Impact Mapping - Gestión de Excedentes en Restaurante

<img width="1240" height="1962" alt="Image" src="https://github.com/user-attachments/assets/ab4709c6-3287-4274-9be9-06f1df8d1df3" />

##### Impact Mapping - Búsqueda de Donaciones en Albergue

<img alt="Image" src="https://github.com/user-attachments/assets/88e5e207-45db-4780-a043-7535bada593a" />

## 3.5. Technical Stories

Las siguientes technical stories complementan las historias funcionales del producto y describen capacidades tecnicas necesarias para soportar la arquitectura propuesta de **Alimenta**, especialmente en mensajeria asincrona, geolocalizacion y trazabilidad de evidencias.

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>TS ID</th>
      <th>Titulo</th>
      <th>Descripcion</th>
      <th>Criterios de aceptacion</th>
      <th>Epic de referencia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>TS01</td>
      <td>Configurar broker de mensajeria con RabbitMQ</td>
      <td>Como equipo de desarrollo<br>Queremos configurar RabbitMQ como broker central<br>Para que los microservicios puedan comunicarse de forma asincrona mediante eventos.</td>
      <td><strong>E01 - Broker operativo:</strong> Dado que el entorno esta desplegado, cuando un servicio publica un mensaje en un exchange, entonces RabbitMQ lo enruta correctamente a la cola correspondiente.<br><br><strong>E02 - Fallo tolerado:</strong> Dado que un consumidor no esta disponible temporalmente, cuando el broker recibe el mensaje, entonces lo retiene en la cola hasta que el consumidor se recupere.</td>
      <td>EP03, EP04</td>
    </tr>
    <tr>
      <td>TS02</td>
      <td>Publicar evento paquete.publicado desde Inventory Service</td>
      <td>Como equipo de desarrollo<br>Queremos que al registrar un paquete se emita un evento al broker<br>Para que el Notification Service y el Geo Service puedan reaccionar de forma desacoplada.</td>
      <td><strong>E01 - Evento emitido:</strong> Dado que un restaurante publica un paquete exitosamente, cuando el Inventory Service procesa la operacion, entonces emite el evento paquete.publicado con los datos del paquete al exchange correspondiente.<br><br><strong>E02 - Fallo no bloquea al usuario:</strong> Dado que el broker no esta disponible momentaneamente, cuando el Inventory Service intenta emitir el evento, entonces el registro del paquete se completa igualmente y el evento se reintenta de forma controlada.</td>
      <td>EP03, EP04</td>
    </tr>
    <tr>
      <td>TS03</td>
      <td>Consumir eventos en Notification Service y disparar push</td>
      <td>Como equipo de desarrollo<br>Queremos que el Notification Service consuma eventos del broker y envie push notifications via FCM<br>Para alertar a ONGs cercanas sin acoplar este proceso al flujo principal.</td>
      <td><strong>E01 - Notificacion enviada:</strong> Dado que el Notification Service recibe el evento paquete.publicado, cuando procesa el mensaje, entonces envia una push notification a las ONGs dentro del radio configurado.<br><br><strong>E02 - Evento consumido una sola vez:</strong> Dado que el evento llega al broker, cuando el consumidor lo procesa, entonces el mensaje es removido de la cola y no se reenvia.</td>
      <td>EP04, EP06</td>
    </tr>
    <tr>
      <td>TS04</td>
      <td>Configurar Geo Service con tiles .pmtiles desde S3</td>
      <td>Como equipo de desarrollo<br>Queremos que el Geo Service sirva mapas vectoriales desde archivos .pmtiles almacenados en S3<br>Para operar sin dependencia de servicios externos de mapas como Google Maps o Mapbox.</td>
      <td><strong>E01 - Tiles disponibles:</strong> Dado que el archivo .pmtiles esta almacenado en S3, cuando la app movil solicita un tile por coordenadas /{z}/{x}/{y}, entonces el Geo Service lo recupera y responde correctamente.<br><br><strong>E02 - Cache activo:</strong> Dado que un tile ya fue solicitado recientemente, cuando se repite la misma solicitud, entonces el servicio lo sirve desde cache sin consultar S3 nuevamente.</td>
      <td>EP04</td>
    </tr>
    <tr>
      <td>TS05</td>
      <td>Implementar calculo de cercania en Geo Service</td>
      <td>Como equipo de desarrollo<br>Queremos que el Geo Service calcule la distancia entre un paquete publicado y las ONGs registradas<br>Para que solo se notifique y muestre paquetes dentro de un radio relevante.</td>
      <td><strong>E01 - Filtro por radio:</strong> Dado que se publica un paquete con coordenadas, cuando el Geo Service evalua las ONGs activas, entonces retorna unicamente las que se encuentran dentro del radio configurado.<br><br><strong>E02 - Sin coordenadas validas:</strong> Dado que una ONG no tiene ubicacion registrada en su perfil, cuando el sistema ejecuta el calculo, entonces la excluye del resultado sin generar error.</td>
      <td>EP04, EP05</td>
    </tr>
    <tr>
      <td>TS06</td>
      <td>Configurar bucket S3 para almacenamiento de evidencias</td>
      <td>Como equipo de desarrollo<br>Queremos configurar un bucket S3 con politicas de acceso controlado<br>Para que el Tracking Service pueda subir y recuperar fotos de evidencia de forma segura.</td>
      <td><strong>E01 - Carga exitosa:</strong> Dado que la ONG adjunta una foto valida, cuando el Tracking Service recibe el archivo, entonces lo sube al bucket S3 y retorna la URL de acceso asociada a esa donacion.<br><br><strong>E02 - Acceso restringido:</strong> Dado que un usuario intenta acceder a una evidencia de otra organizacion, cuando realiza la solicitud, entonces el sistema deniega el acceso por politica de bucket.</td>
      <td>EP08</td>
    </tr>
    <tr>
      <td>TS07</td>
      <td>Publicar evento entrega.validada y persistir trazabilidad</td>
      <td>Como equipo de desarrollo<br>Queremos que al confirmar una entrega se emita un evento al broker y se persista el registro en la base de datos del Tracking Service<br>Para garantizar trazabilidad completa del intercambio.</td>
      <td><strong>E01 - Evento emitido y registro guardado:</strong> Dado que una entrega es confirmada mediante QR o validacion digital, cuando el Tracking Service procesa la accion, entonces persiste el registro y emite el evento entrega.validada al broker.<br><br><strong>E02 - Idempotencia garantizada:</strong> Dado que el mismo evento llega duplicado al broker por error de red, cuando el consumidor lo procesa, entonces detecta el duplicado y no genera un segundo registro.</td>
      <td>EP07, EP08</td>
    </tr>
  </tbody>
</table>

## 3.6. Product Backlog

<table border="1">
    <tr>
        <th>#Orden</th>
        <th>User Story Id</th>
        <th>Título</th>
        <th>Description</th>
        <th>Story Points (1/2/3/5/8)</th>
    </tr>
    <tr><td>1</td><td>US01</td><td>Registrarse como restaurante</td><td>Permite que un restaurante cree su cuenta para publicar excedentes y operar en la plataforma.</td><td>3</td></tr>
    <tr><td>2</td><td>US02</td><td>Registrarse como ONG</td><td>Permite que una ONG o albergue cree su cuenta para buscar y recibir donaciones.</td><td>3</td></tr>
    <tr><td>3</td><td>US03</td><td>Iniciar sesion en la plataforma</td><td>Permite que un usuario registrado acceda con sus credenciales.</td><td>2</td></tr>
    <tr><td>4</td><td>US04</td><td>Acceder segun rol</td><td>Redirige al usuario autenticado segun su rol y restringe funcionalidades no autorizadas.</td><td>3</td></tr>
    <tr><td>5</td><td>US05</td><td>Completar perfil de restaurante</td><td>Permite registrar nombre del local, ubicacion y contacto del restaurante.</td><td>2</td></tr>
    <tr><td>6</td><td>US06</td><td>Completar perfil de ONG</td><td>Permite registrar nombre, ubicacion y contacto de la organizacion social.</td><td>2</td></tr>
    <tr><td>7</td><td>US07</td><td>Editar informacion institucional</td><td>Permite actualizar los datos basicos del perfil institucional.</td><td>2</td></tr>
    <tr><td>8</td><td>US09</td><td>Registrar paquete alimentario</td><td>Permite que el restaurante publique un paquete con tipo, cantidad y horario limite.</td><td>5</td></tr>
    <tr><td>9</td><td>US10</td><td>Definir horario limite de recojo</td><td>Permite indicar hasta que hora puede ser recogida la donacion.</td><td>2</td></tr>
    <tr><td>10</td><td>US13</td><td>Visualizar paquetes cercanos en el mapa</td><td>Permite a la ONG ver en el mapa los paquetes disponibles cerca de su ubicacion.</td><td>5</td></tr>
    <tr><td>11</td><td>US14</td><td>Ver detalle de paquete disponible</td><td>Permite revisar tipo de alimento, cantidad, ubicacion y horario limite antes de reservar.</td><td>2</td></tr>
    <tr><td>12</td><td>US15</td><td>Recibir alerta de paquete cercano</td><td>Permite que la ONG reciba una notificacion cuando se publique una donacion cercana.</td><td>5</td></tr>
    <tr><td>13</td><td>US16</td><td>Actualizar disponibilidad de paquetes en tiempo real</td><td>Permite mostrar si un paquete sigue disponible, reservado o vencido.</td><td>5</td></tr>
    <tr><td>14</td><td>US17</td><td>Reservar paquete disponible</td><td>Permite a la ONG asegurar un paquete antes de que otra organizacion lo tome.</td><td>5</td></tr>
    <tr><td>15</td><td>US18</td><td>Bloquear paquete tras reserva</td><td>Bloquea el paquete para otras organizaciones luego de confirmar la reserva.</td><td>3</td></tr>
    <tr><td>16</td><td>US19</td><td>Confirmar reserva realizada</td><td>Muestra confirmacion de que el paquete fue asignado correctamente a la ONG.</td><td>2</td></tr>
    <tr><td>17</td><td>US20</td><td>Visualizar paquetes reservados</td><td>Permite consultar las reservas activas pendientes de recojo.</td><td>2</td></tr>
    <tr><td>18</td><td>US21</td><td>Definir punto de recojo</td><td>Permite acordar o visualizar el punto donde se realizara la entrega.</td><td>3</td></tr>
    <tr><td>19</td><td>US22</td><td>Establecer hora de recojo</td><td>Permite acordar la hora de recojo dentro del tiempo disponible.</td><td>3</td></tr>
    <tr><td>20</td><td>US24</td><td>Consultar detalle de coordinacion</td><td>Permite revisar lugar, hora y estado del recojo antes del traslado.</td><td>2</td></tr>
    <tr><td>21</td><td>US25</td><td>Recibir notificacion de coordinacion confirmada</td><td>Notifica a ambas partes cuando la coordinacion del recojo queda establecida o cambia.</td><td>3</td></tr>
    <tr><td>22</td><td>US08</td><td>Visualizar perfil institucional en una coordinacion</td><td>Permite consultar nombre, ubicacion y contacto del otro actor desde una donacion activa.</td><td>2</td></tr>
    <tr><td>23</td><td>US23</td><td>Actualizar estado del recojo</td><td>Permite visualizar el avance del flujo de recojo entre ambas partes.</td><td>3</td></tr>
    <tr><td>24</td><td>US26</td><td>Generar codigo o validacion de entrega</td><td>Permite disponer de un QR o mecanismo digital para validar la entrega.</td><td>5</td></tr>
    <tr><td>25</td><td>US27</td><td>Confirmar entrega de donacion</td><td>Permite cerrar la donacion validando digitalmente que fue entregada.</td><td>3</td></tr>
    <tr><td>26</td><td>US28</td><td>Registrar trazabilidad del intercambio</td><td>Permite que la entrega confirmada quede registrada en el historial.</td><td>3</td></tr>
    <tr><td>27</td><td>US29</td><td>Evitar duplicidad de validacion</td><td>Impide que una misma entrega sea confirmada mas de una vez.</td><td>2</td></tr>
    <tr><td>28</td><td>US30</td><td>Subir evidencia fotografica del recojo</td><td>Permite que la ONG adjunte fotos como evidencia visual de la recepcion.</td><td>3</td></tr>
    <tr><td>29</td><td>US31</td><td>Consultar historial de donaciones realizadas</td><td>Permite revisar el historial de operaciones asociadas a la cuenta.</td><td>3</td></tr>
    <tr><td>30</td><td>US32</td><td>Ver detalle de evidencias por donacion</td><td>Permite revisar los archivos y respaldos asociados a una donacion especifica.</td><td>2</td></tr>
    <tr><td>31</td><td>US33</td><td>Filtrar historial de donaciones</td><td>Permite filtrar el historial por tipo de accion o estado.</td><td>2</td></tr>
    <tr><td>32</td><td>US11</td><td>Editar publicacion de paquete</td><td>Permite corregir la informacion de una donacion antes de que sea reservada.</td><td>2</td></tr>
    <tr><td>33</td><td>US12</td><td>Cancelar publicacion de paquete</td><td>Permite retirar un paquete disponible cuando ya no puede ser entregado.</td><td>2</td></tr>
    <tr><td>34</td><td>TS01</td><td>Configurar broker de mensajeria con RabbitMQ</td><td>Habilita la comunicacion asincrona entre microservicios mediante colas y eventos.</td><td>5</td></tr>
    <tr><td>35</td><td>TS02</td><td>Publicar evento paquete.publicado desde Inventory Service</td><td>Emite el evento de publicacion del paquete para activar procesos desacoplados.</td><td>5</td></tr>
    <tr><td>36</td><td>TS03</td><td>Consumir eventos en Notification Service y disparar push</td><td>Procesa eventos publicados y envia notificaciones push a ONGs cercanas.</td><td>8</td></tr>
    <tr><td>37</td><td>TS04</td><td>Configurar Geo Service con tiles .pmtiles desde S3</td><td>Sirve mapas vectoriales propios desde S3 sin depender de proveedores externos.</td><td>8</td></tr>
    <tr><td>38</td><td>TS05</td><td>Implementar calculo de cercania en Geo Service</td><td>Calcula que ONGs se encuentran dentro del radio relevante de un paquete publicado.</td><td>5</td></tr>
    <tr><td>39</td><td>TS06</td><td>Configurar bucket S3 para almacenamiento de evidencias</td><td>Permite almacenar y recuperar fotos de evidencia con acceso controlado.</td><td>5</td></tr>
    <tr><td>40</td><td>TS07</td><td>Publicar evento entrega.validada y persistir trazabilidad</td><td>Registra la entrega confirmada y emite el evento correspondiente para trazabilidad completa.</td><td>5</td></tr>
    </table>
