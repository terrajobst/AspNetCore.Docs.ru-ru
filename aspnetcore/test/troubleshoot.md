---
title: Устранение неполадок ASP.NET Core проектов
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: test/troubleshoot
ms.openlocfilehash: b434af2dd046045836d2f6f7f7b7b2d57699bedc
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308284"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="df677-103">Устранение неполадок ASP.NET Core проектов</span><span class="sxs-lookup"><span data-stu-id="df677-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="df677-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="df677-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="df677-105">Рекомендации по устранению неполадок приведены по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="df677-105">The following links provide troubleshooting guidance:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="df677-106">Конференция NDC (Лондон, 2018): Диагностика проблем в ASP.NET Core приложениях</span><span class="sxs-lookup"><span data-stu-id="df677-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="df677-107">Блог ASP.NET: Устранение неполадок ASP.NET Core проблем с производительностью</span><span class="sxs-lookup"><span data-stu-id="df677-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="df677-108">Предупреждения пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="df677-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="df677-109">Установлены как 32-разрядные, так и 64-разрядные версии пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="df677-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="df677-110">В диалоговом окне **Новый проект** для ASP.NET Core может отобразиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="df677-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="df677-111">Устанавливаются как 32-разрядные, так и 64-разрядные версии пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df677-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="df677-112">Отображаются только шаблоны из 64-разрядных версий, установленных на диске\\C:\\Program\\Files DotNet SDK\\.</span><span class="sxs-lookup"><span data-stu-id="df677-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="df677-113">Это предупреждение появляется при установке 32-разрядных (x86) и 64-разрядных (x64) версий [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) .</span><span class="sxs-lookup"><span data-stu-id="df677-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="df677-114">Ниже перечислены распространенные причины, по которым можно установить обе версии:</span><span class="sxs-lookup"><span data-stu-id="df677-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="df677-115">Вы первоначально загрузили установщик пакет SDK для .NET Core с помощью 32-разрядного компьютера, но затем скопировали его и установили на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="df677-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="df677-116">32-разрядная пакет SDK для .NET Core была установлена другим приложением.</span><span class="sxs-lookup"><span data-stu-id="df677-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="df677-117">Была загружена и установлена неправильная версия.</span><span class="sxs-lookup"><span data-stu-id="df677-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="df677-118">Удалите 32-разрядную пакет SDK для .NET Core, чтобы предотвратить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df677-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="df677-119">Удаление из **панели** > управления**программы и компоненты** > **Удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="df677-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="df677-120">Если вы понимаете, почему возникает предупреждение и его последствия, вы можете проигнорировать это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df677-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="df677-121">Пакет SDK для .NET Core устанавливается в нескольких расположениях</span><span class="sxs-lookup"><span data-stu-id="df677-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="df677-122">В диалоговом окне **Новый проект** для ASP.NET Core может отобразиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="df677-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="df677-123">Пакет SDK для .NET Core устанавливается в нескольких расположениях.</span><span class="sxs-lookup"><span data-stu-id="df677-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="df677-124">Отображаются только шаблоны из пакетов SDK, установленных на сайте\\"C\\: Program\\Files DotNet\\SDK".</span><span class="sxs-lookup"><span data-stu-id="df677-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="df677-125">Это сообщение появляется при наличии хотя бы одной установки пакет SDK для .NET Core в каталоге за пределами *C:\\Program\\Files DotNet\\SDK\\* .</span><span class="sxs-lookup"><span data-stu-id="df677-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="df677-126">Обычно это происходит, когда пакет SDK для .NET Core развертывается на компьютере с помощью команды копировать/вставить вместо установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="df677-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="df677-127">Удалите все 32-разрядные пакеты SDK и среды выполнения .NET Core, чтобы предотвратить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df677-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="df677-128">Удаление из **панели** > управления**программы и компоненты** > **Удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="df677-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="df677-129">Если вы понимаете, почему возникает предупреждение и его последствия, вы можете проигнорировать это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df677-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="df677-130">Пакеты SDK для .NET Core не обнаружены</span><span class="sxs-lookup"><span data-stu-id="df677-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="df677-131">В диалоговом окне **Новый проект** Visual Studio для ASP.NET Core может отобразиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="df677-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="df677-132">Пакеты SDK для .NET Core не обнаружены, убедитесь, что они включены в `PATH`переменную среды.</span><span class="sxs-lookup"><span data-stu-id="df677-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="df677-133">При выполнении `dotnet` команды предупреждение выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="df677-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="df677-134">Не удалось найти установленные пакеты SDK DotNet.</span><span class="sxs-lookup"><span data-stu-id="df677-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="df677-135">Эти предупреждения появляются, когда переменная `PATH` среды не указывает на пакеты SDK для .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="df677-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="df677-136">Чтобы устранить эту проблему, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="df677-136">To resolve this problem:</span></span>

* <span data-ttu-id="df677-137">Установите пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df677-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="df677-138">Получите последнюю версию установщика из [скачивания .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="df677-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="df677-139">Убедитесь, что `PATH` переменная среды указывает на расположение, в котором установлен пакет SDK (`C:\Program Files\dotnet\` для 64-разрядной версии, `C:\Program Files (x86)\dotnet\` x64 или 32-разрядной версии/x86).</span><span class="sxs-lookup"><span data-stu-id="df677-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="df677-140">Установщик пакета SDK обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="df677-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="df677-141">Всегда устанавливайте одинаковые пакеты SDK и среды выполнения разрядов на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="df677-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="df677-142">Отсутствует пакет SDK после установки пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="df677-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="df677-143">Установка [пакета размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) изменяет `PATH` при установке среды выполнения .NET Core, чтобы она указывала на 32-разрядную (x86) версию .NET Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="df677-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="df677-144">Это может привести к отсутствию пакетов SDK при использовании команды .NET Core `dotnet` с 32-разрядной платформой ([пакеты SDK для .NET Core не обнаружены](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="df677-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="df677-145">Чтобы устранить эту проблему, перейдите `C:\Program Files\dotnet\` к нужной `C:\Program Files (x86)\dotnet\` должности `PATH`.</span><span class="sxs-lookup"><span data-stu-id="df677-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="df677-146">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="df677-146">Obtain data from an app</span></span>

<span data-ttu-id="df677-147">Если приложение может отвечать на запросы, можно получить следующие данные из приложения с помощью по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="df677-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="df677-148">Метод &ndash; Request, схема, узел, пасбасе, путь, строка запроса, заголовки</span><span class="sxs-lookup"><span data-stu-id="df677-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="df677-149">Удаленный &ndash; IP-адрес подключения, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="df677-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="df677-150">Имя &ndash; удостоверения, отображаемое имя</span><span class="sxs-lookup"><span data-stu-id="df677-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="df677-151">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="df677-151">Configuration settings</span></span>
* <span data-ttu-id="df677-152">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="df677-152">Environment variables</span></span>

<span data-ttu-id="df677-153">Поместите следующий код по [промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в начало `Startup.Configure` конвейера обработки запроса метода.</span><span class="sxs-lookup"><span data-stu-id="df677-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="df677-154">Среда проверяется перед выполнением по промежуточного слоя, чтобы гарантировать выполнение кода только в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="df677-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="df677-155">Чтобы получить среду, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="df677-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="df677-156">`IHostingEnvironment` Вставьтевметодипроверьтеокружение`Startup.Configure` с помощью локальной переменной.</span><span class="sxs-lookup"><span data-stu-id="df677-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="df677-157">Этот подход демонстрируется в следующем образце кода.</span><span class="sxs-lookup"><span data-stu-id="df677-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="df677-158">Назначьте среду свойству в `Startup` классе.</span><span class="sxs-lookup"><span data-stu-id="df677-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="df677-159">Проверьте окружение с помощью свойства (например, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="df677-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
