---
title: Migración a herramientas de Azure Resource Manager para HDInsight
description: Migración a las herramientas de desarrollo de Azure Resource Manager para clústeres de HDInsight
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: adf5bbaee6669ac232c7d28a8c46cd2dd89c3d48
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955210"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migración a las herramientas de desarrollo basadas en Azure Resource Manager para clústeres de HDInsight

HDInsight está abandonando el uso de herramientas basadas en Azure Service Manager (ASM) para HDInsight. Si ha estado usando Azure PowerShell, la CLI de Azure clásica o el SDK de HDInsight para .NET para trabajar con clústeres de HDInsight, se le recomienda que use las versiones de Azure Resource Manager de PowerShell, la CLI y el SDK de .NET. Este artículo proporciona sugerencias sobre cómo migrar al nuevo enfoque basado en Resource Manager. Siempre que sea aplicable, en este artículo se destacan las diferencias entre los enfoques de ASM y Resource Manager para HDInsight.

> [!IMPORTANT]
> El soporte para ASM basado en PowerShell, CLI, y SDK de .NET dejará de estar disponible el **1 de enero de 2017**.
> 
> 

## <a name="migrating-azure-classic-cli-to-azure-resource-manager"></a>Migración de la CLI de Azure clásica a Azure Resource Manager

> [!IMPORTANT]
> La CLI de Azure no proporciona compatibilidad para trabajar con clústeres de HDInsight. Aunque puede seguir usando la CLI de Azure clásica con HDInsight, está en desuso.

Estos son los comandos básicos para trabajar con HDInsight mediante la CLI de Azure clásica:

* `azure hdinsight cluster create` : crea un clúster de HDInsight
* `azure hdinsight cluster delete` : elimina un clúster de HDInsight que ya existe
* `azure hdinsight cluster show` : muestra información acerca de un clúster que ya existe
* `azure hdinsight cluster list` : enumera los clústeres de HDInsight para la suscripción de Azure

Utilice el conmutador `-h` para inspeccionar los parámetros y conmutadores disponibles para cada comando.

### <a name="new-commands"></a>Nuevos comandos
Los nuevos comandos disponibles con Azure Resource Manager son:

* `azure hdinsight cluster resize` : cambia dinámicamente el número de nodos de trabajo del clúster
* `azure hdinsight cluster enable-http-access` : habilita el acceso HTTP al clúster (de forma predeterminada)
* `azure hdinsight cluster disable-http-access` : deshabilita el acceso HTTP al clúster
* `azure hdinsight script-action`: proporciona comandos para crear y administrar las acciones de script en un clúster
* `azure hdinsight config`: proporciona comandos para crear un archivo de configuración que se puede usar con el comando `hdinsight cluster create` para proporcionar información de configuración.

### <a name="deprecated-commands"></a>Comandos en desuso
Si utiliza los comandos `azure hdinsight job` para enviar trabajos al clúster de HDInsight, estos comandos no estarán disponibles a través de los comandos de Resource Manager. Si necesita enviar trabajos a HDInsight mediante programación desde scripts, debe usar las API de REST proporcionadas por HDInsight. Para más información sobre el envío de trabajos mediante las API de REST, consulte los siguientes documentos.

* [Ejecución de trabajos de MapReduce con Hadoop en HDInsight con Curl](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [Ejecución de consultas de Hive con Hadoop en HDInsight con cURL](hadoop/apache-hadoop-use-hive-curl.md)
* [Ejecución de trabajos de Pig con Hadoop en HDInsight con Curl](hadoop/apache-hadoop-use-pig-curl.md)

Para más información sobre otras formas de ejecutar MapReduce, Hive y Pig interactivamente, consulte [Uso de MapReduce con Hadoop en HDInsight](hadoop/hdinsight-use-mapreduce.md), [Uso de Hive con Hadoop en HDInsight](hadoop/hdinsight-use-hive.md) y [Uso de Pig con Hadoop en HDInsight](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Ejemplos
**Creación de un clúster**

* Comando anterior (ASM): `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Nuevo comando: `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Eliminación de un cluster**

* Comando anterior (ASM): `azure hdinsight cluster delete myhdicluster`
* Nuevo comando: `azure hdinsight cluster delete mycluster -g myresourcegroup`

**Enumeración de clústeres**

* Comando anterior (ASM): `azure hdinsight cluster list`
* Nuevo comando: `azure hdinsight cluster list`

> [!NOTE]
> Para el comando list, si especifica el grupo de recursos mediante `-g` devolverá solo los clústeres del grupo de recursos especificado.
> 
> 

**Presentación de la información de clúster**

* Comando anterior (ASM): `azure hdinsight cluster show myhdicluster`
* Nuevo comando: `azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Migración de Azure PowerShell a Azure Resource Manager
La información general acerca de Azure PowerShell en el modo de Azure Resource Manager se puede encontrar en [Uso de Azure PowerShell con Azure Resource Manager](../powershell-azure-resource-manager.md).

Los cmdlets de Resource Manager de Azure PowerShell se pueden instalar en paralelo con los cmdlets de ASM. Los cmdlets de los dos modos se pueden distinguir por sus nombres.  El modo de Resource Manager tiene *AzureRmHDInsight* en los nombres de cmdlet en lugar de *AzureHDInsight*, que es lo que aparece en el modo ASM.  Por ejemplo, *AzureRmHDInsightCluster New* frente a *New-AzureHDInsightCluster*. Parámetros y conmutadores pueden tener nombres nuevos, y hay muchos parámetros nuevos disponibles si utiliza Resource Manager.  Por ejemplo, varios cmdlets requieren un nuevo modificador llamado *- ResourceGroupName*. 

Para poder usar los cmdlets de HDInsight, debe conectarse a su cuenta de Azure y crear un nuevo grupo de recursos:

* Connect-AzureRmAccount o [Select-AzureRmProfile](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/select-azurermprofile?view=azuresmps-4.0.0). Consulte [Autenticación de una entidad de servicio con el Administrador de recursos de Azure](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Cmdlets cuyo nombre ha cambiado
Para enumerar los cmdlets de ASM para HDInsight en la consola de Windows PowerShell:

    help *azurermhdinsight*

En la tabla siguiente se enumeran los cmdlets de ASM y sus nombres en el modo de Resource Manager:

| Cmdlets de ASM | Cmdlets de Resource Manager |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Add-AzureRmHDInsightConfigValues](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightconfigvalues?view=azurermps-6.6.0) |
| Add-AzureHDInsightMetastore |[Add-AzureRmHDInsightMetastore](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightmetastore?view=azurermps-6.6.0) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightscriptaction?view=azurermps-6.6.0) |
| Add-AzureHDInsightStorage |[Add-AzureRmHDInsightStorage](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightstorage?view=azurermps-6.6.0) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightjob?view=azurermps-6.6.0) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightjoboutput?view=azurermps-6.6.0) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightproperties?view=azurermps-6.6.0) |
| Grant-AzureHDInsightHttpServicesAccess |[Grant-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsighthttpservicesaccess?view=azurermps-6.6.0) |
| Grant-AzureHdinsightRdpAccess |[Grant-AzureRmHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsightrdpservicesaccess?view=azurermps-6.6.0) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzureRmHDInsightHiveJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/invoke-azurermhdinsighthivejob?view=azurermps-6.6.0) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightclusterconfig?view=azurermps-6.6.0) |
| New-AzureHDInsightHiveJobDefinition |[New-AzureRmHDInsightHiveJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsighthivejobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightMapReduceJobDefinition |[New-AzureRmHDInsightMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightmapreducejobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightpigjobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightsqoopjobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[New-AzureRmHDInsightStreamingMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightstreamingmapreducejobdefinition?view=azurermps-6.6.0) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/remove-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Revoke-AzureHDInsightHttpServicesAccess |[Revoke-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsighthttpservicesaccess?view=azurermps-6.6.0) |
| Revoke-AzureHdinsightRdpAccess |[Revoke-AzureRmHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsightrdpservicesaccess?view=azurermps-6.6.0) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize?view=azurermps-6.6.0) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightdefaultstorage?view=azurermps-6.6.0) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/start-azurermhdinsightjob?view=azurermps-6.6.0) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/stop-azurermhdinsightjob?view=azurermps-6.6.0) |
| Use-AzureHDInsightCluster |[Use-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/use-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/wait-azurermhdinsightjob?view=azurermps-6.6.0) |

### <a name="new-cmdlets"></a>Nuevos cmdlets
Los siguientes son los nuevos cmdlets que solo están disponibles en el modo de Resource Manager. 

**Cmdlets relacionados con acciones de script:**

* **Get-AzureRmHDInsightPersistedScriptAction**: obtiene las acciones de script persistentes para un clúster y las muestra en orden cronológico u obtiene detalles de una acción de script persistente concreta. 
* **Get-AzureRmHDInsightScriptActionHistory**: obtiene el historial de una acción de script para un clúster y los datos se enumeran en orden cronológico inverso u obtiene detalles de una acción de script ejecutada anteriormente. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: quita una acción de script persistente de un clúster de HDInsight.
* **Set-AzureRmHDInsightPersistedScriptAction**: establece una acción de script ejecutada anteriormente para que sea una acción de script persistente.
* **Submit-AzureRmHDInsightScriptAction**: envía una nueva acción de script a un clúster de HDInsight de Azure. 

Para más información, consulte [Personalización de clústeres de HDInsight mediante la acción de scripts (Linux)](hdinsight-hadoop-customize-cluster-linux.md).

**Cmdlets relacionados con la identidad del clúster:**

* **Add-AzureRmHDInsightClusterIdentity**: agrega una identidad de clúster a un objeto de configuración de clúster para que el clúster de HDInsight pueda acceder a almacenes de Azure Data Lake. Consulte [Creación de un clúster de HDInsight con el Almacén de Data Lake mediante Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Ejemplos
**Crear clúster**

Comando anterior (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Nuevo comando:

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Eliminación de clúster**

Comando anterior (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Nuevo comando:

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Enumeración de clústeres**

Comando anterior (ASM):

    Get-AzureHDInsightCluster

Nuevo comando:

    Get-AzureRmHDInsightCluster 

**Presentación de clústeres**

Comando anterior (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Nuevo comando:

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Otros ejemplos
* [Creación de clústeres de HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Envío de trabajos de Hive](hadoop/apache-hadoop-use-hive-powershell.md)
* [Envío de trabajos de Pig](hadoop/apache-hadoop-use-pig-powershell.md)
* [Envío de trabajos de Sqoop](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-new-hdinsight-net-sdk"></a>Migración al nuevo SDK de .NET de HDInsight
El [SDK de .NET de HDInsight](https://msdn.microsoft.com/library/azure/mt416619.aspx) basado en Azure Service Management (ASM) ya está en desuso. Se recomienda utilizar el [SDK de .NET de HDInsight basado en Resource Manager](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight). Los siguientes paquetes de HDInsight basado en ASM quedarán obsoletos.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Esta sección proporciona indicadores a más información sobre cómo realizar determinadas tareas mediante el SDK de Resource Manager.

| Cómo... mediante el uso del SDK de HDInsight basado en Resource Manager | Vínculos |
| --- | --- |
| Crear clústeres basados en Linux en HDInsight con el SDK de .NET |Consulte [Crear clústeres basados en Linux en HDInsight con el SDK de .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| Personalizar un clúster mediante una acción de script con el SDK de .NET |Consulte [Crear clústeres basados en Linux en HDInsight con el SDK de .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Autenticar interactivamente aplicaciones mediante Azure Active Directory con el SDK de .NET |Consulte [Ejecución de consultas de Hive mediante el SDK de .NET de HDInsight](hadoop/apache-hadoop-use-hive-dotnet-sdk.md). El fragmento de código de este artículo usa el enfoque de autenticación interactiva. |
| Autenticar aplicaciones de forma no interactiva mediante Azure Active Directory con el SDK de .NET |Consulte [Crear aplicaciones .NET para HDInsight de autenticación no interactiva](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Enviar un trabajo de Hive mediante el SDK de .NET |Consulte [Ejecución de consultas de Hive mediante el SDK de .NET de HDInsight](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| Enviar un trabajo de Pig mediante el SDK de .NET |Consulte [Ejecución de trabajos de Pig con el SDK de .NET para Hadoop en HDInsight](hadoop/apache-hadoop-use-pig-dotnet-sdk.md) |
| Enviar un trabajo de Sqoop mediante el SDK de .NET |Consulte [Ejecución de trabajos de Sqoop con el SDK de .NET para Hadoop en HDInsight](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| Enumerar clústeres de HDInsight con el SDK. de .NET. |Consulte [Enumeración de clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Escalar clústeres de HDInsight con el SDK. de .NET. |Consulte [Escalamiento de clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Conceder o revocar acceso a los clústeres de HDInsight con el SDK de .NET |Consulte [Concesión o revocamiento de acceso a los clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Actualizar las credenciales de usuario HTTP para clústeres de HDInsight mediante el SDK de .NET |Consulte [Actualización de las credenciales de usuario HTTP para clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Encontrar la cuenta de almacenamiento predeterminada para los clústeres de HDInsight mediante el SDK de .NET |Consulte [Búsqueda de la cuenta de almacenamiento predeterminada para los clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Eliminar clústeres de HDInsight con el SDK de .NET. |Consulte [Eliminación de clústeres de HDInsight](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Ejemplos
A continuación se muestran algunos ejemplos de cómo se realiza una operación mediante el SDK basado en ASM y el fragmento de código equivalente para el SDK basado en Resource Manager.

**Creación de un cliente CRUD de clúster**

* Comando anterior (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Nuevo comando (autorización de la entidad de servicio)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Nuevo comando (autorización de usuario)
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Creación de un clúster**

* Comando anterior (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Nuevo comando
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**Habilitación del acceso HTTP**

* Comando anterior (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Nuevo comando
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Eliminación de un cluster**

* Comando anterior (ASM)
  
        client.DeleteCluster(dnsName);
* Nuevo comando
  
        client.Clusters.Delete(resourceGroup, dnsname);

