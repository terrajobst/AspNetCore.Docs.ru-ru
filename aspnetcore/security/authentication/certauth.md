---
title: Настройка проверки подлинности сертификата в ASP.NET Core
author: blowdart
description: Узнайте, как настроить проверку подлинности сертификата в ASP.NET Core для IIS и HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: bb375cf380175daf2399f3b56f543819ee5692b8
ms.sourcegitcommit: 07cd66e367d080acb201c7296809541599c947d1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039240"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="05428-103">Настройка проверки подлинности сертификата в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05428-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="05428-104">`Microsoft.AspNetCore.Authentication.Certificate`содержит реализацию, похожую на [проверку подлинности с помощью сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05428-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="05428-105">Проверка подлинности сертификата выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05428-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="05428-106">Точнее, это обработчик проверки подлинности, который проверяет сертификат, а затем предоставляет событие, в котором можно разрешить этот сертификат в `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="05428-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="05428-107">[Настройте узел](#configure-your-host-to-require-certificates) для проверки подлинности на основе сертификата, как IIS, Kestrel, веб-приложения Azure или любое другое приложение, которое вы используете.</span><span class="sxs-lookup"><span data-stu-id="05428-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="05428-108">Сценарии прокси-сервера и подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="05428-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="05428-109">Проверка подлинности с помощью сертификата — это сценарий с отслеживанием состояния, который в основном используется, когда прокси или балансировщик нагрузки не обрабатывает трафик между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="05428-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="05428-110">Если используется прокси-сервер или балансировщик нагрузки, проверка подлинности сертификата работает только в том случае, если прокси-сервер или балансировщик нагрузки:</span><span class="sxs-lookup"><span data-stu-id="05428-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="05428-111">Обрабатывает проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="05428-111">Handles the authentication.</span></span>
* <span data-ttu-id="05428-112">Передает сведения о проверке подлинности пользователя в приложение (например, в заголовке запроса), которое действует на данные проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="05428-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="05428-113">Альтернативой для проверки подлинности на сертификат в средах, где используются прокси-серверы и подсистемы балансировки нагрузки, являются Active Directory Федеративные службы (ADFS) с OpenID Connect Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="05428-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="05428-114">Начало работы</span><span class="sxs-lookup"><span data-stu-id="05428-114">Get started</span></span>

<span data-ttu-id="05428-115">Получите HTTPS сертификат, примените его и [Настройте узел](#configure-your-host-to-require-certificates) так, чтобы он затребовал сертификаты.</span><span class="sxs-lookup"><span data-stu-id="05428-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="05428-116">В веб-приложении добавьте ссылку на `Microsoft.AspNetCore.Authentication.Certificate` пакет.</span><span class="sxs-lookup"><span data-stu-id="05428-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="05428-117">Затем в `Startup.Configure` методе вызовите `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` с вашими параметрами, предоставив делегат для `OnCertificateValidated` , чтобы выполнить дополнительную проверку сертификата клиента, отправляемого с запросами.</span><span class="sxs-lookup"><span data-stu-id="05428-117">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="05428-118">Включите эту информацию в `ClaimsPrincipal` и установите ее `context.Principal` в свойстве.</span><span class="sxs-lookup"><span data-stu-id="05428-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="05428-119">Если проверка подлинности завершается неудачно, `401 (Unauthorized)`этот обработчик возвращает `403 (Forbidden)` ответ, а не, как вы можете ожидать.</span><span class="sxs-lookup"><span data-stu-id="05428-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="05428-120">Причина заключается в том, что проверка подлинности должна выполняться во время первоначального TLS-подключения.</span><span class="sxs-lookup"><span data-stu-id="05428-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="05428-121">К моменту, когда он достигает обработчика, он слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="05428-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="05428-122">Невозможно обновить подключение между анонимным подключением и сертификатом.</span><span class="sxs-lookup"><span data-stu-id="05428-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="05428-123">Кроме того `app.UseAuthentication();` , `Startup.Configure` добавьте в метод.</span><span class="sxs-lookup"><span data-stu-id="05428-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="05428-124">В противном случае HttpContext. User не будет настроен на `ClaimsPrincipal` создание из сертификата.</span><span class="sxs-lookup"><span data-stu-id="05428-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="05428-125">Например:</span><span class="sxs-lookup"><span data-stu-id="05428-125">For example:</span></span>

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

<span data-ttu-id="05428-126">В предыдущем примере показан способ добавления проверки подлинности с помощью сертификата по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="05428-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="05428-127">Обработчик конструирует субъект-пользователя, используя общие свойства сертификата.</span><span class="sxs-lookup"><span data-stu-id="05428-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="05428-128">Настройка проверки сертификата</span><span class="sxs-lookup"><span data-stu-id="05428-128">Configure certificate validation</span></span>

<span data-ttu-id="05428-129">`CertificateAuthenticationOptions` Обработчик содержит некоторые встроенные проверки, которые являются минимальными проверками, которые следует выполнить с сертификатом.</span><span class="sxs-lookup"><span data-stu-id="05428-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="05428-130">Каждый из этих параметров включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="05428-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="05428-131">Алловедцертификатетипес = цепочка, Селфсигнед или все (цепочка | Селфсигнед)</span><span class="sxs-lookup"><span data-stu-id="05428-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="05428-132">Эта проверка подтверждает, что разрешен только соответствующий тип сертификата.</span><span class="sxs-lookup"><span data-stu-id="05428-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="05428-133">валидатецертификатеусе</span><span class="sxs-lookup"><span data-stu-id="05428-133">ValidateCertificateUse</span></span>

<span data-ttu-id="05428-134">Эта проверка проверяет, что сертификат, предоставленный клиентом, имеет расширенное использование ключа проверки подлинности клиента (EKU) или вообще не содержит EKU.</span><span class="sxs-lookup"><span data-stu-id="05428-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="05428-135">Как говорится в спецификации, если EKU не указано, все EKU считаются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="05428-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="05428-136">валидатевалидитипериод</span><span class="sxs-lookup"><span data-stu-id="05428-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="05428-137">Эта проверка проверяет, что сертификат находится в пределах срока действия.</span><span class="sxs-lookup"><span data-stu-id="05428-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="05428-138">В каждом запросе обработчик гарантирует, что сертификат, который был действителен, когда он был предоставлен, не истечет в течение текущего сеанса.</span><span class="sxs-lookup"><span data-stu-id="05428-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="05428-139">ревокатионфлаг</span><span class="sxs-lookup"><span data-stu-id="05428-139">RevocationFlag</span></span>

<span data-ttu-id="05428-140">Флаг, указывающий, какие сертификаты в цепочке проверяются на отзыв.</span><span class="sxs-lookup"><span data-stu-id="05428-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="05428-141">Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="05428-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="05428-142">ревокатионмоде</span><span class="sxs-lookup"><span data-stu-id="05428-142">RevocationMode</span></span>

<span data-ttu-id="05428-143">Флаг, указывающий, как выполняются проверки отзыва.</span><span class="sxs-lookup"><span data-stu-id="05428-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="05428-144">Указание проверки в сети может привести к длительной задержке при обращении к центру сертификации.</span><span class="sxs-lookup"><span data-stu-id="05428-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="05428-145">Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.</span><span class="sxs-lookup"><span data-stu-id="05428-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="05428-146">Можно ли настроить в приложении требование сертификата только для определенных путей?</span><span class="sxs-lookup"><span data-stu-id="05428-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="05428-147">Это невозможно.</span><span class="sxs-lookup"><span data-stu-id="05428-147">This isn't possible.</span></span> <span data-ttu-id="05428-148">Помните, что обмен сертификатами выполняется в начале диалога HTTPS, он выполняется сервером до получения первого запроса на это подключение, поэтому невозможно ограничить область на основе каких-либо полей запроса.</span><span class="sxs-lookup"><span data-stu-id="05428-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="05428-149">События обработчика</span><span class="sxs-lookup"><span data-stu-id="05428-149">Handler events</span></span>

<span data-ttu-id="05428-150">Обработчик имеет два события:</span><span class="sxs-lookup"><span data-stu-id="05428-150">The handler has two events:</span></span>

* <span data-ttu-id="05428-151">`OnAuthenticationFailed`&ndash; Вызывается, если исключение происходит во время проверки подлинности и позволяет реагировать.</span><span class="sxs-lookup"><span data-stu-id="05428-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="05428-152">`OnCertificateValidated`&ndash; Вызывается после проверки сертификата, прошли проверку и был создан участник по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="05428-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="05428-153">Это событие позволяет выполнять собственную проверку и дополнять или заменять субъект.</span><span class="sxs-lookup"><span data-stu-id="05428-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="05428-154">Ниже приведены примеры.</span><span class="sxs-lookup"><span data-stu-id="05428-154">For examples include:</span></span>
  * <span data-ttu-id="05428-155">Определение, известен ли сертификат службам.</span><span class="sxs-lookup"><span data-stu-id="05428-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="05428-156">Создание собственного участника.</span><span class="sxs-lookup"><span data-stu-id="05428-156">Constructing your own principal.</span></span> <span data-ttu-id="05428-157">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05428-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="05428-158">Если входящий сертификат не соответствует дополнительной проверке, позвоните `context.Fail("failure reason")` по причине сбоя.</span><span class="sxs-lookup"><span data-stu-id="05428-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="05428-159">Для реальной функциональности вам, вероятно, потребуется вызвать службу, зарегистрированную в внедрении зависимостей, которая подключается к базе данных или другому хранилищу пользователей.</span><span class="sxs-lookup"><span data-stu-id="05428-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="05428-160">Получите доступ к службе с помощью контекста, переданного в делегат.</span><span class="sxs-lookup"><span data-stu-id="05428-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="05428-161">Рассмотрим следующий пример в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05428-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="05428-162">По сути, проверка сертификата является проблемой авторизации.</span><span class="sxs-lookup"><span data-stu-id="05428-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="05428-163">Добавление проверки, например, издателя или отпечатка в политике авторизации, а не внутри `OnCertificateValidated`, вполне приемлемо.</span><span class="sxs-lookup"><span data-stu-id="05428-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="05428-164">Настройка узла для использования сертификатов</span><span class="sxs-lookup"><span data-stu-id="05428-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="05428-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="05428-165">Kestrel</span></span>

<span data-ttu-id="05428-166">В *Program.CS*настройте Kestrel следующим образом:</span><span class="sxs-lookup"><span data-stu-id="05428-166">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="05428-167">IIS</span><span class="sxs-lookup"><span data-stu-id="05428-167">IIS</span></span>

<span data-ttu-id="05428-168">В диспетчере служб IIS выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="05428-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="05428-169">Выберите сайт на вкладке " **подключения** ".</span><span class="sxs-lookup"><span data-stu-id="05428-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="05428-170">Дважды щелкните параметр **Параметры SSL** в окне **просмотра функций** .</span><span class="sxs-lookup"><span data-stu-id="05428-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="05428-171">Установите флажок **требовать SSL** и установите переключатель **требуется** в разделе **Сертификаты клиента** .</span><span class="sxs-lookup"><span data-stu-id="05428-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Параметры сертификата клиента в службах IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="05428-173">Azure и настраиваемые веб-прокси</span><span class="sxs-lookup"><span data-stu-id="05428-173">Azure and custom web proxies</span></span>

<span data-ttu-id="05428-174">Сведения о настройке по промежуточного слоя для переадресации сертификатов см. в [документации по узлу и развертыванию](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .</span><span class="sxs-lookup"><span data-stu-id="05428-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
