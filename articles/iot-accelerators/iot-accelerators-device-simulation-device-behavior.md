---
title: 'Comportamiento de un dispositivo simulado en la solución de simulación de dispositivo: Azure | Microsoft Docs'
description: En este artículo se describe cómo usar JavaScript para definir el comportamiento de un dispositivo simulado en la solución de simulación de dispositivo.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 43edbc653ddbd55aab5e722071de1f2cf4bcd1c4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344523"
---
# <a name="implement-the-device-model-behavior"></a>Implementación del comportamiento de modelo del dispositivo

El artículo [Descripción del esquema de modelo del dispositivo](iot-accelerators-device-simulation-device-schema.md) describe el esquema que define un modelo de dispositivo simulado. Ese artículo hace referencia a dos tipos de archivo de JavaScript que implementan el comportamiento de un dispositivo simulado:

- **Estado**: archivos JavaScript que se ejecutan a intervalos fijos para actualizar el estado interno del dispositivo.
- **Método**: archivos JavaScript que se ejecutan cuando la solución invoca un método en el dispositivo.

En este artículo, aprenderá a:

>[!div class="checklist"]
> * Controlar el estado de un dispositivo simulado
> * Definir cómo responde un dispositivo simulado a una llamada de método del servicio IoT Hub al que está conectado
> * Depurar los scripts

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe cómo definir el comportamiento de su propio modelo de dispositivo simulado personalizado. En este artículo le mostramos cómo:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Controlar el estado de un dispositivo simulado
> * Definir cómo responde un dispositivo simulado a una llamada de método del servicio IoT Hub al que está conectado
> * Depurar los scripts

Ahora que ha aprendido a especificar el comportamiento de un dispositivo simulado, el siguiente paso que se propone es aprender a [crear un dispositivo simulado](iot-accelerators-device-simulation-create-simulated-device.md).

Si busca más información para desarrolladores sobre la solución de simulación de dispositivo, vea la [Guía de referencia para desarrolladores](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
