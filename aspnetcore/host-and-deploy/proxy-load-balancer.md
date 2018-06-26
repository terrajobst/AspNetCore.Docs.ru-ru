---
title: Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки
author: guardrex
description: Сведения о конфигурации приложений, размещаемых за прокси-серверами и подсистемами балансировки нагрузки, которые могут мешать передаче важных сведений в запросах.
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 1797962d6eada9c48b31cd94e2c7481380301a0d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276780"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

В рекомендуемой конфигурации для ASP.NET Core приложение размещается с использованием модуля ASP.NET Core для IIS, Nginx или Apache. Прокси-серверы, подсистемы балансировки нагрузки и другие сетевые устройства часто скрывают сведения о запросах, еще не достигших целевого приложения.

* Если прокси-сервер передает HTTPS-запросы через протокол HTTP, исходная схема (HTTPS) теряется и ее нужно дополнительно передать в заголовке.
* Так как приложение получает запрос от прокси-сервера, а не от правильного источника в Интернете или корпоративной сети, исходный IP-адрес клиента также нужно передать в заголовке.

Эти сведения могут быть важны при обработке запросов, например для перенаправления, аутентификации, создания ссылок, оценки политик и геолокации клиентов.

## <a name="forwarded-headers"></a>Перенаправленные заголовки

По действующему соглашению прокси-серверы передают данные в заголовках HTTP.

| Header | Описание: |
| ------ | ----------- |
| X-Forwarded-For | Содержит сведения о клиенте, который создал этот запрос, и обо всех предыдущих узлах в цепочке прокси-серверов. Этот параметр может содержать IP-адреса (и номера портов, если потребуется). В цепочке прокси-серверов первый параметр обозначает клиента, отправившего исходный запрос. За ним следуют идентификаторы всех последующих прокси-серверов. В списке параметров отсутствует последний прокси-сервер в цепочке. IP-адрес последнего прокси-сервера (и номер порта, если нужно) поступает в формате удаленного IP-адреса на транспортном уровне. |
| X-Forwarded-Proto | Значение исходной схемы передачи данных (HTTP/HTTPS). Здесь может быть указан целый список схем, если запрос прошел через несколько прокси-серверов. |
| X-Forwarded-Host | Исходное значение поля Host в заголовке запроса. Обычно прокси-серверы не изменяют заголовок Host. В [рекомендациях Макрософт по безопасности CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) представлены сведения об уязвимости, связанной с повышением привилегий, которая затрагивает системы, где прокси-сервер не проверяет заголовок Host и не ограничивает его значения известными допустимыми значениями. |

ПО промежуточного слоя перенаправления заголовков, входящее в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/), считывает эти заголовки и заполняет соответствующие поля [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

Это ПО промежуточного слоя обновляет следующие сведения:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; устанавливается на основе значения заголовка `X-Forwarded-For`. Дополнительные параметры влияют на значение `RemoteIpAddress`, которое устанавливает ПО промежуточного слоя. Дополнительные сведения см. в разделе [Параметры ПО промежуточного слоя перенаправления заголовков](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; устанавливается на основе значения заголовка `X-Forwarded-Proto`.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; устанавливается на основе значения заголовка `X-Forwarded-Host`.

Обратите внимание, что не все сетевые устройства добавляют заголовки `X-Forwarded-For` и `X-Forwarded-Proto` без дополнительной настройки. Если запросы, поступающие в приложение через прокси-сервер, не содержат эти заголовки, обратитесь к руководствам изготовителя устройства.

Для ПО промежуточного слоя перенаправления заголовков можно настроить [параметры по умолчанию](#forwarded-headers-middleware-options). По умолчанию используются следующие параметры:

* Существует только *один прокси* между приложением и источником запросов.
* Для известных прокси-серверов и известных сетей настраиваются только петлевые адреса.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS и IIS Express с модулем ASP.NET Core

ПО промежуточного слоя перенаправления заголовков по умолчанию включается в ПО промежуточного слоя интеграции IIS при выполнении приложения за IIS с модулем ASP.NET Core. При использовании модуля ASP.NET Core ПО промежуточного слоя перенаправления заголовков настраивается так, чтобы оно запускалось первым в конвейере ПО промежуточного слоя и использовало специальную ограниченную конфигурацию, так как перенаправленные заголовки создают дополнительные проблемы с доверием (например, проблему [IP-спуфинга](https://www.iplocation.net/ip-spoofing)). В ПО промежуточного слоя настраивается пересылка заголовков `X-Forwarded-For` и `X-Forwarded-Proto`, и оно может работать только с одним локальным прокси-сервером. Если вам нужны другие настройки, изучите раздел [Параметры ПО промежуточного слоя перенаправления заголовков](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Другие сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Если не используется ПО промежуточного слоя интеграции IIS, по умолчанию ПО промежуточного слоя перенаправления заголовков не включается. ПО промежуточного слоя перенаправления заголовков нужно включить, чтобы приложение могло обрабатывать перенаправленные заголовки с [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). После включения ПО промежуточного слоя, если не задан параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions), для свойства [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) по умолчанию устанавливается значение [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Настройте ПО промежуточного слоя с помощью [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions), чтобы заголовки `X-Forwarded-For` и `X-Forwarded-Proto` перенаправлялись в `Startup.ConfigureServices`. Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure` прежде, чем вызывать другое ПО промежуточного слоя.

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
> Если параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) на задан в `Startup.ConfigureServices` и не передан напрямую методу расширения через [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), по умолчанию перенаправляются заголовки [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). Свойство [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) должно содержать заголовки для перенаправления.

## <a name="nginx-configuration"></a>Конфигурация Nginx

Сведения о перенаправлении заголовков `X-Forwarded-For` и `X-Forwarded-Proto` см. в разделе [Размещение на Linux с Nginx. Настройка Nginx](xref:host-and-deploy/linux-nginx#configure-nginx). Дополнительные сведения см. в разделе [NGINX. Использование перенаправленного заголовка](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Конфигурация Apache

`X-Forwarded-For` добавляется автоматически (см. раздел [Apache Module mod_proxy: заголовки обратных прокси-запросов](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). Сведения о том, как перенаправить заголовок `X-Forwarded-Proto`, см. в разделе [Размещение на Linux с Apache. Настройка Apache](xref:host-and-deploy/linux-apache#configure-apache).

## <a name="forwarded-headers-middleware-options"></a>Параметры ПО промежуточного слоя перенаправления заголовков

Параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) позволяет управлять поведением ПО промежуточного слоя перенаправления заголовков.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| Параметр | Описание: |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Значение по умолчанию — `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Определяет, какие серверы пересылки будут обрабатываться. В разделе [Перечисление ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) приведен список применимых полей. Обычно для этого свойства задаются такие значения: <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Значение по умолчанию — [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Значение по умолчанию — `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Значение по умолчанию — `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ограничивает количество записей в обрабатываемых заголовках. Установите значение `null`, чтобы отключить это ограничение, но только в том случае, если настроено свойство `KnownProxies` или `KnownNetworks`.<br><br>Значение по умолчанию — 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Диапазоны адресов известных прокси-серверов, от которых можно принимать перенаправленные заголовки. Диапазоны IP-адресов указываются в нотации CIDR.<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> с одной записью для `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Адреса известных прокси-серверов, от которых можно принимать перенаправленные заголовки. Используйте `KnownProxies`, чтобы указать точные IP-адреса.<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> с одной записью для `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Значение по умолчанию — `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Значение по умолчанию — `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Значение по умолчанию — `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Требует синхронизации некоторых значений заголовков во всех обрабатываемых [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders).<br><br>Значение по умолчанию в ASP.NET Core 1.x — `true`. Значение по умолчанию в ASP.NET Core 2.0 или более поздней версии — `false`. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Параметр | Описание: |
| ------ | ----------- |
| AllowedHosts | Ограничивает узлы в заголовке `X-Forwarded-Host` списком указанных значений.<ul><li>Значения сравниваются по порядковым номерам без учета регистра.</li><li>Номера портов указывать не нужно.</li><li>Если список пуст, разрешены все узлы.</li><li>Подстановочный знак верхнего уровня `*` разрешает все непустые значения узлов.</li><li>Подстановочные знаки поддоменов допускаются, но не могут указывать на корневой домен. Например, `*.contoso.com` соответствует поддомену `foo.contoso.com`, но не корневому домену `contoso.com`.</li><li>Имена узлов в Юникоде разрешены, но для сопоставления они преобразуются в [Punycode](https://tools.ietf.org/html/rfc3492).</li><li>[IPv6-адреса](https://tools.ietf.org/html/rfc4291) должны включать ограничивающие квадратные скобки и иметь [стандартный формат](https://tools.ietf.org/html/rfc4291#section-2.2) (например, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6-адреса не приводятся к специальным правилам регистра для логического соответствия разных форматов, и к ним не применяется канонизация.</li><li>Если список разрешенных узлов не будет ограничен, злоумышленник сможет подделать ссылки, создаваемые службой.</li></ul>Значение по умолчанию — пустая строка [IList\<string>](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Значение по умолчанию — `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Определяет, какие серверы пересылки будут обрабатываться. В разделе [Перечисление ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) приведен список применимых полей. Обычно для этого свойства задаются такие значения: <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Значение по умолчанию — [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Значение по умолчанию — `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Значение по умолчанию — `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ограничивает количество записей в обрабатываемых заголовках. Установите значение `null`, чтобы отключить это ограничение, но только в том случае, если настроено свойство `KnownProxies` или `KnownNetworks`.<br><br>Значение по умолчанию — 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Диапазоны адресов известных прокси-серверов, от которых можно принимать перенаправленные заголовки. Диапазоны IP-адресов указываются в нотации CIDR.<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> с одной записью для `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Адреса известных прокси-серверов, от которых можно принимать перенаправленные заголовки. Используйте `KnownProxies`, чтобы указать точные IP-адреса.<br><br>Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> с одной записью для `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Значение по умолчанию — `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Значение по умолчанию — `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Заголовок, заданный этим свойством, используется вместо заголовка, указанного в параметре [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Значение по умолчанию — `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Требует синхронизации некоторых значений заголовков во всех обрабатываемых [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders).<br><br>Значение по умолчанию в ASP.NET Core 1.x — `true`. Значение по умолчанию в ASP.NET Core 2.0 или более поздней версии — `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Сценарии и варианты использования

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Отсутствие возможности добавить перенаправленные заголовки, и все запросы считаются безопасными

В некоторых случаях нет возможности добавлять перенаправленные заголовки в запросы, передаваемые приложению через прокси-сервер. Если прокси-сервер требует схему HTTPS для всех внешних запросов, ее можно задать вручную в `Startup.Configure` перед использованием любого ПО промежуточного слоя:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

В тестовой среде или среде разработки этот код можно отключить с помощью переменной среды или другого параметра конфигурации.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Базовый путь и прокси-серверы, которые изменяют путь запроса

Некоторые прокси-серверы передают путь, но добавляют к нему базовый путь приложения, который нужно удалять для правильной работы маршрутизации. ПО промежуточного слоя [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) разделяет путь на два сегмента: [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) и базовый путь приложения [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Если `/foo` является базовым путем приложения в пути `/foo/api/1`, полученном от прокси-сервера, это ПО промежуточного слоя устанавливает для `Request.PathBase` значение `/foo`, а для `Request.Path` значение `/api/1` с помощью следующей команды:

```csharp
app.UsePathBase("/foo");
```

Исходный путь и базовый путь подставляются в прежнем виде, когда создается обратный запрос к ПО промежуточного слоя. Дополнительные сведения о порядке обработки для ПО промежуточного слоя см. в статье [о ПО промежуточного слоя](xref:fundamentals/middleware/index).

Если прокси-сервер усекает путь (например, перенаправляет `/foo/api/1` в `/api/1`), такие перенаправления и ссылки можно исправить, задав для запроса свойство [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase):

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Если прокси-сервер добавляет к пути дополнительные данные, лишнюю часть можно отсечь с помощью [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) и присвоить правильно значение свойству [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path):

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

Если заголовки перенаправляются не так, как ожидалось, включите [ведение журнала](xref:fundamentals/logging/index). Если журналы не содержат достаточно информации для устранения неполадок, просмотрите список заголовков в запросе, полученном сервером. Заголовки можно записать в ответ, создаваемый приложением, с помощью встроенного ПО промежуточного слоя:

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

Следите за тем, чтобы полученные сервером заголовки X-Forwarded-* содержали ожидаемые значения. Если в некотором заголовке содержится несколько значений, учитывайте, что ПО промежуточного слоя перенаправления заголовков обрабатывает их в обратном порядке: справа налево.

Удаленный IP-адрес исходного запроса должен совпадать с записью в списке `KnownProxies` или `KnownNetworks`, чтобы происходила обработка X-Forwarded-For. Это позволяет избежать некоторых методов спуфинга, отклоняя запросы от ненадежных прокси-серверов пересылки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability](https://github.com/aspnet/Announcements/issues/295) (Рекомендации Майкрософт по безопасности CVE-2018-0787: уязвимость, связанная с повышением привилегий в ASP.NET Core).
