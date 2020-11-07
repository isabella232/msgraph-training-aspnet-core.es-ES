---
ms.openlocfilehash: 308938efbedc4618c7b0ca3ea6b2eebc0582da10
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822518"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="32f13-101">Empiece por crear una aplicación Web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32f13-101">Start by creating an ASP.NET Core web app.</span></span>

1. <span data-ttu-id="32f13-102">Abra la interfaz de línea de comandos (CLI) en un directorio donde desee crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="32f13-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="32f13-103">Ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="32f13-103">Run the following command.</span></span>

    ```Shell
    dotnet new mvc -o GraphTutorial
    ```

1. <span data-ttu-id="32f13-104">Una vez creado el proyecto, compruebe que funciona cambiando el directorio actual al directorio **GraphTutorial** y ejecutando el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="32f13-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

1. <span data-ttu-id="32f13-105">Abra el explorador y vaya a `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="32f13-105">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="32f13-106">Si todo funciona, debería ver una página de núcleo de ASP.NET predeterminada.</span><span class="sxs-lookup"><span data-stu-id="32f13-106">If everything is working, you should see a default ASP.NET Core page.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32f13-107">Si recibe una advertencia de que el certificado para **localhost** no es de confianza, puede usar .net Core CLI para instalar y confiar en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="32f13-107">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="32f13-108">Consulte [forzar HTTPS en ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) para obtener instrucciones sobre sistemas operativos específicos.</span><span class="sxs-lookup"><span data-stu-id="32f13-108">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="32f13-109">Agregar paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="32f13-109">Add NuGet packages</span></span>

<span data-ttu-id="32f13-110">Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="32f13-110">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="32f13-111">[Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) para solicitar y administrar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="32f13-111">[Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) for requesting and managing access tokens.</span></span>
- <span data-ttu-id="32f13-112">[Microsoft. Identity. Web. MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) para agregar el SDK de Microsoft Graph mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="32f13-112">[Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph/) for adding the Microsoft Graph SDK via dependency injection.</span></span>
- <span data-ttu-id="32f13-113">[Microsoft. Identity. Web. UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI/) para la interfaz de usuario de inicio y de cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="32f13-113">[Microsoft.Identity.Web.UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI/) for sign-in and sign-out UI.</span></span>
- <span data-ttu-id="32f13-114">[Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="32f13-114">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="32f13-115">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) para tratar los identificadores de zona horaria entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="32f13-115">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for handling time zoned identifiers cross-platform.</span></span>

1. <span data-ttu-id="32f13-116">Ejecute los siguientes comandos en su CLI para instalar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="32f13-116">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Identity.Web --version 1.1.0
    dotnet add package Microsoft.Identity.MicrosoftGraph --version 1.1.0
    dotnet add package Microsoft.Identity.Web.UI --version 1.1.0
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="32f13-117">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="32f13-117">Design the app</span></span>

<span data-ttu-id="32f13-118">En esta sección, creará la estructura básica de la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32f13-118">In this section you will create the basic UI structure of the application.</span></span>

### <a name="implement-alert-extension-methods"></a><span data-ttu-id="32f13-119">Implementación de métodos de extensión de alertas</span><span class="sxs-lookup"><span data-stu-id="32f13-119">Implement alert extension methods</span></span>

<span data-ttu-id="32f13-120">En esta sección, creará métodos de extensión para el `IActionResult` tipo que devuelven las vistas de controlador.</span><span class="sxs-lookup"><span data-stu-id="32f13-120">In this section you will create extension methods for the `IActionResult` type returned by controller views.</span></span> <span data-ttu-id="32f13-121">Esta extensión permite que se pasen mensajes de error temporales o correctos a la vista.</span><span class="sxs-lookup"><span data-stu-id="32f13-121">This extension will enable passing temporary error or success messages to the view.</span></span>

> [!TIP]
> <span data-ttu-id="32f13-122">Puede usar cualquier editor de texto para editar los archivos de origen de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="32f13-122">You can use any text editor to edit the source files for this tutorial.</span></span> <span data-ttu-id="32f13-123">Sin embargo, [Visual Studio Code](https://code.visualstudio.com/) proporciona características adicionales, como la depuración e IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="32f13-123">However, [Visual Studio Code](https://code.visualstudio.com/) provides additional features, such as debugging and Intellisense.</span></span>

1. <span data-ttu-id="32f13-124">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Alerts**.</span><span class="sxs-lookup"><span data-stu-id="32f13-124">Create a new directory in the **GraphTutorial** directory named **Alerts**.</span></span>

1. <span data-ttu-id="32f13-125">Cree un nuevo archivo denominado **WithAlertResult.CS** en el directorio **./Alerts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="32f13-125">Create a new file named **WithAlertResult.cs** in the **./Alerts** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Alerts/WithAlertResult.cs" id="WithAlertResultSnippet":::

1. <span data-ttu-id="32f13-126">Cree un nuevo archivo denominado **AlertExtensions.CS** en el directorio **./Alerts** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="32f13-126">Create a new file named **AlertExtensions.cs** in the **./Alerts** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Alerts/AlertExtensions.cs" id="AlertExtensionsSnippet":::

### <a name="implement-user-data-extension-methods"></a><span data-ttu-id="32f13-127">Implementación de métodos de extensión de datos de usuario</span><span class="sxs-lookup"><span data-stu-id="32f13-127">Implement user data extension methods</span></span>

<span data-ttu-id="32f13-128">En esta sección, creará métodos de extensión para el `ClaimsPrincipal` objeto generado por la plataforma de identidad de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32f13-128">In this section you will create extension methods for the `ClaimsPrincipal` object generated by the Microsoft Identity platform.</span></span> <span data-ttu-id="32f13-129">Esto le permitirá ampliar la identidad de usuario existente con datos de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="32f13-129">This will allow you to extend the existing user identity with data from Microsoft Graph.</span></span>

> [!NOTE]
> <span data-ttu-id="32f13-130">Este código es solo un marcador de posición por ahora, se completará en una sección posterior.</span><span class="sxs-lookup"><span data-stu-id="32f13-130">This code is just a placeholder for now, you will complete it in a later section.</span></span>

1. <span data-ttu-id="32f13-131">Cree un nuevo directorio en el directorio **GraphTutorial** denominado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="32f13-131">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>

1. <span data-ttu-id="32f13-132">Cree un nuevo archivo denominado **GraphClaimsPrincipalExtensions.CS** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="32f13-132">Create a new file named **GraphClaimsPrincipalExtensions.cs** and add the following code.</span></span>

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

### <a name="create-views"></a><span data-ttu-id="32f13-133">Crear vistas</span><span class="sxs-lookup"><span data-stu-id="32f13-133">Create views</span></span>

<span data-ttu-id="32f13-134">En esta sección, implementará las vistas de Razor para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32f13-134">In this section you will implement the Razor views for the application.</span></span>

1. <span data-ttu-id="32f13-135">Agregue un nuevo archivo con el nombre **_LoginPartial. cshtml** en el directorio **./views/Shared** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="32f13-135">Add a new file named **_LoginPartial.cshtml** in the **./Views/Shared** directory and add the following code.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_LoginPartial.cshtml" id="LoginPartialSnippet":::

1. <span data-ttu-id="32f13-136">Agregue un nuevo archivo con el nombre **_AlertPartial. cshtml** en el directorio **./views/Shared** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="32f13-136">Add a new file named **_AlertPartial.cshtml** in the **./Views/Shared** directory and add the following code.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_AlertPartial.cshtml" id="AlertPartialSnippet":::

1. <span data-ttu-id="32f13-137">Abra el archivo **./Views/Shared/_Layout. cshtml** y reemplace todo su contenido por el código siguiente para actualizar el diseño global de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32f13-137">Open the **./Views/Shared/_Layout.cshtml** file, and replace its entire contents with the following code to update the global layout of the app.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Shared/_Layout.cshtml" id="LayoutSnippet":::

1. <span data-ttu-id="32f13-138">Abra **./wwwroot/CSS/site.CSS** y agregue el siguiente código al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="32f13-138">Open **./wwwroot/css/site.css** and add the following code at the bottom of the file.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/site.css" id="CssSnippet":::

1. <span data-ttu-id="32f13-139">Abra el archivo **./views/home/index.cshtml** y reemplace el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="32f13-139">Open the **./Views/Home/index.cshtml** file and replace its contents with the following.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Home/Index.cshtml" id="HomeIndexSnippet":::

1. <span data-ttu-id="32f13-140">Cree un nuevo directorio en el directorio **./wwwroot** denominado **IMG**.</span><span class="sxs-lookup"><span data-stu-id="32f13-140">Create a new directory in the **./wwwroot** directory named **img**.</span></span> <span data-ttu-id="32f13-141">Agregue un archivo de imagen de su elección con nombre **no-profile-photo.png** en este directorio.</span><span class="sxs-lookup"><span data-stu-id="32f13-141">Add an image file of your choosing named **no-profile-photo.png** in this directory.</span></span> <span data-ttu-id="32f13-142">Esta imagen se utilizará como foto del usuario cuando el usuario no tenga foto en Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="32f13-142">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

    > [!TIP]
    > <span data-ttu-id="32f13-143">Puede descargar la imagen usada en estas capturas de pantallas desde [GitHub](https://github.com/microsoftgraph/msgraph-training-aspnet-core/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span><span class="sxs-lookup"><span data-stu-id="32f13-143">You can download the image used in these screenshots from [GitHub](https://github.com/microsoftgraph/msgraph-training-aspnet-core/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span></span>

1. <span data-ttu-id="32f13-144">Guarde todos los cambios y reinicie el servidor ( `dotnet run` ).</span><span class="sxs-lookup"><span data-stu-id="32f13-144">Save all of your changes and restart the server (`dotnet run`).</span></span> <span data-ttu-id="32f13-145">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="32f13-145">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la Página principal rediseñada](./images/create-app-01.png)
