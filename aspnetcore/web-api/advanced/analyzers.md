---
title: Использование анализаторов веб-API
author: pranavkm
description: Сведения об анализаторах веб-API в Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635404"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="c62a0-103">Использование анализаторов веб-API</span><span class="sxs-lookup"><span data-stu-id="c62a0-103">Use web API analyzers</span></span>

<span data-ttu-id="c62a0-104">В ASP.NET Core 2.2 представлен пакет NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers), содержащий анализаторы для веб-API.</span><span class="sxs-lookup"><span data-stu-id="c62a0-104">ASP.NET Core 2.2 introduces the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="c62a0-105">Анализаторы работают с контроллерами, которые аннотированы <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>. Кроме того, к ним применяются [соглашения об использовании API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="c62a0-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="c62a0-106">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="c62a0-106">Package installation</span></span>

<span data-ttu-id="c62a0-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` можно добавить одним из описанных ниже способов.</span><span class="sxs-lookup"><span data-stu-id="c62a0-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c62a0-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c62a0-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c62a0-109">В окне **Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="c62a0-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c62a0-110">Перейдите в раздел **Представление** > **Другие окна** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="c62a0-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="c62a0-111">Перейдите в каталог, в котором находится файл *ApiConventions.csproj*</span><span class="sxs-lookup"><span data-stu-id="c62a0-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="c62a0-112">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c62a0-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="c62a0-113">В диалоговом окне **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="c62a0-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c62a0-114">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c62a0-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="c62a0-115">В качестве **источника пакета** выберите "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c62a0-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="c62a0-116">В поле поиска введите "Microsoft.AspNetCore.Mvc.Api.Analyzers".</span><span class="sxs-lookup"><span data-stu-id="c62a0-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="c62a0-117">Выберите пакет "Microsoft.AspNetCore.Mvc.Api.Analyzers" на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="c62a0-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c62a0-118">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="c62a0-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c62a0-119">Щелкните правой кнопкой мыши папку *Пакеты* на **панели решения** > **Добавление пакетов…**.</span><span class="sxs-lookup"><span data-stu-id="c62a0-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="c62a0-120">В раскрывающемся списке **Источник** в окне **Добавление пакетов** выберите вариант "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c62a0-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="c62a0-121">В поле поиска введите "Microsoft.AspNetCore.Mvc.Api.Analyzers".</span><span class="sxs-lookup"><span data-stu-id="c62a0-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="c62a0-122">В области результатов выберите пакет "Microsoft.AspNetCore.Mvc.Api.Analyzers", а затем нажмите кнопку **Добавить пакет**.</span><span class="sxs-lookup"><span data-stu-id="c62a0-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c62a0-123">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c62a0-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c62a0-124">Во **встроенном терминале** выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="c62a0-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c62a0-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c62a0-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c62a0-126">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c62a0-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="c62a0-127">Анализаторы для соглашений API</span><span class="sxs-lookup"><span data-stu-id="c62a0-127">Analyzers for API conventions</span></span>

<span data-ttu-id="c62a0-128">Документы OpenAPI содержат коды состояний и типы ответов, которые может возвращать действие.</span><span class="sxs-lookup"><span data-stu-id="c62a0-128">Open API documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="c62a0-129">В ASP.NET Core MVC атрибуты, такие как <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> и <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>, используются для документирования действия.</span><span class="sxs-lookup"><span data-stu-id="c62a0-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="c62a0-130"><xref:tutorials/web-api-help-pages-using-swagger> позволяет более подробно документировать API.</span><span class="sxs-lookup"><span data-stu-id="c62a0-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="c62a0-131">Один из анализаторов в пакете проверяет контроллеры, которые аннотированы <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, и определяет действия, которые не полностью документируют их ответы.</span><span class="sxs-lookup"><span data-stu-id="c62a0-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="c62a0-132">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="c62a0-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="c62a0-133">Предыдущее действие документирует тип возврата "Успех HTTP 200", но не документирует код состояния "Ошибка HTTP 404".</span><span class="sxs-lookup"><span data-stu-id="c62a0-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="c62a0-134">Анализатор сообщает об отсутствующем документировании для кода состояния HTTP 404 в виде предупреждения.</span><span class="sxs-lookup"><span data-stu-id="c62a0-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="c62a0-135">Эту проблему можно устранить.</span><span class="sxs-lookup"><span data-stu-id="c62a0-135">An option to fix the problem is provided.</span></span>
