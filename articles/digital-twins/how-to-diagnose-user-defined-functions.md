---
title: Depuración de las UDF en Azure Digital Twins | Microsoft Docs
description: Instrucciones para depurar las UDF en Azure Digital Twins
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: stefanmsft
ms.openlocfilehash: 852b2d35ae605f5529d162d52655fd258ca07c5a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49946103"
---
# <a name="how-to-debug-issues-with-user-defined-functions-in-azure-digital-twins"></a>Depuración de problemas con las funciones definidas por el usuario en Azure Digital Twins

En este artículo se resume cómo diagnosticar las funciones definidas por el usuario. A continuación, se identifican algunos de los escenarios más comunes que se producen al trabajar con ellas.

## <a name="debug-issues"></a>Depuración de problemas

Conocer la manera de diagnosticar cualquier problema que surja dentro de la instancia de Azure Digital Twins le ayudará a determinar de forma eficaz el problema, su causa y una solución.

### <a name="enable-log-analytics-for-your-instance"></a>Habilitación de Log Analytics para su instancia

Los registros y las métricas para la instancia de Azure Digital Twins se exponen a través de Azure Monitor. En la siguiente documentación se supone que ha creado un área de trabajo de [Azure Log Analytics](../log-analytics/log-analytics-queries.md) mediante [Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md), la [CLI de Azure](../log-analytics/log-analytics-quick-create-workspace-cli.md) o [PowerShell](../log-analytics/log-analytics-quick-create-workspace-posh.md).

> [!NOTE]
> Podría experimentar un retraso de 5 minutos al enviar eventos a **Log Analytics** por primera vez.

Lea el artículo [Recopile y use los datos de registro provenientes de los recursos de Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) para habilitar la configuración de diagnóstico para la instancia de Azure Digital Twins mediante el Portal, la CLI de Azure o PowerShell. Asegúrese de seleccionar todas las categorías de registro, las métricas y el área de trabajo de Azure Log Analytics.

### <a name="trace-sensor-telemetry"></a>Seguimiento de la telemetría del sensor

Asegúrese de que la configuración de diagnóstico está habilitada en la instancia de Azure Digital Twins, que estén seleccionadas todas las categorías de registro y que los registros se estén enviando a Azure Log Analytics.

Para hacer coincidir un mensaje de telemetría de sensor con sus registros correspondientes, puede especificar un identificador de correlación en los datos del evento que se envían. Para ello, establezca la propiedad `x-ms-client-request-id` en un GUID.

Después de enviar la telemetría, abra Azure Log Analytics para consultar los registros con el identificador de correlación configurado:

```Kusto
AzureDiagnostics
| where CorrelationId = 'yourCorrelationIdentifier'
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suIdentificadorDeCorrelación* | El identificador de correlación que se especificó en los datos del evento |

Si registra su función definida por el usuario, esos registros aparecerán en su instancia de Azure Log Analytics con la categoría `UserDefinedFunction`. Para recuperarlos, escriba la siguiente condición de consulta en Azure Log Analytics:

```Kusto
AzureDiagnostics
| where Category = 'UserDefinedFunction'
```

Para obtener más información acerca de estas eficaces operaciones de consulta, lea [Introducción a las consultas](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="identify-common-issues"></a>Identificación de los problemas comunes

Es importante tanto diagnosticar como identificar los problemas más comunes para buscar una solución. A continuación se resumen varios problemas comunes encontrados al desarrollar funciones definidas por el usuario.

### <a name="ensure-a-role-assignment-was-created"></a>Asegúrese de que se creó una asignación de roles

Sin no se crea una asignación de roles dentro de la API de administración, la función definida por el usuario no tendrá acceso para realizar acciones como enviar notificaciones, recuperar metadatos y configurar valores calculados dentro de la topología.

Compruebe si existe una asignación de roles para la función definida por el usuario a través de la API de administración:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/roleassignments?path=/&traverse=Down&objectId=yourUserDefinedFunctionId
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suIdDeFunciónDefinidaPorElUsuario* | El identificador de la función definida por el usuario para la que recuperar las asignaciones de roles|

Si no se recupera ninguna asignación de roles, siga este artículo en [How to create a role assignment for your user-defined function](./how-to-user-defined-functions.md) (Creación de una asignación de roles para su función definida por el usuario).

### <a name="check-if-the-matcher-will-work-for-a-sensors-telemetry"></a>Compruebe si funcionará el buscador de coincidencias para la telemetría de un sensor

Con la siguiente llamada a la API de administración de las instancias de Azure Digital Twins, podrá determinar si se aplica un buscador de coincidencias determinado para el sensor específico.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/matchers/yourMatcherIdentifier/evaluate/yourSensorIdentifier?enableLogging=true
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suIdentificadorDeBuscadorDeCoincidencias* | El identificador del buscador de coincidencias que quiere evaluar |
| *suIdentificadorDeSensor* | El identificador del sensor que quiere evaluar |

Respuesta:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-will-trigger"></a>Compruebe lo que desencadenará un sensor

Con la siguiente llamada a la API de administración de las instancias de Azure Digital Twins, podrá determinar los identificadores de las funciones definidas por el usuario que desencadenará la telemetría entrante del sensor determinado:

```plaintext
GET https://yourManagementApiUrl/api/v1.0/sensors/yourSensorIdentifier/matchers?includes=UserDefinedFunctions
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración  |
| *suIdentificadorDeSensor* | El identificador del sensor que enviará datos de telemetría |

Respuesta:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Problema con la recepción de notificaciones

Si no recibe notificaciones desde dentro de la función definida por el usuario desencadenada, asegúrese de que su parámetro de tipo de objeto de topología coincide con el tipo del identificador que se utiliza.

Ejemplo **incorrecto**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Esta situación se produce porque el identificador usado hace referencia a un sensor mientras el tipo de objeto de la topología especificado es "Espacio".

Ejemplo **correcto**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

La manera más fácil de no experimentar este problema es usar el método `Notify` en el objeto de metadatos.

Ejemplo:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Excepciones de diagnóstico comunes

Si habilita la configuración de diagnóstico, es posible que se produzcan estas excepciones comunes:

1. **Limitación**: la función definida por el usuario se restringirá si supera los límites de la tasa de ejecución descrita en el artículo [Service Limits](./concepts-service-limits.md) (Límites del servicio). La limitación implica que ninguna operación se ejecutará correctamente hasta que expiren los límites.

1. **No se encontraron datos**: si la función definida por el usuario intenta tener acceso a metadatos que no existen, se producirá un error en la operación.

1. **No autorizado**: si la función definida por el usuario no tiene una asignación de roles establecida o no tiene permisos suficientes para tener acceso a ciertos metadatos de la topología, se producirá un error en la operación.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo habilitar [la supervisión y los registros](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) en Azure Digital Twins.
