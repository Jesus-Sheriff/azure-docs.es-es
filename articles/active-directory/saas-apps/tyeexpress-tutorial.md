---
title: 'Tutorial: Integración de Azure Active Directory con T&E Express | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y T&E Express.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: ff4d634fb7f6f8057e5f370a694e46ca5e0d772d
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114080"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Tutorial: Integración de Azure Active Directory con T&E Express

En este tutorial, aprenderá a integrar T&E Express con Azure Active Directory (Azure AD).

La integración de T&E Express con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a T&E Express.
- Puede permitir que los usuarios inicien sesión automáticamente en T&E Express (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el Portal de administración de Azure

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con T&E Express se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en T&E Express

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No debe usar el entorno de producción, a menos que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar T & E Express desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-te-express-from-the-gallery"></a>Agregar T & E Express desde la galería
Para configurar la integración de T&E Express en Azure AD, será preciso que agregue T&E Express desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar T&amp;E Express desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo del **[Portal de administración de Azure](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Haga clic en el botón **Agregar** situado en la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **T&E Express**.

    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/tutorial_tyeexpress_search.png)

1. En el panel de resultados, seleccione **T&E Express** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con T&E Express con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de T&E Express para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de T&E Express.

Esta relación de vínculo se establece mediante la asignación del valor del **nombre de usuario** en Azure AD como el valor del **nombre de usuario** en T&E Express.

Para configurar y probar el inicio de sesión único de Azure AD con T&E Express, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de T&amp;E Express](#creating-a-te-express-test-user)**: para tener un homólogo de Britta Simon en T&amp;E Express que esté vinculado a la representación de ella en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en el Portal de administración de Azure y configurará el inicio de sesión único en la aplicación T&E Express.

**Para configurar el inicio de sesión único de Azure AD con T&amp;E Express, realice los pasos siguientes:**

1. En el Portal de administración de Azure, en la página de integración de la aplicación **T&E Express**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo**, seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

1. En la sección de **dominio y direcciones URL de T&E Express**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    a. En el cuadro de texto **Identificador**, escriba el valor como `https://<domain>.tyeexpress.com`.

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`.

    > [!NOTE] 
    > Tenga en cuenta que estos no son valores reales. Estos valores se tienen que actualizar con los valores reales de Identificador y URL de respuesta. Aquí le recomendamos que utilice el valor de cadena único en el identificador. Póngase en contacto con el [equipo de soporte técnico de T&E Express](http://www.tyeexpress.com/contacto.aspx) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo XML en el equipo.

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en **T&E Express**, inicie sesión en la aplicación T&E Express sin inicio de sesión único de SAML, sino usando credenciales de administrador.

1. En la pestaña **Administrar**, haga clic en **Dominio de SAML** para abrir la página de configuración de SAML.

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tye-SAML.png)

1. Invierta la opción **Activar** de **No** a **SI**. En el cuadro de texto **Metadatos del proveedor de identidades**, pegue los metadatos XML que descargó desde Azure Portal.

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tyeAdmin.png)

1. Haga clic en el botón **Guardar** para guardar la configuración.  


### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en el Portal de administración de Azure llamado Britta Simon.

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo del **Portal de administración de Azure**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/create_aaduser_01.png) 

1. Vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios** para mostrar la lista de usuarios.
    
    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/create_aaduser_02.png) 

1. En la parte superior del diálogo, haga clic en **Agregar** para abrir el diálogo **Usuario**.
 
    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/tyeexpress-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-te-express-test-user"></a>Crear un usuario de prueba T & E Express

Para permitir que los usuarios de Azure AD inicien sesión en T&E Express, deben aprovisionarse en T&E Express.  
En el caso de T&E Express, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de la compañía de T&E Express como administrador.

1. En la etiqueta de administración, haga clic en Usuarios para abrir la página principal de los usuarios.

    ![Agregar empleado](./media/tyeexpress-tutorial/tye-adminusers.png)

1. En la página principal, haga clic en **+** para agregar los usuarios.

    ![Agregar empleado](./media/tyeexpress-tutorial/tye-usershome.png)

1. Especificar todos los detalles obligatorios que solicita el formulario y haga clic en el botón Guardar para guardar los detalles.

    ![Agregar empleado](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![Agregar empleado](./media/tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a T&E Express.

![Asignar usuario][200] 

**Para asignar Britta Simon a T&amp;E Express, siga estos pasos:**

1. En el Portal de administración de Azure, abra la vista de aplicaciones, vaya a la vista de directorio y vaya a **Aplicaciones empresariales**. A continuación, haga clic en **All applications** (Todas las aplicaciones).

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **T&E Express**.

    ![Configurar inicio de sesión único](./media/tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de T&E Express en el panel de acceso, debería iniciar sesión automáticamente en su aplicación T&E Express.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/tyeexpress-tutorial/tutorial_general_203.png

