---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472338"
---
*  <span data-ttu-id="07734-101">Настройте доверие сертификату разработки HTTPS с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="07734-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

    <span data-ttu-id="07734-102">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="07734-102">The preceding command displays the following dialog:</span></span>

    ![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

*    <span data-ttu-id="07734-104">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="07734-104">Select **Yes** if you agree to trust the development certificate.</span></span>

     <span data-ttu-id="07734-105">Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="07734-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>