---
title: Устранение неполадок и отладка проектов ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: 73a73fb51571e5f7b706ff4b958217854750c1fb
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354714"
---
# <a name="troubleshoot-and-debug-aspnet-core-projects"></a>Устранение неполадок и отладка проектов ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Рекомендации по устранению неполадок приведены по следующим ссылкам:

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Конференция NDC (Лондон, 2018): Диагностика проблем в ASP.NET Core приложениях](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Блог ASP.NET: Устранение неполадок ASP.NET Core проблем с производительностью](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Предупреждения пакет SDK для .NET Core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Установлены как 32-разрядные, так и 64-разрядные версии пакет SDK для .NET Core

В диалоговом окне **Новый проект** для ASP.NET Core может отобразиться следующее предупреждение:

> Устанавливаются как 32-разрядные, так и 64-разрядные версии пакет SDK для .NET Core. Отображаются только шаблоны из 64-разрядных версий, установленных в папке "C:\\Program Files\\DotNet\\SDK\\".

Это предупреждение появляется при установке 32-разрядных (x86) и 64-разрядных (x64) версий [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) . Ниже перечислены распространенные причины, по которым можно установить обе версии:

* Вы первоначально загрузили установщик пакет SDK для .NET Core с помощью 32-разрядного компьютера, но затем скопировали его и установили на 64-разрядном компьютере.
* 32-разрядная пакет SDK для .NET Core была установлена другим приложением.
* Была загружена и установлена неправильная версия.

Удалите 32-разрядную пакет SDK для .NET Core, чтобы предотвратить это предупреждение. Удаление из **панели управления** > **программы и компоненты** > **Удаление или изменение программы**. Если вы понимаете, почему возникает предупреждение и его последствия, вы можете проигнорировать это предупреждение.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Пакет SDK для .NET Core устанавливается в нескольких расположениях

В диалоговом окне **Новый проект** для ASP.NET Core может отобразиться следующее предупреждение:

> Пакет SDK для .NET Core устанавливается в нескольких расположениях. Отображаются только шаблоны из пакетов SDK, установленных в папке "C:\\Program Files\\DotNet\\SDK\\".

Это сообщение появляется при наличии хотя бы одной установки пакет SDK для .NET Core в каталоге вне файла *C:\\Program files\\пакета SDK dotnet\\\\* . Обычно это происходит, когда пакет SDK для .NET Core развертывается на компьютере с помощью команды копировать/вставить вместо установщика MSI.

Удалите все 32-разрядные пакеты SDK и среды выполнения .NET Core, чтобы предотвратить это предупреждение. Удаление из **панели управления** > **программы и компоненты** > **Удаление или изменение программы**. Если вы понимаете, почему возникает предупреждение и его последствия, вы можете проигнорировать это предупреждение.

### <a name="no-net-core-sdks-were-detected"></a>Пакеты SDK для .NET Core не обнаружены

* В диалоговом окне **Новый проект** Visual Studio для ASP.NET Core может отобразиться следующее предупреждение:

  > Пакеты SDK для .NET Core не обнаружены, убедитесь, что они включены в переменную среды `PATH`.

* При выполнении команды `dotnet` предупреждение выглядит следующим образом:

  > Не удалось найти установленные пакеты SDK DotNet.

Эти предупреждения появляются, когда переменная среды `PATH` не указывает на пакеты SDK .NET Core на компьютере. Для устранения этой проблемы выполните следующие действия:

* Установите пакет SDK для .NET Core. Получите последнюю версию установщика из [скачивания .NET](https://dotnet.microsoft.com/download).
* Убедитесь, что переменная среды `PATH` указывает на расположение, где установлен пакет SDK (`C:\Program Files\dotnet\` для 64-разрядной версии, x64 или `C:\Program Files (x86)\dotnet\` для 32-разрядной версии/x86). Установщик пакета SDK обычно задает `PATH`. Всегда устанавливайте одинаковые пакеты SDK и среды выполнения разрядов на одном компьютере.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>Отсутствует пакет SDK после установки пакета размещения .NET Core

Установка [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) изменяет `PATH` при установке среды выполнения .NET Core для указания на 32-разрядную (x86) версию .NET core (`C:\Program Files (x86)\dotnet\`). Это может привести к отсутствию пакетов SDK при использовании 32-разрядной (x86) .NET Core `dotnet` ([пакеты SDK для .NET Core не обнаружены](#no-net-core-sdks-were-detected)). Чтобы устранить эту проблему, переместите `C:\Program Files\dotnet\` в нужное положение перед `C:\Program Files (x86)\dotnet\` `PATH`.

## <a name="obtain-data-from-an-app"></a>Получение данных из приложения

Если приложение может отвечать на запросы, можно получить следующие данные из приложения с помощью по промежуточного слоя:

* Запрос &ndash; метод, схема, узел, пасбасе, путь, строка запроса, заголовки
* Удаленный IP-адрес &ndash; подключения, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента
* Имя &ndash; удостоверения, отображаемое имя
* Параметры конфигурации
* Переменные среды

Поместите следующий код по [промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в начало конвейера обработки запроса `Startup.Configure` метода. Среда проверяется перед выполнением по промежуточного слоя, чтобы гарантировать выполнение кода только в среде разработки.

Чтобы получить среду, используйте один из следующих подходов:

* Вставьте `IHostingEnvironment` в метод `Startup.Configure` и проверьте окружение с помощью локальной переменной. Этот подход демонстрируется в следующем образце кода.

* Назначьте среду свойству в классе `Startup`. Проверьте окружение с помощью свойства (например, `if (Environment.IsDevelopment())`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```

## <a name="debug-aspnet-core-apps"></a>Отладка ASP.NET Core приложений

Следующие ссылки содержат сведения об отладке приложений ASP.NET Core.

* [Отладка ASP Core в Linux](https://devblogs.microsoft.com/premier-developer/debugging-asp-core-on-linux-with-visual-studio-2017/)
* [Отладка .NET Core в UNIX через SSH](https://devblogs.microsoft.com/devops/debugging-net-core-on-unix-over-ssh/)
* [Краткое руководство. Отладка ASP.NET с помощью отладчика Visual Studio](/visualstudio/debugger/quickstart-debug-aspnet)
* Дополнительные сведения об отладке см. в [этой статье GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/2960) .
