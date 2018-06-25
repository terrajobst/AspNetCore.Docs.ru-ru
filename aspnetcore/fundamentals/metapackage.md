---
title: Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0 и более поздних версий
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.All включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core, а также их зависимости.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: 2fddc59d74dce4b114b5b4ed0646f773eb66ffb9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277827"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0

> [!NOTE]
> Для приложений, предназначенных для ASP.NET Core 2.1 и более поздних версий, вместо этого пакета рекомендуется использовать пакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). См. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](#migrate) в этой статье.

Для этой функции нужен ASP.NET Core 2.x, нацеленный на .NET Core 2.x.

Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:

* все пакеты, поддерживаемые командой ASP.NET Core;
* все пакеты, поддерживаемые Entity Framework Core; 
* внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core. 

В пакет `Microsoft.AspNetCore.All` входят все компоненты ASP.NET Core 2.x и Entity Framework Core 2.x. Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.0.

Номер версии метапакета `Microsoft.AspNetCore.All` представляет версию ASP.NET Core и версию Entity Framework Core.

Приложения, использующие метапакет `Microsoft.AspNetCore.All`, автоматически получают все преимущества [хранилища среды выполнения .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Это хранилище содержит все ресурсы среды выполнения, необходимые для запуска приложений ASP.NET Core 2.x. При использовании метапакета `Microsoft.AspNetCore.All` с приложением не развертываются **никакие** ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core &mdash;, хранилище среды выполнения .NET Core уже содержит эти ресурсы. Для сокращения времени запуска приложения ресурсы в хранилище среды выполнения подвергаются предварительной компиляции.

Для удаления неиспользуемых пакетов можно воспользоваться процессом усечения пакета. Усеченные пакеты исключаются из выходных данных опубликованного приложения.

Следующий файл *CSPROJ* ссылается на метапакет `Microsoft.AspNetCore.All` для ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App

Следующие пакеты включены в пакет `Microsoft.AspNetCore.All`, но не выключены в пакет `Microsoft.AspNetCore.App`. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

При переходе от `Microsoft.AspNetCore.All` к `Microsoft.AspNetCore.App`, если приложение использует API из пакетов выше или включенных в них пакетов, необходимо добавить в проект ссылки на эти пакеты.

Любые зависимости пакетов выше, которые не являются зависимостями пакета `Microsoft.AspNetCore.App`, не добавляются автоматически. Пример:

* `StackExchange.Redis` как зависимость `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` как зависимость `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
