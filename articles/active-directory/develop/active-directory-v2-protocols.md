---
title: Más información acerca de los protocolos de autorización compatibles con Azure AD v2.0 | Microsoft Docs
description: Guía de los protocolos que admite el punto de conexión de Azure AD v2.0.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 4245cd4cf1f67007ced190e15d95929d854e303a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992752"
---
# <a name="v20-protocols---oauth-20--openid-connect"></a>Protocolos de v2.0: OAuth 2.0 y OpenID Connect

El punto de conexión v2.0 puede usar Azure AD para identidad como servicio con protocolos estándar del sector, OpenID Connect y OAuth 2.0. Aunque el servicio sea compatible con el estándar, puede haber diferencias sutiles entre dos implementaciones cualquiera de estos protocolos. La información que aquí se describe será útil si decide escribir su código mediante el envío y la administración directos de solicitudes HTTP o mediante el uso de una biblioteca de código abierto de terceros, en lugar de usar una de nuestras [bibliotecas de código abierto](reference-v2-libraries.md).

> [!NOTE]
> No todas las características y escenarios de Azure Active Directory son compatibles con el punto de conexión v2.0. Para determinar si debe utilizar el punto de conexión v2.0, lea acerca de las [limitaciones de v2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Conceptos básicos

En casi todos los flujos de OAuth y OpenID Connect hay cuatro partes implicadas en el intercambio:

![Funciones de OAuth 2.0](../../media/active-directory-v2-flows/protocols_roles.png)

* El **servidor de autorización** es el punto de conexión v2.0. Es responsable de garantizar la identidad del usuario, conceder y revocar el acceso a los recursos y emitir tokens. También se le conoce como proveedor de identidad: controla de forma segura todo lo que tenga que ver con la información del usuario, su acceso y las relaciones de confianza entre las partes de un flujo.
* El **propietario del recurso** suele ser el usuario final. Es la parte que posee los datos y tiene la capacidad de permitir que terceros tengan acceso a esos datos o recursos.
* El **cliente de OAuth** es su aplicación, identificada por su identificador de aplicación. Suele ser la parte con la que interactúa el usuario final y solicita tokens del servidor de autorización. El cliente debe contar con el permiso del propietario del recurso para acceder a este.
* El **servidor de recursos** es donde residen el recurso o los datos. Confía en el servidor de autorización para autenticar y autorizar al cliente de OAuth de forma segura y usa access_tokens de portador para garantizar que se puede conceder el acceso a un recurso.

## <a name="app-registration"></a>Registro de aplicaciones
Todas las aplicaciones que usen el punto de conexión v2.0 tendrán que registrarse en [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) para que puedan interactuar con el uso de OAuth o de OpenID Connect. El proceso de registro de la aplicación recopilará y asignará algunos valores a la aplicación:

* Un **Id. de aplicación** que identifica de forma única su aplicación
* Un **URI de redirección** o **identificador de paquete** que puede utilizarse para dirigir las respuestas de nuevo a la aplicación
* Algunos otros valores específicos de cada escenario.

Para obtener más información, aprenda a [registrar una aplicación](quickstart-v2-register-an-app.md).

## <a name="endpoints"></a>Puntos de conexión

Una vez registrada, la aplicación se comunica a Azure AD mediante el envío de solicitudes al extremo v2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Donde `{tenant}` puede adoptar uno de cuatro valores:

| Valor | DESCRIPCIÓN |
| --- | --- |
| `common` |Permite que los usuarios con cuentas personales de Microsoft y con cuentas profesionales o educativas de Azure Active Directory inicien sesión en la aplicación. |
| `organizations` |Permite que solo los usuarios con cuentas profesionales o educativas de Azure Active Directory inicien sesión en la aplicación. |
| `consumers` |Permite que solo los usuarios con cuentas personales de Microsoft (MSA) inicien sesión en la aplicación. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` o `contoso.onmicrosoft.com` |Permite que solo los usuarios con cuentas profesionales o educativas de un inquilino específico de Azure Active Directory inicien sesión en la aplicación. Puede usarse el nombre de dominio descriptivo del inquilino de Azure AD o bien el identificador de GUID del inquilino. |

Para obtener más información sobre cómo interactuar con estos puntos de conexión, elija un tipo de aplicación en particular a continuación.

## <a name="tokens"></a>Tokens

La implementación v2.0 de OAuth 2.0 y OpenID Connect hace un uso generalizado de tokens de portador, incluidos los representados como JWT. Un token portador es un token de seguridad ligero que concede al "portador" acceso a un recurso protegido. En este sentido, el "portador" es cualquier parte que pueda presentar el token. Aunque una parte debe autenticarse primero con Azure AD para recibir el token portador, si no se realizan los pasos necesarios para asegurar el token en la transmisión y el almacenamiento, este puede interceptarse y ser utilizado por un usuario no deseado. Mientras que algunos tokens de seguridad disponen de un mecanismo integrado para evitar ser usados por partes no autorizadas, los tokens portadores no tienen este mecanismo y deben transportarse en un canal seguro como, por ejemplo, la seguridad de la capa de transporte (HTTPS). Si un token portador se transmite sin cifrar, un usuario malintencionado puede utilizar un ataque de tipo "Man in the middle" para adquirir el token y usarlo para obtener acceso sin autorización a un recurso protegido. Los mismos principios de seguridad se aplican al almacenamiento o almacenamiento en caché de tokens portadores para su uso posterior. Asegúrate siempre de que la aplicación transmite y almacena los tokens de portador de manera segura. Para otras consideraciones sobre la seguridad de los tokens portadores, consulte la [Sección 5 de RFC 6750](http://tools.ietf.org/html/rfc6750).

Encontrará más detalles sobre los diferentes tipos de token que se usan en el punto de conexión v2.0 en [la referencia de token del punto de conexión v2.0](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protocolos

Si está listo para ver algunas solicitudes de ejemplo, comience con uno de los siguiente tutoriales. Cada uno de ellos corresponde a un escenario de autenticación determinado. Si necesita ayuda para determinar cuál es el flujo correcto para usted, vea [los tipos de aplicaciones que puede compilar con v2.0](v2-app-types.md).

* [Creación de aplicaciones móviles y nativas con OAuth 2.0](v2-oauth2-auth-code-flow.md)
* [Creación de aplicaciones web con OpenID Connect](v2-protocols-oidc.md)
* [Creación de aplicaciones de una sola página con el flujo implícito de OAuth 2.0](v2-oauth2-implicit-grant-flow.md)
* [Creación de demonios o procesos del lado servidor con el flujo de credenciales de cliente de OAuth 2.0](v2-oauth2-client-creds-grant-flow.md)
* [Obtención de tokens en una API web con el flujo "en nombre de" de OAuth 2.0](v2-oauth2-on-behalf-of-flow.md)
