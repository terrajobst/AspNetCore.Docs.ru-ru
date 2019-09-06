---
title: Настройка проверки подлинности сертификата в ASP.NET Core
author: blowdart
description: Узнайте, как настроить проверку подлинности сертификата в ASP.NET Core для IIS и HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: ce7bcdbfb8ce0f1febf34b49786e92c917be139c
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384836"
---
# <a name="overview"></a>Обзор

`Microsoft.AspNetCore.Authentication.Certificate`содержит реализацию, похожую на [проверку подлинности с помощью сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) для ASP.NET Core. Проверка подлинности сертификата выполняется на уровне TLS, прежде чем он когда-либо получит ASP.NET Core. Точнее, это обработчик проверки подлинности, который проверяет сертификат, а затем предоставляет событие, в котором можно разрешить этот сертификат в `ClaimsPrincipal`. 

[Настройте узел](#configure-your-host-to-require-certificates) для проверки подлинности на основе сертификата, как IIS, Kestrel, веб-приложения Azure или любое другое приложение, которое вы используете.

## <a name="proxy-and-load-balancer-scenarios"></a>Сценарии прокси-сервера и подсистемы балансировки нагрузки

Проверка подлинности с помощью сертификата — это сценарий с отслеживанием состояния, который в основном используется, когда прокси или балансировщик нагрузки не обрабатывает трафик между клиентами и серверами. Если используется прокси-сервер или балансировщик нагрузки, проверка подлинности сертификата работает только в том случае, если прокси-сервер или балансировщик нагрузки:

* Обрабатывает проверку подлинности.
* Передает сведения о проверке подлинности пользователя в приложение (например, в заголовке запроса), которое действует на данные проверки подлинности.

Альтернативой для проверки подлинности на сертификат в средах, где используются прокси-серверы и подсистемы балансировки нагрузки, являются Active Directory Федеративные службы (ADFS) с OpenID Connect Connect (OIDC).

## <a name="get-started"></a>Начало работы

Получите HTTPS сертификат, примените его и [Настройте узел](#configure-your-host-to-require-certificates) так, чтобы он затребовал сертификаты.

В веб-приложении добавьте ссылку на `Microsoft.AspNetCore.Authentication.Certificate` пакет. Затем в `Startup.Configure` методе вызовите `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` с вашими параметрами, предоставив делегат для `OnCertificateValidated` , чтобы выполнить дополнительную проверку сертификата клиента, отправляемого с запросами. Включите эту информацию в `ClaimsPrincipal` и установите ее `context.Principal` в свойстве.

Если проверка подлинности завершается неудачно, `401 (Unauthorized)`этот обработчик возвращает `403 (Forbidden)` ответ, а не, как вы можете ожидать. Причина заключается в том, что проверка подлинности должна выполняться во время первоначального TLS-подключения. К моменту, когда он достигает обработчика, он слишком поздно. Невозможно обновить подключение между анонимным подключением и сертификатом.

Кроме того `app.UseAuthentication();` , `Startup.Configure` добавьте в метод. В противном случае HttpContext. User не будет настроен на `ClaimsPrincipal` создание из сертификата. Пример:

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

В предыдущем примере показан способ добавления проверки подлинности с помощью сертификата по умолчанию. Обработчик конструирует субъект-пользователя, используя общие свойства сертификата.

## <a name="configure-certificate-validation"></a>Настройка проверки сертификата

`CertificateAuthenticationOptions` Обработчик содержит некоторые встроенные проверки, которые являются минимальными проверками, которые следует выполнить с сертификатом. Каждый из этих параметров включен по умолчанию.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>Алловедцертификатетипес = цепочка, Селфсигнед или все (цепочка | Селфсигнед)

Эта проверка подтверждает, что разрешен только соответствующий тип сертификата.

### <a name="validatecertificateuse"></a>валидатецертификатеусе

Эта проверка проверяет, что сертификат, предоставленный клиентом, имеет расширенное использование ключа проверки подлинности клиента (EKU) или вообще не содержит EKU. Как говорится в спецификации, если EKU не указано, все EKU считаются допустимыми.

### <a name="validatevalidityperiod"></a>валидатевалидитипериод

Эта проверка проверяет, что сертификат находится в пределах срока действия. В каждом запросе обработчик гарантирует, что сертификат, который был действителен, когда он был предоставлен, не истечет в течение текущего сеанса.

### <a name="revocationflag"></a>ревокатионфлаг

Флаг, указывающий, какие сертификаты в цепочке проверяются на отзыв.

Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.

### <a name="revocationmode"></a>ревокатионмоде

Флаг, указывающий, как выполняются проверки отзыва.

Указание проверки в сети может привести к длительной задержке при обращении к центру сертификации.

Проверки отзыва выполняются только в том случае, если сертификат связан с корневым сертификатом.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Можно ли настроить в приложении требование сертификата только для определенных путей?

Это невозможно. Помните, что обмен сертификатами выполняется в начале диалога HTTPS, он выполняется сервером до получения первого запроса на это подключение, поэтому невозможно ограничить область на основе каких-либо полей запроса.

## <a name="handler-events"></a>События обработчика

Обработчик имеет два события:

* `OnAuthenticationFailed`&ndash; Вызывается, если исключение происходит во время проверки подлинности и позволяет реагировать.
* `OnCertificateValidated`&ndash; Вызывается после проверки сертификата, прошли проверку и был создан участник по умолчанию. Это событие позволяет выполнять собственную проверку и дополнять или заменять субъект. Ниже приведены примеры.
  * Определение, известен ли сертификат службам.
  * Создание собственного участника. Рассмотрим следующий пример в `Startup.ConfigureServices`:

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

Если входящий сертификат не соответствует дополнительной проверке, позвоните `context.Fail("failure reason")` по причине сбоя.

Для реальной функциональности вам, вероятно, потребуется вызвать службу, зарегистрированную в внедрении зависимостей, которая подключается к базе данных или другому хранилищу пользователей. Получите доступ к службе с помощью контекста, переданного в делегат. Рассмотрим следующий пример в `Startup.ConfigureServices`:

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

По сути, проверка сертификата является проблемой авторизации. Добавление проверки, например, издателя или отпечатка в политике авторизации, а не внутри `OnCertificateValidated`, вполне приемлемо.

## <a name="configure-your-host-to-require-certificates"></a>Настройка узла для использования сертификатов

### <a name="kestrel"></a>Kestrel

В *Program.CS*настройте Kestrel следующим образом:

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

### <a name="iis"></a>IIS

В диспетчере служб IIS выполните следующие действия.

1. Выберите сайт на вкладке " **подключения** ".
1. Дважды щелкните параметр **Параметры SSL** в окне **просмотра функций** .
1. Установите флажок **требовать SSL** и установите переключатель **требуется** в разделе **Сертификаты клиента** .

![Параметры сертификата клиента в службах IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure и настраиваемые веб-прокси

Сведения о настройке по промежуточного слоя для переадресации сертификатов см. в [документации по узлу и развертыванию](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .
