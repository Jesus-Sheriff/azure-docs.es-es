---
title: 'Uso del control de versiones y el etiquetado con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar el control de versiones y el etiquetado con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c7f23d989cbfa0ece9e404a0fe0feb68cf5fddb2
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170552"
---
# <a name="how-to-use-versioning-and-tagging"></a>Uso del control de versiones y el etiquetado

En este tutorial se ilustra cómo etiquetar versiones del modelo de Conversation Learner y definir qué versión está "activa".  

## <a name="requirements"></a>Requisitos
Este tutorial requiere usar el emulador de bots para crear diálogos de registro, no la interfaz de usuario web de diálogos de registros.  

Para poder realizar este tutorial debe ejecutar el bot de tutorial general:

    npm run tutorial-general

## <a name="details"></a>Detalles

Al editar, siempre edita la etiqueta denominada "principal"; puede crear versiones etiquetadas a partir de la principal (que básicamente realiza una instantánea de la principal), pero no puede editar versiones etiquetadas.

## <a name="steps"></a>Pasos

### <a name="install-the-bot-framework-emulator"></a>Instalación de Bot Framework Emulator

- Vaya a [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Descargue e instale el emulador.

### <a name="create-an-model"></a>Creación de un modelo

1. Haga clic en New Modelo (Modelo nuevo)
2. En el campo Nombre, escriba Tutorial-16-Versioning.
3. Click Create 
4. Hacer clic en Configuración
5. Copie el identificador del modelo

### <a name="configure-the-emulator"></a>Configuración del emulador

- En la carpeta raíz de Conversation Learner, abra el archivo .env.
- Pegue el identificador del modelo como el valor de CONVERSATION_LEARNER_MODEL_ID
- Reinicie el servicio Conversation Learner cerrando el símbolo del sistema y volviendo a ejecutar:
 
    npm run tutorial-general 

### <a name="actions"></a>Acciones

Se va a crear una acción:

1. Cambie a la interfaz de usuario web.
1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Respuesta, escriba “hi there (version 1)” [Hola, (versión 1)].
3. Haga clic en Guardar.


![](../media/tutorial16_action1.PNG)

Cree una etiqueta:

3. Haga clic en “Configuración” y cree una “etiqueta”.
    - Asígnele el nombre "versión 1".
4. Establezca "versión 1" en "activa".  
    - El efecto de configurar la etiqueta activa en la "versión 1" es que los canales que usan este bot utilizarán la etiqueta "versión 1".
    - Las ediciones (modificación de acciones, entidades e incorporación de diálogos de entrenamiento) no afectan a las versiones etiquetadas de los modelos.  
    - Las ediciones de un modelo (modificación de acciones, entidades y adición de diálogos de entrenamiento) siempre se realizan en la etiqueta "principal".  En otras palabras, la etiqueta "principal" es la única que se puede modificar; las demás etiquetas son instantáneas fijas.
    - Los diálogos del registro de la interfaz de usuario de Conversation Learner siempre usan la etiqueta principal (no la etiqueta activa).

![](../media/tutorial16_v1_create.PNG)

La versión se creó en la configuración:

![](../media/tutorial16_settings.PNG)

Se va a agregar una segunda acción:

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Respuesta, escriba “bye bye (version 2)” [Adiós, (versión 2)].

Edite la primera acción:

1. Haga clic en Acciones.
2. En Acciones, haga clic en "hi there (version 1)" [Hola, (versión 1)].
3. Cambie la respuesta a "hi there (version 2)" [Hola, (versión 2)].

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Cambio al emulador de bot

1. En la interfaz de usuario del bot, escriba "goodbye" (adiós).
2. El bot responde "hi there (version 1)" [Hola, (versión 1)].
    - Muestra que la versión 1 está "activa". 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Cambio a la interfaz de usuario web

1. Haga clic en los cuadros de diálogo de registro (si no ve los cuadros de diálogo, haga clic en el botón Actualizar).
2. Haga clic en "hi there (version 2)" [Hola, (versión 2)].

> [!NOTE]
> Podemos hacer correcciones al elegir entre todas las acciones disponibles actualmente. Estas modificaciones se realizarán en la etiqueta principal.

Ya ha visto cómo funciona el control de versiones y cómo puede interactuar con el bot mediante Bot Framework Emulator.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Demostración: restablecimiento de contraseña](./demo-password-reset.md)
