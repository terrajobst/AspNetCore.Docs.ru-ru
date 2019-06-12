---
title: Настройка проверки подлинности сертификата в ASP.NET Core
author: blowdart
description: Узнайте, как настроить проверку подлинности сертификата в ASP.NET Core для IIS и HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: barry.dorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 37567128531187bfe7dd6c9f5aa990226e70f35f
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837541"
---
# <a name="overview"></a><span data-ttu-id="67d5a-103">Обзор</span><span class="sxs-lookup"><span data-stu-id="67d5a-103">Overview</span></span>

<span data-ttu-id="67d5a-104">`Microsoft.AspNetCore.Authentication.Certificate` содержит реализацию, аналогичную [аутентификации на основе сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67d5a-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="67d5a-105">Сертификат проверки подлинности происходит на уровне TLS, long, прежде, чем когда-либо возвращает на ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67d5a-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="67d5a-106">Точнее говоря, это обработчик проверки подлинности, который проверяет сертификат, а затем сообщает событие, где можно устранить этот сертификат для `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="67d5a-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="67d5a-107">[Настройка вашего узла](#configure-your-host-to-require-certificates) для проверки подлинности сертификата, будь то, как IIS, Kestrel, веб-приложений Azure или что-нибудь еще используется.</span><span class="sxs-lookup"><span data-stu-id="67d5a-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="67d5a-108">Начало работы</span><span class="sxs-lookup"><span data-stu-id="67d5a-108">Get started</span></span>

<span data-ttu-id="67d5a-109">Получить сертификат HTTPS, применить его, и [Настройка вашего узла](#configure-your-host-to-require-certificates) подавался запрос на сертификат.</span><span class="sxs-lookup"><span data-stu-id="67d5a-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="67d5a-110">В веб-приложении, добавьте ссылку на `Microsoft.AspNetCore.Authentication.Certificate` пакета.</span><span class="sxs-lookup"><span data-stu-id="67d5a-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="67d5a-111">Затем в `Startup.Configure` мы вызываем метод `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` с вариантами, предоставьте делегат для `OnCertificateValidated` для выполнения какой-либо дополнительных проверки сертификата клиента, в ответ на запросы.</span><span class="sxs-lookup"><span data-stu-id="67d5a-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="67d5a-112">Включить эту информацию в `ClaimsPrincipal` и задайте его в `context.Principal` свойство.</span><span class="sxs-lookup"><span data-stu-id="67d5a-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="67d5a-113">Если не пройдет проверку подлинности, этот обработчик возвращает `403 (Forbidden)` ответ скорее `401 (Unauthorized)`, как следует из названия.</span><span class="sxs-lookup"><span data-stu-id="67d5a-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="67d5a-114">Обоснованием является то, что проверка подлинности должны быть выполнены во время начального подключения TLS.</span><span class="sxs-lookup"><span data-stu-id="67d5a-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="67d5a-115">К моменту достижения обработчик уже слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="67d5a-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="67d5a-116">Нет способа для обновления подключения из анонимного соединения к одному с помощью сертификата.</span><span class="sxs-lookup"><span data-stu-id="67d5a-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="67d5a-117">Кроме того, добавить `app.UseAuthentication();` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="67d5a-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="67d5a-118">В противном случае HttpContext.User не устанавливается в `ClaimsPrincipal` создан на основе сертификата.</span><span class="sxs-lookup"><span data-stu-id="67d5a-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="67d5a-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="67d5a-119">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

<span data-ttu-id="67d5a-120">В предыдущем примере стандартного способа для добавления проверки подлинности сертификата.</span><span class="sxs-lookup"><span data-stu-id="67d5a-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="67d5a-121">Обработчик создает участника-пользователя, с помощью общих свойств сертификата.</span><span class="sxs-lookup"><span data-stu-id="67d5a-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="67d5a-122">Настройка проверки сертификата</span><span class="sxs-lookup"><span data-stu-id="67d5a-122">Configure certificate validation</span></span>

<span data-ttu-id="67d5a-123">`CertificateAuthenticationOptions` Обработчик имеет некоторые встроенные проверки, которые являются минимальным проверки следует выполнять на сертификат.</span><span class="sxs-lookup"><span data-stu-id="67d5a-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="67d5a-124">Каждый из этих параметров включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="67d5a-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="67d5a-125">AllowedCertificateTypes = объединять в цепочки, SelfSigned или все (по цепочке | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="67d5a-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="67d5a-126">Эта проверка проверяет, что разрешен только тип соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="67d5a-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="67d5a-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="67d5a-127">ValidateCertificateUse</span></span>

<span data-ttu-id="67d5a-128">Эта проверка проверяет наличие сертификата, предоставленного клиентом проверки подлинности клиента, вообще расширенного использования ключа (EKU) или нет EKU.</span><span class="sxs-lookup"><span data-stu-id="67d5a-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="67d5a-129">Так как спецификации скажем, если не указан, то всех областей EKU являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="67d5a-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="67d5a-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="67d5a-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="67d5a-131">Эта проверка проверяет, что сертификат действителен в течение срока его действия.</span><span class="sxs-lookup"><span data-stu-id="67d5a-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="67d5a-132">При каждом запросе обработчик гарантирует, что сертификат, который был допустимым, если он был представлен еще не истек, во время текущего сеанса.</span><span class="sxs-lookup"><span data-stu-id="67d5a-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="67d5a-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="67d5a-133">RevocationFlag</span></span>

<span data-ttu-id="67d5a-134">Флаг, указывающий, какие сертификаты в цепочке проверяются для отзыва.</span><span class="sxs-lookup"><span data-stu-id="67d5a-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="67d5a-135">Проверку отзыва выполняются только в том случае, если сертификат был связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="67d5a-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="67d5a-136">revocationMode</span><span class="sxs-lookup"><span data-stu-id="67d5a-136">RevocationMode</span></span>

<span data-ttu-id="67d5a-137">Флаг, указывающий, как выполняются проверки отзыва.</span><span class="sxs-lookup"><span data-stu-id="67d5a-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="67d5a-138">Указание проверки в сети может привести длительная задержка при связи с центром сертификации.</span><span class="sxs-lookup"><span data-stu-id="67d5a-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="67d5a-139">Проверку отзыва выполняются только в том случае, если сертификат был связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="67d5a-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="67d5a-140">Можно ли настроить мое приложение требует сертификата только на определенные пути?</span><span class="sxs-lookup"><span data-stu-id="67d5a-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="67d5a-141">Это невозможно.</span><span class="sxs-lookup"><span data-stu-id="67d5a-141">This isn't possible.</span></span> <span data-ttu-id="67d5a-142">Помните, что выполняется обмен сертификатами, что начала диалога HTTPS, это будет сделано на сервере до получения первого запроса для этого соединения, он уже не сможете область в зависимости от полей запроса.</span><span class="sxs-lookup"><span data-stu-id="67d5a-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="67d5a-143">Обработчик события</span><span class="sxs-lookup"><span data-stu-id="67d5a-143">Handler events</span></span>

<span data-ttu-id="67d5a-144">Обработчик имеет два события:</span><span class="sxs-lookup"><span data-stu-id="67d5a-144">The handler has two events:</span></span>

* <span data-ttu-id="67d5a-145">`OnAuthenticationFailed` &ndash; Вызывается, если исключение происходит во время проверки подлинности и позволяет реагировать.</span><span class="sxs-lookup"><span data-stu-id="67d5a-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="67d5a-146">`OnCertificateValidated` &ndash; Вызывается после проверки сертификата, прошедшие проверку на соответствие и создания участника по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="67d5a-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="67d5a-147">Это событие позволяет выполнять собственную проверку и дополнения или замены основного сервера.</span><span class="sxs-lookup"><span data-stu-id="67d5a-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="67d5a-148">Примеры включают:</span><span class="sxs-lookup"><span data-stu-id="67d5a-148">For examples include:</span></span>
  * <span data-ttu-id="67d5a-149">Определение, если сертификат известен в службы.</span><span class="sxs-lookup"><span data-stu-id="67d5a-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="67d5a-150">Создание собственного участника.</span><span class="sxs-lookup"><span data-stu-id="67d5a-150">Constructing your own principal.</span></span> <span data-ttu-id="67d5a-151">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="67d5a-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="67d5a-152">Если входящий сертификат не соответствует требованиям вашей дополнительную проверку, вызовите `context.Fail("failure reason")` с указанием причины.</span><span class="sxs-lookup"><span data-stu-id="67d5a-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="67d5a-153">Для выполнения реальных задач функциональные возможности, возможно, стоит для вызова службы, зарегистрированные в системе внедрения зависимостей, который подключается к базе данных или других видов хранилище пользователя.</span><span class="sxs-lookup"><span data-stu-id="67d5a-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="67d5a-154">Доступ к службе с помощью контекста, передаваемые в делегате.</span><span class="sxs-lookup"><span data-stu-id="67d5a-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="67d5a-155">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="67d5a-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="67d5a-156">По существу проверку сертификата является аспектом авторизации.</span><span class="sxs-lookup"><span data-stu-id="67d5a-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="67d5a-157">Добавление проверки, например, издателя или отпечаток в политику авторизации, а не внутри `OnCertificateValidated`, тогда вполне приемлемым.</span><span class="sxs-lookup"><span data-stu-id="67d5a-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="67d5a-158">Настройка узла для запроса сертификатов</span><span class="sxs-lookup"><span data-stu-id="67d5a-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="67d5a-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="67d5a-159">Kestrel</span></span>

<span data-ttu-id="67d5a-160">В *Program.cs*, настройте Kestrel следующим образом:</span><span class="sxs-lookup"><span data-stu-id="67d5a-160">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a><span data-ttu-id="67d5a-161">IIS</span><span class="sxs-lookup"><span data-stu-id="67d5a-161">IIS</span></span>

<span data-ttu-id="67d5a-162">Выполните следующие действия в диспетчере служб IIS.</span><span class="sxs-lookup"><span data-stu-id="67d5a-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="67d5a-163">Выберите сайт из **подключений** вкладки.</span><span class="sxs-lookup"><span data-stu-id="67d5a-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="67d5a-164">Дважды щелкните **параметры SSL** в диалоговом окне **Просмотр возможностей** окна.</span><span class="sxs-lookup"><span data-stu-id="67d5a-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="67d5a-165">Проверьте **Требовать SSL** флажок и выберите **требуют** переключателя в **сертификаты клиента** раздел.</span><span class="sxs-lookup"><span data-stu-id="67d5a-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Параметры сертификата клиента в службах IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="67d5a-167">Azure и пользовательского веб-прокси</span><span class="sxs-lookup"><span data-stu-id="67d5a-167">Azure and custom web proxies</span></span>

<span data-ttu-id="67d5a-168">См. в разделе [размещение и развертывание документации](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) для настройки сертификата, пересылку по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="67d5a-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
