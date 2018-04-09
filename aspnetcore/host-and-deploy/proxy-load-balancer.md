---
title: Настройка ASP.NET Core для работы с прокси-серверы и подсистемы балансировки нагрузки.
author: guardrex
description: Дополнительные сведения о конфигурации для приложений, размещенных за прокси-серверы и подсистем балансировки нагрузки, которые часто скрывать запрос важные сведения.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Настройка ASP.NET Core для работы с прокси-серверы и подсистемы балансировки нагрузки.

По [Latham Люк](https://github.com/guardrex) и [Ross Крис](https://github.com/Tratcher)

В рекомендуемой конфигурации для ASP.NET Core приложение размещается с использованием модуля Core IIS/ASP.NET, Nginx или Apache. Прокси-серверы, подсистемы балансировки нагрузки и другие сетевые устройства часто скрывать сведения о запросе, прежде чему будет достигнут приложения:

* Когда HTTPS-запросы по протоколу HTTP через прокси, первоначальную схему (HTTPS) теряется и следует перенаправить в заголовке.
* Так как приложение получает запрос от прокси-сервера и не true источником в Интернете или интрасети, исходный IP-адрес клиента, также следует перенаправить в заголовке.

Эти сведения могут быть важны в обработке запросов, например перенаправления, проверки подлинности, компоновки, оценки политики и geoloation клиента.

## <a name="forwarded-headers"></a>Перенаправленных заголовки

По соглашению прокси-серверы пересылки данные в заголовках HTTP.

| Header | Описание |
| ------ | ----------- |
| X-перенаправленных для | Содержит сведения о клиенте, который инициировал запрос и последующие прокси в цепочке учетных записей-посредников. Этот параметр может содержать IP адресов (и, при необходимости, номера портов). В цепочке прокси-серверов первый параметр указывает клиента, где сначала был сделан запрос. Выполните идентификаторы последующих прокси-сервера. Последний прокси-сервера в цепочке отсутствует в списке параметров. IP-адрес последнего прокси и при необходимости номер порта, доступны как удаленный IP-адрес на транспортном уровне. |
| X-Forwarded-Proto | Значение исходную схему (HTTP/HTTPS). Значение также может быть список схем, если запрос обход нескольких учетных записей-посредников. |
| Пересылаемые узлов X | Исходное значение поля заголовка узла. Как правило учетные записи-посредники не следует изменять заголовок узла. В разделе [Microsoft безопасности рекомендация CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) сведения о уязвимости повышение привилегий, влияет на системах, где не проверить учетную запись-посредник или заголовки узлов restict для известных значений хорошо. |

Пересылаемые заголовки по промежуточного слоя, из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) упаковки, считывает эти заголовки и заполнит связанные поля на [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Обновления по промежуточного слоя:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; задать с помощью `X-Forwarded-For` значение заголовка. Дополнительные параметры влияют как по промежуточного слоя задает `RemoteIpAddress`. Дополнительные сведения см. в разделе [параметры пересылаются по промежуточного слоя заголовки](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; задать с помощью `X-Forwarded-Proto` значение заголовка.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; задать с помощью `X-Forwarded-Host` значение заголовка.

Обратите внимание, что не все сетевые устройства добавить `X-Forwarded-For` и `X-Forwarded-Proto` заголовки без дополнительной настройки. Если запросы через прокси не содержит эти заголовки, когда они достигнут приложения, обратитесь к инструкции изготовителем устройства.

Пересылаются по промежуточного слоя заголовки [параметры по умолчанию](#forwarded-headers-middleware-options) можно настроить. Значения по умолчанию.

* Может быть только *прокси* между приложением и источником запросов.
* Только адреса замыкания на себя настраиваются для известных учетных записей-посредников и известные сетей.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS и IIS Express и модулей ASP.NET Core

По промежуточного слоя перенаправленных заголовки включена по умолчанию по промежуточного слоя интеграции IIS, при запуске приложения под IIS и модуль ASP.NET Core. Для запуска первого конвейера по промежуточного слоя с ограниченным доступом определенных конфигурацией в модуль ASP.NET Core из-за доверия опасения перенаправленных заголовки активируется перенаправленных заголовки по промежуточного слоя (например, [подмена IP-](https://www.iplocation.net/ip-spoofing)). По промежуточного слоя настроен на пересылку `X-Forwarded-For` и `X-Forwarded-Proto` заголовки и предназначен только для localhost один прокси-сервер. Если требуется дополнительная настройка, см. раздел [параметры пересылаются по промежуточного слоя заголовки](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Другие прокси-сервера и сценарии подсистемы балансировки нагрузки

Помимо использования по промежуточного слоя интеграции IIS, пересылаемые по промежуточного слоя заголовки не включен по умолчанию. Перенаправленных заголовки по промежуточного слоя должна быть включена для приложения, чтобы процесс перенаправленных заголовков с [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). После включения по промежуточного слоя, если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) задаются по промежуточного слоя, значение по умолчанию [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) , [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Настройки по промежуточного слоя с [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) переслать `X-Forwarded-For` и `X-Forwarded-Proto` заголовки в `Startup.ConfigureServices`. Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом другого по промежуточного слоя:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) указываются в `Startup.ConfigureServices` или напрямую в метод расширения с [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), значение по умолчанию заголовки для пересылки [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) свойства должны быть настроены заголовки для пересылки.

## <a name="forwarded-headers-middleware-options"></a>Перенаправленных параметры по промежуточного слоя заголовки

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) управляют поведением по промежуточного слоя перенаправленных заголовки:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Параметр | Описание |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Значение по умолчанию — `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Определяет, какие серверы пересылки будут обрабатываться. В разделе [ForwardedHeaders перечисления](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) список полей, которые относятся. Допустимые значения этого свойства: <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Значение по умолчанию — [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Значение по умолчанию — `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Значение по умолчанию — `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ограничивает количество записей в заголовках, которые обрабатываются. Значение `null` отключить ограничение, но это следует делать только если `KnownProxies` или `KnownNetworks` настроены.<br><br>Значение по умолчанию — 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Диапазоны известных учетных записей-посредников для приема перенаправленных заголовки из адресов. Укажите диапазоны IP-адресов, используя нотацию бесклассовой междоменного маршрутизации (CIDR).<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-сети](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> содержащий одну запись для `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Адреса для принятия перенаправленных заголовки из известных прокси-сервера. Используйте `KnownProxies` для указания точного IP-адрес соответствует.<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-адрес](/dotnet/api/system.net.ipaddress)> содержащий одну запись для `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Значение по умолчанию — `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Значение по умолчанию — `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Значение по умолчанию — `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Требуется номер значений заголовка, должны быть синхронизированы между [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) обрабатывается.<br><br>Значение по умолчанию в ASP.NET Core 1.x — `true`. Значение по умолчанию в ASP.NET Core 2.0 или более поздней версии — `false`. |

## <a name="scenarios-and-use-cases"></a>Сценарии и примеры

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>При нельзя добавить перенаправленных заголовки и все запросы являются безопасными

В некоторых случаях может быть невозможно добавить перенаправленных заголовки для запросов через прокси, чтобы приложение. Если прокси-сервер установлен режим принудительного применения, все открытые внешние запросы HTTPS, схему можно вручную задать в `Startup.Configure` перед использованием любой тип по промежуточного слоя:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Этот код можно отключить с помощью переменной среды или другого параметра конфигурации при разработке или в тестовой среде.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Работать с база пути и учетных записей-посредников, которые изменяют путь запроса

Некоторые учетные записи-посредники передать путь без изменений, но с приложением, базовый путь, который должен быть удален, чтобы маршрутизации работает правильно. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) по промежуточного слоя разделяет путь в [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) и базовой папке приложения в [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Если `/foo` является базовой папке приложения, для прокси-сервера путь передается в качестве `/foo/api/1`, по промежуточного слоя наборы `Request.PathBase` для `/foo` и `Request.Path` для `/api/1` с помощью следующей команды:

```csharp
app.UsePathBase("/foo");
```

Исходный путь и базовый путь применяются, когда по промежуточного слоя вызывается повторно в обратном порядке. Дополнительные сведения об обработке заказов по промежуточного слоя см. в разделе [по промежуточного слоя](xref:fundamentals/middleware/index).

Если прокси-сервер усекает путь (например, пересылка `/foo/api/1` для `/api/1`), исправление перенаправляет и ссылки, задав запроса [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) свойства:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Если прокси-сервер добавляет данные пути, извлеките часть пути устранения ошибки перенаправления и ссылки [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) и присвоение [путь](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) свойство:

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a>Устранение неполадок

Включить заголовки не перенаправления должным образом, [ведение журнала](xref:fundamentals/logging/index). Если журналы не предоставляет достаточно сведений для устранения неполадок, указаны заголовки запросов, полученных сервером. Заголовки могут записываться для ответа на приложения, с помощью встроенного ПО промежуточного слоя:

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

Убедитесь, что X - перенаправленных-* заголовки, полученных от сервера с ожидаемыми значениями. Если существует несколько значений в данный заголовок, обратите внимание, пересылаемые по промежуточного слоя заголовки процессы заголовки в обратном порядке справа налево.

Исходный IP удаленного запроса должно соответствовать запись в `KnownProxies` или `KnownNetworks` перечислены до X-перенаправленных-для обработки. Это ограничивает заголовок спуфинг, не принимая пересылки из ненадежных учетных записей-посредников.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core повышения уровня прав доступа](https://github.com/aspnet/Announcements/issues/295)
