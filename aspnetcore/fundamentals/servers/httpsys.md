---
title: "Реализации веб-сервера HTTP.sys в ASP.NET Core"
author: rick-anderson
description: "Общие сведения о веб-сервере HTTP.sys для ASP.NET Core в Windows. HTTP.sys, основанный на работающем в режиме ядра драйвере Http.Sys, является альтернативой для Kestrel, которую можно использовать для прямого подключения к Интернету без служб IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Реализации веб-сервера HTTP.sys в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

> [!NOTE]
> Сведения из этого раздела распространяются только на ASP.NET Core 2.0 и более поздних версий. В более ранних версиях ASP.NET Core HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys — это [веб-сервер для ASP.NET Core](index.md), который запускается только в Windows. Он основан на [работающем в режиме ядра драйвере Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys является альтернативой для [Kestrel](kestrel.md) и предлагает некоторые функции, отсутствующие в Kestel. **HTTP.sys не подходит для использования с IIS или IIS Express, так как он не совместим с [модулем ASP.NET Core](aspnet-core-module.md).**

HTTP.sys поддерживает следующие функции:

- [Проверка подлинности Windows](xref:security/authentication/windowsauth)
- Совместное использование портов
- HTTPS с SNI
- HTTP/2 через TLS (Windows 10)
- Прямая передача файлов
- Кэширование откликов
- WebSockets (Windows 8)

Поддерживаемые версии Windows:

- Windows 7 и Windows Server 2008 R2 и более поздних версий

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Условия для применения HTTP.sys

HTTP.sys удобен для развертываний, где нужно подключить сервер к Интернету напрямую без использования служб IIS.

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

Так как HTTP.sys основан на Http.Sys, ему не нужен обратный прокси-сервер для защиты от атак. Http.Sys — это проверенная технология, которая защищает от многих типов атак, а также обеспечивает надежность, безопасность и масштабируемость полнофункционального веб-сервера. Сами службы IIS выполняются в качестве HTTP-прослушивателя поверх Http.Sys. 

HTTP.sys хорошо подходит для внутренних развертываний, когда нужна функция, отсутствующая в Kestrel, например проверка подлинности Windows.

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Способы применения HTTP.sys

Ниже приведен обзор задач установки для ОС узла и приложения ASP.NET Core.

### <a name="configure-windows-server"></a>Настройка Windows Server

* Установите нужную вашему приложению версию платформы .NET, например [.NET Core](https://www.microsoft.com/net/download/core) или [.NET Framework](https://www.microsoft.com/net/download/framework).

* Предварительно зарегистрируйте префиксы URL-адресов для привязки к HTTP.sys и настройте SSL-сертификаты.

   Если вы не выполняете предварительную регистрацию префиксов URL-адресов в Windows, приложение потребуется запустить с правами администратора. Единственным исключением является привязка к localhost через HTTP (не HTTPS) с номером порта больше 1024. В этом случае права администратора не требуются.

   Дополнительные сведения см. в разделе [Предварительная регистрация префиксов и настройка SSL](#preregister-url-prefixes-and-configure-ssl) ниже.

* Откройте порты брандмауэра, чтобы разрешить трафик в HTTP.sys.

   Можно использовать *netsh.exe* или [командлеты PowerShell](https://technet.microsoft.com/library/jj554906).

Кроме того, существуют [параметры реестра Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Настройка приложения ASP.NET Core для использования HTTP.sys

* При использовании метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) установка никаких пакетов не требуется. Пакет [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) входит в состав метапакета.

* Вызовите метод расширения `UseHttpSys` для `WebHostBuilder` в вашем методе `Main`, указав все нужные [параметры HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs), как показано в следующем примере:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Настройка параметров HTTP.sys

Ниже приведены некоторые ограничения и параметры HTTP.sys, которые можно настроить.

**Максимальное число клиентских подключений**

Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода в файле *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

По умолчанию максимальное число подключений не ограничено (null).

**Maximum request body size** (Максимальный размер текста запроса)

По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.

Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) в методе действия:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Приведенный ниже пример показывает, как настроить ограничение для всего приложения и каждого запроса:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Можно переопределить параметр для конкретного запроса в файле *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение. Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.

Дополнительные сведения о других параметрах HTTP.sys см. в разделе [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Настройка URL-адресов и портов для прослушивания 

По умолчанию ASP.NET Core привязан к `http://localhost:5000`. Чтобы настроить префиксы URL-адресов и порты, можно использовать метод расширения `UseUrls`, аргумент командной строки `urls`, переменную среды ASPNETCORE_URLS или свойство `UrlPrefixes` объекта [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Приведенный ниже пример кода использует `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Преимуществом `UrlPrefixes` является то, что вы получаете сообщение об ошибке сразу, как только попытаетесь добавить префикс в неправильном формате. Преимуществом `UseUrls` (совместно с `urls` и ASPNETCORE_URLS) является упрощенное переключение между Kestrel и HTTP.sys.

Если вы используете как `UseUrls` (либо `urls` или ASPNETCORE_URLS), так и `UrlPrefixes`, параметры в `UrlPrefixes` переопределяют значения в `UseUrls`. Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).

HTTP.sys использует [форматы строк UrlPrefix API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Убедитесь, что указали одинаковые строки префикса в `UseUrls` или `UrlPrefixes`, предварительно регистрируемых на сервере. 

### <a name="dont-use-iis"></a>Отказ от использования IIS

Убедитесь, что приложение не настроено для запуска IIS или IIS Express.

В Visual Studio профиль запуска по умолчанию использует IIS Express. Чтобы запустить проект как консольное приложение, вручную измените выбранный профиль, как показано на следующем снимке экрана.

![Выбор профиля консольного приложения](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Предварительная регистрация префиксов URL-адресов и настройка SSL

Как IIS, так и HTTP.sys основаны на работающем в режиме ядра драйвере Http.Sys, который осуществляет прослушивание запросов и первоначальную обработку. В IIS пользовательский интерфейс управления позволяет сравнительно легко настраивать все необходимое. Однако Http.Sys нужно настроить самостоятельно. Для этого служит встроенное средство *netsh.exe*. 

С помощью*netsh.exe* можно зарезервировать префиксы URL-адресов и назначить SSL-сертификаты. Для этого средства нужны права администратора.

Следующий пример показывает минимальный код, позволяющий зарезервировать префиксы URL-адресов для портов 80 и 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Следующий пример показывает, как назначить SSL-сертификат:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Ниже указана справочная документация по *netsh.exe*:

* [Команды Netsh для протокола HTTP](https://technet.microsoft.com/library/cc725882.aspx)
* [Строки UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Приведенные ниже ресурсы содержат подробные инструкции по нескольким сценариям. Статьи, описывающие HttpListener, в равной степени применимы и к HTTP.sys, так как оба этих компонента основаны на Http.Sys.

* [Практическое руководство. Настройка порта с использованием SSL-сертификата](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Взаимодействие через HTTPS: размещение и сертификация клиента на основе HttpListener](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) — это довольно старый блог сторонних разработчиков, однако в нем все равно есть полезные сведения.
* [Практическое руководство. Пошаговые инструкции по использованию HttpListener или неуправляемого кода HTTP-сервера (C++) в качестве простого сервера SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) — это еще один старый блог с полезными сведениями.

Ниже приведены некоторые сторонние средства, которые могут быть удобнее, чем командная строка *netsh.exe*. Они не предоставляются и не поддерживаются корпорацией Майкрософт. По умолчанию эти средства запускаются от имени администратора, так как права администратора нужны и *netsh.exe*.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) предоставляет пользовательский интерфейс для перечисления и настройки SSL-сертификатов и параметров, резервирований префиксов и списков доверия сертификатов. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) позволяет перечислить или настроить SSL-сертификаты и префиксы URL-адресов. По сравнению с http.sys Manager пользовательский интерфейс более подробный и предоставляет несколько дополнительных параметров конфигурации, а в остальном аналогичен. В нем невозможно создать список доверия сертификатов (CTL), но можно назначить уже существующий.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Пример приложения для этой статьи](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Исходный код HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Размещение](xref:fundamentals/hosting)
