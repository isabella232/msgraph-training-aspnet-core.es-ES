---
ms.openlocfilehash: 17394dd6283464eabcbea1f60c48640412b55431
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822524"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d7220-101">En esta sección, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7220-101">In this section you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="d7220-102">Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d7220-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d7220-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="d7220-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="d7220-104">Empiece por crear un nuevo controlador para las vistas de calendario.</span><span class="sxs-lookup"><span data-stu-id="d7220-104">Start by creating a new controller for calendar views.</span></span>

1. <span data-ttu-id="d7220-105">Agregue un nuevo archivo denominado **CalendarController.CS** en el directorio **./Controllers** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-105">Add a new file named **CalendarController.cs** in the **./Controllers** directory and add the following code.</span></span>

    ```csharp
    using GraphTutorial.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Identity.Web;
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using TimeZoneConverter;

    namespace GraphTutorial.Controllers
    {
        public class CalendarController : Controller
        {
            private readonly GraphServiceClient _graphClient;
            private readonly ILogger<HomeController> _logger;

            public CalendarController(
                GraphServiceClient graphClient,
                ILogger<HomeController> logger)
            {
                _graphClient = graphClient;
                _logger = logger;
            }
        }
    }
    ```

1. <span data-ttu-id="d7220-106">Agregue las siguientes funciones a la `CalendarController` clase para obtener la vista de calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="d7220-106">Add the following functions to the `CalendarController` class to get the user's calendar view.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="GetCalendarViewSnippet":::

    <span data-ttu-id="d7220-107">Tenga en cuenta lo que `GetUserWeekCalendar` hace el código.</span><span class="sxs-lookup"><span data-stu-id="d7220-107">Consider what the code in `GetUserWeekCalendar` does.</span></span>

    - <span data-ttu-id="d7220-108">Usa la zona horaria del usuario para obtener los valores de fecha y hora de inicio y finalización de UTC para la semana.</span><span class="sxs-lookup"><span data-stu-id="d7220-108">It uses the user's time zone to get UTC start and end date/time values for the week.</span></span>
    - <span data-ttu-id="d7220-109">Consulta la [vista de calendario](/graph/api/calendar-list-calendarview?view=graph-rest-1.0) del usuario para obtener todos los eventos comprendidos entre la fecha y hora de inicio y finalización.</span><span class="sxs-lookup"><span data-stu-id="d7220-109">It queries the user's [calendar view](/graph/api/calendar-list-calendarview?view=graph-rest-1.0) to get all events that fall between the start and end date/times.</span></span> <span data-ttu-id="d7220-110">El uso de una vista de calendario en lugar de una [lista de eventos](/graph/api/user-list-events?view=graph-rest-1.0) expande los eventos periódicos y devuelve todas las repeticiones que se produzcan en la ventana de tiempo especificada.</span><span class="sxs-lookup"><span data-stu-id="d7220-110">Using a calendar view instead of [listing events](/graph/api/user-list-events?view=graph-rest-1.0) expands recurring events, returning any occurrences that happen in the specified time window.</span></span>
    - <span data-ttu-id="d7220-111">Usa el `Prefer: outlook.timezone` encabezado para obtener resultados de vuelta en la zona horaria del usuario.</span><span class="sxs-lookup"><span data-stu-id="d7220-111">It uses the `Prefer: outlook.timezone` header to get results back in the user's timezone.</span></span>
    - <span data-ttu-id="d7220-112">Usa `Select` para limitar los campos que se devuelven solo a los que usa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d7220-112">It uses `Select` to limit the fields that come back to just those used by the app.</span></span>
    - <span data-ttu-id="d7220-113">Usa `OrderBy` para ordenar los resultados cronológicamente.</span><span class="sxs-lookup"><span data-stu-id="d7220-113">It uses `OrderBy` to sort the results chronologically.</span></span>
    - <span data-ttu-id="d7220-114">Utiliza una `PageIterator` Página para a [través de la colección Events](/graph/sdks/paging).</span><span class="sxs-lookup"><span data-stu-id="d7220-114">It uses a `PageIterator` to [page through the events collection](/graph/sdks/paging).</span></span> <span data-ttu-id="d7220-115">Esto controla el caso en el que el usuario tiene más eventos en su calendario que el tamaño de página solicitado.</span><span class="sxs-lookup"><span data-stu-id="d7220-115">This handles the case where the user has more events on their calendar than the requested page size.</span></span>

1. <span data-ttu-id="d7220-116">Agregue la siguiente función a la `CalendarController` clase para implementar una vista temporal de los datos devueltos.</span><span class="sxs-lookup"><span data-stu-id="d7220-116">Add the following function to the `CalendarController` class to implement a temporary view of the returned data.</span></span>

    ```csharp
    // Minimum permission scope needed for this view
    [AuthorizeForScopes(Scopes = new[] { "Calendars.Read" })]
    public async Task<IActionResult> Index()
    {
        try
        {
            var userTimeZone = TZConvert.GetTimeZoneInfo(
                User.GetUserGraphTimeZone());
            var startOfWeek = CalendarController.GetUtcStartOfWeekInTimeZone(
                DateTime.Today, userTimeZone);

            var events = await GetUserWeekCalendar(startOfWeek);

            // Return a JSON dump of events
            return new ContentResult {
                Content = _graphClient.HttpProvider.Serializer.SerializeObject(events),
                ContentType = "application/json"
            };
        }
        catch (ServiceException ex)
        {
            if (ex.InnerException is MicrosoftIdentityWebChallengeUserException)
            {
                throw ex;
            }

            return new ContentResult {
                Content = $"Error getting calendar view: {ex.Message}",
                ContentType = "text/plain"
            };
        }
    }
    ```

1. <span data-ttu-id="d7220-117">Inicie la aplicación, inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="d7220-117">Start the app, sign in, and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="d7220-118">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="d7220-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="d7220-119">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="d7220-119">Display the results</span></span>

<span data-ttu-id="d7220-120">Ahora puede Agregar una vista para mostrar los resultados de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="d7220-120">Now you can add a view to display the results in a more user-friendly manner.</span></span>

### <a name="create-view-models"></a><span data-ttu-id="d7220-121">Crear modelos de vista</span><span class="sxs-lookup"><span data-stu-id="d7220-121">Create view models</span></span>

1. <span data-ttu-id="d7220-122">Cree un nuevo archivo denominado **CalendarViewEvent.CS** en el directorio **./Models** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-122">Create a new file named **CalendarViewEvent.cs** in the **./Models** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/CalendarViewEvent.cs" id="CalendarViewEventSnippet":::

1. <span data-ttu-id="d7220-123">Cree un nuevo archivo denominado **DailyViewModel.CS** en el directorio **./Models** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-123">Create a new file named **DailyViewModel.cs** in the **./Models** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/DailyViewModel.cs" id="DailyViewModelSnippet":::

1. <span data-ttu-id="d7220-124">Cree un nuevo archivo denominado **CalendarViewModel.CS** en el directorio **./Models** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-124">Create a new file named **CalendarViewModel.cs** in the **./Models** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/CalendarViewModel.cs" id="CalendarViewModelSnippet":::

### <a name="create-views"></a><span data-ttu-id="d7220-125">Crear vistas</span><span class="sxs-lookup"><span data-stu-id="d7220-125">Create views</span></span>

1. <span data-ttu-id="d7220-126">Cree un nuevo directorio denominado **Calendar** en el directorio **./views** .</span><span class="sxs-lookup"><span data-stu-id="d7220-126">Create a new directory named **Calendar** in the **./Views** directory.</span></span>

1. <span data-ttu-id="d7220-127">Cree un nuevo archivo con el nombre **_DailyEventsPartial. cshtml** en el directorio **./views/Calendar** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-127">Create a new file named **_DailyEventsPartial.cshtml** in the **./Views/Calendar** directory and add the following code.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Calendar/_DailyEventsPartial.cshtml" id="DailyEventsPartialSnippet":::

1. <span data-ttu-id="d7220-128">Cree un nuevo archivo denominado **index. cshtml** en el directorio **./views/Calendar** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="d7220-128">Create a new file named **Index.cshtml** in the **./Views/Calendar** directory and add the following code.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Calendar/Index.cshtml" id="CalendarIndexSnippet":::

### <a name="update-calendar-controller"></a><span data-ttu-id="d7220-129">Actualizar el controlador de calendario</span><span class="sxs-lookup"><span data-stu-id="d7220-129">Update calendar controller</span></span>

1. <span data-ttu-id="d7220-130">Abra **./Controllers/CalendarController.CS** y reemplace la `Index` función existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="d7220-130">Open **./Controllers/CalendarController.cs** and replace the existing `Index` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="IndexSnippet":::

1. <span data-ttu-id="d7220-131">Inicie la aplicación, inicie sesión y haga clic en el vínculo **calendario** .</span><span class="sxs-lookup"><span data-stu-id="d7220-131">Start the app, sign in, and click the **Calendar** link.</span></span> <span data-ttu-id="d7220-132">La aplicación ahora debería representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="d7220-132">The app should now render a table of events.</span></span>

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
