---
title: Migrar de Custom Speech Service al servicio Voz
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre la migración de Custom Speech Service al servicio Voz.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: 656a5df21d1f7dd2e7662dc3a0415581eae02b00
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471244"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrar de Custom Speech Service al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Custom Speech Service al servicio Voz.

Custom Speech Service ahora forma parte del servicio Voz. Cambie al servicio Voz para aprovechar las actualizaciones más recientes de calidad y características.
 
## <a name="migration-for-new-customers"></a>Migración para los nuevos clientes

El modelo de precios es más sencillo, al pasar a un modelo de precios por hora para el servicio Voz.   

1. Cree un recurso de Azure en cada región donde la aplicación esté disponible. El nombre del recurso de Azure es **Voz**. Puede usar un único recurso de Azure para los siguientes servicios en la misma región, en lugar de crear recursos independientes:

    * Voz a texto
    * Conversión de voz a texto personalizada
    * Texto a voz
    * Traducción de voz

2. Descargue el [SDK de Voz](speech-sdk.md). 

3. Siga las guías de inicio rápido y los ejemplos del SDK para usar las API correctas. Si usa las API de REST, también debe usar los puntos de conexión y las claves de recursos correctos. 

4. Actualice la aplicación cliente para usar las API y el servicio Voz. 

> [!NOTE]
> * LUIS: si habilitó la voz en Language Understanding (LUIS), un único recurso de LUIS en la misma región funcionará para LUIS, así como todos los servicios de voz. Consulte la documentación de [Reconocimiento de intenciones a partir de contenido de voz](how-to-recognize-intents-from-speech-csharp.md).
> * La traducción de texto a texto no forma parte del servicio Voz. Necesita su propia suscripción a recursos de Azure.
  


## <a name="migration-for-existing-customers"></a>Migración para los clientes actuales

Los clientes existentes tienen que migrar sus claves de recursos existentes con el servicio Voz en el portal del servicios Voz. Para ello, siga los pasos que se describen a continuación: 

> [!NOTE] 
> Las claves de recursos solo se pueden migrar en la misma región. 

1. Inicie sesión en el portal [cris.ai](http://www.cris.ai) y seleccione la suscripción en el menú superior derecho. 

2. Seleccione **Migrate selected subscription** (Migrar suscripción seleccionada).

3. Escriba la clave de suscripción en el cuadro de texto y seleccione **Migrar**.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita del servicio Voz](get-started.md)
* Obtenga información sobre los conceptos de [convertir voz en texto](./speech-to-text.md)

## <a name="see-also"></a>Otras referencias

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación del servicio Voz y del SDK correspondiente](speech-sdk.md#get-the-sdk)