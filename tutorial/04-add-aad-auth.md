---
ms.openlocfilehash: 6e6c476b4ff0901f50d8e35a17f584d73b48b533
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822475"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a la API de Microsoft Graph. En este paso, configurará la biblioteca [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) .

> [!IMPORTANT]
> Para evitar almacenar el identificador de aplicación y el secreto en el origen, deberá usar el [Administrador de secretos de .net](/aspnet/core/security/app-secrets) para almacenar estos valores. El administrador de secretos solo se usa con fines de desarrollo, las aplicaciones de producción deben usar un administrador de secretos de confianza para almacenar secretos.

1. Abra **./appsettings.jsen** y reemplace su contenido por lo siguiente.

    :::code language="json" source="../demo/GraphTutorial/appsettings.json" highlight="2-6":::

1. Abra su CLI en el directorio donde se encuentra **GraphTutorial. csproj** y ejecute los siguientes comandos, sustituyendo `YOUR_APP_ID` por el identificador de la aplicación del portal de Azure y `YOUR_APP_SECRET` por el secreto de la aplicación.

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

Empiece agregando Microsoft Identity Platform Services a la aplicación.

1. Cree un nuevo archivo denominado **GraphConstants.CS** en el directorio **./Graph** y agregue el siguiente código.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphConstants.cs" id="GraphConstantsSnippet":::

1. Abra el archivo **./startup.CS** y agregue las siguientes `using` instrucciones en la parte superior del archivo.

    ```csharp
    using Microsoft.AspNetCore.Authentication.OpenIdConnect;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Mvc.Authorization;
    using Microsoft.Identity.Web;
    using Microsoft.Identity.Web.UI;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.Graph;
    using System.Net;
    using System.Net.Http.Headers;
    ```

1. Reemplace la función `ConfigureServices` existente por lo siguiente.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services
            // Use OpenId authentication
            .AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
            // Specify this is a web app and needs auth code flow
            .AddMicrosoftIdentityWebApp(Configuration)
            // Add ability to call web API (Graph)
            // and get access tokens
            .EnableTokenAcquisitionToCallDownstreamApi(options => {
                Configuration.Bind("AzureAd", options);
            }, GraphConstants.Scopes)
            // Use in-memory token cache
            // See https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization
            .AddInMemoryTokenCaches();

        // Require authentication
        services.AddControllersWithViews(options =>
        {
            var policy = new AuthorizationPolicyBuilder()
                .RequireAuthenticatedUser()
                .Build();
            options.Filters.Add(new AuthorizeFilter(policy));
        })
        // Add the Microsoft Identity UI pages for signin/out
        .AddMicrosoftIdentityUI();
    }
    ```

1. En la `Configure` función, agregue la siguiente línea encima de la `app.UseAuthorization();` línea.

    ```csharp
    app.UseAuthentication();
    ```

1. Abra **./Controllers/HomeController.CS** y reemplace su contenido por lo siguiente.

    ```csharp
    using GraphTutorial.Models;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Identity.Web;
    using System.Diagnostics;
    using System.Threading.Tasks;

    namespace GraphTutorial.Controllers
    {
        public class HomeController : Controller
        {
            ITokenAcquisition _tokenAcquisition;
            private readonly ILogger<HomeController> _logger;

            // Get the ITokenAcquisition interface via
            // dependency injection
            public HomeController(
                ITokenAcquisition tokenAcquisition,
                ILogger<HomeController> logger)
            {
                _tokenAcquisition = tokenAcquisition;
                _logger = logger;
            }

            public async Task<IActionResult> Index()
            {
                // TEMPORARY
                // Get the token and display it
                try
                {
                    string token = await _tokenAcquisition
                        .GetAccessTokenForUserAsync(GraphConstants.Scopes);
                    return View().WithInfo("Token acquired", token);
                }
                catch (MicrosoftIdentityWebChallengeUserException)
                {
                    return Challenge();
                }
            }

            public IActionResult Privacy()
            {
                return View();
            }

            [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
            public IActionResult Error()
            {
                return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
            }

            [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
            [AllowAnonymous]
            public IActionResult ErrorWithMessage(string message, string debug)
            {
                return View("Index").WithError(message, debug);
            }
        }
    }
    ```

1. Guarde los cambios e inicie el proyecto. Inicie sesión con su cuenta de Microsoft.

1. Examine la solicitud de consentimiento. La lista de permisos se corresponde con la lista de ámbitos de permisos configurados en **./Graph/GraphConstants.CS**.

    - **Mantener el acceso a los datos a los que ha dado acceso a:** ( `offline_access` ) este permiso se solicita mediante la MSAL para recuperar los tokens de actualización.
    - **Inicie sesión y lea su perfil:** ( `User.Read` ) este permiso permite a la aplicación obtener el perfil y la foto de perfil del usuario que ha iniciado sesión.
    - **Leer la configuración del buzón de correo:** ( `MailboxSettings.Read` ) este permiso permite que la aplicación Lea la configuración del buzón del usuario, incluido el formato de hora y zona horaria.
    - **Tener acceso completo a sus calendarios:** ( `Calendars.ReadWrite` ) este permiso permite a la aplicación leer eventos en el calendario del usuario, agregar nuevos eventos y modificar los existentes.

    ![Captura de pantalla del mensaje de consentimiento de Microsoft Identity Platform](./images/add-aad-auth-03.png)

    Para obtener más información sobre el consentimiento, consulte [Understanding Azure ad Application consienteon experiencias](/azure/active-directory/develop/application-consent-experience).

1. Consentimiento para los permisos solicitados. El explorador redirige a la aplicación, que muestra el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

Una vez que el usuario haya iniciado sesión, podrá obtener su información de Microsoft Graph.

1. Abra **./Graph/GraphClaimsPrincipalExtensions.CS** y reemplace todo el contenido por lo siguiente.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

1. Abra **./startup.CS** y reemplace la `.AddMicrosoftIdentityWebApp(Configuration)` línea existente con el código siguiente.

    :::code language="csharp" source="../demo/GraphTutorial/Startup.cs" id="AddSignInSnippet":::

    Considere lo que hace este código.

    - Agrega un controlador de eventos para el `OnTokenValidated` evento.
        - Usa la `ITokenAcquisition` interfaz para obtener un token de acceso.
        - Llama a Microsoft Graph para obtener el perfil y la foto del usuario.
        - Agrega la información del gráfico a la identidad del usuario.

1. Agregue la siguiente llamada de función después de la `EnableTokenAcquisitionToCallDownstreamApi` llamada y antes de la `AddInMemoryTokenCaches` llamada.

    :::code language="csharp" source="../demo/GraphTutorial/Startup.cs" id="AddGraphClientSnippet":::

    Esto hará que un **GraphServiceClient** autenticado esté disponible para los controladores a través de la inserción de dependencia.

1. Abra **./Controllers/HomeController.CS** y reemplace la `Index` función por lo siguiente.

    ```csharp
    public IActionResult Index()
    {
        return View();
    }
    ```

1. Quite todas las referencias a `ITokenAcquisition` en la clase **HomeController** .

1. Guarde los cambios, inicie la aplicación y pase por el proceso de inicio de sesión. Deberás volver a la Página principal, pero la interfaz de usuario debe cambiar para indicar que has iniciado sesión.

    ![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** . Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.

    ![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenamiento y actualización de tokens

En este punto, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas a la API. Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Aquí es donde el token de actualización se vuelve útil. El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.

Debido a que la aplicación usa la biblioteca Microsoft. Identity. Web, no es necesario implementar ninguna lógica de almacenamiento o actualización de tokens.

La aplicación usa la caché de token en memoria, que es suficiente para las aplicaciones que no necesitan conservar los tokens cuando se reinicia la aplicación. Las aplicaciones de producción pueden usar en su lugar las [Opciones de caché distribuida](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization) de la biblioteca Microsoft. Identity. Web.

El `GetAccessTokenForUserAsync` método controla la expiración y la actualización de los tokens. Primero comprueba el token almacenado en caché y, si no lo ha expirado, lo devuelve. Si ha expirado, usa el token de actualización almacenado en caché para obtener uno nuevo.

El **GraphServiceClient** que los controladores obtienen mediante la inyección de dependencia estarán preconfigurados con un proveedor de autenticación que usará `GetAccessTokenForUserAsync` para usted.
