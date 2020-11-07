---
ms.openlocfilehash: 308938efbedc4618c7b0ca3ea6b2eebc0582da10
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822518"
---
<!-- markdownlint-disable MD002 MD041 -->

Empiece por crear una aplicación Web de ASP.NET Core.

1. Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto. Ejecute el comando siguiente.

    ```Shell
    dotnet new mvc -o GraphTutorial
    ```

1. Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.

    ```Shell
    dotnet run
    ```

1. Abra el explorador y vaya a `https://localhost:5001` . Si todo funciona, debería ver una página de núcleo de ASP.NET predeterminada.

> [!IMPORTANT]
> Si recibe una advertencia de que el certificado para **localhost** no es de confianza, puede usar .net Core CLI para instalar y confiar en el certificado de desarrollo. Consulte [forzar HTTPS en ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) para obtener instrucciones sobre sistemas operativos específicos.

## <a name="add-nuget-packages"></a>Agregar paquetes NuGet

Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.

- [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) para solicitar y administrar tokens de acceso.
- [Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) para agregar el SDK de Microsoft Graph mediante la inserción de dependencias.
- [Microsoft. Identity. Web. UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI/) para la interfaz de usuario de inicio y de cierre de sesión.
- [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) para realizar llamadas a Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para tratar los identificadores de zona horaria entre plataformas.

1. Ejecute los siguientes comandos en su CLI para instalar las dependencias.

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Identity.Web.UI --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará la estructura básica de la interfaz de usuario de la aplicación.

### <a name="implement-alert-extension-methods"></a>Implementación de métodos de extensión de alertas

En esta sección, creará métodos de extensión para el `IActionResult` tipo que devuelven las vistas de controlador. Esta extensión permite que se pasen mensajes de error temporales o correctos a la vista.

> [!TIP]
> Puede usar cualquier editor de texto para editar los archivos de origen de este tutorial. Sin embargo, [Visual Studio Code](https://code.visualstudio.com/) proporciona características adicionales, como la depuración e IntelliSense.

1. Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Alerts**.

1. Cree un nuevo archivo denominado **WithAlertResult.CS** en el directorio **./Alerts** y agregue el siguiente código.

    :::code language="csharp" source="../demo/GraphTutorial/Alerts/WithAlertResult.cs" id="WithAlertResultSnippet":::

1. Cree un nuevo archivo denominado **AlertExtensions.CS** en el directorio **./Alerts** y agregue el siguiente código.

    :::code language="csharp" source="../demo/GraphTutorial/Alerts/AlertExtensions.cs" id="AlertExtensionsSnippet":::

### <a name="implement-user-data-extension-methods"></a>Implementación de métodos de extensión de datos de usuario

En esta sección, creará métodos de extensión para el `ClaimsPrincipal` objeto generado por la plataforma de identidad de Microsoft. Esto le permitirá ampliar la identidad de usuario existente con datos de Microsoft Graph.

> [!NOTE]
> Este código es solo un marcador de posición por ahora, se completará en una sección posterior.

1. Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Graph**.

1. Cree un nuevo archivo denominado **GraphClaimsPrincipalExtensions.CS** y agregue el siguiente código.

    ```csharp
    using System.Security.Claims;

    namespace GraphTutorial
    {
        public static class GraphClaimTypes {
            public const string DisplayName ="graph_name";
            public const string Email = "graph_email";
            public const string Photo = "graph_photo";
            public const string TimeZone = "graph_timezone";
            public const string DateTimeFormat = "graph_datetimeformat";
        }

        // Helper methods to access Graph user data stored in
        // the claims principal
        public static class GraphClaimsPrincipalExtensions
        {
            public static string GetUserGraphDisplayName(this ClaimsPrincipal claimsPrincipal)
            {
                return "Adele Vance";
            }

            public static string GetUserGraphEmail(this ClaimsPrincipal claimsPrincipal)
            {
                return "adelev@contoso.com";
            }

            public static string GetUserGraphPhoto(this ClaimsPrincipal claimsPrincipal)
            {
                return "/img/no-profile-photo.png";
            }
        }
    }
    ```

### <a name="create-views"></a>Crear vistas

En esta sección, implementará las vistas de Razor para la aplicación.

1. Agregue un nuevo archivo con el nombre **_LoginPartial. cshtml** en el directorio **./views/Shared** y agregue el siguiente código.

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_LoginPartial.cshtml" id="LoginPartialSnippet":::

1. Agregue un nuevo archivo con el nombre **_AlertPartial. cshtml** en el directorio **./views/Shared** y agregue el siguiente código.

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_AlertPartial.cshtml" id="AlertPartialSnippet":::

1. Abra el archivo **./Views/Shared/_Layout. cshtml** y reemplace todo su contenido por el código siguiente para actualizar el diseño global de la aplicación.

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_Layout.cshtml" id="LayoutSnippet":::

1. Abra **./wwwroot/CSS/site.CSS** y agregue el siguiente código al final del archivo.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. Abra el archivo **./views/home/index.cshtml** y reemplace el contenido por lo siguiente.

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Home/Index.cshtml" id="HomeIndexSnippet":::

1. Cree un nuevo directorio en el directorio **./wwwroot** denominado **IMG**. Agregue un archivo de imagen de su elección con nombre **no-profile-photo.png** en este directorio. Esta imagen se utilizará como foto del usuario cuando el usuario no tenga foto en Microsoft Graph.

    > [!TIP]
    > Puede descargar la imagen usada en estas capturas de pantallas desde [GitHub](https://github.com/microsoftgraph/msgraph-training-aspnet-core/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).

1. Guarde todos los cambios y reinicie el servidor ( `dotnet run` ). Ahora, la aplicación debe tener un aspecto muy diferente.

    ![Una captura de pantalla de la Página principal rediseñada](./images/create-app-01.png)
