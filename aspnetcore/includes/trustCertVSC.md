---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472338"
---
*  Настройте доверие сертификату разработки HTTPS с помощью следующей команды:

    ```console
    dotnet dev-certs https --trust
    ```

    Приведенная выше команда отображает следующее диалоговое окно.

    ![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

*    Выберите **Да**, если согласны доверять сертификату разработки.

     Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).