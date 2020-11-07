---
ms.openlocfilehash: ed1bb3d97791ecfdd3b63a1bb4a1dfd63f2f03bf
ms.sourcegitcommit: 9d0d10a9e8e5a1d80382d89bc412df287bee03f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "48822526"
---
<!-- markdownlint-disable MD002 MD041 -->

Este tutorial le enseña a crear una aplicación Web de ASP.NET Core que use la API de Microsoft Graph para recuperar la información de calendario de un usuario.

> [!TIP]
> Si prefiere descargar solo el tutorial completo, puede descargar o clonar el repositorio de [GitHub](https://github.com/microsoftgraph/msgraph-training-aspnet-core). Consulte el archivo Léame de la carpeta **Demo** para obtener instrucciones sobre cómo configurar la aplicación con un identificador de aplicación y un secreto.

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar este tutorial, debe tener instalado [.net Core SDK](https://dotnet.microsoft.com/download) en el equipo de desarrollo. Si no tiene el SDK, visite el vínculo anterior para las opciones de descarga.

También debe tener una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft. Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

> [!NOTE]
> Este tutorial se ha escrito con .NET Core SDK versión 3.1.201. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.

## <a name="feedback"></a>Comentarios

Envíe sus comentarios sobre este tutorial en el [repositorio de github](https://github.com/microsoftgraph/msgraph-training-aspnet-core).
