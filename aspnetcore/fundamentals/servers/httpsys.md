---
title: Реализация веб-сервера HTTP.sys в ASP.NET Core
author: guardrex
description: Общие сведения о веб-сервере HTTP.sys для ASP.NET Core в Windows. Веб-сервер HTTP.sys на основе работающего в режиме ядра драйвера Http.Sys — это альтернатива Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: a779fee53109d4c1cabb2005896e757f23467540
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637629"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Реализация веб-сервера HTTP.sys в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Люк Латам](https://github.com/guardrex) (Luke Latham)

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) — это [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index), который запускается только в Windows. HTTP.sys является альтернативой серверу [Kestrel](xref:fundamentals/servers/kestrel), предлагая некоторые функции, отсутствующие в Kestrel.

> [!IMPORTANT]
> HTTP.sys не подходит для использования с IIS или IIS Express из-за несовместимости с [модулем ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

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

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Условия для применения HTTP.sys

HTTP.sys удобно использовать с развертываниями в таких случаях:

* когда нужно подключить сервер к Интернету напрямую без использования служб IIS;

  ![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

* когда для внутренних развертываний нужна функция, отсутствующая в Kestrel, например [аутентификация Windows](xref:security/authentication/windowsauth).

  ![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

HTTP.sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера. Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх HTTP.sys.

## <a name="http2-support"></a>Поддержка HTTP/2

Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) включен для приложений ASP.NET Core, если выполнены следующие базовые требования:

* установлена ОС Windows Server 2016 либо Windows 10 или более поздних версий;
* установлено подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3);
* установлено подключение TLS 1.2 или более поздней версии.

::: moniker range=">= aspnetcore-2.2"

Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/1.1`.

::: moniker-end

Протокол HTTP/2 включен по умолчанию. Если не удается установить подключение HTTP/2, применяется резервный вариант HTTP/1.1. В будущих версиях Windows будут доступны флаги конфигурации HTTP/2, в том числе возможность отключения HTTP/2 с использованием HTTP.sys.

## <a name="kernel-mode-authentication-with-kerberos"></a>Проверка подлинности в режиме ядра с помощью Kerberos

HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos. Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys. Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя. Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.

## <a name="how-to-use-httpsys"></a>Способы применения HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Настройка приложения ASP.NET Core для использования HTTP.sys

1. Ссылка на пакет в файле проекта не требуется при использовании [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 или более поздней версии). Если метапакет `Microsoft.AspNetCore.App` не используется, добавьте ссылку на пакет в файл [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Вызовите метод расширения [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) при создании веб-узла, указав все необходимые [параметры HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Дополнительная настройка HTTP.sys выполняется с помощью [параметров реестра](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

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
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Предоставляет конфигурацию [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) HTTP.sys, которую также можно настроить в реестре. Дополнительные сведения о каждом параметре, включая значения по умолчанию, см. здесь:<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; время, выделенное для API сервера HTTP для очистки текста сущности при активном подключении.</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; время, выделенное на получение текста сущности запроса.</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; время, выделенное для API сервера HTTP для выполнения синтаксического анализа заголовка запроса.</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; время, выделенное для неактивного подключения.</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; минимальная скорость отправки ответа.</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; время, выделенное для пребывания запроса в очереди до его получения приложением.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes). | Указывает [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) для регистрации с использованием HTTP.sys. Удобнее всего использовать параметр [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), который добавляет префикс к коллекции. Могут быть изменены в любое время до удаления прослушивателя. |  |

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

3. При использовании Visual Studio убедитесь, что приложение не настроено для запуска IIS или IIS Express.

   В Visual Studio профиль запуска по умолчанию использует IIS Express. Чтобы запустить проект как консольное приложение, измените выбранный профиль вручную, как показано на следующем снимке экрана.

   ![Выбор профиля консольного приложения](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Настройка Windows Server

1. Если приложение является [развертыванием, не зависящим от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd), установите NET Core или .NET Framework (или обе платформы, если это приложение .NET Core, предназначенное для .NET Framework).

   * **.NET Core** &ndash; если приложению требуется .NET Core, скачайте и запустите установщик .NET Core из раздела [Все загрузки .NET Core](https://www.microsoft.com/net/download/all).
   * **.NET Framework** &ndash; если приложение требует .NET Framework, см. раздел [.NET Framework: руководство по установке](/dotnet/framework/install/), чтобы ознакомиться с инструкциями по установке. Установите требуемую платформу .NET Framework. Установщик последней версии .NET Framework можно скачать [отсюда](https://www.microsoft.com/net/download/all).

2. Настройте URL-адреса и порты для приложения.

   По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`. Чтобы настроить префиксы URL-адресов и порты, используйте следующие параметры:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls).
   * Аргументы командной строки `urls`.
   * Переменная среды `ASPNETCORE_URLS`.
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes).

   В примере кода ниже показано, как используется [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Преимущество `UrlPrefixes` заключается в том, что при неправильном формате префиксов сразу же создается сообщение об ошибке.

   Этот параметр в `UrlPrefixes` переопределяет параметры `UseUrls`/`urls`/`ASPNETCORE_URLS`. Таким образом, преимущество переменных среды`UseUrls`, `urls`и `ASPNETCORE_URLS` заключается в возможности быстрого переключения между Kestrel и HTTP.sys. Дополнительные сведения о `UseUrls`, `urls` и `ASPNETCORE_URLS` см. в статье [Размещение в ASP.NET Core](xref:fundamentals/host/index).

   HTTP.sys использует [форматы строк UrlPrefix API сервера HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > **Не используйте** привязки с подстановочными знаками (`http://*:80/` и `http://+:80`) на верхнем уровне. Это может создать уязвимость и поставить ваше приложение под угрозу. Сюда относятся и строгие, и нестрогие подстановочные знаки. Вместо этого используйте имена узлов в явном виде. Привязки с подстановочными знаками на уровне дочерних доменов (например `*.mysub.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость). Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

3. Предварительно зарегистрируйте префиксы URL-адресов для привязки к серверу HTTP.sys и настройте сертификаты x.509.

   Если URL-префиксы предварительно не зарегистрированы в Windows, приложение можно запустить с правами администратора. Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.

   1. Встроенным средством для настройки сервера HTTP.sys является *netsh.exe*. С помощью*netsh.exe* можно зарезервировать префиксы URL-адресов и назначить сертификаты X.509. Для использования этого средства требуются права администратора.

      Следующий пример показывает команды для резервирования префиксов URL-адресов для портов 80 и 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Следующий пример показывает, как назначить сертификат X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Дополнительные сведения см. в справочной документации по *netsh.exe*:

      * [Команды netsh для протокола HTTP](https://technet.microsoft.com/library/cc725882.aspx)
      * [Строки UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. При необходимости создайте самозаверяющие сертификаты X.509.

      [!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

4. Откройте порты брандмауэра, чтобы разрешить трафик в HTTP.sys. Можно использовать средство *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для приложений, размещенных с помощью файла HTTP.sys, которые взаимодействуют с запросами из Интернета или корпоративной сети, может потребоваться дополнительная настройка при размещении за прокси-серверами и подсистемами балансировки нагрузки. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [API сервера HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [Репозиторий GitHub ASPNET/HttpSysServer (исходный код)](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
