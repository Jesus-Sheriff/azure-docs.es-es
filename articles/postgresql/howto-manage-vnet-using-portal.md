---
title: Creación y administración de reglas y puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante Azure Portal | Microsoft Docs
description: Creación y administración de reglas y puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante Azure Portal
services: postgresql
author: mbolz
ms.author: mbolz
ms.reviewer: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/15/2018
ms.openlocfilehash: 3884b594c903c234ce83c0752f92ddc34b24cd5a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239326"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Creación y administración de reglas y puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL mediante Azure Portal
Las reglas y los puntos de conexión de servicios de red virtual (VNet) amplían el espacio de direcciones privadas de una red virtual al servidor de Azure Database for PostgreSQL. Para obtener información general sobre los puntos de conexión de servicio de red virtual de Azure Database for PostgreSQL, incluidas las limitaciones, consulte [Azure Database for PostgreSQL Server VNet service endpoints](concepts-data-access-and-security-vnet.md) (Puntos de conexión de servicio de red virtual del servidor de Azure Database for PostgreSQL). Los puntos de conexión de servicio de red virtual están disponibles en todas las regiones admitidas para Azure Database for PostgreSQL.

> [!NOTE]
> La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Creación de una regla de red virtual y habilitación de los puntos de conexión de servicio en Azure Portal

1. En la página del servidor de PostgreSQL, en el encabezado Configuración, haga clic en **Seguridad de conexión** para abrir la página de seguridad de conexión para Azure Database for PostgreSQL. A continuación, haga clic en **+ Agregar una red virtual existente**. Si no tiene ninguna red virtual, puede hacer clic en **+ Crear nueva red virtual** para crear una. Consulte [Guía de inicio rápido: Creación de una red virtual mediante Azure Portal](../virtual-network/quick-create-portal.md)

   ![Azure Portal: haga clic en Seguridad de conexión](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Escriba un nombre de regla de red virtual, seleccione la suscripción, el nombre de la subred y red virtual y, luego, haga clic en **Habilitar**. Esto habilita automáticamente puntos de conexión de servicio de red virtual en la subred mediante la etiqueta de servicio **Microsoft.SQL**.

   ![Azure Portal: configuración de red virtual](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   > [!IMPORTANT]
   > Se recomienda encarecidamente leer este artículo sobre las configuraciones y las consideraciones de puntos de conexión de servicio antes de configurarlos. **Punto de conexión de servicio de Virtual Network:** un [punto de conexión de servicio de Virtual Network](../virtual-network/virtual-network-service-endpoints-overview.md) es una subred cuyos valores de propiedad incluyen uno o más nombres formales de tipo de servicio de Azure. Los puntos de conexión de servicio de red virtual usan el nombre de tipo de servicio **Microsoft.Sql**, que hace referencia al servicio de Azure denominado SQL Database. Esta etiqueta de servicio también se aplica a los servicios Azure SQL Database, Azure Database for PostgreSQL y MySQL. Es importante tener en cuenta que, al aplicar la etiqueta de servicio **Microsoft.Sql** a un punto de conexión de servicio de red virtual, se configura el tráfico de punto de conexión de servicio de todos los servicios de Azure Database, incluidos los servidores de Azure SQL Database, Azure Database for PostgreSQL y Azure Database for MySQL de la subred. 
   > 

3. Una vez habilitada, haga clic en **Aceptar** y verá que se habilitan los puntos de conexión de servicio de red virtual junto con una regla de red virtual.

   ![Se han habilitado los puntos de conexión de servicio de red virtual y se ha creado la regla de red virtual](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Pasos siguientes
- De forma similar, puede crear scripts para [habilitar puntos de conexión de servicio de red virtual y crear una regla de red virtual para Azure Database for PostgreSQL mediante la CLI de Azure](howto-manage-vnet-using-cli.md).
- Para obtener ayuda para la conexión a un servidor de Azure Database for PostgreSQL, consulte [Bibliotecas de conexión para Azure Database for PostgreSQL](./concepts-connection-libraries.md).
