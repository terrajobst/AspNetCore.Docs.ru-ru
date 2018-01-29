---
title: "Реализации веб-сервера в ASP.NET Core"
author: tdykstra
description: "Статья представляет веб-серверы Kestrel и WebListener для ASP.NET Core. Рекомендации по выбору одного из веб-серверов и информация о том, когда следует использовать веб-сервер с обратным прокси-сервером."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 750b81c2c715598dbcfb6671e34016e30c1878d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Реализации веб-сервера в ASP.NET Core

Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)

Приложение ASP.NET Core выполняется вместе с реализацией HTTP-сервера. Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](https://docs.microsoft.com/aspnet/core/fundamentals/request-features), объединенных в `HttpContext`.

ASP.NET Core предоставляет две реализации серверов.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.

* [HTTP.sys](httpsys.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.

* [WebListener](weblistener.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

Kestrel — это веб-сервер, который по умолчанию включается в шаблоны новых проектов ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.

Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*. Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки, как показано на следующей схеме.

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

Основная причина использования обратного прокси-сервера для развертывания пограничного сервера (открытого для Интернет-трафика) — это безопасность. Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак. Сюда входят, например, соответствующее время ожидания, предельные размеры и максимальное количество одновременных подключений.

Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).

---

Использовать IIS, Nginx или Apache без Kestrel или какой-либо [реализации пользовательского сервера](#custom-servers) нельзя. ASP.NET Core рассчитан на выполнение в качестве отдельного процесса, что позволяет ему на разных платформах вести себя одинаково. IIS, Nginx и Apache определяют собственный процесс запуска и среду; чтобы использовать их напрямую, ASP.NET Core необходимо адаптировать к требованиям каждого из них. Применение веб-сервера, такого как Kestrel, позволяет ASP.NET Core контролировать процесс запуска и среду. Вот почему вместо того, чтобы адаптировать ASP.NET Core для IIS, Nginx или Apache, можно настроить их таким образом, чтобы они передавали запросы в Kestrel через прокси. Эта схема позволяет использовать классы `Program.Main` и `Startup` практически без изменений независимо от места развертывания.

### <a name="iis-with-kestrel"></a>IIS с Kestrel

При использовании IIS или IIS Express в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS. В процессе IIS для координации связи с обратным прокси-сервером выполняется специальный модуль IIS.  Это *модуль ASP.NET Core*. Основные функции модуля ASP.NET Core — это запуск приложения ASP.NET Core, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение. Дополнительные сведения см. в статье [Модуль ASP.NET Core](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx с Kestrel

Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache с Kestrel

Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys. HTTP.sys подойдет для сценариев, когда приложение имеет доступ к Интернету и вам требуются функции HTTP.sys, которые Kestrel не поддерживает. 

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети. 

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только HTTP.sys. Сведения о функциях HTTP.sys см. в разделе [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

В ASP.NET Core 1.x HTTP.sys называется WebListener. Если приложение ASP.NET Core запускается в Windows, в случаях, когда приложение должно иметь доступ в Интернет, но использовать IIS нельзя, пригодится WebListener.

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются функции Weblistener, которые Kestrel не поддерживает. 

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только Weblistener. Сведения о функциях WebListener см. в разделе [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Примечания об инфраструктуре сервера ASP.NET Core

[`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), доступный к методе `Configure` класса `Startup`, предоставляет свойство `ServerFeatures` типа [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). И Kestrel, и WebListener предоставляют только один компонент, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), но различные реализации серверов могут предоставлять дополнительные функциональные возможности.

`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.

## <a name="custom-servers"></a>Пользовательские серверы

Если встроенные серверы не отвечают вашим требованиям, можно создать реализацию пользовательского сервера. В [руководстве по открытому веб-интерфейсу .NET (OWIN)](../owin.md) демонстрируется запись реализации [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin). Вы можете реализовать интерфейсы только тех компонентов, которые требуются вашему приложению, но как минимум требуется поддержка [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel с IIS](aspnet-core-module.md)
- [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx)
- [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel с IIS](aspnet-core-module.md)
- [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx)
- [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
