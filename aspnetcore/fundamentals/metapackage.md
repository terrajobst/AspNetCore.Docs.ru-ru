---
title: Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.All включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core, а также их зависимости.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123831"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="00dd8-103">Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="00dd8-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="00dd8-104">Для приложений, предназначенных для ASP.NET Core 2.1 и более поздних версий, вместо этого пакета рекомендуется использовать пакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="00dd8-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="00dd8-105">См. раздел [Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App](#migrate) в этой статье.</span><span class="sxs-lookup"><span data-stu-id="00dd8-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="00dd8-106">Для этой функции нужен ASP.NET Core 2.x, нацеленный на .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="00dd8-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="00dd8-107">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="00dd8-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="00dd8-108">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="00dd8-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="00dd8-109">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="00dd8-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="00dd8-110">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="00dd8-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="00dd8-111">В пакет `Microsoft.AspNetCore.All` входят все компоненты ASP.NET Core 2.x и Entity Framework Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="00dd8-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="00dd8-112">Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="00dd8-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="00dd8-113">Номер версии метапакета `Microsoft.AspNetCore.All` представляет версию ASP.NET Core и версию Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="00dd8-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="00dd8-114">Приложения, использующие метапакет `Microsoft.AspNetCore.All`, автоматически получают все преимущества [хранилища среды выполнения .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="00dd8-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="00dd8-115">Это хранилище содержит все ресурсы среды выполнения, необходимые для запуска приложений ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="00dd8-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="00dd8-116">При использовании метапакета `Microsoft.AspNetCore.All` с приложением не развертываются **никакие** ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core &mdash;, хранилище среды выполнения .NET Core уже содержит эти ресурсы.</span><span class="sxs-lookup"><span data-stu-id="00dd8-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="00dd8-117">Для сокращения времени запуска приложения ресурсы в хранилище среды выполнения подвергаются предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="00dd8-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="00dd8-118">Для удаления неиспользуемых пакетов можно воспользоваться процессом усечения пакета.</span><span class="sxs-lookup"><span data-stu-id="00dd8-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="00dd8-119">Усеченные пакеты исключаются из выходных данных опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="00dd8-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="00dd8-120">Следующий файл *CSPROJ* ссылается на метапакет `Microsoft.AspNetCore.All` для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="00dd8-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="00dd8-121">Переход от Microsoft.AspNetCore.All к Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="00dd8-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="00dd8-122">Следующие пакеты включены в пакет `Microsoft.AspNetCore.All`, но не выключены в пакет `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="00dd8-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

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

<span data-ttu-id="00dd8-123">При переходе от `Microsoft.AspNetCore.All` к `Microsoft.AspNetCore.App`, если приложение использует API из пакетов выше или включенных в них пакетов, необходимо добавить в проект ссылки на эти пакеты.</span><span class="sxs-lookup"><span data-stu-id="00dd8-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="00dd8-124">Любые зависимости пакетов выше, которые не являются зависимостями пакета `Microsoft.AspNetCore.App`, не добавляются автоматически.</span><span class="sxs-lookup"><span data-stu-id="00dd8-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="00dd8-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="00dd8-125">For example:</span></span>

* <span data-ttu-id="00dd8-126">`StackExchange.Redis` как зависимость `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="00dd8-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="00dd8-127">`Microsoft.ApplicationInsights` как зависимость `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="00dd8-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
