---
ms.openlocfilehash: 30398bafbf47aaa374a200dd1834c9f2003e967f
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822443"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="bfe0b-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="bfe0b-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfe0b-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bfe0b-102">Prerequisites</span></span>

<span data-ttu-id="bfe0b-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bfe0b-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="bfe0b-104">[.Net Core SDK](https://dotnet.microsoft.com/download) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="bfe0b-105">( **Nota:** este tutorial se ha escrito con .net Core SDK versión 3.1.201.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-105">( **Note:** This tutorial was written with .NET Core SDK version 3.1.201.</span></span> <span data-ttu-id="bfe0b-106">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="bfe0b-107">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="bfe0b-108">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="bfe0b-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="bfe0b-109">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="bfe0b-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="bfe0b-110">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="bfe0b-111">Registro de una aplicación web con el centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bfe0b-111">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="bfe0b-112">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bfe0b-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="bfe0b-113">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-113">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="bfe0b-114">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="bfe0b-115">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="bfe0b-115">A screenshot of the App registrations</span></span> ](../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="bfe0b-116">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-116">Select **New registration**.</span></span> <span data-ttu-id="bfe0b-117">En la página **Registrar una aplicación** , establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="bfe0b-118">Establezca **Nombre** como `ASP.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-118">Set **Name** to `ASP.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="bfe0b-119">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="bfe0b-120">En **URI de redirección** , establezca la primera lista desplegable en `Web` y establezca el valor `https://localhost:5001/`.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-120">Under **Redirect URI** , set the first drop-down to `Web` and set the value to `https://localhost:5001/`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="bfe0b-122">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-122">Select **Register**.</span></span> <span data-ttu-id="bfe0b-123">En la página del **tutorial de ASP.net Core Graph** , copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-123">On the **ASP.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](../tutorial/images/aad-application-id.png)

1. <span data-ttu-id="bfe0b-125">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-125">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="bfe0b-126">En **URI de redireccionamiento** , agregue un URI con el valor `https://localhost:5001/signin-oidc` .</span><span class="sxs-lookup"><span data-stu-id="bfe0b-126">Under **Redirect URIs** add a URI with the value `https://localhost:5001/signin-oidc`.</span></span>

1. <span data-ttu-id="bfe0b-127">Establezca la **dirección URL de cierre de sesión** en `https://localhost:5001/signout-oidc` .</span><span class="sxs-lookup"><span data-stu-id="bfe0b-127">Set the **Logout URL** to `https://localhost:5001/signout-oidc`.</span></span>

1. <span data-ttu-id="bfe0b-128">Busque la sección **Concesión implícita** y habilite los **tokens de ID**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-128">Locate the **Implicit grant** section and enable **ID tokens**.</span></span> <span data-ttu-id="bfe0b-129">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-129">Select **Save**.</span></span>

    ![Una captura de pantalla de la configuración de la plataforma web en Azure portal](../tutorial/images/aad-web-platform.png)

1. <span data-ttu-id="bfe0b-131">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-131">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="bfe0b-132">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-132">Select the **New client secret** button.</span></span> <span data-ttu-id="bfe0b-133">Escriba un valor en **Descripción** y seleccione una de las opciones para **Expires** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-133">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

    ![Captura de pantalla del cuadro de diálogo Agregar un secreto de cliente](../tutorial/images/aad-new-client-secret.png)

1. <span data-ttu-id="bfe0b-135">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-135">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="bfe0b-136">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-136">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="bfe0b-137">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-137">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Captura de pantalla del secreto de cliente recién agregado](../tutorial/images/aad-copy-client-secret.png)

## <a name="configure-the-sample"></a><span data-ttu-id="bfe0b-139">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="bfe0b-139">Configure the sample</span></span>

1. <span data-ttu-id="bfe0b-140">Abra la interfaz de línea de comandos (CLI) en el directorio donde se encuentra **GraphTutorial. csproj** y ejecute los siguientes comandos, sustituyendo `YOUR_APP_ID` con el identificador de la aplicación del portal de Azure y `YOUR_APP_SECRET` con el secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-140">Open your command line interface (CLI) in the directory where **GraphTutorial.csproj** is located, and run the following commands, substituting `YOUR_APP_ID` with your application ID from the Azure portal, and `YOUR_APP_SECRET` with your application secret.</span></span>

    ```Shell
    dotnet user-secrets init
    dotnet user-secrets set "AzureAd:ClientId" "YOUR_APP_ID"
    dotnet user-secrets set "AzureAd:ClientSecret" "YOUR_APP_SECRET"
    ```

## <a name="run-the-sample"></a><span data-ttu-id="bfe0b-141">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="bfe0b-141">Run the sample</span></span>

<span data-ttu-id="bfe0b-142">En la CLI, ejecute el siguiente comando para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bfe0b-142">In your CLI, run the following command to start the application.</span></span>

```Shell
dotnet run
```
