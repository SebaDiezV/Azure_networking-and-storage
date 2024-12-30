# Proyecto 2 : Networking and Storage

## Temas Tratados
Virtual Networks, NSGs, Storage Accounts, Encryption, Protocols, ARM Templates

## Resumen
Este proyecto cubre la implementación de una cuenta de almacenamiento segura, la creación de redes virtuales y grupos de seguridad de red para el aislamiento, la habilitación del cifrado y la automatización de la implementación de recursos mediante plantillas ARM. Demuestra los aspectos básicos de las redes y el almacenamiento.

## Escenario
Una empresa desea implementar una cuenta de almacenamiento segura, garantizar el aislamiento de la red para sus recursos, aplicar el cifrado de datos y utilizar plantillas para automatizar la implementación de recursos.

## Diagrama
![proyecto 2](https://github.com/user-attachments/assets/59700e17-1ddb-4302-94d3-bd8cef0956f9)

## Tarea 1: Crear una Red Virtual (Vnet)

1. En el **Portal Azure** seleccionar **Redes Virtuales** > **Crear**
<img width="248" alt="01" src="https://github.com/user-attachments/assets/defb1d49-6e7d-4c0c-88ea-ddea0a824e50" />

2. Selecciona Subscripción, Grupo de Recursos, Nombre de red virtual y Región > Siguiente
<img width="446" alt="02" src="https://github.com/user-attachments/assets/c5667a37-9c78-4f07-a807-9448277e7c4a" />

3. En la pestaña **Direcciones IP**, eliminar la la subred Default, luego seleccionar **+ Agregar una subred** para crear una subred con el nombre **Database**

    |Configuración|Valor|
    |---|---|
    |Proposito de la subred| Default |
    |Nombre| **Database** |
    |Dirección inicial| **10.0.0.0** |
    |Tamaño| **/27 (32 direcciones)** |

<img width="939" alt="03" src="https://github.com/user-attachments/assets/43039caa-1d1c-4769-b955-50de65971a60" />

4. En el apartado **Subred privada**, seleccionar opción **Habilitar Subred privada**
<img width="463" alt="04" src="https://github.com/user-attachments/assets/62309328-d1a8-4b26-9cbf-5762ee65edf5" />

5. En el apartado **Puntos de conexión del servicio**, seleccionar el servicio **Microsoft.Storage** > Agregar
<img width="459" alt="05" src="https://github.com/user-attachments/assets/5e6bf920-c2ec-48d9-a551-9310739d94d8" />

6. Seleccionar **+ Agregar una subred** para crear una segunda subred con el nombre **Web**

    |Configuración|Valor|
    |---|---|
    |Proposito de la subred| Default |
    |Nombre| **Web** |
    |Dirección inicial| **10.0.1.0** |
    |Tamaño| **/27 (32 direcciones)** |
<img width="475" alt="06" src="https://github.com/user-attachments/assets/e605a7f5-ffd2-45d4-a95a-4e731a548d00" />

7. Agregar Subred, Revisar y crear > Crear

## Tarea 2: Implementar un grupo de seguridad de red (NSG)

1. En el **Portal Azure** seleccionar **Grupos de seguridad de red** > **Crear**
<img width="250" alt="07" src="https://github.com/user-attachments/assets/679e0bcb-aa82-412e-be49-3a1ac3d069ef" />

2. Selecciona Subscripción, Grupo de Recursos, Nombre y Región > Revisar y crear > Crear
<img width="443" alt="08" src="https://github.com/user-attachments/assets/71e7e580-9252-43e1-9a62-b4d744e79074" />

3. Ingresar al grupo de seguridad creado, en Configuración > Subredes, seleccionar **+ Agregar** para asociar el grupo de seguridad a una sured
<img width="303" alt="09" src="https://github.com/user-attachments/assets/55d9307c-49f4-41f8-90cd-ee6095227c5c" />

4. Seleccionar Vnet y la Subred **Web** > Aceptar
<img width="326" alt="10" src="https://github.com/user-attachments/assets/2fad5ca5-1fbf-4ec3-a184-51d72170c93f" />

5. En Configuración > Reglas de segurirdad de entrada, seleccionar **+ Agregar** para agregar una regla de seguridad de entrada
<img width="396" alt="11" src="https://github.com/user-attachments/assets/dbbf2748-700c-4f3a-9d49-cf6aa198b9e9" />

6. Agregar la siguiente información para crear una regla de entrada que permita el tráfico desde HTTPS desde internet > Crear
   
    |Configuración|Valor|
    |---|---|
    |Origen| **Any** |
    |Intervalos de puerto de origen| * |
    |Servicio| **HTTPS** |
    |Intervalos de puerto de destino| **443 (Autocompletado)** |
    |Protocolo| **TCP (Autocompletado)** |
    |Acción| **Permitir** |
    |Prioridad| Seleccionar número de prioridad según se necesite |
   
>**Nota**: Mientras mas bajo sea el número, más alta es la prioridad

<img width="331" alt="12" src="https://github.com/user-attachments/assets/4e836b74-337d-4e85-87db-ea707750faf4" />

 7. seleccionar **+ Agregar**, gregar la siguiente información para crear una regla de entrada que deniegue todo el resto del tráfico > Crear

    |Configuración|Valor|
    |---|---|
    |Origen| **Any** |
    |Intervalos de puerto de origen| * |
    |Servicio| **Custom** |
    |Intervalos de puerto de destino| * |
    |Protocolo| **Any** |
    |Acción| **Denegar** |
    |Prioridad| Seleccionar número de prioridad según se necesite |

>**Nota**: El número de prioridad no debe ser mas bajo que la regla anteriormente creada.
>
>**Nota**: Si se necesita permitir puertos específicos como SSH/RDP, simplemente se agregan reglas con una prioridad mayor (número mas bajo que la regla Denegar).

<img width="330" alt="12-2" src="https://github.com/user-attachments/assets/c97be70a-64f3-4079-b2ee-efa434e4355a" />

## Tarea 3: Crear y configurar una Cuenta de Almacenamiento

1. En el **Portal Azure** seleccionar **Cuentas de Almacenamiento** > **Crear**
<img width="247" alt="13" src="https://github.com/user-attachments/assets/364f72ed-210a-454e-a77c-99cd9fc4824b" />

2. Selecciona Subscripción, Grupo de Recursos, Nombre de la cuenta de almacenamiento, Región, Rendimiento y Redundancia  > Siguiente
<img width="454" alt="14" src="https://github.com/user-attachments/assets/7463b1eb-762b-46a4-b412-05e4a81d04c5" />

>**Nota**: El nombre de la cuenta de almacenamiento debe ser único a nivel global

3. En la pestaña **Avanzado**, en la opción **Ámbito permitido para las operaciones de copia (versión preliminar)** seleccionar **Desde cuentas de almacenamiento que tienen un punto de conexión privado** > Siguiente
<img width="539" alt="15" src="https://github.com/user-attachments/assets/7723bd8f-b0c1-4bc3-ac16-d63140868c55" />

4. En la pestaña **Redes**, en la sección Acceso de red, selecciona la opción **Deshabilitar el acceso público y usar el acceso privado**, con lo cual se habilita que la cuenta de almacenamiento solo puede ser accedida de forma privada con un punto de conexión privado
<img width="447" alt="16" src="https://github.com/user-attachments/assets/a387da5b-be8a-4da5-9384-56377a316585" />

5. En la pestaña **Redes**, en la sección Punto de conexión privado, selecciona la opción **+ Agregar punto de conexión privado**
<img width="429" alt="17" src="https://github.com/user-attachments/assets/83ee2585-3bdb-4eee-8ebb-4509860fc027" />

6. Crea el Punto de conexión privado en el mismo grupo de recursos y ubicación, agrega un Nombre y deja por defecto el Subrecurso de almacenamiento. En el apartado de redes selecciona la red cread con anterioridad y la Subred **Database**, con esta configuración, el uso de la cuenta de almacenamiento queda restringido solo a esa subred > Crear > Siguiente
<img width="479" alt="18" src="https://github.com/user-attachments/assets/29543427-a1a9-4495-b841-cb48c77f7b7c" />

7.  En la pestaña Cifrado, en la opción Tipo de cifrado, seleccionar la opción **Claves administradas por Microsoft (MMK)** > Revisar y Crear > Crear
<img width="321" alt="19" src="https://github.com/user-attachments/assets/ecd7fadb-27b0-4136-9497-e3a341bca5cf" />

## Tarea 4: Implementar la red virtual y la cuenta de almacenamiento mediante una plantilla ARM

>**Nota**: Los siguientes pasos son identicos para ambos servicios pero para este caso solo realizaré los pasos para la VNet

1. ingresa a la VNet creada con anterioridad, selecciona **autonation** > **Exportar planilla** y selecciona descargar, extrae los archivos del rar descargado
<img width="394" alt="21" src="https://github.com/user-attachments/assets/b10f159e-57e2-4ca9-b981-9c6ef53b44ee" />

2. En el **Portal de Azure**, ingresa a **Espesificaciones de plantilla**
<img width="247" alt="22" src="https://github.com/user-attachments/assets/e2d94eb0-df28-4e89-9787-dc730c373406" />

3. Selecciona la opción **Importar plantilla**, luego presiona el botón con el icono de carpeta, navega hacia la ubicación donde dejste el archivo template, seleccionalo y luego presiona el botón Importar
   
<img width="396" alt="23" src="https://github.com/user-attachments/assets/b8ced4ec-72f5-4cd1-80b7-23237c20c2e0" />     ------

<img width="479" alt="24" src="https://github.com/user-attachments/assets/629394e1-eb37-4b95-96c3-a5dc5efe361f" />

4. Llena los datos basicos de la plantilla, Revisar y crear > Crear
<img width="437" alt="25" src="https://github.com/user-attachments/assets/d249c482-661e-4b46-8d57-94dc3c71109b" />

5. Una vez creada la plantilla, para poder usarla selecciona lo 3 puntos  y selecciona la opción **Implementar**
<img width="953" alt="26" src="https://github.com/user-attachments/assets/f8856a93-e3fa-4bc5-b7c8-3623a1f31f6d" />

## Tarea 5: Prueba de acceso y cifrado

>**Nota**: Para el siguiente ejercicio se debe cambiar **Acceso de red pública** de la cuenta de almacenamiento, debido a que en el estado **Deshabilitado** y con la configuración del **Punto de conexión privado**, solo permite conexiónes desde la misma Vnet, invalidando la conexión a través de **Firma de acceso compartido**

1. Ingresa a la Cuenta de almacenamiento creada con anterioridad, ingresa a **Seguridad y redes** > **Redes**, en **Acceso de red pública** selecciona la opción **Habilitado desde redes virtuales y direcciones ip seleccionadas**, en el apartado **Firewall** ingresa la dirección ip pública de donde estas realizando el ejercicio, guardar los cambios
<img width="491" alt="26-a" src="https://github.com/user-attachments/assets/3f7c1b7d-705a-4a47-b35e-e2be4d67a104" />

2. En **Seguridad y redes** > **Firma de acceso compartido**, Selecciona todos los tipos de recusos permitidos
<img width="741" alt="27" src="https://github.com/user-attachments/assets/6c8ba922-3aa5-4ca0-af54-9133bcfaa3ca" />

3. Selecciona la opcion **Generar la cadena de conexión y SAS**, copia la cadena de conexión (comienza con **BlobEndpoint=https...**)
<img width="486" alt="28" src="https://github.com/user-attachments/assets/32f03473-a90d-46c3-8c75-116eeb4bc2b8" />

4. Descarga e instala **Azure Storage Explorer** del siguiente link, instalador se encuentra al final de la página
https://azure.microsoft.com/en-us/products/storage/storage-explorer/

5. En Azure Storage Explorer, selecciona el botón **Conexión a los recursos de Azure**
<img width="553" alt="29" src="https://github.com/user-attachments/assets/f17929f3-39a6-41a6-bde3-0f19b2854cff" />

6. Selecciona **Servicio o cuenta de almacenamineto
<img width="682" alt="30" src="https://github.com/user-attachments/assets/9bd63c09-20c6-4130-b912-a42d86c7440f" />

7. En **Seleccionar el método de conexión**, selecciona la opción **Cadena de conexión (clave o SAS)**
<img width="506" alt="31" src="https://github.com/user-attachments/assets/a0c46753-4d9b-4cac-aee8-f716cd4dd5c7" />

8. Pega la **Cadena de conexión** copiada anteriormente
<img width="505" alt="32" src="https://github.com/user-attachments/assets/62aa2af2-52ac-4a50-8e8a-650dd0b0dec5" />

9. Selecciona **Conectar**
<img width="695" alt="33" src="https://github.com/user-attachments/assets/8352282f-f251-4f2c-8d5e-e8408c026cfb" />

10. haz click con el botón derecho del mouse sobre **Contenedores de blob**, selecciona la opcíon **Crear contenedor de Blob**
<img width="363" alt="34" src="https://github.com/user-attachments/assets/5805f907-65f0-44dc-bee5-adc7174275c0" />
<img width="206" alt="35" src="https://github.com/user-attachments/assets/c334cc31-52be-4699-80c0-474f987c05fd" />

11. Selecciona el nuevo contenedor creado, presiona el botón **cargar** y selecciona la opción **Cargar archivos...**
<img width="114" alt="36" src="https://github.com/user-attachments/assets/fed4720c-2852-4487-aff2-070eb9d92856" />

12. En la ventana **Cargar archivos**, presiona el bóton de tres puntos, selecciona un archivo que quires cargar > Cargar
<img width="417" alt="37" src="https://github.com/user-attachments/assets/fc177d9c-535f-49bf-8b11-f4a26b1d6159" />
<img width="476" alt="38" src="https://github.com/user-attachments/assets/558436b1-f65e-4278-938e-e8390101c094" />

13. Comprueba a través del Portal que se ha creado un nuevo Contenedor y que tiene el archivo cargado a través de Azure Storage Explorer
<img width="555" alt="39" src="https://github.com/user-attachments/assets/af8f1f00-8f91-4313-83b8-d3eb181aa83f" />
<img width="340" alt="40" src="https://github.com/user-attachments/assets/ad8ae896-a1a1-4188-8aae-a4220d7051eb" />

## Limpia tus recursos

Si está trabajando con **su propia suscripción**, tómese un minuto para eliminar los recursos del laboratorio. Esto garantizará que se liberen recursos y se minimicen los costos. La manera más sencilla de eliminar los recursos de laboratorio es eliminar el grupo de recursos de laboratorio. 
