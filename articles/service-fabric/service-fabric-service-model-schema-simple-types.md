---
title: Tipos simples del esquema XML del modelo de servicio de Azure Service Fabric | Microsoft Docs
description: Describe los tipos simples del esquema XML del modelo de servicio de Service Fabric.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/06/2018
ms.author: ryanwi
ms.openlocfilehash: 385d950498730af6c665270111f0206cda1c7315
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809223"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>Tipos simples del esquema XML del modelo de servicio

## <a name="fabricuri-simpletype"></a>FabricUri simpleType
URI que el sistema de nomenclatura de Microsoft Azure Service Fabric utiliza como identificador estable de servicios. 

### <a name="xml-source"></a>Origen XML
```xml
<xs:simpleType xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
