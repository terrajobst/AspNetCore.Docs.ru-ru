---
title: Настройка проверки подлинности сертификата в ASP.NET Core
author: blowdart
description: Узнайте, как настроить проверку подлинности сертификата в ASP.NET Core для IIS и HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 8609c58265340da1d618135795915d6c49e750a3
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538724"
---
# <a name="overview"></a>Обзор

`Microsoft.AspNetCore.Authentication.Certificate` содержит реализацию, аналогичную [аутентификации на основе сертификата](https://tools.ietf.org/html/rfc5246#section-7.4.4) для ASP.NET Core. Сертификат проверки подлинности происходит на уровне TLS, long, прежде, чем когда-либо возвращает на ASP.NET Core. Точнее говоря, это обработчик проверки подлинности, который проверяет сертификат, а затем сообщает событие, где можно устранить этот сертификат для `ClaimsPrincipal`. 

[Настройка вашего узла](#configure-your-host-to-require-certificates) для проверки подлинности сертификата, будь то, как IIS, Kestrel, веб-приложений Azure или что-нибудь еще используется.

## <a name="get-started"></a>Начало работы

Получить сертификат HTTPS, применить его, и [Настройка вашего узла](#configure-your-host-to-require-certificates) подавался запрос на сертификат.

В веб-приложении, добавьте ссылку на `Microsoft.AspNetCore.Authentication.Certificate` пакета. Затем в `Startup.Configure` мы вызываем метод `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` с вариантами, предоставьте делегат для `OnCertificateValidated` для выполнения какой-либо дополнительных проверки сертификата клиента, в ответ на запросы. Включить эту информацию в `ClaimsPrincipal` и задайте его в `context.Principal` свойство.

Если не пройдет проверку подлинности, этот обработчик возвращает `403 (Forbidden)` ответ скорее `401 (Unauthorized)`, как следует из названия. Обоснованием является то, что проверка подлинности должны быть выполнены во время начального подключения TLS. К моменту достижения обработчик уже слишком поздно. Нет способа для обновления подключения из анонимного соединения к одному с помощью сертификата.

Кроме того, добавить `app.UseAuthentication();` в `Startup.Configure` метод. В противном случае HttpContext.User не устанавливается в `ClaimsPrincipal` создан на основе сертификата. Пример:

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

В предыдущем примере стандартного способа для добавления проверки подлинности сертификата. Обработчик создает участника-пользователя, с помощью общих свойств сертификата.

## <a name="configure-certificate-validation"></a>Настройка проверки сертификата

`CertificateAuthenticationOptions` Обработчик имеет некоторые встроенные проверки, которые являются минимальным проверки следует выполнять на сертификат. Каждый из этих параметров включены по умолчанию.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = объединять в цепочки, SelfSigned или все (по цепочке | SelfSigned)

Эта проверка проверяет, что разрешен только тип соответствующий сертификат.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Эта проверка проверяет наличие сертификата, предоставленного клиентом проверки подлинности клиента, вообще расширенного использования ключа (EKU) или нет EKU. Так как спецификации скажем, если не указан, то всех областей EKU являются допустимыми.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Эта проверка проверяет, что сертификат действителен в течение срока его действия. При каждом запросе обработчик гарантирует, что сертификат, который был допустимым, если он был представлен еще не истек, во время текущего сеанса.

### <a name="revocationflag"></a>RevocationFlag

Флаг, указывающий, какие сертификаты в цепочке проверяются для отзыва.

Проверку отзыва выполняются только в том случае, если сертификат был связан с корневым сертификатом.

### <a name="revocationmode"></a>revocationMode

Флаг, указывающий, как выполняются проверки отзыва.

Указание проверки в сети может привести длительная задержка при связи с центром сертификации.

Проверку отзыва выполняются только в том случае, если сертификат был связан с корневым сертификатом.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Можно ли настроить мое приложение требует сертификата только на определенные пути?

Это невозможно. Помните, что выполняется обмен сертификатами, что начала диалога HTTPS, это будет сделано на сервере до получения первого запроса для этого соединения, он уже не сможете область в зависимости от полей запроса.

## <a name="handler-events"></a>Обработчик события

Обработчик имеет два события:

* `OnAuthenticationFailed` &ndash; Вызывается, если исключение происходит во время проверки подлинности и позволяет реагировать.
* `OnCertificateValidated` &ndash; Вызывается после проверки сертификата, прошедшие проверку на соответствие и создания участника по умолчанию. Это событие позволяет выполнять собственную проверку и дополнения или замены основного сервера. Примеры включают:
  * Определение, если сертификат известен в службы.
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

Если входящий сертификат не соответствует требованиям вашей дополнительную проверку, вызовите `context.Fail("failure reason")` с указанием причины.

Для выполнения реальных задач функциональные возможности, возможно, стоит для вызова службы, зарегистрированные в системе внедрения зависимостей, который подключается к базе данных или других видов хранилище пользователя. Доступ к службе с помощью контекста, передаваемые в делегате. Рассмотрим следующий пример в `Startup.ConfigureServices`:

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

По существу проверку сертификата является аспектом авторизации. Добавление проверки, например, издателя или отпечаток в политику авторизации, а не внутри `OnCertificateValidated`, тогда вполне приемлемым.

## <a name="configure-your-host-to-require-certificates"></a>Настройка узла для запроса сертификатов

### <a name="kestrel"></a>Kestrel

В *Program.cs*, настройте Kestrel следующим образом:

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

Выполните следующие действия в диспетчере служб IIS.

1. Выберите сайт из **подключений** вкладки.
1. Дважды щелкните **параметры SSL** в диалоговом окне **Просмотр возможностей** окна.
1. Проверьте **Требовать SSL** флажок и выберите **требуют** переключателя в **сертификаты клиента** раздел.

![Параметры сертификата клиента в службах IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure и пользовательского веб-прокси

См. в разделе [размещение и развертывание документации](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) для настройки сертификата, пересылку по промежуточного слоя.
