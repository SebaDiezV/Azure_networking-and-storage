# Proyecto 2 : Networking and Storage

## Temas Tratados
Virtual Networks, NSGs, Storage Accounts, Encryption, Protocols, ARM Templates

## Resumen
Este proyecto cubre la implementación de una cuenta de almacenamiento segura, la creación de redes virtuales y grupos de seguridad de red para el aislamiento, la habilitación del cifrado y la automatización de la implementación de recursos mediante plantillas ARM. Demuestra los aspectos básicos de las redes y el almacenamiento.

## Escenario
Una empresa desea implementar una cuenta de almacenamiento segura, garantizar el aislamiento de la red para sus recursos, aplicar el cifrado de datos y utilizar plantillas para automatizar la implementación de recursos.

## Diagrama

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

**PLACEHOLDER**
