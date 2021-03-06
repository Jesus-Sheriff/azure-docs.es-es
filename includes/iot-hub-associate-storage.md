---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/20/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 8aa4695ea1175fe9d558e02bae661c9610123299
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43086395"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Asociación de una cuenta de Azure Storage a IoT Hub

Como la aplicación de dispositivo simulado carga un archivo en un blob, debe tener una cuenta de [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md) asociada a IoT Hub. Al asociar una cuenta de Azure Storage a un centro de IoT Hub, el centro genera un URI de SAS. Un dispositivo puede usar este URI de SAS para cargar de forma segura un archivo en un contenedor de blobs. El servicio IoT Hub y los SDK de dispositivo coordinan el proceso que genera el URI de SAS y permite que esté disponible para un dispositivo para cargar un archivo.

Siga las instrucciones de [Configuración de cargas de archivos mediante Azure Portal](../articles/iot-hub/iot-hub-configure-file-upload.md) para asociar una cuenta de Azure Storage al centro de IoT Hub. Asegúrese de que hay un contenedor de blobs asociado a su centro de IoT Hub y que las notificaciones de archivo están habilitadas.

![Habilitación de las notificaciones de archivo en el Portal](./media/iot-hub-associate-storage/enable-file-notifications.png)