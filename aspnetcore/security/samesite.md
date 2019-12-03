---
title: Работа с файлами cookie SameSite в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать для SameSite файлов cookie в ASP.NET Core
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite
ms.openlocfilehash: 988069a66cc4772583444303948bff2e47ff4310
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733990"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a>Работа с файлами cookie SameSite в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

[SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07) — это черновик [IETF](https://ietf.org/about/) , предназначенный для обеспечения защиты от атак с подделкой межсайтовых запросов (CSRF). [Черновик SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* По умолчанию обрабатывает файлы cookie как `SameSite=Lax`.
* Указывает, что файлы cookie, явно подтверждающие `SameSite=None` для включения межсайтовой доставки, должны быть помечены как `Secure`.

`Lax` работает для большинства файлов cookie приложения. Некоторые формы проверки подлинности, такие как [OpenID Connect Connect](https://openid.net/connect/) (OIDC) и [WS-Federation](https://auth0.com/docs/protocols/ws-fed) , по умолчанию перенаправления на основе POST. Перенаправления на основе POST активируют защиту обозревателя SameSite, поэтому SameSite для этих компонентов отключен. Большинство имен входа [OAuth](https://oauth.net/) не затрагивается из-за различий в способах передачи запросов.

Параметр `None` вызывает проблемы совместимости с клиентами, которые реализовали ранее 2016 черновой версии Standard (например, iOS 12). См. раздел [Поддержка более старых браузеров](#sob) в этом документе.

Каждый компонент ASP.NET Core, который создает файлы cookie, должен решить, подходит ли SameSite.

## <a name="api-usage-with-samesite"></a>Использование API с SameSite

[HttpContext. Response. сookies. append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) по умолчанию `Unspecified`, означающее, что в файл cookie не добавлен атрибут SameSite, и клиент будет использовать его поведение по умолчанию (нестрогий для новых браузеров, ни для старых). В следующем примере кода показано, как изменить значение SameSite файла cookie на `SameSiteMode.Lax`:

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

Все ASP.NET Core компоненты, которые создают файлы cookie, переопределяют предыдущие значения по умолчанию параметрами, подходящими для их сценариев. Переопределенные ранее значения по умолчанию не изменились.

| Компонент | " | Значение по умолчанию |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [Сессионоптионс. cookie](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [Кукиетемпдатапровидероптионс. cookie](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [Антифоржерйоптионс. cookie](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [Проверка подлинности файлов cookie](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [CookieAuthenticationOptions. cookie](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [Твиттероптионс. Статекукие](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [ремотеаусентикатионоптионс. коррелатионкукие](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None` |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [Опенидконнектоптионс. Нонцекукие](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [HttpContext. Response. cookies. append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

ASP.NET Core 3,1 и более поздних версий предоставляет следующую поддержку SameSite:

* Переопределяет поведение `SameSiteMode.None` для выдачи `SameSite=None`
* Добавляет новое значение `SameSiteMode.Unspecified`, чтобы опустить атрибут SameSite.
* Все API-интерфейсы cookie по умолчанию имеют значение `Unspecified`. Некоторые компоненты, использующие файлы cookie, задают значения более специфичными для их сценариев. Примеры см. в таблице выше.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

В ASP.NET Core 3,0 и более поздних версиях значения SameSite по умолчанию были изменены, чтобы избежать конфликтов с несовместимыми по умолчанию для клиентов. Следующие API изменили значение по умолчанию с `SameSiteMode.Lax ` на `-1`, чтобы избежать выпуска атрибута SameSite для этих файлов cookie:

* <xref:Microsoft.AspNetCore.Http.CookieOptions> используется с [HttpContext. Response. cookies. append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)
* <xref:Microsoft.AspNetCore.Http.CookieBuilder>, используемые в качестве фабрики для `CookieOptions`
* [Кукиеполициоптионс. Минимумсамеситеполици](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a>Журнал и изменения

Поддержка SameSite была впервые реализована в ASP.NET Core в 2,0 с использованием [черновой стандартов 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1). Стандарт 2016 был явно принят. ASP.NET Core участие, задав для нескольких файлов cookie `Lax` по умолчанию. После обнаружения нескольких [проблем](https://github.com/aspnet/Announcements/issues/318) с проверкой подлинности большая часть использования SameSite была [отключена](https://github.com/aspnet/Announcements/issues/348).

Исправления были выпущены в ноябре 2019, чтобы обновить стандарт 2016 до стандарта 2019. [2019. черновик спецификации SameSite](https://github.com/aspnet/Announcements/issues/390):

* **Не** имеет обратной совместимости с черновиком 2016. Дополнительные сведения см. в разделе [поддержка старых браузеров](#sob) в этом документе.
* Указывает, что файлы cookie обрабатываются как `SameSite=Lax` по умолчанию.
* Указывает файлы cookie, явно подтверждающие `SameSite=None` для включения межсайтовой доставки, должны быть помечены как `Secure`. `None` — это новая запись для отказаться.
* Поддерживается исправлениями, выпущенными для ASP.NET Core 2,1, 2,2 и 3,0. ASP.NET Core 3,1 имеет дополнительную поддержку SameSite.
* По умолчанию в [фев 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)планируется включить по [Chrome](https://chromestatus.com/feature/5088147346030592) . Браузеры начали переходить к этому стандарту в 2019.

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a>API-интерфейсы, затрагиваемые изменением стандарта 2016 SameSite с черновым стандартом на версию 2019

* [HTTP. Самеситемоде](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [CookieOptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Кукиебуилдер. SameSite](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [Кукиеполициоптионс. Минимумсамеситеполици](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Поддержка старых браузеров

Стандарт 2016 SameSite требует, чтобы неизвестные значения считались `SameSite=Strict` значениями. Приложения, доступ к которым осуществляется из старых браузеров, поддерживающих стандарт 2016 SameSite, могут прерываться при получении свойства SameSite со значением `None`. Веб-приложения должны реализовывать обнаружение браузера, если они будут поддерживать старые браузеры. ASP.NET Core не реализует обнаружение браузеров, так как значения агентов пользователя очень энергонезависимы и часто меняются. Точка расширения в <xref:Microsoft.AspNetCore.CookiePolicy> позволяет подключать определенные логики агента пользователя.

В `Startup.Configure`добавьте код, который вызывает <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> перед вызовом <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> или *любым* методом, записывающим файлы cookie:

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

В `Startup.ConfigureServices`добавьте код, аналогичный приведенному ниже:

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

В предыдущем примере `MyUserAgentDetectionLib.DisallowsSameSiteNone` является библиотекой, предоставляемой пользователем, которая определяет, не поддерживает ли агент пользователя SameSite `None`:

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

В следующем коде показан пример метода `DisallowsSameSiteNone`:

> [!WARNING]
> Следующий код предназначен только для демонстрации:
> * Его не следует считать полным.
> * Он не поддерживается.

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a>Тестирование приложений на наличие проблем SameSite

Приложения, взаимодействующие с удаленными сайтами, например с помощью имени входа от стороннего разработчика, должны:

* Протестируйте взаимодействие в нескольких браузерах.
* Примените [Обнаружение и устранение проблем обозревателя кукиеполици](#sob) , описанные в этом документе.

Протестируйте веб-приложения с помощью версии клиента, которая может явно использовать новое поведение SameSite. У Chrome, Firefox и Chromium ребра есть новые флаги функций выбора, которые можно использовать для тестирования. Когда приложение применяет исправления SameSite, протестируйте его с помощью более старых версий клиента, особенно Safari. Дополнительные сведения см. в разделе [поддержка старых браузеров](#sob) в этом документе.

### <a name="test-with-chrome"></a>Тестирование с помощью Chrome

Chrome 78 + дает неверные результаты, так как это временное устранение рисков. Временная защита "Chrome 78 +" позволяет создавать файлы cookie менее чем за две минуты. Chrome 76 или 77 с включенными соответствующими флагами тестирования предоставляют более точные результаты. Чтобы проверить новое поведение SameSite, переключите `chrome://flags/#same-site-by-default-cookies` в состояние **включено**. В более ранних версиях Chrome (75 и ниже) сообщается о сбое нового параметра `None`. См. раздел [Поддержка более старых браузеров](#sob) в этом документе.

Google не делает доступными более старые версии Chrome. Следуйте инструкциям в [скачивании Chromium](https://www.chromium.org/getting-involved/download-chromium) , чтобы протестировать старые версии Chrome. **Не** скачивать Chrome из ссылок, предоставленных при поиске старых версий Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Тестирование с помощью Safari

Safari 12 строго реализовал ранее созданный черновик и завершается ошибкой, если новое значение `None` находится в файле cookie. `None` исключается с помощью кода обнаружения браузеров, [поддерживающего более старые браузеры](#sob) в этом документе. Протестируйте имена входа в стиле ОС Safari 12, Safari 13 и WebKit с помощью MSAL, ADAL или любой используемой библиотеки. Проблема зависит от базовой версии ОС. Известно, что в OSX Можаве (10,14) и iOS 12 возникают проблемы совместимости с новым поведением SameSite. Обновление операционной системы до OSX Catalina (10,15) или iOS 13 решает проблему. В настоящее время Safari не имеет флага согласия для проверки нового поведения спецификации.

### <a name="test-with-firefox"></a>Тестирование с помощью Firefox

Поддержку Firefox для нового стандарта можно протестировать в версии 68 +, проверив ее на `about:config` странице с флагом компонента `network.cookie.sameSite.laxByDefault`. В предыдущих версиях Firefox не было отчетов о проблемах совместимости.

### <a name="test-with-edge-browser"></a>Тестирование с помощью браузера пограничных устройств

Ребро поддерживает старый стандарт SameSite. Версия 44 не имеет известных проблем совместимости с новым стандартом.

### <a name="test-with-edge-chromium"></a>Тестирование с помощью ребра (Chromium)

Флаги SameSite задаются на странице `edge://flags/#same-site-by-default-cookies`. С пограничным Chromium проблем совместимости не обнаружено.

### <a name="test-with-electron"></a>Тестирование с электронными данными

В версии электронного хранения входят старые версии Chromium. Например, используемая командами версия электронного характера — Chromium 66, которая приводит к более старому поведению. Необходимо выполнить собственное тестирование совместимости с версией электронного продукта, используемого вашим продуктом. См. раздел [Поддержка более старых браузеров](#sob) в следующем разделе.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Блог по Chromium: разработчики: готовы к созданию нового SameSite = None; Параметры защиты файлов cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Описание файлов cookie SameSite](https://web.dev/samesite-cookies-explained/)
