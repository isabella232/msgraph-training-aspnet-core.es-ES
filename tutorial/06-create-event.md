---
ms.openlocfilehash: f70714a0bc2588fc67d63096b4ab746380bb8e2c
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822397"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

## <a name="create-model"></a>Crear modelo

1. Cree un nuevo archivo denominado **NewEvent.CS** en el directorio **./Models** y agregue el siguiente código.

    :::code language="csharp" source="../demo/GraphTutorial/Models/NewEvent.cs" id="NewEventSnippet":::

## <a name="create-view"></a>Crear vista

1. Cree un nuevo archivo denominado **New. cshtml** en el directorio **/views/Calendar** y agregue el siguiente código.

    :::code language="cshtml" source="../demo/GraphTutorial/Views/Calendar/New.cshtml" id="NewFormSnippet":::

## <a name="add-controller-actions"></a>Agregar acciones de controlador

1. Abra **./Controllers/CalendarController.CS** y agregue la siguiente acción a la `CalendarController` clase para representar el nuevo formulario de evento.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="CalendarNewGetSnippet":::

1. Agregue la siguiente acción a la `CalendarController` clase para recibir el nuevo evento del formulario cuando el usuario haga clic en **Guardar** y use Microsoft Graph para agregar el evento al calendario del usuario.

    :::code language="csharp" source="../demo/GraphTutorial/Controllers/CalendarController.cs" id="CalendarNewPostSnippet":::

1. Inicie la aplicación, inicie sesión y haga clic en el vínculo **calendario** . Haga clic en el botón **nuevo evento** , rellene el formulario y haga clic en **Guardar**.

    ![Captura de pantalla del nuevo formulario de eventos](./images/create-event-01.png)
