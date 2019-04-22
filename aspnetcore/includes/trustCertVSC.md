---
ms.openlocfilehash: 260f774fdba4d16a4fcb00ac1c699acf4d1bf5b5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615402"
---
* Настройте доверие сертификату разработки HTTPS с помощью следующей команды:

  ```console
  dotnet dev-certs https --trust
  ```

  Приведенная выше команда отображает следующее диалоговое окно.

  ![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

* Выберите **Да**, если согласны доверять сертификату разработки.

  Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
  
