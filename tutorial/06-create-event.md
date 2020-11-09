---
ms.openlocfilehash: f70714a0bc2588fc67d63096b4ab746380bb8e2c
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822397"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="89073-101">En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="89073-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-model"></a><span data-ttu-id="89073-102">Crear modelo</span><span class="sxs-lookup"><span data-stu-id="89073-102">Create model</span></span>

1. <span data-ttu-id="89073-103">Cree un nuevo archivo denominado **NewEvent.CS** en el directorio **./Models** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="89073-103">Create a new file named **NewEvent.cs** in the **./Models** directory and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

## <a name="create-view"></a><span data-ttu-id="89073-104">Crear vista</span><span class="sxs-lookup"><span data-stu-id="89073-104">Create view</span></span>

1. <span data-ttu-id="89073-105">Cree un nuevo archivo denominado **New. cshtml** en el directorio **/views/Calendar** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="89073-105">Create a new file named **New.cshtml** in he **./Views/Calendar** directory and add the following code.</span></span>

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Calendar/New.cshtml" id="NewFormSnippet":::

## <a name="add-controller-actions"></a><span data-ttu-id="89073-106">Agregar acciones de controlador</span><span class="sxs-lookup"><span data-stu-id="89073-106">Add controller actions</span></span>

1. <span data-ttu-id="89073-107">Abra **./Controllers/CalendarController.CS** y agregue la siguiente acción a la `CalendarController` clase para representar el nuevo formulario de evento.</span><span class="sxs-lookup"><span data-stu-id="89073-107">Open **./Controllers/CalendarController.cs** and add the following action to the `CalendarController` class to render the new event form.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="CalendarNewGetSnippet":::

1. <span data-ttu-id="89073-108">Agregue la siguiente acción a la `CalendarController` clase para recibir el nuevo evento del formulario cuando el usuario haga clic en **Guardar** y use Microsoft Graph para agregar el evento al calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="89073-108">Add the following action to the `CalendarController` class to receive the new event from the form when the user clicks **Save** and use Microsoft Graph to add the event to the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="CalendarNewPostSnippet":::

1. <span data-ttu-id="89073-109">Inicie la aplicación, inicie sesión y haga clic en el vínculo **calendario** .</span><span class="sxs-lookup"><span data-stu-id="89073-109">Start the app, sign in, and click the **Calendar** link.</span></span> <span data-ttu-id="89073-110">Haga clic en el botón **nuevo evento** , rellene el formulario y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="89073-110">Click the **New event** button, fill in the form, and click **Save**.</span></span>

    ![Captura de pantalla del nuevo formulario de eventos](./images/create-event-01.png)
