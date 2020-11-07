---
ms.openlocfilehash: debd685996df22a83110a14ca585cfafa4e08a0d
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822430"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="11265-101">En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD con el centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="11265-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="11265-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11265-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="11265-103">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="11265-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="11265-104">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="11265-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="11265-105">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="11265-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="11265-106">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="11265-106">Select **New registration**.</span></span> <span data-ttu-id="11265-107">En la página **Registrar una aplicación** , establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="11265-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="11265-108">Establezca **Nombre** como `ASP.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="11265-108">Set **Name** to `ASP.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="11265-109">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="11265-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="11265-110">En **URI de redirección** , establezca la primera lista desplegable en `Web` y establezca el valor `https://localhost:5001/`.</span><span class="sxs-lookup"><span data-stu-id="11265-110">Under **Redirect URI** , set the first drop-down to `Web` and set the value to `https://localhost:5001/`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="11265-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="11265-112">Select **Register**.</span></span> <span data-ttu-id="11265-113">En la página del **tutorial de ASP.net Core Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="11265-113">On the **ASP.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="11265-115">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="11265-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="11265-116">En **URI de redireccionamiento** , agregue un URI con el valor `https://localhost:5001/signin-oidc` .</span><span class="sxs-lookup"><span data-stu-id="11265-116">Under **Redirect URIs** add a URI with the value `https://localhost:5001/signin-oidc`.</span></span>

1. <span data-ttu-id="11265-117">Establezca la **dirección URL de cierre de sesión** en `https://localhost:5001/signout-oidc` .</span><span class="sxs-lookup"><span data-stu-id="11265-117">Set the **Logout URL** to `https://localhost:5001/signout-oidc`.</span></span>

1. <span data-ttu-id="11265-118">Busque la sección **Concesión implícita** y habilite los **tokens de ID**.</span><span class="sxs-lookup"><span data-stu-id="11265-118">Locate the **Implicit grant** section and enable **ID tokens**.</span></span> <span data-ttu-id="11265-119">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="11265-119">Select **Save**.</span></span>

    ![Una captura de pantalla de la configuración de la plataforma web en Azure portal](./images/aad-web-platform.png)

1. <span data-ttu-id="11265-121">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="11265-121">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="11265-122">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="11265-122">Select the **New client secret** button.</span></span> <span data-ttu-id="11265-123">Escriba un valor en **Descripción** y seleccione una de las opciones para **Expires** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="11265-123">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

    ![Captura de pantalla del cuadro de diálogo Agregar un secreto de cliente](./images/aad-new-client-secret.png)

1. <span data-ttu-id="11265-125">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="11265-125">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="11265-126">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="11265-126">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="11265-127">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="11265-127">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Captura de pantalla del secreto de cliente recién agregado](./images/aad-copy-client-secret.png)
