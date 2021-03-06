---
title: Envío de métricas de SO invitado al almacén de datos de Azure Monitor para una máquina virtual Windows (clásica)
description: Envío de métricas de SO invitado al almacén de datos de Azure Monitor para una máquina virtual Windows (clásica)
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: 235eda231dfb0f936bf55c7c8d93a8f709fdf9bc
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954867"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Envío de métricas de SO invitado al almacén de datos de Azure Monitor para una máquina virtual Windows (clásica)

La [extensión Microsoft Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (WAD) de Azure Monitor le permite recopilar métricas y registros del sistema operativo invitado (SO invitado) que se ejecuta como parte de un clúster de Service Fabric, Cloud Service o Virtual Machine. La extensión puede enviar datos de telemetría a muchas ubicaciones diferentes que aparecen en el artículo vinculado anteriormente.

En este artículo se describe el proceso de envío de métricas de rendimiento del SO invitado para una máquina virtual Windows (clásica) al almacén de métricas de Azure Monitor. A partir de Microsoft Azure Diagnostics versión 1.11, puede escribir las métricas directamente en el almacén de métricas de Azure Monitor, donde ya se recopilan métricas de la plataforma estándar. Almacenarlas en esta ubicación permite tener acceso a las mismas acciones disponibles para las métricas de la plataforma.  Las acciones incluyen la generación de alertas casi en tiempo real, la creación de gráficos, el enrutamiento, el acceso desde la API REST y mucho más.  Anteriormente, la extensión Microsoft Azure Diagnostics se escribía en Azure Storage, pero no el almacén de datos de Azure Monitor. 

El proceso descrito en este artículo solo funciona para máquinas virtuales clásicas que ejecutan el sistema operativo Windows.

## <a name="pre-requisites"></a>Requisitos previos

- Debe ser [administrador de servicios o coadministrador](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) en su suscripción de Azure 

- La suscripción debe estar registrada en [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1). 

- Debe tener instalado [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1), o bien puede usar [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md). 

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Crear una máquina virtual clásica y una cuenta de almacenamiento

1. Cree una máquina virtual clásica mediante Azure Portal ![Crear una VM clásica](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png)

1. Al crear esta VM, elija crear una nueva cuenta de almacenamiento clásica. Usaremos esta cuenta de almacenamiento en pasos posteriores.

1. En Azure Portal, vaya a la hoja de recursos Cuenta de almacenamiento, elija **Claves** y anote el nombre de la cuenta de almacenamiento y la clave de la cuenta de almacenamiento. Necesita estas claves en pasos posteriores ![Claves de acceso de almacenamiento](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Cree una entidad de servicio en el inquilino de Azure Active Directory según las instrucciones que encontrará en [Creación de una entidad de servicio](../active-directory/develop/howto-create-service-principal-portal.md). Tenga en cuenta lo siguiente al realizar este proceso: 
- Cree un nuevo secreto de cliente para esta aplicación.  
- Guarde la clave y el identificador de cliente para su uso en pasos posteriores.

Asigne a esta aplicación permisos "Supervisión del publicador de métricas" al recurso para el cual quiere emitir métricas. Puede usar un grupo de recursos o una suscripción completa.  

> [!NOTE]
> La extensión de diagnóstico usará la entidad de servicio para autenticarse en Azure Monitor y emitir métricas para la VM clásica.

## <a name="author-diagnostics-extension-configuration"></a>Creación de la configuración de la extensión de diagnóstico

1. Prepare el archivo de configuración de la extensión de diagnóstico WAD. Este archivo determina qué registros y contadores de rendimiento debe recopilar la extensión de diagnóstico para la VM clásica. Sigue un ejemplo.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
            </PerformanceCounters>
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" />
                <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" />
            </WindowsEventLog>
            <CrashDumps>
                <CrashDumpConfiguration processName="WaIISHost.exe" />
                <CrashDumpConfiguration processName="WaWorkerHost.exe" />
                <CrashDumpConfiguration processName="w3wp.exe" />
            </CrashDumps>
            <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" />
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
        </DiagnosticMonitorConfiguration>
        <SinksConfig>
        </SinksConfig>
        </WadCfg>
        <StorageAccount />
    </PublicConfig>
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <StorageAccount name="" endpoint="" />
    </PrivateConfig>
    <IsEnabled>true</IsEnabled>
    </DiagnosticsConfiguration>
    ```
1. En la sección "SinksConfig" del archivo de diagnóstico, defina un nuevo receptor de Azure Monitor:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide your Classic VM’s Resource ID </ResourceId>
                <Region>Region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. En la sección del archivo de configuración donde se enumera la lista de contadores de rendimiento que se van a recopilar, enrute los contadores de rendimiento al receptor de Azure Monitor "AzMonSink".

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. En la configuración privada defina la cuenta de Azure Monitor y agregue la información de la entidad de servicio que se usará para emitir métricas.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Guarde este archivo localmente.

## <a name="deploy-diagnostics-extension-to-your-cloud-service"></a>Implementación de la extensión de diagnóstico en el servicio en la nube

1. Inicie PowerShell e inicie sesión.

    ```powershell
    Login-AzureRmAccount
    ```

1. Empiece por establecer el contexto en la VM clásica.

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Establezca el contexto de la cuenta de almacenamiento clásica que se creó con la VM.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Establezca la ruta de acceso del archivo de diagnóstico en una variable mediante el comando siguiente.

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Prepare la actualización de la VM clásica con el archivo de diagnóstico con el receptor de Azure Monitor configurado.

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Ejecute el comando siguiente para implementar la actualización en la VM:

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Sigue siendo obligatorio proporcionar una cuenta de almacenamiento como parte de la instalación de la extensión de diagnóstico. Todos los registros o contadores de rendimiento especificados en el archivo de configuración de diagnóstico se escribirán en la cuenta de almacenamiento especificada.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Trazado de métricas en Azure Portal

1.  Vaya a Azure Portal.

1.  En el menú de la izquierda, haga clic en Monitor.

1.  En la hoja Monitor, haga clic en **Métricas**
   ![Navigate metrics](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png) (Navegar por las métricas).

1. En la lista desplegable de recursos, seleccione la VM clásica.

1. En la lista desplegable de espacios de nombres, seleccione **azure.vm.windows.guest**.

1. En la lista desplegable de métricas, seleccione **Memory\Committed Bytes in Use** (Memoria\Bytes confirmados en uso)
   ![Trazar métricas](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png).


## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de las [métricas personalizadas](metrics-custom-overview.md).
