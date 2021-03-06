---
title: 'Tutorial: Integración de Azure Active Directory con Citrix ShareFile | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Citrix ShareFile.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 9919be128ae651b589a37f957cc59ce6d171143f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431739"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Tutorial: Integración de Azure Active Directory con Citrix ShareFile

En este tutorial, aprenderá a integrar Citrix ShareFile con Azure Active Directory (Azure AD).

La integración de Citrix ShareFile con Azure AD le proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Citrix ShareFile.
- Puede permitir que los usuarios inicien sesión automáticamente en Citrix ShareFile (Inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Citrix ShareFile, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Citrix ShareFile

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Citrix ShareFile desde la galería
1. Configuración y prueba del inicio de sesión único en Azure AD

## <a name="add-citrix-sharefile-from-the-gallery"></a>Incorporación de Citrix ShareFile desde la galería
Para configurar la integración de Citrix ShareFile en Azure AD, deberá agregar Citrix ShareFile desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Citrix ShareFile desde la galería, realice los siguientes pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Citrix ShareFile**, seleccione **Citrix ShareFile** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Citrix ShareFile en la lista de resultados](./media/sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Citrix ShareFile con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo en Citrix ShareFile de un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Citrix ShareFile.

Para establecer la relación de vínculo, en Citrix ShareFile, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Citrix ShareFile, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Citrix ShareFile ](#create-a-citrix-sharefile-test-user)**: para tener un homólogo de Britta Simon en Citrix ShareFile que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Citrix ShareFile.

**Para configurar el inicio de sesión único de Azure AD con Citrix ShareFile, realice los siguientes pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Citrix ShareFile**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/sharefile-tutorial/tutorial_sharefile_samlbase.png)

1. En la sección **Dominio y direcciones URL de Citrix ShareFile**, lleve a cabo los siguientes pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Citrix ShareFile](./media/sharefile-tutorial/tutorial_sharefile_url.png)
    
    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.sharefile.com/saml/login`.

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|
    | |

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Citrix ShareFile](https://www.citrix.co.in/products/sharefile/support.html) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/sharefile-tutorial/tutorial_sharefile_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/sharefile-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Citrix ShareFile**, haga clic en **Configurar Citrix ShareFile** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Citrix ShareFile](./media/sharefile-tutorial/tutorial_sharefile_configure.png)

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de **Citrix ShareFile** .

1. En la barra de herramientas de la parte superior, haga clic en **Administración**.

1. En el panel de navegación izquierdo, haga clic en **Configurar inicio de sesión único**.
   
    ![Administración de cuentas](./media/sharefile-tutorial/ic773627.png "Administración de cuentas")

1. En la página del cuadro de diálogo **Configuración de inicio de sesión único/SAML 2.0** en **Configuración básica**, realice los pasos siguientes:
   
    ![Inicio de sesión único](./media/sharefile-tutorial/ic773628.png "Inicio de sesión único")
   
    a. Haga clic en **Enable SAML**(Habilitar SAML).
    
    b. En el cuadro de texto **Your IDP Issuer/ Entity ID** (Emisor del IDP/Id. de entidad), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que copió de Azure Portal.

    c. Haga clic en **Cambiar** junto al campo **Certificado X.509** y luego cargue el certificado que descargó desde Azure Portal.
    
    d. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.
    
    e. En el cuadro de texto **Dirección URL de cierre de sesión**, pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

1. Haga clic en **Guardar** en el portal de administración de Citrix ShareFile.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/sharefile-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/sharefile-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/sharefile-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/sharefile-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-citrix-sharefile-test-user"></a>Creación de un usuario de prueba de Citrix ShareFile

Para permitir que los usuarios de Azure AD inicien sesión en Citrix ShareFile, tienen que aprovisionarse en Citrix ShareFile. En el caso de Citrix ShareFile, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino **Citrix ShareFile** .

1. Haga clic en **Manage Users \> (Administrar usuarios) Manage Users Home \> (Administrar página de inicio de usuarios) + Create Employee** (Crear empleado).
   
   ![Crear empleado](./media/sharefile-tutorial/IC781050.png "Crear empleado")

1. En la sección **Basic Information** (Información básica), siga los siguientes pasos:
   
   ![Información básica](./media/sharefile-tutorial/IC799951.png "Información básica")
   
   a. En el cuadro de texto **E-mail Address** (Dirección de correo electrónico), escriba la dirección de correo electrónico de Britta Simon como **brittasimon@contoso.com**.
   
   b. En el cuadro de texto **First name** (Nombre), escriba el **nombre** del usuario, en este caso, **Britta**.
   
   c. En el cuadro de texto **Last name** (Apellido), escriba el **apellido** del usuario, en este caso **Simon**.

1. Haga clic en **Agregar usuario**.
  
   >[!NOTE]
   >El titular de la cuenta de Azure AD recibirá un correo electrónico y seguirá un vínculo para confirmar la cuenta antes de que se active. Puede usar cualquier otra herramienta de creación de cuentas de usuario de Citrix ShareFile o API que Citrix ShareFile proporcione para aprovisionar las cuentas de usuario de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure, para lo cual le concederá acceso a Citrix ShareFile.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Citrix ShareFile, lleve a cabo los siguientes pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Citrix ShareFile**.

    ![Vínculo de Citrix ShareFile en la lista de aplicaciones](./media/sharefile-tutorial/tutorial_sharefile_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Citrix ShareFile en el Panel de acceso, debe iniciar sesión automáticamente en la aplicación Citrix ShareFile.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sharefile-tutorial/tutorial_general_01.png
[2]: ./media/sharefile-tutorial/tutorial_general_02.png
[3]: ./media/sharefile-tutorial/tutorial_general_03.png
[4]: ./media/sharefile-tutorial/tutorial_general_04.png

[100]: ./media/sharefile-tutorial/tutorial_general_100.png

[200]: ./media/sharefile-tutorial/tutorial_general_200.png
[201]: ./media/sharefile-tutorial/tutorial_general_201.png
[202]: ./media/sharefile-tutorial/tutorial_general_202.png
[203]: ./media/sharefile-tutorial/tutorial_general_203.png
