---
title: Устранение неполадок с проектов ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: 1f53375a6aa504c989920a0bff83b400b7bd83ec
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208847"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="e6dae-103">Устранение неполадок с проектов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6dae-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="e6dae-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e6dae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6dae-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="e6dae-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="e6dae-106">Представленная им ndc Прошедшей конференции (Лондон, 2018 г.): Диагностика проблем в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6dae-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="e6dae-107">Блог разработчиков ASP.NET. Устранение неполадок производительности ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6dae-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="e6dae-108">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="e6dae-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="e6dae-109">Установлены 32- и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6dae-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="e6dae-110">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="e6dae-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e6dae-111">Установлены 32- и 64 разрядной версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6dae-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="e6dae-112">Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="e6dae-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="e6dae-114">Это предупреждение появляется, когда 32-(x86) и 64-разрядные (x 64) версии [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="e6dae-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="e6dae-115">Распространенные причины, которые могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="e6dae-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="e6dae-116">Изначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировали его через и его установки на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="e6dae-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="e6dae-117">Пакет SDK для 32-разрядной .NET Core был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="e6dae-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="e6dae-118">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="e6dae-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="e6dae-119">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="e6dae-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="e6dae-120">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="e6dae-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="e6dae-121">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="e6dae-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="e6dae-122">Пакет SDK для .NET Core устанавливается в нескольких местах</span><span class="sxs-lookup"><span data-stu-id="e6dae-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="e6dae-123">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="e6dae-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e6dae-124">Пакет SDK для .NET Core устанавливается в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="e6dae-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="e6dae-125">Только шаблоны из установленных здесь: "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="e6dae-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="e6dae-127">Вы видите это сообщение при наличии по крайней мере по одному разу на пакет SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="e6dae-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="e6dae-128">Обычно это происходит после развертывания пакета SDK для .NET Core на компьютере, с помощью копирования и вставки вместо установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="e6dae-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="e6dae-129">Удалите все 32-разрядных пакетов SDK для .NET Core и среды выполнения для предотвращения этого предупреждения.</span><span class="sxs-lookup"><span data-stu-id="e6dae-129">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="e6dae-130">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="e6dae-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="e6dae-131">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="e6dae-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="e6dae-132">Нет пакетов SDK для .NET Core не обнаружены</span><span class="sxs-lookup"><span data-stu-id="e6dae-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="e6dae-133">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="e6dae-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="e6dae-134">Нет пакетов SDK для .NET Core были обнаружены, убедитесь, что они включаются в переменной среды «PATH».</span><span class="sxs-lookup"><span data-stu-id="e6dae-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="e6dae-136">Это предупреждение появляется, когда переменная среды `PATH` не указывает на пакеты SDK для Core любой .NET на компьютере (например, `C:\Program Files\dotnet\` и `C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="e6dae-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="e6dae-137">Чтобы устранить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="e6dae-137">To resolve this problem:</span></span>

* <span data-ttu-id="e6dae-138">Установите или убедитесь, что установлен пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6dae-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="e6dae-139">Получить последнюю версию установщика из [загрузки .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="e6dae-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="e6dae-140">Убедитесь, что `PATH` переменной среды указывает на расположение, где установлен пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="e6dae-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="e6dae-141">Установщик обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="e6dae-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="e6dae-142">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="e6dae-142">Obtain data from an app</span></span>

<span data-ttu-id="e6dae-143">Если приложение может отвечать на запросы, можно получить следующие данные из приложения, с помощью по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="e6dae-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="e6dae-144">Запросить &ndash; строка заголовков запроса метода, схему, узел, pathbase, путь,</span><span class="sxs-lookup"><span data-stu-id="e6dae-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="e6dae-145">Подключение &ndash; удаленный IP-адрес, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="e6dae-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="e6dae-146">Удостоверение &ndash; имя, отображаемое имя</span><span class="sxs-lookup"><span data-stu-id="e6dae-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="e6dae-147">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="e6dae-147">Configuration settings</span></span>
* <span data-ttu-id="e6dae-148">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="e6dae-148">Environment variables</span></span>

<span data-ttu-id="e6dae-149">Поместите следующий [по промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) кода в начале `Startup.Configure` конвейера обработки запросов метода.</span><span class="sxs-lookup"><span data-stu-id="e6dae-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="e6dae-150">Среде проверяется перед запуском по промежуточного слоя, чтобы убедиться, что код выполняется только в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="e6dae-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="e6dae-151">Чтобы получить среду, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="e6dae-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="e6dae-152">Внедрить `IHostingEnvironment` в `Startup.Configure` метод и проверьте среды с локальной переменной.</span><span class="sxs-lookup"><span data-stu-id="e6dae-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="e6dae-153">В следующем примере кода демонстрируется этот подход.</span><span class="sxs-lookup"><span data-stu-id="e6dae-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="e6dae-154">Присвоить свойству в среде `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="e6dae-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="e6dae-155">Проверьте среду, с помощью свойства (например, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="e6dae-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
