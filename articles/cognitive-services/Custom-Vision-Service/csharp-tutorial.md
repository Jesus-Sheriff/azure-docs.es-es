---
title: 'Tutorial: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para C#'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción mediante el punto de conexión predeterminado.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: e046fe452a13384ae7929be805c6252d6ad2fbf9
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953050"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-c"></a>Tutorial: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para C#

Aprenda a usar el SDK de Custom Vision Service en una aplicación de C#. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Use este ejemplo de código abierto como plantilla para compilar su propia aplicación para Windows con la API de Custom Vision Service.

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de Visual Studio 2017 para Windows.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Obtener el SDK y ejemplos de Custom Vision
Para compilar este ejemplo, necesita los paquetes NuGet del SDK de Custom Vision:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Puede descargar las imágenes con los [ejemplos de C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision).

## <a name="get-the-training-and-prediction-keys"></a>Obtener las claves de entrenamiento y predicción

Para obtener las claves que se utilizan en este ejemplo, visite la [página web de Custom Vision](https://customvision.ai) y seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, copie los valores de los campos __Training Key__ (Clave de entrenamiento) y __Prediction Key__ (Clave de predicción).

![Imagen de la interfaz de usuario de claves](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Comprensión del código

En Visual Studio, abra el proyecto que se encuentra en el directorio `Samples/CustomVision.Sample/` del proyecto del SDK.

Esta aplicación utiliza la clave de entrenamiento que recuperó anteriormente para crear un nuevo proyecto denominado __My New Project__. A continuación, carga las imágenes para entrenar y probar un clasificador. El clasificador identifica si el árbol es un __abeto canadiense__ o un __cerezo japonés__.

Los siguientes fragmentos de código implementan la funcionalidad principal de este ejemplo:

* __Creación de un proyecto de Custom Vision Service__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Creación de etiquetas en un proyecto__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Carga y etiquetado de imágenes__:

    ```csharp
    // Add some images to the tags
    Console.WriteLine("\tUploading images");
    LoadImagesFromDisk();

    // Images can be uploaded one at a time
    foreach (var image in hemlockImages)
    {
        using (var stream = new MemoryStream(File.ReadAllBytes(image)))
        {
            trainingApi.CreateImagesFromData(project.Id, stream, new List<string>() { hemlockTag.Id.ToString() });
        }
    }

    // Or uploaded in a single batch 
    var imageFiles = japaneseCherryImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
    trainingApi.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(imageFiles, new List<Guid>() { japaneseCherryTag.Id }));
    ```

* __Entrenamiento del clasificador__:

    ```csharp
    // Now there are images with tags start training the project
    Console.WriteLine("\tTraining");
    var iteration = trainingApi.TrainProject(project.Id);

    // The returned iteration will be in progress, and can be queried periodically to see when it has completed
    while (iteration.Status == "Completed")
    {
        Thread.Sleep(1000);

        // Re-query the iteration to get it's updated status
        iteration = trainingApi.GetIteration(project.Id, iteration.Id);
    }
    ```

* __Establecimiento de una iteración predeterminada para el punto de conexión de predicción__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Creación de un punto de conexión de predicción__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Envío de una imagen al punto de conexión de predicción__:

    ```csharp
    // Make a prediction against the new project
    Console.WriteLine("Making a prediction:");
    var result = endpoint.PredictImage(project.Id, testImage);

    // Loop over each prediction and write out the results
    foreach (var c in result.Predictions)
    {
        Console.WriteLine($"\t{c.TagName}: {c.Probability:P1}");
    }
    ```

## <a name="run-the-application"></a>Ejecución de la aplicación

1. Realice los cambios siguientes para agregar las claves de aprendizaje y predicción a la aplicación:

    * Agregue su __clave de entrenamiento__ a la línea siguiente:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Agregue su __clave de predicción__ a la línea siguiente:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Ejecute la aplicación. Mientras se ejecuta la aplicación, se escribe la siguiente salida en la consola:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Para salir de la aplicación, presione cualquier tecla.
