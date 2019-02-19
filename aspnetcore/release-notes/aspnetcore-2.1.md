---
title: Новые возможности ASP.NET Core 2.1
author: isaac2004
description: Дополнительные сведения о новых возможностях ASP.NET Core 2.1.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: aspnetcore-2.1
ms.openlocfilehash: 8299af819f86d3d2371650ce3d87deb817f0feb8
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248177"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="2422b-103">Новые возможности ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="2422b-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="2422b-104">В этой статье описываются наиболее важные изменения в ASP.NET Core 2.1 со ссылками на соответствующую документацию.</span><span class="sxs-lookup"><span data-stu-id="2422b-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="2422b-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="2422b-105">SignalR</span></span>

<span data-ttu-id="2422b-106">SignalR переписан для ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2422b-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="2422b-107">ASP.NET Core SignalR включает ряд усовершенствований:</span><span class="sxs-lookup"><span data-stu-id="2422b-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="2422b-108">Упрощенная модель горизонтального масштабирования.</span><span class="sxs-lookup"><span data-stu-id="2422b-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="2422b-109">Новый клиент JavaScript без зависимости jQuery.</span><span class="sxs-lookup"><span data-stu-id="2422b-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="2422b-110">Новый компактный двоичный протокол на базе MessagePack.</span><span class="sxs-lookup"><span data-stu-id="2422b-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="2422b-111">Поддержка пользовательских протоколов.</span><span class="sxs-lookup"><span data-stu-id="2422b-111">Support for custom protocols.</span></span>
* <span data-ttu-id="2422b-112">Новая модель потокового ответа.</span><span class="sxs-lookup"><span data-stu-id="2422b-112">A new streaming response model.</span></span>
* <span data-ttu-id="2422b-113">Поддержка клиентов на базе только протокола WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2422b-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="2422b-114">Дополнительные сведения см. в статье [ASP.NET Core SignalR](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="2422b-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="2422b-115">Библиотеки классов Razor</span><span class="sxs-lookup"><span data-stu-id="2422b-115">Razor class libraries</span></span>

<span data-ttu-id="2422b-116">В ASP.NET Core 2.1 проще создать и включить пользовательский интерфейс на основе Razor в библиотеку, а затем использовать его сразу в нескольких проектах.</span><span class="sxs-lookup"><span data-stu-id="2422b-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="2422b-117">Новый пакет SDK для Razor позволяет создавать файлы Razor в проекте библиотеки классов, который можно поместить в пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="2422b-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="2422b-118">Представления и страницы в библиотеках обнаруживаются автоматически и могут переопределяться приложением.</span><span class="sxs-lookup"><span data-stu-id="2422b-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="2422b-119">Благодаря интеграции компиляции Razor в сборку:</span><span class="sxs-lookup"><span data-stu-id="2422b-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="2422b-120">Время запуска приложения значительно сократилось.</span><span class="sxs-lookup"><span data-stu-id="2422b-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="2422b-121">Быстрые обновления для представлений и страниц Razor во время выполнения по-прежнему доступны в рамках рабочего процесса последовательной разработки.</span><span class="sxs-lookup"><span data-stu-id="2422b-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="2422b-122">Дополнительные сведения см. в разделе [Создание многоразового пользовательского интерфейса с помощью проекта библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="2422b-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="2422b-123">Библиотека пользовательского интерфейса идентификации и формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="2422b-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="2422b-124">ASP.NET Core 2.1 предоставляет [идентификатор ASP.NET Core ](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="2422b-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2422b-125">Приложения, включающие идентификатор, могут применить новый шаблон идентификатора для выборочного добавления исходного кода из библиотеки классов Razor (RCL) для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="2422b-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="2422b-126">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="2422b-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="2422b-127">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="2422b-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="2422b-128">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="2422b-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="2422b-129">Приложения, которые **не** включают проверку подлинности, могут применить шаблон идентификаторов, чтобы добавить пакет RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="2422b-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="2422b-130">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="2422b-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="2422b-131">Дополнительные сведения см. в разделе [Шаблоны идентификаторов в проектах ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="2422b-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="2422b-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="2422b-132">HTTPS</span></span>

<span data-ttu-id="2422b-133">Учитывая повышенное внимание к безопасности и конфиденциальности, важно использовать HTTPS для веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="2422b-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="2422b-134">В Интернете применение HTTPS все чаще становится обязательным.</span><span class="sxs-lookup"><span data-stu-id="2422b-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="2422b-135">Сайты, не использующие HTTPS, считаются небезопасными.</span><span class="sxs-lookup"><span data-stu-id="2422b-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="2422b-136">Браузеры (Chromium, Mozilla) требуют использования веб-компонентов в контексте безопасности.</span><span class="sxs-lookup"><span data-stu-id="2422b-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="2422b-137">[Общий регламент по защите данных](xref:security/gdpr) предписывает использовать HTTPS для защиты конфиденциальности пользователей.</span><span class="sxs-lookup"><span data-stu-id="2422b-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="2422b-138">В то время как использование HTTPS в рабочей среде становится обязательным, применение HTTPS при разработке помогает предотвратить проблемы с развертыванием (например, небезопасные ссылки).</span><span class="sxs-lookup"><span data-stu-id="2422b-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="2422b-139">ASP.NET Core 2.1 включает ряд усовершенствований, упрощающих использование HTTPS в среде разработки и настройку HTTPS в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2422b-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="2422b-140">Дополнительные сведения см. в разделе [Обязательное использование HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2422b-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="2422b-141">По умолчанию включено</span><span class="sxs-lookup"><span data-stu-id="2422b-141">On by default</span></span>

<span data-ttu-id="2422b-142">Чтобы вам было проще разрабатывать безопасные веб-сайты, протокол HTTPS теперь включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2422b-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="2422b-143">Начиная с версии 2.1 Kestrel ожидает передачи данных по `https://localhost:5001`, если присутствует локальный сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="2422b-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="2422b-144">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="2422b-144">A development certificate is created:</span></span>

* <span data-ttu-id="2422b-145">Вы впервые запускаете пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2422b-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="2422b-146">Вы создаете его вручную с помощью нового средства `dev-certs`.</span><span class="sxs-lookup"><span data-stu-id="2422b-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="2422b-147">Запустите `dotnet dev-certs https --trust`, чтобы установить доверие для сертификата.</span><span class="sxs-lookup"><span data-stu-id="2422b-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="2422b-148">Перенаправление и принудительное применение HTTPS</span><span class="sxs-lookup"><span data-stu-id="2422b-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="2422b-149">Веб-приложения обычно используют оба протокола — HTTP и HTTPS, но перенаправляют весь трафик HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2422b-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="2422b-150">В версии 2.1 появилось специальное ПО промежуточного слоя, которое интеллектуально перенаправляет трафик на HTTPS, учитывая конфигурацию или порты связанного сервера.</span><span class="sxs-lookup"><span data-stu-id="2422b-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="2422b-151">Использование протокола HTTPS также можно гарантировать с помощью [протокола HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="2422b-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="2422b-152">HSTS указывает браузерам всегда переходить на сайт через HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2422b-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="2422b-153">В ASP.NET Core 2.1 добавлено ПО промежуточного слоя HSTS, которое поддерживает параметры для максимального возраста, дочерних доменов и списка предварительной загрузки HSTS.</span><span class="sxs-lookup"><span data-stu-id="2422b-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="2422b-154">Конфигурация для рабочей среды</span><span class="sxs-lookup"><span data-stu-id="2422b-154">Configuration for production</span></span>

<span data-ttu-id="2422b-155">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2422b-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="2422b-156">В версии 2.1 добавлена схема конфигурации по умолчанию для настройки HTTPS для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2422b-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="2422b-157">Можно настроить приложения, чтобы они использовали:</span><span class="sxs-lookup"><span data-stu-id="2422b-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="2422b-158">Несколько конечных точек, включая URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="2422b-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="2422b-159">Дополнительные сведения см. в руководстве по [реализации веб-сервера Kestrel. Конфигурация конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="2422b-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="2422b-160">Сертификат для использования HTTPS из файла на диске или из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="2422b-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="2422b-161">Общий регламент по защите данных</span><span class="sxs-lookup"><span data-stu-id="2422b-161">GDPR</span></span>

<span data-ttu-id="2422b-162">ASP.NET Core предоставляет API-интерфейсы и шаблоны, которые помогают соответствовать требованиям [Общего регламента по защите данных в ЕС](https://www.eugdpr.org/).</span><span class="sxs-lookup"><span data-stu-id="2422b-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="2422b-163">Дополнительные сведения см. в разделе [Поддержка общего регламента по защите данных в ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="2422b-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="2422b-164">В [примере приложения](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) показано, как использовать и тестировать большинство точек расширения для общего регламента по защите данных и API-интерфейсов, добавленных в шаблоны ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2422b-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="2422b-165">Интеграционные тесты</span><span class="sxs-lookup"><span data-stu-id="2422b-165">Integration tests</span></span>

<span data-ttu-id="2422b-166">Добавлен новый пакет, который оптимизирует создание и выполнение тестов.</span><span class="sxs-lookup"><span data-stu-id="2422b-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="2422b-167">Пакет [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="2422b-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="2422b-168">Копирует файл зависимостей (*\*.deps*) из протестированного приложения в папку *bin* тестового проекта.</span><span class="sxs-lookup"><span data-stu-id="2422b-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="2422b-169">Задает корневую папку содержимого в корневой папке проекта тестируемого приложения, чтобы можно было найти статические файлы и страницы или представления при выполнении тестов.</span><span class="sxs-lookup"><span data-stu-id="2422b-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="2422b-170">Предоставляет класс [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) для оптимизации начальной загрузки тестируемого приложения на [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="2422b-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="2422b-171">В следующем тесте используется [xUnit](https://xunit.github.io/) для проверки того, что страница индексов загружается с кодом состояния успешного выполнения и правильным заголовком Content-Type:</span><span class="sxs-lookup"><span data-stu-id="2422b-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

<span data-ttu-id="2422b-172">Дополнительные сведения см. в разделе [Интеграционные тесты](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="2422b-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="2422b-173">[ApiController], ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="2422b-173">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="2422b-174">В ASP.NET Core 2.1 добавлены новые соглашения программирования, с которыми проще создавать чистые и выразительные веб-API.</span><span class="sxs-lookup"><span data-stu-id="2422b-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="2422b-175">`ActionResult<T>` — это новый тип, который разрешает приложениям возвращать либо тип ответа, либо любой другой результат действия (аналогично IActionResult), при этом по-прежнему указывая тип ответа.</span><span class="sxs-lookup"><span data-stu-id="2422b-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="2422b-176">Также был добавлен атрибут `[ApiController]`, с помощью которого можно принять соглашения и поведения для веб-API.</span><span class="sxs-lookup"><span data-stu-id="2422b-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="2422b-177">Дополнительные сведения см. в разделе [Сборка веб-API с использованием ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="2422b-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="2422b-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="2422b-178">IHttpClientFactory</span></span>

<span data-ttu-id="2422b-179">ASP.NET Core 2.1 содержит новую службу `IHttpClientFactory`, которая упрощает настройку и использование экземпляров `HttpClient` в приложениях.</span><span class="sxs-lookup"><span data-stu-id="2422b-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="2422b-180">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="2422b-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="2422b-181">Фабрика:</span><span class="sxs-lookup"><span data-stu-id="2422b-181">The factory:</span></span>

* <span data-ttu-id="2422b-182">Упрощает регистрацию экземпляров `HttpClient` каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="2422b-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="2422b-183">Реализует обработчик Polly, который позволяет использовать политики Polly для повтора, размыкателя цепи и т. д.</span><span class="sxs-lookup"><span data-stu-id="2422b-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="2422b-184">Дополнительные сведения см. в разделе [Инициирование HTTP-запросов](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="2422b-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="2422b-185">Конфигурация транспорта Kestrel</span><span class="sxs-lookup"><span data-stu-id="2422b-185">Kestrel transport configuration</span></span>

<span data-ttu-id="2422b-186">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="2422b-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="2422b-187">Дополнительные сведения см. в руководстве по [реализации веб-сервера Kestrel. Конфигурация транспортировки](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="2422b-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="2422b-188">Построитель универсальных узлов</span><span class="sxs-lookup"><span data-stu-id="2422b-188">Generic host builder</span></span>

<span data-ttu-id="2422b-189">Добавлен построитель универсальных узлов (`HostBuilder`).</span><span class="sxs-lookup"><span data-stu-id="2422b-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="2422b-190">Построитель можно использовать для приложений, которые не обрабатывают HTTP-запросы (обмен сообщениями, фоновые задачи и т. д.).</span><span class="sxs-lookup"><span data-stu-id="2422b-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="2422b-191">Дополнительные сведения см. в разделе [Универсальный узел .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2422b-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="2422b-192">Обновленные шаблоны SPA</span><span class="sxs-lookup"><span data-stu-id="2422b-192">Updated SPA templates</span></span>

<span data-ttu-id="2422b-193">Обновлены шаблоны одностраничных приложений для Angular, React и React с Redux. Теперь можно использовать стандартные структуры проектов и создавать системы для каждой платформы.</span><span class="sxs-lookup"><span data-stu-id="2422b-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="2422b-194">Шаблон Angular основан на Angular CLI, а шаблоны React основаны на create-react-app.</span><span class="sxs-lookup"><span data-stu-id="2422b-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>

<span data-ttu-id="2422b-195">Дополнительные сведения:</span><span class="sxs-lookup"><span data-stu-id="2422b-195">For more information, see:</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="2422b-196">Razor Pages ищет ресурсы Razor</span><span class="sxs-lookup"><span data-stu-id="2422b-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="2422b-197">В версии 2.1 Razor Pages ищет ресурсы Razor (например, макеты и частично выполненные строки) в следующих каталогах в указанном порядке:</span><span class="sxs-lookup"><span data-stu-id="2422b-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="2422b-198">Текущая папка Pages.</span><span class="sxs-lookup"><span data-stu-id="2422b-198">Current Pages folder.</span></span>
1. <span data-ttu-id="2422b-199">*/Pages/Shared/*</span><span class="sxs-lookup"><span data-stu-id="2422b-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="2422b-200">*/Views/Shared/*</span><span class="sxs-lookup"><span data-stu-id="2422b-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="2422b-201">Razor Pages в области</span><span class="sxs-lookup"><span data-stu-id="2422b-201">Razor Pages in an area</span></span>

<span data-ttu-id="2422b-202">Razor Pages теперь поддерживает [области](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="2422b-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="2422b-203">Чтобы увидеть пример областей, создайте новое веб-приложение Razor Pages с отдельными учетными записями пользователей.</span><span class="sxs-lookup"><span data-stu-id="2422b-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="2422b-204">Веб-приложение Razor Pages с отдельными учетными записями пользователей включает */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="2422b-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="mvc-compatibility-version"></a><span data-ttu-id="2422b-205">Совместимая версия MVC</span><span class="sxs-lookup"><span data-stu-id="2422b-205">MVC compatibility version</span></span>

<span data-ttu-id="2422b-206">Метод <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> позволяет приложению принимать или отклонять потенциально критические изменения в поведении, появившиеся в ASP.NET Core MVC 2.1 или более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="2422b-206">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="2422b-207">Для получения дополнительной информации см. <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="2422b-207">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="2422b-208">Миграция с 2.0 на 2.1</span><span class="sxs-lookup"><span data-stu-id="2422b-208">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="2422b-209">См. раздел [Миграция с ASP.NET Core 2.0 на 2.1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="2422b-209">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="2422b-210">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="2422b-210">Additional information</span></span>

<span data-ttu-id="2422b-211">Полный список изменений см. в статье [Заметки о выпуске ASP.NET Core 2.1](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="2422b-211">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
