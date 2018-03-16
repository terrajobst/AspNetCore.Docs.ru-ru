---
title: "Реализация веб-сервера HTTP.sys в ASP.NET Core"
author: tdykstra
description: "Общие сведения о веб-сервере HTTP.sys для ASP.NET Core в Windows. Веб-сервер HTTP.sys на основе работающего в режиме ядра драйвера Http.Sys — это альтернатива Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d7ae6c070c7eecfd714086e15f32eff96c0943d9
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Реализация веб-сервера HTTP.sys в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Люк Латам](https://github.com/guardrex) (Luke Latham)

> [!NOTE]
> Сведения из этого раздела распространяются на ASP.NET Core 2.0 и более поздних версий. В более ранних версиях ASP.NET Core HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) — это [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index), который запускается только в Windows. HTTP.sys является альтернативой [Kestrel](xref:fundamentals/servers/kestrel), предлагая некоторые функции, отсутствующие в Kestrel.

> [!IMPORTANT]
> HTTP.sys не подходит для использования с IIS или IIS Express из-за несовместимости с [модулем ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

HTTP.sys поддерживает следующие функции:

* [Аутентификация Windows](xref:security/authentication/windowsauth)
* Совместное использование портов
* Использование HTTPS с SNI
* Использование HTTP/2 через TLS (Windows 10 и более поздние версии)
* Прямая передача файлов
* Кэширование откликов
* Использование WebSockets (Windows 8 и более поздние версии)

Поддерживаемые версии Windows:

* Windows 7 и более поздние версии
* Windows Server 2008 R2 и более поздние версии

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Условия для применения HTTP.sys

HTTP.sys удобно использовать с развертываниями в таких случаях:

* когда нужно подключить сервер к Интернету напрямую без использования служб IIS;

  ![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

* когда для внутренних развертываний нужна функция, отсутствующая в Kestrel, например [аутентификация Windows](xref:security/authentication/windowsauth).

  ![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

HTTP.sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера. Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх HTTP.sys. 

## <a name="how-to-use-httpsys"></a>Способы применения HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Настройка приложения ASP.NET Core для использования HTTP.sys

1. Ссылка на пакет в файле проекта не требуется при использовании [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)) (ASP.NET Core 2.0 или более поздней версии). Если метапакет `Microsoft.AspNetCore.All` не используется, добавьте ссылку на пакет в файл [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

1. Вызовите метод расширения [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) при создании веб-узла, указав все необходимые [параметры HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Дополнительная настройка HTTP.sys выполняется с помощью [параметров реестра](https://support.microsoft.com/kb/820129).

   **Параметры HTTP.sys**

   | Свойство | Описание | Значение по умолчанию |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Указывает, разрешен ли синхронные операции ввода-вывода для `HttpContext.Request.Body` и `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Разрешает анонимные запросы. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Указывает разрешенные схемы аутентификации. Может быть изменен в любое время до удаления прослушивателя. Предоставляет значения, полученные при [перечислении AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None` и `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Выполняет попытку кэшировать [режим ядра](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) для ответов с допустимыми заголовками. Ответ не может включать заголовки `Set-Cookie`, `Vary` или `Pragma`. Он должен включать заголовок `Cache-Control` со значением `public`, а также значение `shared-max-age` или `max-age` заголовок `Expires`. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Максимальное число одновременных попыток. | 5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Максимальное число попыток установить одновременное подключение. Использует `-1` для бесконечных циклов. Использует `null` для работы с параметром реестра на уровне компьютера. | `null`<br>Без ограничений. |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | См. раздел <a href="#maxrequestbodysize">MaxRequestBodySize</a>. | 30 000 000 байт.<br>(~28,6 МБ). |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Максимально допустимое число запросов в очереди. | 1000. |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Указывает, следует ли вызывать исключение или завершать работу нормально, когда запись текста ответа завершается ошибкой из-за отключения клиента. | `false`<br>Нормальное завершение. |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Предоставляет конфигурацию [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) HTTP.sys, которую также можно настроить в реестре. Дополнительные сведения о каждом параметре, включая значения по умолчанию, см. здесь:<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) — время, выделенное API сервера HTTP, для очистки текста сущности при активном подключении.</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) — время, выделенное на получение текста сущности запроса.</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) — время, выделенное для API сервера HTTP для выполнения синтаксического анализа заголовка запроса.</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) — время, выделенное для неактивного подключения.</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) — минимальная скорость отправки ответа.</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) — время, выделенное для пребывания запроса в очереди до его получения приложением.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Указывает [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) для регистрации с использованием HTTP.sys. Удобнее всего использовать параметр [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), который добавляет префикс к коллекции. Могут быть изменены в любое время до удаления прослушивателя. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Максимально допустимый размер текста запроса в байтах. Если задано значение `null`, размер максимального запроса не ограничен. Это ограничение не оказывает влияния на обновленные подключения, которые не имеют ограничений.

   Чтобы переопределить это ограничение в приложении ASP.NET Core MVC для `IActionResult`, рекомендуется использовать атрибут [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) в методе действия:
   
   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   При попытке приложения настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение. Свойство `IsReadOnly` указывает на то, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.

   Если приложение должно переопределять [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) для каждого запроса, используйте [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

1. При использовании Visual Studio убедитесь, что приложение не настроено для запуска IIS или IIS Express.

   В Visual Studio профиль запуска по умолчанию использует IIS Express. Чтобы запустить проект как консольное приложение, измените выбранный профиль вручную, как показано на следующем снимке экрана.

   ![Выбор профиля консольного приложения](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Настройка Windows Server

1. Если приложение является [развертыванием, не зависящим от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), установите NET Core или .NET Framework (или обе платформы, если это приложение .NET Core, предназначенное для .NET Framework).

   * **.NET Core** — если приложению требуется .NET Core, скачайте и запустите установщик [.NET Core отсюда](https://www.microsoft.com/net/download/windows).
   * **.NET Framework** — если приложению требуется .NET Framework, инструкции по установке см. в руководстве по [установке .NET Framework](/dotnet/framework/install/). Установите требуемую платформу .NET Framework. Установщик последней версии .NET Framework можно скачать [отсюда](https://www.microsoft.com/net/download/windows).

1. Настройте URL-адреса и порты для приложения.

   По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`. Чтобы настроить префиксы URL-адресов и порты, используйте следующие параметры:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls).
   * Аргументы командной строки `urls`.
   * Переменная среды `ASPNETCORE_URLS`.
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes).

   В примере кода ниже показано, как используется [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Преимущество `UrlPrefixes` заключается в том, что при неправильном формате префиксов сразу же создается сообщение об ошибке.

   Этот параметр в `UrlPrefixes` переопределяет параметры `UseUrls`/`urls`/`ASPNETCORE_URLS`. Таким образом, преимущество переменных среды`UseUrls`, `urls`и `ASPNETCORE_URLS` заключается в возможности быстрого переключения между Kestrel и HTTP.sys. Дополнительные сведения по `UseUrls`, `urls` и `ASPNETCORE_URLS` см. в инструкциях по [размещению](xref:fundamentals/hosting).

   HTTP.sys использует [форматы строк UrlPrefix API сервера HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > **Не используйте** привязку с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне. Это может создать уязвимость и поставить под угрозу ваше приложение. Сюда относятся и строгие, и нестрогие подстановочные знаки. Вместо этого используйте имена узлов в явном виде. Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость). Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

1. Предварительно зарегистрируйте префиксы URL-адресов для привязки к серверу HTTP.sys и настройте сертификаты x.509.

   Если URL-префиксы предварительно не зарегистрированы в Windows, приложение можно запустить с правами администратора. Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.

   1. Встроенным средством для настройки сервера HTTP.sys является *netsh.exe*. С помощью*netsh.exe* можно зарезервировать префиксы URL-адресов и назначить сертификаты X.509. Для использования этого средства требуются права администратора.

      Следующий пример показывает команды для резервирования префиксов URL-адресов для портов 80 и 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Следующий пример показывает, как назначить сертификат X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
      ```

      Дополнительные сведения см. в справочной документации по *netsh.exe*:

      * [Команды netsh для протокола HTTP](https://technet.microsoft.com/library/cc725882.aspx)
      * [Строки UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   1. При необходимости создайте самозаверяющие сертификаты X.509.

     [!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

1. Откройте порты брандмауэра, чтобы разрешить трафик в HTTP.sys. Можно использовать средство *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [API сервера HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [Репозиторий GitHub ASPNET/HttpSysServer (исходный код)](https://github.com/aspnet/HttpSysServer/)
* [Размещение](xref:fundamentals/hosting)
