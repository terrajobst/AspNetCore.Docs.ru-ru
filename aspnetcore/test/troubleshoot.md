---
title: Устранение неполадок с проектов ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: test/troubleshoot
ms.openlocfilehash: 3d755b2f0c509d65dea86bbe719e42935d87d546
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895331"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="16f59-103">Устранение неполадок с проектов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16f59-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="16f59-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="16f59-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16f59-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="16f59-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="16f59-106">Представленная им ndc Прошедшей конференции (Лондон, 2018 г.): Диагностика проблем в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16f59-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="16f59-107">Блог разработчиков ASP.NET. Устранение неполадок производительности ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16f59-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="16f59-108">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="16f59-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="16f59-109">Установлены 32- и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="16f59-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="16f59-110">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="16f59-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="16f59-111">Установлены 32- и 64 разрядной версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="16f59-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="16f59-112">Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="16f59-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="16f59-113">Это предупреждение появляется, когда 32-(x86) и 64-разрядные (x 64) версии [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="16f59-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="16f59-114">Распространенные причины, которые могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="16f59-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="16f59-115">Изначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировали его через и его установки на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="16f59-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="16f59-116">Пакет SDK для 32-разрядной .NET Core был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="16f59-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="16f59-117">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="16f59-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="16f59-118">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="16f59-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="16f59-119">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="16f59-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="16f59-120">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="16f59-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="16f59-121">Пакет SDK для .NET Core устанавливается в нескольких местах</span><span class="sxs-lookup"><span data-stu-id="16f59-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="16f59-122">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="16f59-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="16f59-123">Пакет SDK для .NET Core устанавливается в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="16f59-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="16f59-124">Только шаблоны из установленных здесь: "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="16f59-124">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

<span data-ttu-id="16f59-125">Вы видите это сообщение при наличии по крайней мере по одному разу на пакет SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="16f59-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="16f59-126">Обычно это происходит после развертывания пакета SDK для .NET Core на компьютере, с помощью копирования и вставки вместо установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="16f59-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="16f59-127">Удалите все 32-разрядных пакетов SDK для .NET Core и среды выполнения для предотвращения этого предупреждения.</span><span class="sxs-lookup"><span data-stu-id="16f59-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="16f59-128">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="16f59-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="16f59-129">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="16f59-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="16f59-130">Нет пакетов SDK для .NET Core не обнаружены</span><span class="sxs-lookup"><span data-stu-id="16f59-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="16f59-131">В Visual Studio **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="16f59-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="16f59-132">Нет пакетов SDK для .NET Core были обнаружены, убедитесь, они включаются в переменной среды `PATH`.</span><span class="sxs-lookup"><span data-stu-id="16f59-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="16f59-133">При выполнении `dotnet` команды, предупреждение появляется в том, как:</span><span class="sxs-lookup"><span data-stu-id="16f59-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="16f59-134">Не удалось найти все установленные dotnet пакеты SDK.</span><span class="sxs-lookup"><span data-stu-id="16f59-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="16f59-135">Эти предупреждения отображаются, когда переменная среды `PATH` не указывает на пакеты SDK для Core любой .NET на компьютере.</span><span class="sxs-lookup"><span data-stu-id="16f59-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="16f59-136">Чтобы устранить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="16f59-136">To resolve this problem:</span></span>

* <span data-ttu-id="16f59-137">Установите пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="16f59-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="16f59-138">Получить последнюю версию установщика из [загрузки .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="16f59-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="16f59-139">Убедитесь, что `PATH` переменной среды указывает на расположение, где установлен пакет SDK (`C:\Program Files\dotnet\` для 64 — бит/x64 или `C:\Program Files (x86)\dotnet\` для 32 бит x86 /).</span><span class="sxs-lookup"><span data-stu-id="16f59-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="16f59-140">Установщик пакета SDK для обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="16f59-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="16f59-141">Всегда устанавливайте же разрядности пакеты SDK и среды выполнения на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="16f59-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="16f59-142">Отсутствует пакет SDK после установкой пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="16f59-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="16f59-143">Установка [пакет размещения .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) изменяет `PATH` при установке среды выполнения .NET Core, чтобы он указывал на 32-разрядной (x 86) версию .NET Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="16f59-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="16f59-144">Это может привести отсутствующие пакеты SDK при 32-разрядной (x 86) .NET Core `dotnet` используется команда ([SDK Core .NET не были обнаружены](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="16f59-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="16f59-145">Чтобы устранить эту проблему, переместите `C:\Program Files\dotnet\` на позицию перед `C:\Program Files (x86)\dotnet\` на `PATH`.</span><span class="sxs-lookup"><span data-stu-id="16f59-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="16f59-146">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="16f59-146">Obtain data from an app</span></span>

<span data-ttu-id="16f59-147">Если приложение может отвечать на запросы, можно получить следующие данные из приложения, с помощью по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="16f59-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="16f59-148">Запросить &ndash; строка заголовков запроса метода, схему, узел, pathbase, путь,</span><span class="sxs-lookup"><span data-stu-id="16f59-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="16f59-149">Подключение &ndash; удаленный IP-адрес, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="16f59-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="16f59-150">Удостоверение &ndash; имя, отображаемое имя</span><span class="sxs-lookup"><span data-stu-id="16f59-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="16f59-151">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="16f59-151">Configuration settings</span></span>
* <span data-ttu-id="16f59-152">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="16f59-152">Environment variables</span></span>

<span data-ttu-id="16f59-153">Поместите следующий [по промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) кода в начале `Startup.Configure` конвейера обработки запросов метода.</span><span class="sxs-lookup"><span data-stu-id="16f59-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="16f59-154">Среде проверяется перед запуском по промежуточного слоя, чтобы убедиться, что код выполняется только в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="16f59-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="16f59-155">Чтобы получить среду, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="16f59-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="16f59-156">Внедрить `IHostingEnvironment` в `Startup.Configure` метод и проверьте среды с локальной переменной.</span><span class="sxs-lookup"><span data-stu-id="16f59-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="16f59-157">В следующем примере кода демонстрируется этот подход.</span><span class="sxs-lookup"><span data-stu-id="16f59-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="16f59-158">Присвоить свойству в среде `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="16f59-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="16f59-159">Проверьте среду, с помощью свойства (например, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="16f59-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
