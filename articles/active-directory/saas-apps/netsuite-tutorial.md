---
title: 'Tutorial: Integración de Azure Active Directory con NetSuite | Microsoft Docs'
description: Más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y NetSuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 511fdcf587d16a59ff2bb11dfc55504b2218a569
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431423"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Integración de Azure Active Directory con NetSuite

En este tutorial, aprenderá a integrar NetSuite con Azure Active Directory (Azure AD).

La integración de NetSuite con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a NetSuite.
- Puede permitir que los usuarios inicien sesión automáticamente en NetSuite (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con NetSuite, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en NetSuite

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de NetSuite desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-netsuite-from-the-gallery"></a>Adición de NetSuite desde la galería
Para configurar la integración de NetSuite en Azure AD, deberá agregar NetSuite desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar NetSuite desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]

1. Haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **NetSuite**, seleccione **NetSuite** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![NetSuite en la lista de resultados](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con NetSuite con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de NetSuite para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de NetSuite.

Para establecer esta relación de vínculo, en NetSuite, se asigna el valor del **nombre de usuario** en Azure AD como el valor del **nombre de usuario** en NetSuite.

Para configurar y probar el inicio de sesión único de Azure AD con NetSuite, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de NetSuite](#creating-a-netsuite-test-user)**: para tener un homólogo de Britta Simon en NetSuite vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación NetSuite.

**Para configurar el inicio de sesión único de Azure AD con NetSuite, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de aplicaciones de **NetSuite**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_NetSuite_samlbase.png)

1. En la sección **NetSuite Domain and URLs** (Dominio y direcciones URL de NetSuite), lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_NetSuite_url.png)

    En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`
    
    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico de NetSuite](http://www.NetSuite.com/portal/services/support.shtml) para obtener estos valores.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo XML en el equipo.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_NetSuite_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de NetSuite**, haga clic en **Configurar NetSuite** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_NetSuite_configure.png)

1. Abra una nueva pestaña en el explorador e inicie sesión en el sitio de la empresa NetSuite como administrador.

1. En la barra de herramientas de la parte superior de la página, haga clic en **Setup** (Configuración), vaya a **Company** (Empresa) y haga clic en **Enable Features** (Habilitar características).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setupsaml.png)

1. En la barra de herramientas de la parte central de la página, haga clic en **SuiteCloud**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-suitecloud.png)

1. En la sección **Manage Authentication** (Administrar autenticación=, seleccione **SAML SINGLE SIGN-ON** (INICIO DE SESIÓN ÚNICO DE SAMLS) para habilitar la opción de inicio de sesión único de SAML en NetSuite.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-ticksaml.png)

1. En la barra de herramientas de la parte superior de la página, haga clic en **Setup** (Configuración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

1. En la lista **SETUP TASKS** (TAREAS DE CONFIGURACIÓN), haga clic en **Integration** (Integración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-integration.png)

1. En la sección **MANAGE AUTHENTICATION** (ADMINISTRAR AUTENTICACIÓN), haga clic en **SAML Single Sign-on** (Inicio de sesión único de SAML).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-saml.png)

1. En la página **SAML Setup** (Configuración de SAML), en la sección **NetSuite Configuration** (Configuración de NetSuite), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Seleccione **PRIMARY AUTHENTICATION METHOD** (MÉTODO DE AUTENTICACIÓN PRINCIPAL).

    b. En el campo con la etiqueta **SAMLV2 IDENTITY PROVIDER METADATA** (METADATOS DEL PROVEEDOR DE IDENTIDADES DE SAMLV2), seleccione **UPLOAD IDP METADATA FILE** (CARGAR ARCHIVO DE METADATOS DE IDP). A continuación, haga clic en **Examinar** para cargar el archivo de metadatos que descargó de Azure Portal.

    c. Haga clic en **Enviar**.

1. En Azure AD, haga clic en la casilla **Ver y editar todos los atributos de usuario** y agregue el atributo.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-attributes.png)

1. Para el campo **Nombre de atributo**, escriba `account`. Para el campo **Valor del atributo**, escriba el identificador de cuenta de NetSuite. Este valor es constante y cambia con la cuenta. A continuación se incluyen instrucciones sobre cómo encontrar el identificador de la cuenta:

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-add-attribute.png)

    a. En NetSuite, haga clic en **Setup** (Configuración), vaya a **Company** (Empresa) y haga clic en **Company Information** (Información de empresa) en el menú de navegación superior.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-com.png)

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-account-id.png)

    b. En la página **Company Information** (Información de empresa), en la columna de la derecha, copie el **identificador de la cuenta**.

    c. Pegue el **identificador de la cuenta** que ha copiado de la cuenta de NetSuite en el campo **Valor de atributo** de Azure AD. 

1. Antes de que los usuarios puedan realizar el inicio de sesión único en NetSuite, se les deben asignar primero los permisos adecuados en NetSuite. Siga las instrucciones siguientes para asignar estos permisos.

    a. En el menú de navegación superior, haga clic en **Setup** (Configuración).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

    b. En el menú de navegación izquierdo, seleccione **Usuarios/Roles** y haga clic en **Administrar roles**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Haga clic en **Nuevo rol**.

    d. Escriba un **Nombre** para el nuevo rol.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-new-role.png)

    e. Haga clic en **Save**(Guardar).

    f. En el menú de la parte superior, haga clic en **Permisos**. A continuación, haga clic en **Configuración**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-sso.png)

    g. Seleccione **SAML Single Sign-on** (Inicio de sesión único de SAML) y haga clic en **Add** (Agregar).

    h. Haga clic en **Save**(Guardar).

    i. En el menú de navegación superior, haga clic en **Configuración** y en **Administrador de instalación**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-setup.png)

    j. En el menú de navegación izquierdo, seleccione **Usuarios/Roles** y haga clic en **Administrar usuarios**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Seleccione un usuario de prueba. A continuación, haga clic en **Edit** (Editar) y, a continuación, vaya a la pestaña **Access** (Acceso).

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-edit-user.png)

    l. En el cuadro de diálogo de roles, asigne el rol adecuado que haya creado.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/ns-add-role.png)

    m. Haga clic en **Save**(Guardar).
 
### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/NetSuite-tutorial/create_aaduser_01.png) 

1.  Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/NetSuite-tutorial/create_aaduser_02.png) 

1. En la parte superior del diálogo, haga clic en **Agregar** para abrir el diálogo **Usuario**.
 
    ![Creación de un usuario de prueba de Azure AD](./media/NetSuite-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/NetSuite-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear). 

### <a name="creating-a-netsuite-test-user"></a>Creación de un usuario de prueba de NetSuite

En esta sección se creará un usuario llamado Britta Simon en NetSuite. NetSuite admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada.
No hay ningún elemento de acción para usted en esta sección. Si el usuario ya no existe en NetSuite, se crea uno nuevo cuando se intenta acceder a NetSuite.


### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a NetSuite.

![Asignar usuario][200] 

**Para asignar a Britta Simon a NetSuite, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **NetSuite**.

    ![Configurar inicio de sesión único](./media/NetSuite-tutorial/tutorial_NetSuite_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Para probar la configuración de inicio de sesión único, abra el Panel de acceso en [https://myapps.microsoft.com](https://myapps.microsoft.com/), inicie sesión en la cuenta de prueba y haga clic en **NetSuite**.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del aprovisionamiento de usuarios](NetSuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/NetSuite-tutorial/tutorial_general_01.png
[2]: ./media/NetSuite-tutorial/tutorial_general_02.png
[3]: ./media/NetSuite-tutorial/tutorial_general_03.png
[4]: ./media/NetSuite-tutorial/tutorial_general_04.png

[100]: ./media/NetSuite-tutorial/tutorial_general_100.png

[200]: ./media/NetSuite-tutorial/tutorial_general_200.png
[201]: ./media/NetSuite-tutorial/tutorial_general_201.png
[202]: ./media/NetSuite-tutorial/tutorial_general_202.png
[203]: ./media/NetSuite-tutorial/tutorial_general_203.png

