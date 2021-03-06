---
title: Recepción de eventos desde Azure Event Hubs mediante .NET Framework | Microsoft Docs
description: Siga este tutorial para recibir eventos desde Azure Event Hubs mediante .NET Framework.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 9e94357216690438446a738400c979d12f387df6
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471091"
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>Recepción de eventos de Azure Event Hubs mediante .NET Framework

## <a name="introduction"></a>Introducción

Event Hubs es un servicio que procesa grandes cantidades de datos de eventos (telemetría) desde aplicaciones y dispositivos conectados. Después de recopilar datos en Event Hubs, puede almacenarlos mediante un clúster de almacenamiento o transformarlos por medio de un proveedor de análisis en tiempo real. Esta funcionalidad de recopilación y procesamiento de eventos a gran escala es un componente clave de las modernas arquitecturas de aplicaciones, entre las que se incluye Internet de las cosas (IoT). Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En este tutorial se muestra cómo escribir una aplicación de consola de .NET Framework que recibe mensajes de un centro de eventos mediante el [host de procesador de eventos](event-hubs-event-processor-host.md). El [host de procesador de eventos](event-hubs-event-processor-host.md) es una clase de .NET que simplifica la recepción de eventos desde Event Hubs mediante la administración de puntos de control persistentes y recepciones paralelas desde dichas instancias de Event Hubs. Mediante el host de procesador de eventos, puede dividir eventos entre varios receptores, aunque estén hospedados en distintos nodos. En este ejemplo se muestra cómo usar el host de procesador de eventos en un solo receptor. En el ejemplo de [escalado horizontal del procesamiento de eventos][Scale out Event Processing with Event Hubs] se muestra cómo usar el host de procesador de eventos con varios receptores.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* [Microsoft Visual Studio 2017, o una versión superior](http://visualstudio.com).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos
El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento de [este artículo](event-hubs-create.md) y después continúe con los pasos siguientes de este tutorial.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Creación de una aplicación de consola

En Visual Studio, cree un nuevo proyecto de aplicación de escritorio de Visual C# con la plantilla de proyecto **Aplicación de consola**. Asigne al proyecto el nombre **Receptor**.
   
![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)

## <a name="add-the-event-hubs-nuget-package"></a>Incorporación del paquete NuGet de Event Hubs

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **Receptor** y luego haga clic en **Administrar paquetes NuGet para la solución**.
2. Haga clic en la pestaña **Examinar** y luego busque `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Haga clic en **Instalar**y acepte las condiciones de uso.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio descarga, instala y agrega una referencia al [paquete de NuGet de Azure Service Bus - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), con todas sus dependencias.

## <a name="implement-the-ieventprocessor-interface"></a>Implementación de la interfaz de IEventProcessor

1. Haga clic con el botón derecho en el proyecto **Receptor**, haga clic en **Agregar** y, después, haga clic en **Clase**. Asigne a la nueva clase el nombre **SimpleEventProcessor** y después haga clic en **Agregar** para crear la clase.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
2. Agregue las siguientes instrucciones en la parte superior del archivo SimpleEventProcessor.cs:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      using System.Diagnostics;
      ```
    
3. Sustituya el código siguiente por el cuerpo de la clase:
    
      ```csharp
      class SimpleEventProcessor : IEventProcessor
      {
        Stopwatch checkpointStopWatch;
        
        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }
        
        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }
        
        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
        
                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }
        
            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
      }
      ```
    
      **EventProcessorHost** llama a esta clase para procesar los eventos recibidos del centro de eventos. La clase `SimpleEventProcessor` usa un cronómetro para llamar periódicamente al método de punto de control en el contexto **EventProcessorHost**. Este procesamiento garantiza que, si se reinicia el destinatario, no se pierden más de cinco minutos de trabajo de procesamiento.

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>Actualización del método Main para usar SimpleEventProcessor

1. En la clase **Program.cs**, agregue la siguiente instrucción `using` al principio del archivo:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      ```
    
2. Reemplace el método `Main` de la clase `Program` por el código siguiente, y sustituya el nombre del centro de eventos y la cadena de conexión de nivel del espacio de nombres que ha guardado anteriormente, y la cuenta de almacenamiento y la clave que ha copiado en las secciones anteriores. 
    
      ```csharp
      static void Main(string[] args)
      {
        string eventHubConnectionString = "{Event Hubs namespace connection string}";
        string eventHubName = "{Event Hub name}";
        string storageAccountName = "{storage account name}";
        string storageAccountKey = "{storage account key}";
        string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
        
        string eventProcessorHostName = Guid.NewGuid().ToString();
        EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
        Console.WriteLine("Registering EventProcessor...");
        var options = new EventProcessorOptions();
        options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
        eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
        
        Console.WriteLine("Receiving. Press enter key to stop worker.");
        Console.ReadLine();
        eventProcessorHost.UnregisterEventProcessorAsync().Wait();
      }
      ```
    
3. Ejecute el programa y asegúrese de que no hay ningún error.
  
Felicidades. Ahora ha recibido mensajes de un centro de eventos por medio del host de procesador de eventos.


> [!NOTE]
> Este tutorial usa una sola instancia de [EventProcessorHost](event-hubs-event-processor-host.md). Para aumentar el rendimiento, se recomienda ejecutar varias instancias de [EventProcessorHost](event-hubs-event-processor-host.md) como se muestra en el ejemplo [Scaled out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) (Escalado horizontal del procesamiento de eventos). En esos casos, las diferentes instancias se coordinan automáticamente entre sí con el fin de equilibrar la carga de los eventos recibidos. 

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado la aplicación de .NET Framework que recibe mensajes de un centro de eventos. Para saber cómo enviar eventos a un centro de eventos mediante .NET Framework, consulte [Envío de eventos a Azure Event Hubs mediante .NET Framework](event-hubs-dotnet-framework-getstarted-send.md).

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Event Hubs overview]: event-hubs-about.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: event-hubs-event-processor-host.md
[Azure portal]: https://portal.azure.com
