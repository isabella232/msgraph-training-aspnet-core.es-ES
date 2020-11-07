---
ms.openlocfilehash: 30398bafbf47aaa374a200dd1834c9f2003e967f
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822443"
---
# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- [.Net Core SDK](https://dotnet.microsoft.com/download) instalado en el equipo de desarrollo. ( **Nota:** este tutorial se ha escrito con .net Core SDK versión 3.1.201. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.
- Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.

Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registro de una aplicación web con el centro de administración de Azure Active Directory

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](../tutorial/images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación** , establezca los valores siguientes.

    - Establezca **Nombre** como `ASP.NET Core Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección** , establezca la primera lista desplegable en `Web` y establezca el valor `https://localhost:5001/`.

    ![Captura de pantalla de la página registrar una aplicación](../tutorial/images/aad-register-an-app.png)

1. Seleccione **Registrar**. En la página del **tutorial de ASP.net Core Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](../tutorial/images/aad-application-id.png)

1. Seleccione **Autenticación** en **Administrar**. En **URI de redireccionamiento** , agregue un URI con el valor `https://localhost:5001/signin-oidc` .

1. Establezca la **dirección URL de cierre de sesión** en `https://localhost:5001/signout-oidc` .

1. Busque la sección **Concesión implícita** y habilite los **tokens de ID**. Seleccione **Guardar**.

    ![Una captura de pantalla de la configuración de la plataforma web en Azure portal](../tutorial/images/aad-web-platform.png)

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción** y seleccione una de las opciones para **Expires** y seleccione **Agregar**.

    ![Captura de pantalla del cuadro de diálogo Agregar un secreto de cliente](../tutorial/images/aad-new-client-secret.png)

1. Copie el valor del secreto de cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

    ![Captura de pantalla del secreto de cliente recién agregado](../tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Abra la interfaz de línea de comandos (CLI) en el directorio donde se encuentra **GraphTutorial. csproj** y ejecute los siguientes comandos, sustituyendo `YOUR_APP_ID` con el identificador de la aplicación del portal de Azure y `YOUR_APP_SECRET` con el secreto de la aplicación.

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="run-the-sample"></a>Ejecutar el ejemplo

En la CLI, ejecute el siguiente comando para iniciar la aplicación.

```Shell
dotnet run
```
