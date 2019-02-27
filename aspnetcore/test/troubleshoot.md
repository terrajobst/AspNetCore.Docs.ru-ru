---
title: Устранение неполадок с проектов ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899297"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="44e8f-103">Устранение неполадок с проектов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44e8f-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="44e8f-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="44e8f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44e8f-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="44e8f-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="44e8f-106">Представленная им ndc Прошедшей конференции (Лондон, 2018 г.): Диагностика проблем в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44e8f-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="44e8f-107">Блог разработчиков ASP.NET. Устранение неполадок производительности ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44e8f-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="44e8f-108">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="44e8f-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="44e8f-109">Установлены 32- и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="44e8f-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="44e8f-110">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="44e8f-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="44e8f-111">Установлены 32- и 64 разрядной версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="44e8f-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="44e8f-112">Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="44e8f-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="44e8f-114">Это предупреждение появляется, когда 32-(x86) и 64-разрядные (x 64) версии [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="44e8f-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="44e8f-115">Распространенные причины, которые могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="44e8f-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="44e8f-116">Изначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировали его через и его установки на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="44e8f-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="44e8f-117">Пакет SDK для 32-разрядной .NET Core был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="44e8f-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="44e8f-118">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="44e8f-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="44e8f-119">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="44e8f-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="44e8f-120">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="44e8f-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="44e8f-121">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="44e8f-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="44e8f-122">Пакет SDK для .NET Core устанавливается в нескольких местах</span><span class="sxs-lookup"><span data-stu-id="44e8f-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="44e8f-123">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="44e8f-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="44e8f-124">Пакет SDK для .NET Core устанавливается в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="44e8f-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="44e8f-125">Только шаблоны из установленных здесь: "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="44e8f-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="44e8f-127">Вы видите это сообщение при наличии по крайней мере по одному разу на пакет SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="44e8f-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="44e8f-128">Обычно это происходит после развертывания пакета SDK для .NET Core на компьютере, с помощью копирования и вставки вместо установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="44e8f-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="44e8f-129">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="44e8f-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="44e8f-130">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="44e8f-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="44e8f-131">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="44e8f-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="44e8f-132">Нет пакетов SDK для .NET Core не обнаружены</span><span class="sxs-lookup"><span data-stu-id="44e8f-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="44e8f-133">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="44e8f-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="44e8f-134">Нет пакетов SDK для .NET Core были обнаружены, убедитесь, что они включаются в переменной среды «PATH».</span><span class="sxs-lookup"><span data-stu-id="44e8f-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="44e8f-136">Это предупреждение появляется, когда переменная среды `PATH` не указывает на пакеты SDK для Core любой .NET на компьютере (например, `C:\Program Files\dotnet\` и `C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="44e8f-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="44e8f-137">Чтобы устранить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="44e8f-137">To resolve this problem:</span></span>

* <span data-ttu-id="44e8f-138">Установите или убедитесь, что установлен пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="44e8f-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="44e8f-139">Получить последнюю версию установщика из [загрузки .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="44e8f-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="44e8f-140">Убедитесь, что `PATH` переменной среды указывает на расположение, где установлен пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="44e8f-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="44e8f-141">Установщик обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="44e8f-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="44e8f-142">Получение данных из приложения</span><span class="sxs-lookup"><span data-stu-id="44e8f-142">Obtain data from an app</span></span>

<span data-ttu-id="44e8f-143">Если приложение может отвечать на запросы, можно получить следующие данные из приложения, с помощью по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="44e8f-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="44e8f-144">Запросить &ndash; строка заголовков запроса метода, схему, узел, pathbase, путь,</span><span class="sxs-lookup"><span data-stu-id="44e8f-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="44e8f-145">Подключение &ndash; удаленный IP-адрес, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="44e8f-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="44e8f-146">Удостоверение &ndash; имя, отображаемое имя</span><span class="sxs-lookup"><span data-stu-id="44e8f-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="44e8f-147">Параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="44e8f-147">Configuration settings</span></span>
* <span data-ttu-id="44e8f-148">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="44e8f-148">Environment variables</span></span>

<span data-ttu-id="44e8f-149">Поместите следующий [по промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) кода в начале `Startup.Configure` конвейера обработки запросов метода.</span><span class="sxs-lookup"><span data-stu-id="44e8f-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="44e8f-150">Среде проверяется перед запуском по промежуточного слоя, чтобы убедиться, что код выполняется только в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="44e8f-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="44e8f-151">Чтобы получить среду, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="44e8f-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="44e8f-152">Внедрить `IHostingEnvironment` в `Startup.Configure` метод и проверьте среды с локальной переменной.</span><span class="sxs-lookup"><span data-stu-id="44e8f-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="44e8f-153">В следующем примере кода демонстрируется этот подход.</span><span class="sxs-lookup"><span data-stu-id="44e8f-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="44e8f-154">Присвоить свойству в среде `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="44e8f-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="44e8f-155">Проверьте среду, с помощью свойства (например, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="44e8f-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
