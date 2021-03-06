---
title: 'Idiomas admitidos: Bing Custom Search API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas y las regiones admitidos para Bing Custom Search API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: v-gedod
ms.openlocfilehash: 8fa70705d1dff5c3150fa5868938a3c50ef863fe
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815570"
---
# <a name="language-and-region-support-for-the-bing-custom-search-api"></a>Compatibilidad de idiomas y regiones para Bing Custom Search API

Bing Custom Search API admite más de 36 países o regiones, muchos de ellos con más de un idioma.

Aunque es opcional, la solicitud debe especificar el parámetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt), que identifica el mercado de donde desea que procedan los resultados. Para ver una lista de parámetros de consulta opcionales, consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) (Parámetros de consulta).

Puede especificar un país o región utilizando el parámetro de consulta `cc`. Si especifica un país o región, también debe especificar uno o más códigos de idioma con el encabezado `Accept-Language`. Los idiomas admitidos varían según el país o región; se facilitan para cada país en la tabla **Mercados**.

El encabezado `Accept-Language` y el parámetro de consulta `setLang` son mutuamente excluyentes, no especifique ambos. Para obtener más información, consulte [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

## <a name="countries"></a>Países

|País/región|Código|
|-------|----|
|Argentina|AR|
|Australia|AU|
|Austria|AT|
|Bélgica|BE|
|Brasil|BR|
|Canadá|CA|
|Chile|CL|
|Dinamarca|DK|
|Finlandia|FI|
|Francia|VF|
|Alemania|DE|
|Hong Kong|HK|
|India|IN|
|Indonesia|ID|
|Italia|IT|
|Japón|JP|
|Corea|KR|
|Malasia|MY|
|México|MX|
|Países Bajos|NL|
|Nueva Zelanda|NZ|
|Noruega|NO|
|China|CN|
|Polonia|PL|
|Portugal|PT|
|Filipinas|PH|
|Rusia|RU|
|Arabia Saudí|SA|
|Sudáfrica|ZA|
|España|ES|
|Suecia|SE|
|Suiza|CH|
|Taiwán|TW|
|Turquía|TR|
|Reino Unido|GB|
|Estados Unidos|US|


## <a name="markets"></a>Mercados

|País/región|Idioma|Código de mercado|
|-------|--------|-----------|
|Argentina|Español|es-AR|
|Australia|English|en-AU|
|Austria|Alemán|de-AT|
|Bélgica|Neerlandés|nl-BE|
|Bélgica|Francés|fr-BE|
|Brasil|Portugués|pt-BR|
|Canadá|English|en-CA|
|Canadá|Francés|fr-CA|
|Chile|Español|es-CL|
|Dinamarca|Danés|da-DK|
|Finlandia|Finés|fi-FI|
|Francia|Francés|fr-FR|
|Alemania|Alemán|de-DE|
|Hong Kong|Chino tradicional|zh-HK|
|India|English|en-IN|
|Indonesia|English|en-ID|
|Italia|Italiano|it-IT|
|Japón|Japonés|ja-JP|
|Corea|Coreano|ko-KR|
|Malasia|English|en-MY|
|México|Español|es-MX|
|Países Bajos|Neerlandés|nl-NL|
|Nueva Zelanda|English|en-NZ|
|Noruega|Noruego|no-NO|
|China|Chino|zh-CN|
|Polonia|Polaco|pl-PL|
|Portugal|Portugués|pt-PT|
|Filipinas|English|en-PH|
|Rusia|Ruso|ru-RU|
|Arabia Saudí|Árabe|ar-SA|
|Sudáfrica|English|en-ZA|
|España|Español|es-ES|
|Suecia|Sueco|sv-SE|
|Suiza|Francés|fr-CH|
|Suiza|Alemán|de-CH|
|Taiwán|Chino tradicional|zh-TW|
|Turquía|Turco|tr-TR|
|Reino Unido|English|en-GB|
|Estados Unidos|English|es-ES|
|Estados Unidos|Español|es-US|
