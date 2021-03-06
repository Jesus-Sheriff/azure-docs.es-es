---
title: Ampliación de la simultaneidad en servicios web de Azure Machine Learning | Microsoft Docs
description: Aprenda a ampliar la simultaneidad en los servicios web de Azure Machine Learning incorporando puntos de conexión adicionales.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
keywords: aprendizaje automático de azure, servicios web, operacionalización, escalado, punto de conexión, simultaneidad
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.component: studio
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.openlocfilehash: 2f950d93c0d923e20451eb1622dd4b1393f343a7
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835906"
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Escalado de un servicio web de Azure Machine Learning mediante la incorporación de puntos de conexión adicionales
> [!NOTE]
> En este tema se describen técnicas que se aplican a un servicio web Machine Learning **clásico**. 
> 
> 

De manera predeterminada, cada servicio web publicado está configurado para admitir 20 solicitudes simultáneas y 200 solicitudes simultáneas como máximo. Azure Machine Learning optimiza automáticamente este valor con el fin de brindar el mejor rendimiento al servicio web, por lo que se omite el valor del portal. 

Si tiene previsto llamar a la API con una carga mayor que un máximo de 200 llamadas simultáneas, debe crear varios puntos de conexión en el mismo servicio web. Acto seguido, podrá distribuir aleatoriamente la carga entre todos ellos.

El escalado de un servicio web es una tarea común. Algunos de los motivos para ello son admitir más de 200 solicitudes simultáneas, aumentar la disponibilidad a través de varios puntos de conexión u ofrecer puntos de conexión independientes para el servicio web. Para aumentar la escala, puede agregar más puntos de conexión para el mismo servicio web mediante el portal [Servicios web Azure Machine Learning](https://services.azureml.net/).

Para obtener más información sobre la incorporación de puntos de conexión nuevos, consulte [Creación de puntos de conexión](create-endpoint.md).

Tenga en cuenta que usar un recuento de simultaneidad alto puede ser perjudicial si no se llama a la API con una tasa elevada en consecuencia. Puede que vea tiempos de espera esporádicos o picos de latencia si pone una carga relativamente baja en una API configurada para una carga elevada.

Las API sincrónicas se usan normalmente en situaciones en las que se desea una latencia baja. Latencia aquí implica el tiempo que tarda la API en completar una solicitud, sin contar los retrasos de red. Supongamos que tiene una API con una latencia de 50 ms. Para utilizar totalmente la capacidad disponible con nivel de limitación alto y un número máximo de llamadas simultáneas de 20, debe llamar a esta API 20 * 1000/50 = 400 veces por segundo. Al ampliar esto aún más, un número máximo de llamadas simultáneas de 200 le permitirá llamar a la API 4000 veces por segundo, si presuponemos que la latencia es de 50 ms.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
