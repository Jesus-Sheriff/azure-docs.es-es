---
title: Uso de la CLI de Azure clásica con Azure Storage | Microsoft Docs
description: Aprenda a usar la interfaz de la línea de comandos (CLI) clásica de Azure con Azure Storage para crear y administrar cuentas de almacenamiento y trabajar con archivos y blobs de Azure.
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.component: common
ms.openlocfilehash: e563c7000b600bed917f42d8ffb87df883564ef8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979335"
---
# <a name="using-the-azure-classic-cli-with-azure-storage"></a>Uso de la CLI de Azure clásica con Azure Storage

## <a name="overview"></a>Información general

La CLI de Azure clásica proporciona un conjunto de comandos de código abierto y multiplataforma para trabajar con la plataforma de Azure. Proporciona muchas de las funcionalidades que se encuentran en el [Portal de Azure](https://portal.azure.com) , así como la funcionalidad de acceso a datos enriquecidos.

En esta guía, exploraremos cómo usar la [CLI de Azure clásica](../../cli-install-nodejs.md) para realizar diversas tareas de desarrollo y administración con Azure Storage. Antes de usar esta guía es aconsejable descargar e instalar la CLI clásica más reciente, o actualizarse a ella.

En esta guía se supone que conoce los conceptos básicos de Azure Storage. La guía incluye varios scripts que muestran cómo se usa la CLI clásica con Azure Storage. Antes de ejecutar cada script, asegúrese de que ha actualizado las variables del mismo según su configuración.

> [!NOTE]
> La guía proporciona ejemplos de comandos y scripts de la CLI de Azure clásica de cuentas de almacenamiento clásico. Consulte [Uso de la CLI de Azure para Mac, Linux y Windows mediante Azure Resource Manager](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) para ver los comandos de la CLI de Azure clásica de las cuentas de almacenamiento de Resource Manager.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-classic-cli-in-5-minutes"></a>Introducción de 5 minutos a Azure Storage y a la CLI de Azure clásica
En esta guía se usa Ubuntu para los ejemplos, pero el funcionamiento debe ser similar en otros sistemas operativos.

**Nuevo en Azure:** obtenga una suscripción de Microsoft Azure y una cuenta de Microsoft asociada a dicha suscripción. Para más información sobre las opciones de compra de Azure, consulte [Evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/), [Opciones de compra](https://azure.microsoft.com/pricing/purchase-options/) y [Ofertas para miembros](https://azure.microsoft.com/pricing/member-offers/) (para miembros de MSDN, Microsoft Partner Network, BizSpark y otros programas de Microsoft).

Para obtener más información sobre las suscripciones a Azure, consulte [Asignación de roles de administrador en Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) .

**Después de crear una suscripción y una cuenta de Microsoft Azure:**

1. Descargue e instale la CLI de Azure clásica mediante las instrucciones que encontrará en [Instalación de la CLI de Azure clásica](../../cli-install-nodejs.md).
2. Una vez instalada la CLI clásica, podrá usar el comando de Azure desde su interfaz de la línea de comandos (Bash, Terminal, símbolo del sistema) para tener acceso a los comandos de la CLI clásica. Escriba el comando _azure_ y debería ver la siguiente salida.

    ![Resultado del comando de Azure](./media/storage-azure-cli/azure_command.png)   
3. En la interfaz de la línea de comandos, escriba `azure storage` para enumerar todos los comandos de Azure Storage y obtener una primera impresión de las funcionalidades que proporciona la CLI clásica. Para ver los detalles de la sintaxis del comando, escriba el nombre del comando con el parámetro **-h** (por ejemplo, `azure storage share create -h`).
4. A continuación, encontrará un sencillo script que muestra los comandos básicos de la CLI clásica para obtener acceso a Azure Storage. En primer lugar, el script le pedirá que configure dos variables para la cuenta de almacenamiento y la clave. A continuación, el script creará un nuevo contenedor en la nueva cuenta de almacenamiento y cargará un archivo de imagen existente (blob) en dicho contenedor. Una vez el script haya enumerado todos los blobs del contenedor, descargará el archivo de imagen en el directorio de destino del equipo local.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. En el equipo local, abra el editor de texto que desee (por ejemplo, vim). Escriba el script anterior en un editor de texto.
6. Ahora deberá actualizar las variables del script basadas en la configuración.

   * **&lt;storage_account_name&gt;** Use el nombre proporcionado en el script o escriba un nuevo nombre para la cuenta de almacenamiento. **Importante:** el nombre de la cuenta de almacenamiento debe ser exclusivo en Azure. También debe estar en minúscula.
   * **&lt;storage_account_key&gt;** La clave de acceso de su cuenta de almacenamiento.
   * **&lt;container_name&gt;** Use el nombre proporcionado en el script o escriba un nuevo nombre para el contenedor.
   * **&lt;image_to_upload&gt;** Escriba una ruta de acceso a una imagen en el equipo local, por ejemplo: "~/images/HolaMundo.png".
   * **&lt;destination_folder&gt;** Escriba una ruta de acceso a un directorio local para almacenar los archivos descargados desde Azure Storage, por ejemplo: "~/imágenesDescargadas".
7. Después de haber actualizado las variables necesarias en vim, presione las combinaciones de teclas `ESC`, `:`, `wq!` para guardar el script.
8. Para ejecutar este script, solo es preciso escribir el nombre del archivo de script en la consola de bash. Una vez que se ejecuta el script, debería tener una carpeta de destino local que incluyera el archivo de imagen descargado. La siguiente captura de pantalla le muestra un ejemplo del resultado:

Una vez ejecutado el script, debería tener una carpeta de destino local que incluye el archivo de imagen descargado.

## <a name="manage-storage-accounts-with-the-azure-classic-cli"></a>Administración de cuentas de almacenamiento con la CLI de Azure clásica
### <a name="connect-to-your-azure-subscription"></a>Conexión a su suscripción de Azure
Aunque la mayoría de los comandos de almacenamiento funcionarán sin suscripción a Azure, es aconsejable que se conecte a su suscripción desde la CLI clásica.

### <a name="create-a-new-storage-account"></a>Creación de una cuenta de almacenamiento nueva
Para utilizar Almacenamiento de Azure, necesitará una cuenta de almacenamiento. Puede crear una nueva cuenta de almacenamiento de Azure después de configurar el equipo para que pueda conectarse a su suscripción.

```azurecli
azure storage account create <account_name>
```

El nombre de la cuenta de almacenamiento debe tener entre 3 y 24 caracteres, y usar solo números y letras minúsculas.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Establecimiento de una cuenta predeterminada de Almacenamiento de Azure en las variables de entorno
Puede tener varias cuentas de almacenamiento en su suscripción. Puede elegir una de ellas y establecerla en las variables de entorno de todos los comandos de almacenamiento de la misma sesión. Esto le permite ejecutar los comandos de almacenamiento de la CLI clásica sin especificar explícitamente la cuenta de almacenamiento y la clave.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Otra forma de establecer una cuenta de almacenamiento predeterminada es mediante una cadena de conexión. En primer lugar obtenga la cadena de conexión con el comando:

```azurecli
azure storage account connectionstring show <account_name>
```

A continuación, copie la cadena de conexión de salida y establézcala como variable de entorno:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Creación y administración de blobs
El almacenamiento de blobs de Azure es un servicio para almacenar grandes cantidades de datos no estructurados, como texto o datos binarios, a los que puede acceder desde cualquier lugar del mundo a través de HTTP o HTTPS. En esta sección se supone que ya está familiarizado con los conceptos del almacenamiento de blobs de Azure. Para más información, consulte [Introducción a Azure Blob Storage mediante .NET](../blobs/storage-dotnet-how-to-use-blobs.md) y [Conceptos de Blob service](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Crear un contenedor
Todos los blobs del almacenamiento de Azure han de estar en un contenedor. Puede crear un contenedor privado con el comando `azure storage container create` :

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Existen tres niveles de acceso de lectura anónimo: **Desactivado**, **Blob** y **Contenedor**. Para evitar el acceso anónimo a los blobs, establezca el parámetro de permiso en **Desactivado**. El nuevo contenedor es privado por defecto y solo puede acceder a él el propietario de la cuenta. Para permitir un acceso de lectura público y anónimo a los recursos de blob, pero no a los metadatos del contenedor ni a la lista de blobs del contenedor, seleccione **Blob**en el parámetro Permiso. Para hacer que el acceso de lectura a los recursos de blob, a los metadatos del contenedor y a la lista de blobs del contenedor sean totalmente públicos, elija **Contenedor**en el parámetro Permiso. Para obtener más información, consulte [Administración del acceso de lectura anónimo a contenedores y blobs](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Cargar un blob en un contenedor
Azure Blob Storage admite blobs en bloques y en páginas. Para más información, consulte [Descripción de los blobs en bloques, en anexos y en páginas](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Para cargar blobs en un contenedor, puede utilizar `azure storage blob upload`. Este comando carga de forma predeterminada los archivos locales a un blob en bloque. Para especificar el tipo de blob, puede usar el parámetro `--blobtype` .

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Descarga  de blobs de un contenedor
En el siguiente ejemplo le mostraremos cómo descargar blobs de un contenedor.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Copia de blobs
Los blobs se pueden copiar dentro de las cuentas de almacenamiento y regiones, o entre ellas, de forma asincrónica.

En el siguiente ejemplo se muestra cómo copiar blobs de una cuenta de almacenamiento a otra. En este ejemplo crearemos un contenedor donde al que es posible acceder de forma pública y anónima a los blobs.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

En este ejemplo se realiza una copia asincrónica. Para supervisar el estado de cada operación de copia, ejecute la operación `azure storage blob copy show` .

Tenga en cuenta que debe ser posible acceder públicamente a la dirección URL de origen proporcionada para la operación de copia, o bien incluir un token SAS (firma de acceso compartido).

### <a name="delete-a-blob"></a>Eliminar un blob
Para eliminar un blob, use el siguiente comando:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Creación y administración de recursos compartidos de archivos
Azure Files ofrece almacenamiento compartido para aplicaciones que usan el protocolo SMB estándar. Los servicios en la nube y las máquinas virtuales de Microsoft Azure, así como las aplicaciones locales, pueden compartir datos de archivos a través de recursos compartidos montados. Los recursos compartidos de archivos y datos de archivos se pueden administrar a través de la CLI clásica. Para obtener más información sobre Azure Files, consulte [Introducción a Azure Files](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos
Un recurso compartido de archivos de Azure es un recurso compartido de archivos de SMB en Azure. Todos los directorios y archivos se deben crear en un recurso compartido de archivos. Una cuenta puede contener un número ilimitado de recursos compartidos y un recurso compartido puede almacenar un número ilimitado de archivos, hasta los límites de capacidad de la cuenta de almacenamiento. En el siguiente ejemplo se crea un recurso compartido de archivos denominado **myshare**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Creación de directorios
Un directorio proporciona una estructura jerárquica opcional para los recursos compartidos de archivos de Azure. En el ejemplo siguiente se crea el directorio **myDir** en el recurso compartido de archivos.

```azurecli
azure storage directory create myshare myDir
```

Tenga en cuenta que la ruta de acceso al directorio puede incluir varios niveles, *p. ej.*, **a/b**. Pero debe asegurarse de que existen todos los directorios principales. Por ejemplo, para la ruta de acceso **a/b**, primero se debe crear el directorio **a** y luego el directorio **b**.

### <a name="upload-a-local-file-to-directory"></a>Carga de un archivo local a un directorio
El siguiente ejemplo carga un archivo de **~/temp/samplefile.txt** en el directorio **myDir**. Edite la ruta de acceso al archivo de forma que apunte a un archivo válido situado en la máquina local:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Tenga en cuenta que los archivos del recurso compartido pueden tener un tamaño máximo de 1 TB.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Enumeración de los archivos de la raíz o directorio compartidos
Los archivos y subdirectorios del directorio raíz del recurso compartido o de otro directorio se pueden enumerar con el comando siguiente:

```azurecli
azure storage file list myshare myDir
```

Tenga en cuenta que el nombre de directorio es opcional en la operación de enumeración. Si se omite, el comando muestra el contenido del directorio raíz del recurso compartido.

### <a name="copy-files"></a>Copiar archivos
A partir de la versión 0.9.8 de la CLI clásica, puede copiar un archivo en otro, un archivo en un blob o un blob en un archivo. A continuación mostramos cómo realizar estas operaciones de copia mediante comandos de CLI. Para copiar un archivo en el nuevo directorio:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

Para copiar un blob en un directorio de archivos:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Pasos siguientes

Aquí puede encontrar la referencia a los comandos de la CLI de Azure clásica para trabajar con recursos de Storage:

* [Comandos de la CLI de Azure clásica en el modo de Resource Manager](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Comandos de la CLI de Azure clásica en modo de Azure Service Management](../../cli-install-nodejs.md)

También puede probar la versión más reciente de la [CLI de Azure](../storage-azure-cli.md), que se usa con el modelo de implementación de Resource Manager.
