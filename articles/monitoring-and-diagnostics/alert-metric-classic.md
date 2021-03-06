---
title: Crear, ver y administrar las alertas de métricas clásicas mediante Azure Monitor
description: Aprenda a usar Azure Portal, CLI o Powershell para crear, ver y administrar las reglas de alerta de métricas clásicas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: ffac9511ab93367cba3f006249bd92658a520e09
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386949"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Creación, visualización y administración de alertas de métricas clásicas mediante Azure Monitor

Las alertas de métricas clásicas en Azure Monitor proporcionan una forma de recibir notificaciones cuando una de sus métricas cruza un umbral. Las alertas de métricas clásicas son una funcionalidad más antigua que permiten enviar alertas solo en métricas no dimensionales. Existe una funcionalidad más reciente existente llamada Alertas de métricas que ha mejorado la funcionalidad de las alertas de métricas clásicas. Puede obtener más información acerca de la nueva funcionalidad de alertas de indicadores en la [información general de las alertas de métricas](alert-metric-overview.md). En este artículo describiremos cómo crear, ver y administrar las reglas de alerta de métricas clásicas a través de Azure Portal, Azure CLI y Powershell.

## <a name="with-azure-portal"></a>Con Azure Portal

1. En el [portal](https://portal.azure.com/), busque el recurso que desea supervisar y, a continuación, selecciónelo.

2. En la sección **SUPERVISIÓN**, seleccione **Alertas (clásico)**. El texto y el icono pueden variar ligeramente en los distintos recursos. Si no encuentra **Alertas (clásico)** aquí, puede que lo encuentre en **Alertas** o en **Reglas de alertas**.

    ![Supervisión](./media/alerts-metric-classic/AlertRulesButton.png)

3. Seleccione el comando **Agregar una alerta de métrica (clásica)** y rellene los campos.

    ![Agregar alerta](./media/alerts-metric-classic/AddAlertOnlyParamsPage.png)

4. Asigne un **nombre** a la regla de alerta. Después, elija una **descripción**, que también aparecerá en los correos electrónicos de notificación.

5. Seleccione la **métrica** que desea supervisar. Posteriormente, elija una **condición** y el valor de **umbral** de la métrica. También debe elegir el **período** de la regla de métrica que se debe cumplir antes de que se desencadene la alerta. Por ejemplo, si usa el período "En los últimos 5 minutos" y la alerta busca una CPU por encima del 80 %, la alerta se desencadena cuando la CPU ha estado por encima del 80 % durante 5 minutos de manera uniforme. Una vez que se desencadena por primera vez, se vuelve a desencadenar cuando la CPU se mantiene por debajo del 80 % durante 5 minutos. La medición de la métrica de CPU se produce cada minuto.

6. Seleccione **Enviar correo electrónico a propietarios...** si desea que se envíe una notificación por correo electrónico a los administradores y coadministradores cuando se active la alerta.

7. Si quiere enviar notificaciones a otras direcciones de correo electrónico cuando se active la alerta, agréguelas en el campo **Correos electrónicos adicionales del administrador**. Separe las direcciones de correo electrónico con punto y coma y procure que tengan el siguiente formato: *email@contoso.com; email2@contoso.com*

8. Escriba un identificador URI válido en el campo **Webhook** si desea llamarlo cuando se active la alerta.

9. Si usa Azure Automation, puede seleccionar un runbook para que se ejecute cuando se active la alerta.

10. Seleccione **Aceptar** para crear la alerta.

En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

Una vez que haya creado una alerta, puede seleccionarla y realizar alguna de las acciones siguientes:

* Ver un gráfico que muestre el umbral de las métricas y los valores reales del día anterior.
* Editar la alerta o eliminarla.
* **Deshabilitar** la alerta, si desea dejar de recibir notificaciones de esa alerta de manera temporal, o **habilitarla** si desea reanudar sus notificaciones.

## <a name="with-azure-cli"></a>Con la CLI de Azure

En las secciones anteriores se describía cómo crear, ver y administrar las reglas de alertas de métricas mediante Azure Portal. En esta sección se describe cómo hacer lo mismo con la multiplataforma [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). La forma más rápida de comenzar a utilizar la CLI de Azure es a través de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Obtener todas las reglas de alerta de métricas clásicas en un grupo de recursos

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Ver los detalles de una regla de alerta de métricas clásica específica

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Crear una regla de alerta de métricas clásica

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Eliminar una regla de alerta de métricas clásica

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>Con PowerShell

En esta sección se muestra cómo usar los comandos de PowerShell para crear, ver y administrar las alertas de métricas clásicas. Los ejemplos de este artículo muestran cómo puede usar los cmdlets de Azure Monitor en las alertas de métricas clásicas.

1. Si no lo ha hecho ya, configure PowerShell para que se ejecute en el equipo. Para más información, consulte el artículo de [instalación y configuración de PowerShell](/powershell/azure/overview). También puede consultar toda la lista de cmdlets de PowerShell de Azure Monitor en [Azure Monitor Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.insights) (Cmdlets de Azure Monitor).

2. Primero, inicie sesión en la suscripción de Azure.

    ```PowerShell
    Connect-AzureRmAccount
    ```

3. Verá una pantalla de inicio de sesión. Una vez que inicie sesión, se muestran el identificador predeterminado de la suscripción, el identificador de inquilino y la cuenta. Todos los cmdlets de Azure funcionan en el contexto de la suscripción predeterminada. Para ver la lista de suscripciones a las que tiene acceso, use el siguiente comando:

    ```PowerShell
    Get-AzureRmSubscription
    ```

4. Para cambiar el contexto de trabajo a una suscripción diferente, utilice el siguiente comando:

    ```PowerShell
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

5. Puede obtener todas las reglas de alerta de métricas clásicas en un grupo de recursos:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest
    ```

6. Puede ver los detalles de una regla de alerta de métricas clásica.

    ```PowerShell
    Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Puede recuperar todas las reglas de alerta de un recurso de destino. Por ejemplo, todas las reglas de alerta se establecen en una máquina virtual.

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig

8. You can use the `Add-AlertRule` cmdlet to create, update, or disable an alert rule. You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively. In the Alert rule cmdlet, assign these properties as actions to the **Actions** property of the Alert Rule. The following table describes the parameters and values used to create an alert using a metric.

    | parameter | value |
    | --- | --- |
    | Name |simpletestdiskwrite |
    | Location of this alert rule |East US |
    | ResourceGroup |montest |
    | TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
    | MetricName of the alert that is created |\PhysicalDisk(_Total)\Disk Writes/sec. See the `Get-MetricDefinitions` cmdlet about how to retrieve the exact metric names |
    | operator |GreaterThan |
    | Threshold value (count/sec in for this metric) |1 |
    | WindowSize (hh:mm:ss format) |00:05:00 |
    | aggregator (statistic of the metric, which uses Average count, in this case) |Average |
    | custom emails (string array) |'foo@example.com','bar@example.com' |
    | send email to owners, contributors and readers |-SendToServiceOwners |

9. Create an Email action

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    ```

10. Crear una acción de Webhook

    ```PowerShell
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Crear una alerta de métricas clásica con una plantilla de Resource Manager](monitoring-enable-alerts-using-template.md).
- [Cómo hacer que una alerta de métrica clásica notifique a un sistema que no es de Azure mediante un webhook](insights-webhooks-alerts.md).

