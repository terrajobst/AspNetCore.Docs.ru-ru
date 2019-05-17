---
title: Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.All не рекомендуется использовать для ASP.NET Core 2.1 и более поздних версий.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 5d49213e6d694f121d8301c94ba71782b2dc45cf
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086938"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0

> [!NOTE]
> Для приложений, предназначенных для ASP.NET Core 2.1 и более поздних версий, вместо этого пакета рекомендуется использовать метапакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). См. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](#migrate) в этой статье.

Для этой функции нужен ASP.NET Core 2.x, нацеленный на .NET Core 2.x.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) является метапакетом, который ссылается на общую платформу. *Общая платформа* — это набор сборок (*DLL*-файлов), которые не находятся в папках приложения. Чтобы запустить приложение, необходимо установить на компьютере общую платформу. Дополнительную информацию см. в этой публикации об [общей платформе](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

Общая платформа, на которую ссылается `Microsoft.AspNetCore.All`, включает в себя следующее:

* все пакеты, поддерживаемые командой ASP.NET Core;
* все пакеты, поддерживаемые Entity Framework Core;
* внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.

В пакет `Microsoft.AspNetCore.All` входят все компоненты ASP.NET Core 2.x и Entity Framework Core 2.x. Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.0.

Номер версии метапакета `Microsoft.AspNetCore.All` соответствует минимальной версии ASP.NET Core и версии Entity Framework Core.

Следующий файл *CSPROJ* ссылается на метапакет `Microsoft.AspNetCore.All` для ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Неявное указание версий

В ASP.NET Core 2.1 или более поздних версиях можно указывать ссылку на пакет `Microsoft.AspNetCore.All` без версии. Если версия не указана, она задается неявно пакетом SDK (`Microsoft.NET.Sdk.Web`). Рекомендуется использовать неявное указание версии через пакет SDK, а не задавать номер версии явно в ссылке на пакет. Если у вас возникли вопросы по этому подходу, оставьте комментарий на GitHub в [обсуждении неявного указания версий Microsoft.AspNetCore.App](https://github.com/aspnet/AspNetCore.Docs/issues/6430).

Для переносимых приложений при неявном указании версии устанавливается значение `major.minor.0`. Механизм выбора последней общей платформы запускает приложение на последней совместимой версии среди установленных общих платформ. Чтобы гарантировать, что используется одна и та же версия при разработке, тестировании и эксплуатации, убедитесь, что установлена одинаковая версия общей платформы во всех средах. Для автономных приложений неявный номер версии общей платформы, включенной в установленный пакет SDK, устанавливается в значение `major.minor.patch`.

Указание номера версии в ссылке на пакет `Microsoft.AspNetCore.All` **не** гарантирует, что выбирается эта версия общей платформы. Например, пусть указана версия "2.1.1", но установлена версия "2.1.3". В этом случае приложение будет использовать версию "2.1.3". Хотя это не рекомендуется, можно отключить функцию выбора последней версии (для исправлений и (или) вспомогательных версий). Дополнительную информацию см. в статье о [выборе последней версии на узле .NET](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Чтобы использовать неявную версию `Microsoft.AspNetCore.All`, для пакета SDK проекта следует указать `Microsoft.NET.Sdk.Web` в файле проекта. Если указан пакет SDK `Microsoft.NET.Sdk` (`<Project Sdk="Microsoft.NET.Sdk">` в верхней части файла проекта), выводится следующее предупреждение:

*Предупреждение NU1604. Зависимость проекта Microsoft.AspNetCore.All не содержит включенную нижнюю границу. Включите нижнюю границу в версию зависимости, чтобы гарантировать согласованные результаты восстановления.*

Это известная проблема с пакетом SDK для .NET Core 2.1. Она будет устранена в пакете SDK для .NET Core 2.2.

::: moniker-end

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

Любые зависимости пакетов выше, которые не являются зависимостями пакета `Microsoft.AspNetCore.App`, не добавляются автоматически. Например:

* `StackExchange.Redis` как зависимость `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` как зависимость `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>Обновление до версии ASP.NET Core 2.1

Мы рекомендуем перейти к использованию метапакета `Microsoft.AspNetCore.App` для 2.1 и более поздних версий. Чтобы продолжить использование метапакета `Microsoft.AspNetCore.All` и обеспечить развертывание версии с последними исправлениями, сделайте следующее:

* На компьютерах разработки и серверах сборки выполните следующее: Установите [пакет SDK для .NET Core](https://www.microsoft.com/net/download) последней версии.
* На серверах развертывания выполните следующее: Установите [среду выполнения .NET Core](https://www.microsoft.com/net/download) последней версии.
 Ваше приложение обновится до последней установленной версии при перезапуске.
