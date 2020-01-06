---
title: Настройка проверки подлинности сертификата в ASP.NET Core
author: blowdart
description: Узнайте, как настроить проверку подлинности сертификата в ASP.NET Core для IIS и HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 01/02/2020
uid: security/authentication/certauth
ms.openlocfilehash: 9c175439c0313d62c75898f1af097774b06f353a
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608149"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="b633e-103">Настройка проверки подлинности сертификата в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b633e-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="b633e-104">`Microsoft.AspNetCore.Authentication.Certificate` содержит реализацию, похожую на [проверку подлинности с помощью сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b633e-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="b633e-105">Проверка подлинности сертификата выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b633e-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="b633e-106">Точнее, это обработчик проверки подлинности, который проверяет сертификат, а затем предоставляет событие, в котором можно разрешить этот сертификат в `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="b633e-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="b633e-107">[Настройте узел](#configure-your-host-to-require-certificates) для проверки подлинности на основе сертификата, как IIS, Kestrel, веб-приложения Azure или любое другое приложение, которое вы используете.</span><span class="sxs-lookup"><span data-stu-id="b633e-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="b633e-108">Сценарии прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="b633e-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="b633e-109">Проверка подлинности с помощью сертификата — это сценарий с отслеживанием состояния, который в основном используется, когда прокси или балансировщик нагрузки не обрабатывает трафик между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="b633e-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="b633e-110">Если используется прокси-сервер или балансировщик нагрузки, проверка подлинности сертификата работает только в том случае, если прокси-сервер или балансировщик нагрузки:</span><span class="sxs-lookup"><span data-stu-id="b633e-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="b633e-111">Обрабатывает проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="b633e-111">Handles the authentication.</span></span>
* <span data-ttu-id="b633e-112">Передает сведения о проверке подлинности пользователя в приложение (например, в заголовке запроса), которое действует на данные проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b633e-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="b633e-113">Альтернативой для проверки подлинности на сертификат в средах, где используются прокси-серверы и подсистемы балансировки нагрузки, являются Active Directory Федеративные службы (ADFS) с OpenID Connect Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="b633e-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="b633e-114">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="b633e-114">Get started</span></span>

<span data-ttu-id="b633e-115">Получите HTTPS сертификат, примените его и [Настройте узел](#configure-your-host-to-require-certificates) так, чтобы он затребовал сертификаты.</span><span class="sxs-lookup"><span data-stu-id="b633e-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="b633e-116">В веб-приложении добавьте ссылку на пакет `Microsoft.AspNetCore.Authentication.Certificate`.</span><span class="sxs-lookup"><span data-stu-id="b633e-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="b633e-117">Затем в методе `Startup.ConfigureServices` вызовите `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` с вашими параметрами, предоставляя делегат для `OnCertificateValidated` выполнить любую дополнительную проверку сертификата клиента, отправляемого с запросами.</span><span class="sxs-lookup"><span data-stu-id="b633e-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="b633e-118">Включите эту информацию в `ClaimsPrincipal` и установите ее в свойстве `context.Principal`.</span><span class="sxs-lookup"><span data-stu-id="b633e-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="b633e-119">Если проверка подлинности завершается неудачно, этот обработчик возвращает `403 (Forbidden)`ный ответ, а не `401 (Unauthorized)`, как можно было бы ожидать.</span><span class="sxs-lookup"><span data-stu-id="b633e-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="b633e-120">Причина заключается в том, что проверка подлинности должна выполняться во время первоначального TLS-подключения.</span><span class="sxs-lookup"><span data-stu-id="b633e-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="b633e-121">К моменту, когда он достигает обработчика, он слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="b633e-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="b633e-122">Невозможно обновить подключение между анонимным подключением и сертификатом.</span><span class="sxs-lookup"><span data-stu-id="b633e-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="b633e-123">Также добавьте `app.UseAuthentication();` в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b633e-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="b633e-124">В противном случае `HttpContext.User` не будет настроена для `ClaimsPrincipal`, созданного из сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-124">Otherwise, the `HttpContext.User` will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="b633e-125">Например:</span><span class="sxs-lookup"><span data-stu-id="b633e-125">For example:</span></span>

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

<span data-ttu-id="b633e-126">В предыдущем примере показан способ добавления проверки подлинности с помощью сертификата по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b633e-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="b633e-127">Обработчик конструирует субъект-пользователя, используя общие свойства сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="b633e-128">Настройка проверки сертификата</span><span class="sxs-lookup"><span data-stu-id="b633e-128">Configure certificate validation</span></span>

<span data-ttu-id="b633e-129">Обработчик `CertificateAuthenticationOptions` имеет некоторые встроенные проверки, которые являются минимальными проверками, которые следует выполнить с сертификатом.</span><span class="sxs-lookup"><span data-stu-id="b633e-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="b633e-130">Каждый из этих параметров включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b633e-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="b633e-131">AllowedCertificateTypes = Chained, SelfSigned или All (Chained | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="b633e-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="b633e-132">Значение по умолчанию — `CertificateTypes.Chained`.</span><span class="sxs-lookup"><span data-stu-id="b633e-132">Default value: `CertificateTypes.Chained`</span></span>

<span data-ttu-id="b633e-133">Эта проверка подтверждает, что разрешен только соответствующий тип сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-133">This check validates that only the appropriate certificate type is allowed.</span></span> <span data-ttu-id="b633e-134">Если приложение использует самозаверяющие сертификаты, для этого параметра необходимо задать значение `CertificateTypes.All` или `CertificateTypes.SelfSigned`.</span><span class="sxs-lookup"><span data-stu-id="b633e-134">If the app is using self-signed certificates, this option needs to be set to `CertificateTypes.All` or `CertificateTypes.SelfSigned`.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="b633e-135">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="b633e-135">ValidateCertificateUse</span></span>

<span data-ttu-id="b633e-136">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="b633e-136">Default value: `true`</span></span>

<span data-ttu-id="b633e-137">Эта проверка проверяет, что сертификат, предоставленный клиентом, имеет расширенное использование ключа проверки подлинности клиента (EKU) или вообще не содержит EKU.</span><span class="sxs-lookup"><span data-stu-id="b633e-137">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="b633e-138">Как говорится в спецификации, если EKU не указано, все EKU считаются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="b633e-138">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="b633e-139">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="b633e-139">ValidateValidityPeriod</span></span>

<span data-ttu-id="b633e-140">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="b633e-140">Default value: `true`</span></span>

<span data-ttu-id="b633e-141">Эта проверка проверяет, что сертификат находится в пределах срока действия.</span><span class="sxs-lookup"><span data-stu-id="b633e-141">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="b633e-142">В каждом запросе обработчик гарантирует, что сертификат, который был действителен, когда он был предоставлен, не истечет в течение текущего сеанса.</span><span class="sxs-lookup"><span data-stu-id="b633e-142">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="b633e-143">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="b633e-143">RevocationFlag</span></span>

<span data-ttu-id="b633e-144">Значение по умолчанию — `X509RevocationFlag.ExcludeRoot`.</span><span class="sxs-lookup"><span data-stu-id="b633e-144">Default value: `X509RevocationFlag.ExcludeRoot`</span></span>

<span data-ttu-id="b633e-145">Флаг, указывающий, какие сертификаты в цепочке проверяются на отзыв.</span><span class="sxs-lookup"><span data-stu-id="b633e-145">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="b633e-146">Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="b633e-146">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="b633e-147">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="b633e-147">RevocationMode</span></span>

<span data-ttu-id="b633e-148">Значение по умолчанию — `X509RevocationMode.Online`.</span><span class="sxs-lookup"><span data-stu-id="b633e-148">Default value: `X509RevocationMode.Online`</span></span>

<span data-ttu-id="b633e-149">Флаг, указывающий, как выполняются проверки отзыва.</span><span class="sxs-lookup"><span data-stu-id="b633e-149">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="b633e-150">Указание проверки в сети может привести к длительной задержке при обращении к центру сертификации.</span><span class="sxs-lookup"><span data-stu-id="b633e-150">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="b633e-151">Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="b633e-151">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="b633e-152">Можно ли настроить в приложении требование сертификата только для определенных путей?</span><span class="sxs-lookup"><span data-stu-id="b633e-152">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="b633e-153">Это невозможно.</span><span class="sxs-lookup"><span data-stu-id="b633e-153">This isn't possible.</span></span> <span data-ttu-id="b633e-154">Помните, что обмен сертификатами выполняется в начале диалога HTTPS, он выполняется сервером до получения первого запроса на это подключение, поэтому невозможно ограничить область на основе каких-либо полей запроса.</span><span class="sxs-lookup"><span data-stu-id="b633e-154">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="b633e-155">События обработчика</span><span class="sxs-lookup"><span data-stu-id="b633e-155">Handler events</span></span>

<span data-ttu-id="b633e-156">Обработчик имеет два события:</span><span class="sxs-lookup"><span data-stu-id="b633e-156">The handler has two events:</span></span>

* <span data-ttu-id="b633e-157">`OnAuthenticationFailed` &ndash; вызывается при возникновении исключения во время проверки подлинности и позволяет реагировать на них.</span><span class="sxs-lookup"><span data-stu-id="b633e-157">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="b633e-158">`OnCertificateValidated` &ndash; вызывается после проверки сертификата, прошел проверку и создается участник по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b633e-158">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="b633e-159">Это событие позволяет выполнять собственную проверку и дополнять или заменять субъект.</span><span class="sxs-lookup"><span data-stu-id="b633e-159">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="b633e-160">Ниже приведены примеры.</span><span class="sxs-lookup"><span data-stu-id="b633e-160">For examples include:</span></span>
  * <span data-ttu-id="b633e-161">Определение, известен ли сертификат службам.</span><span class="sxs-lookup"><span data-stu-id="b633e-161">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="b633e-162">Создание собственного участника.</span><span class="sxs-lookup"><span data-stu-id="b633e-162">Constructing your own principal.</span></span> <span data-ttu-id="b633e-163">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b633e-163">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="b633e-164">Если входящий сертификат не соответствует дополнительной проверке, вызовите `context.Fail("failure reason")` с причиной сбоя.</span><span class="sxs-lookup"><span data-stu-id="b633e-164">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="b633e-165">Для реальной функциональности вам, вероятно, потребуется вызвать службу, зарегистрированную в внедрении зависимостей, которая подключается к базе данных или другому хранилищу пользователей.</span><span class="sxs-lookup"><span data-stu-id="b633e-165">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="b633e-166">Получите доступ к службе с помощью контекста, переданного в делегат.</span><span class="sxs-lookup"><span data-stu-id="b633e-166">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="b633e-167">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b633e-167">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="b633e-168">По сути, проверка сертификата является проблемой авторизации.</span><span class="sxs-lookup"><span data-stu-id="b633e-168">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="b633e-169">Добавление проверки, например, издателя или отпечатка в политике авторизации, а не в `OnCertificateValidated`, вполне приемлемо.</span><span class="sxs-lookup"><span data-stu-id="b633e-169">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="b633e-170">Настройка узла для использования сертификатов</span><span class="sxs-lookup"><span data-stu-id="b633e-170">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="b633e-171">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b633e-171">Kestrel</span></span>

<span data-ttu-id="b633e-172">В *Program.CS*настройте Kestrel следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b633e-172">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            webBuilder.ConfigureKestrel(o =>
            {
                o.ConfigureHttpsDefaults(o => 
            o.ClientCertificateMode = 
                ClientCertificateMode.RequireCertificate);
            });
        });
}
```

> [!NOTE]
> <span data-ttu-id="b633e-173">Для конечных точек, созданных путем вызова <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **до** вызова <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*>, значения по умолчанию не применяются.</span><span class="sxs-lookup"><span data-stu-id="b633e-173">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="iis"></a><span data-ttu-id="b633e-174">IIS</span><span class="sxs-lookup"><span data-stu-id="b633e-174">IIS</span></span>

<span data-ttu-id="b633e-175">В диспетчере служб IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="b633e-175">Complete the following steps in IIS Manager:</span></span>

1. <span data-ttu-id="b633e-176">Выберите сайт на вкладке " **подключения** ".</span><span class="sxs-lookup"><span data-stu-id="b633e-176">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="b633e-177">Дважды щелкните параметр **Параметры SSL** в окне **просмотра функций** .</span><span class="sxs-lookup"><span data-stu-id="b633e-177">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="b633e-178">Установите флажок **требовать SSL** и установите переключатель **требуется** в разделе **Сертификаты клиента** .</span><span class="sxs-lookup"><span data-stu-id="b633e-178">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Параметры сертификата клиента в службах IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="b633e-180">Azure и настраиваемые веб-прокси</span><span class="sxs-lookup"><span data-stu-id="b633e-180">Azure and custom web proxies</span></span>

<span data-ttu-id="b633e-181">Сведения о настройке по промежуточного слоя для переадресации сертификатов см. в [документации по узлу и развертыванию](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .</span><span class="sxs-lookup"><span data-stu-id="b633e-181">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>

### <a name="use-certificate-authentication-in-azure-web-apps"></a><span data-ttu-id="b633e-182">Использование проверки подлинности сертификата в веб-приложениях Azure</span><span class="sxs-lookup"><span data-stu-id="b633e-182">Use certificate authentication in Azure Web Apps</span></span>

<span data-ttu-id="b633e-183">Метод `AddCertificateForwarding` используется для указания:</span><span class="sxs-lookup"><span data-stu-id="b633e-183">The `AddCertificateForwarding` method is used to specify:</span></span>

* <span data-ttu-id="b633e-184">Имя заголовка клиента.</span><span class="sxs-lookup"><span data-stu-id="b633e-184">The client header name.</span></span>
* <span data-ttu-id="b633e-185">Способ загрузки сертификата (с помощью свойства `HeaderConverter`).</span><span class="sxs-lookup"><span data-stu-id="b633e-185">How the certificate is to be loaded (using the `HeaderConverter` property).</span></span>

<span data-ttu-id="b633e-186">В веб-приложениях Azure сертификат передается в виде пользовательского заголовка запроса с именем `X-ARR-ClientCert`.</span><span class="sxs-lookup"><span data-stu-id="b633e-186">In Azure Web Apps, the certificate is passed as a custom request header named `X-ARR-ClientCert`.</span></span> <span data-ttu-id="b633e-187">Чтобы использовать его, Настройте пересылку сертификатов в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b633e-187">To use it, configure certificate forwarding in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCertificateForwarding(options =>
    {
        options.CertificateHeader = "X-ARR-ClientCert";
        options.HeaderConverter = (headerValue) =>
        {
            X509Certificate2 clientCertificate = null;
        
            if(!string.IsNullOrWhiteSpace(headerValue))
            {
                byte[] bytes = StringToByteArray(headerValue);
                clientCertificate = new X509Certificate2(bytes);
            }

            return clientCertificate;
        };
    });
}

private static byte[] StringToByteArray(string hex)
{
    int NumberChars = hex.Length;
    byte[] bytes = new byte[NumberChars / 2];

    for (int i = 0; i < NumberChars; i += 2)
    {
        bytes[i / 2] = Convert.ToByte(hex.Substring(i, 2), 16);
    }

    return bytes;
}
```

<span data-ttu-id="b633e-188">Затем метод `Startup.Configure` добавляет по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="b633e-188">The `Startup.Configure` method then adds the middleware.</span></span> <span data-ttu-id="b633e-189">`UseCertificateForwarding` вызывается перед вызовами метода `UseAuthentication` и `UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="b633e-189">`UseCertificateForwarding` is called before the calls to `UseAuthentication` and `UseAuthorization`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseRouting();

    app.UseCertificateForwarding();
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="b633e-190">Для реализации логики проверки можно использовать отдельный класс.</span><span class="sxs-lookup"><span data-stu-id="b633e-190">A separate class can be used to implement validation logic.</span></span> <span data-ttu-id="b633e-191">Так как в этом примере используется тот же самозаверяющий сертификат, убедитесь, что только ваш сертификат можно использовать.</span><span class="sxs-lookup"><span data-stu-id="b633e-191">Because the same self-signed certificate is used in this example, ensure that only your certificate can be used.</span></span> <span data-ttu-id="b633e-192">Убедитесь, что отпечатки сертификата клиента и сертификата сервера совпадают, в противном случае любой сертификат может быть использован и будет достаточно для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b633e-192">Validate that the thumbprints of both the client certificate and the server certificate match, otherwise any certificate can be used and will be enough to authenticate.</span></span> <span data-ttu-id="b633e-193">Он будет использоваться в методе `AddCertificate`.</span><span class="sxs-lookup"><span data-stu-id="b633e-193">This would be used inside the `AddCertificate` method.</span></span> <span data-ttu-id="b633e-194">Вы также можете проверить субъект или его издателя, если вы используете промежуточные или дочерние сертификаты.</span><span class="sxs-lookup"><span data-stu-id="b633e-194">You could also validate the subject or the issuer here if you're using intermediate or child certificates.</span></span>

```csharp
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            // Do not hardcode passwords in production code
            // Use thumbprint or key vault
            var cert = new X509Certificate2(
                Path.Combine("sts_dev_cert.pfx"), "1234");

            if (clientCertificate.Thumbprint == cert.Thumbprint)
            {
                return true;
            }

            return false;
        }
    }
}
```

#### <a name="implement-an-httpclient-using-a-certificate"></a><span data-ttu-id="b633e-195">Реализация HttpClient с помощью сертификата</span><span class="sxs-lookup"><span data-stu-id="b633e-195">Implement an HttpClient using a certificate</span></span>

<span data-ttu-id="b633e-196">Клиент веб-API использует `HttpClient`, который был создан с помощью экземпляра `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b633e-196">The web API client uses an `HttpClient`, which was created using an `IHttpClientFactory` instance.</span></span> <span data-ttu-id="b633e-197">Это не дает возможности определить обработчик для `HttpClient`, поэтому используйте `HttpRequestMessage`, чтобы добавить сертификат в заголовок запроса `X-ARR-ClientCert`.</span><span class="sxs-lookup"><span data-stu-id="b633e-197">This doesn't provide a way to define a handler for the `HttpClient`, so use an `HttpRequestMessage` to add the certificate to the `X-ARR-ClientCert` request header.</span></span> <span data-ttu-id="b633e-198">Сертификат добавляется в виде строки с помощью метода `GetRawCertDataString`.</span><span class="sxs-lookup"><span data-stu-id="b633e-198">The certificate is added as a string using the `GetRawCertDataString` method.</span></span> 

```csharp
private async Task<JsonDocument> GetApiDataAsync()
{
    try
    {
        // Do not hardcode passwords in production code
        // Use thumbprint or key vault
        var cert = new X509Certificate2(
            Path.Combine(_environment.ContentRootPath, 
                "sts_dev_cert.pfx"), "1234");
        var client = _clientFactory.CreateClient();
        var request = new HttpRequestMessage()
        {
            RequestUri = new Uri("https://localhost:44379/api/values"),
            Method = HttpMethod.Get,
        };

        request.Headers.Add("X-ARR-ClientCert", cert.GetRawCertDataString());
        var response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            var responseContent = await response.Content.ReadAsStringAsync();
            var data = JsonDocument.Parse(responseContent);

            return data;
        }

        throw new ApplicationException(
            $"Status code: {response.StatusCode}, " +
            $"Error: {response.ReasonPhrase}");
    }
    catch (Exception e)
    {
        throw new ApplicationException($"Exception {e}");
    }
}
```

<span data-ttu-id="b633e-199">Если на сервер отправляется правильный сертификат, возвращаются данные.</span><span class="sxs-lookup"><span data-stu-id="b633e-199">If the correct certificate is sent to the server, the data is returned.</span></span> <span data-ttu-id="b633e-200">Если сертификат или неправильный сертификат не отправляется, возвращается код состояния HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="b633e-200">If no certificate or the wrong certificate is sent, an HTTP 403 status code is returned.</span></span>

### <a name="create-certificates-in-powershell"></a><span data-ttu-id="b633e-201">Создание сертификатов в PowerShell</span><span class="sxs-lookup"><span data-stu-id="b633e-201">Create certificates in PowerShell</span></span>

<span data-ttu-id="b633e-202">Создание сертификатов — самая сложная часть настройки этого потока.</span><span class="sxs-lookup"><span data-stu-id="b633e-202">Creating the certificates is the hardest part in setting up this flow.</span></span> <span data-ttu-id="b633e-203">Корневой сертификат можно создать с помощью командлета PowerShell `New-SelfSignedCertificate`.</span><span class="sxs-lookup"><span data-stu-id="b633e-203">A root certificate can be created using the `New-SelfSignedCertificate` PowerShell cmdlet.</span></span> <span data-ttu-id="b633e-204">При создании сертификата используйте надежный пароль.</span><span class="sxs-lookup"><span data-stu-id="b633e-204">When creating the certificate, use a strong password.</span></span> <span data-ttu-id="b633e-205">Важно добавить параметр `KeyUsageProperty` и параметр `KeyUsage`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b633e-205">It's important to add the `KeyUsageProperty` parameter and the `KeyUsage` parameter as shown.</span></span>

#### <a name="create-root-ca"></a><span data-ttu-id="b633e-206">Создать корневой ЦС</span><span class="sxs-lookup"><span data-stu-id="b633e-206">Create root CA</span></span>

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

> [!NOTE]
> <span data-ttu-id="b633e-207">Значение параметра `-DnsName` должно соответствовать целевому объекту развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="b633e-207">The `-DnsName` parameter value must match the deployment target of the app.</span></span> <span data-ttu-id="b633e-208">Например, "localhost" для разработки.</span><span class="sxs-lookup"><span data-stu-id="b633e-208">For example, "localhost" for development.</span></span>

#### <a name="install-in-the-trusted-root"></a><span data-ttu-id="b633e-209">Установить в доверенном корневом каталоге</span><span class="sxs-lookup"><span data-stu-id="b633e-209">Install in the trusted root</span></span>

<span data-ttu-id="b633e-210">Корневой сертификат должен быть доверенным в системе узла.</span><span class="sxs-lookup"><span data-stu-id="b633e-210">The root certificate needs to be trusted on your host system.</span></span> <span data-ttu-id="b633e-211">Корневой сертификат, который не был создан центром сертификации, не будет доверенным по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b633e-211">A root certificate which was not created by a certificate authority won't be trusted by default.</span></span> <span data-ttu-id="b633e-212">Следующая ссылка объясняет, как это можно сделать в Windows:</span><span class="sxs-lookup"><span data-stu-id="b633e-212">The following link explains how this can be accomplished on Windows:</span></span>

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a><span data-ttu-id="b633e-213">Промежуточный сертификат</span><span class="sxs-lookup"><span data-stu-id="b633e-213">Intermediate certificate</span></span>

<span data-ttu-id="b633e-214">Теперь промежуточный сертификат можно создать на основе корневого сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-214">An intermediate certificate can now be created from the root certificate.</span></span> <span data-ttu-id="b633e-215">Это не является обязательным для всех вариантов использования, но может потребоваться создать несколько сертификатов или необходимо активировать или отключить группы сертификатов.</span><span class="sxs-lookup"><span data-stu-id="b633e-215">This isn't required for all use cases, but you might need to create many certificates or need to activate or disable groups of certificates.</span></span> <span data-ttu-id="b633e-216">Параметр `TextExtension` требуется для задания длины пути в базовых ограничениях сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-216">The `TextExtension` parameter is required to set the path length in the basic constraints of the certificate.</span></span>

<span data-ttu-id="b633e-217">Затем промежуточный сертификат можно добавить в доверенный промежуточный сертификат в системе узла Windows.</span><span class="sxs-lookup"><span data-stu-id="b633e-217">The intermediate certificate can then be added to the trusted intermediate certificate in the Windows host system.</span></span>

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a><span data-ttu-id="b633e-218">Создать дочерний сертификат из промежуточного сертификата</span><span class="sxs-lookup"><span data-stu-id="b633e-218">Create child certificate from intermediate certificate</span></span>

<span data-ttu-id="b633e-219">Дочерний сертификат можно создать из промежуточного сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-219">A child certificate can be created from the intermediate certificate.</span></span> <span data-ttu-id="b633e-220">Это конечная сущность, и ей не нужно создавать больше дочерних сертификатов.</span><span class="sxs-lookup"><span data-stu-id="b633e-220">This is the end entity and doesn't need to create more child certificates.</span></span>

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a><span data-ttu-id="b633e-221">Создать дочерний сертификат из корневого сертификата</span><span class="sxs-lookup"><span data-stu-id="b633e-221">Create child certificate from root certificate</span></span>

<span data-ttu-id="b633e-222">Дочерний сертификат можно также создать непосредственно из корневого сертификата.</span><span class="sxs-lookup"><span data-stu-id="b633e-222">A child certificate can also be created from the root certificate directly.</span></span> 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a><span data-ttu-id="b633e-223">Пример корневого промежуточного сертификата — сертификат</span><span class="sxs-lookup"><span data-stu-id="b633e-223">Example root - intermediate certificate - certificate</span></span>

```powershell
$mypwdroot = ConvertTo-SecureString -String "1234" -Force -AsPlainText
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

Get-ChildItem -Path cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwdroot

Export-Certificate -Cert cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 -FilePath root_ca_dev_damienbod.crt

$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\0C89639E4E2998A93E423F919B36D4009A0F9991 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 -FilePath child_a_dev_damienbod.crt

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\BA9BF91ED35538A01375EFC212A2F46104B33A44 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_b_from_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_b_from_a_dev_damienbod.com" 

Get-ChildItem -Path cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_b_from_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A -FilePath child_b_from_a_dev_damienbod.crt
```

<span data-ttu-id="b633e-224">При использовании корневого, промежуточного или дочернего сертификата сертификаты можно проверять с помощью отпечатка или PublicKey по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="b633e-224">When using the root, intermediate, or child certificates, the certificates can be validated using the Thumbprint or PublicKey as required.</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService 
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            return CheckIfThumbprintIsValid(clientCertificate);
        }

        private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
        {
            var listOfValidThumbprints = new List<string>
            {
                "141594A0AE38CBBECED7AF680F7945CD51D8F28A",
                "0C89639E4E2998A93E423F919B36D4009A0F9991",
                "BA9BF91ED35538A01375EFC212A2F46104B33A44"
            };

            if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
            {
                return true;
            }

            return false;
        }
    }
}
```
