---
ms.openlocfilehash: debd685996df22a83110a14ca585cfafa4e08a0d
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822430"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD con el centro de administración de Azure Active Directory.

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](./images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación** , establezca los valores siguientes.

    - Establezca **Nombre** como `ASP.NET Core Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección** , establezca la primera lista desplegable en `Web` y establezca el valor `https://localhost:5001/`.

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. Seleccione **Registrar**. En la página del **tutorial de ASP.net Core Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. Seleccione **Autenticación** en **Administrar**. En **URI de redireccionamiento** , agregue un URI con el valor `https://localhost:5001/signin-oidc` .

1. Establezca la **dirección URL de cierre de sesión** en `https://localhost:5001/signout-oidc` .

1. Busque la sección **Concesión implícita** y habilite los **tokens de ID**. Seleccione **Guardar**.

    ![Una captura de pantalla de la configuración de la plataforma web en Azure portal](./images/aad-web-platform.png)

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción** y seleccione una de las opciones para **Expires** y seleccione **Agregar**.

    ![Captura de pantalla del cuadro de diálogo Agregar un secreto de cliente](./images/aad-new-client-secret.png)

1. Copie el valor del secreto de cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

    ![Captura de pantalla del secreto de cliente recién agregado](./images/aad-copy-client-secret.png)
