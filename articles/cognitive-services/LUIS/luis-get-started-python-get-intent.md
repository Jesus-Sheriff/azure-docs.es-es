---
title: 'Inicio rápido de Python: Predicción de la intención en LUIS'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, aprenderá a llamar a una aplicación de LUIS mediante Python.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: e560aeffecf63f63966a49053e0f79d012b4a0a3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038279"
---
# <a name="quickstart-get-intent-using-python"></a>Inicio rápido: Obtención de la intención con Python
En esta guía de inicio rápido, pasará expresiones a un punto de conexión de LUIS y obtendrá entidades e intenciones.

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obtención de la intención con el explorador

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Obtención de la intención mediante programación

Puede usar Python para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior.

1. Copie uno de los siguientes fragmentos de código en un archivo denominado `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Sustituya el valor del campo `Ocp-Apim-Subscription-Key` por su clave de punto de conexión de LUIS.

3. Instale las dependencias con `pip install requests`.

4. Ejecute el script con `python ./quickstart-call-endpoint.py`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

## <a name="luis-keys"></a>Claves de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos
Elimine el archivo de python. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-python-add-utterance.md)