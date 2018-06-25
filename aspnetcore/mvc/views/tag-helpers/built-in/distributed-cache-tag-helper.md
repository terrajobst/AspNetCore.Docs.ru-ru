---
title: Вспомогательная функция тега распределенного кэша в ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега кэша
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: d33c22802030eb9bc77baa64b83c9bbd7e902195
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275179"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="f3f46-103">Вспомогательная функция тега распределенного кэша в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3f46-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="f3f46-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="f3f46-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="f3f46-105">Вспомогательная функция тега распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core за счет кэширования его содержимого в источник распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="f3f46-106">Вспомогательная функция тега распределенного кэша наследуется от того же базового класса, что и вспомогательная функция тега кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="f3f46-107">Все атрибуты, связанные со вспомогательной функцией тега кэша, будут также работать во вспомогательной функции тега распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="f3f46-108">Во вспомогательной функции тега распределенного кэша соблюдается **принцип явных зависимостей**, который также известен как **внедрение конструктора**.</span><span class="sxs-lookup"><span data-stu-id="f3f46-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="f3f46-109">В частности, контейнер интерфейса `IDistributedCache` передается в конструктор вспомогательной функции тега распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="f3f46-110">Если в методе `ConfigureServices`, который обычно находится в файле startup.cs, не создана определенная реализация интерфейса `IDistributedCache`, вспомогательная функция тега распределенного кэша будет использовать для хранения кэшированных данных тот же поставщик в памяти, что и базовая вспомогательная функция тега кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="f3f46-111">Атрибуты вспомогательной функции тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f3f46-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="f3f46-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="f3f46-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="f3f46-113">Описание этих атрибутов см. в статье, посвященной вспомогательной функции тега кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="f3f46-114">Вспомогательная функция тега распределенного кэша наследуется от того же класса, что и вспомогательная функция тега кэша, поэтому все эти атрибуты являются для них общими.</span><span class="sxs-lookup"><span data-stu-id="f3f46-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="f3f46-115">name (обязательный)</span><span class="sxs-lookup"><span data-stu-id="f3f46-115">name (required)</span></span>

| <span data-ttu-id="f3f46-116">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="f3f46-116">Attribute Type</span></span>    | <span data-ttu-id="f3f46-117">Пример значения</span><span class="sxs-lookup"><span data-stu-id="f3f46-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="f3f46-118">string</span><span class="sxs-lookup"><span data-stu-id="f3f46-118">string</span></span>    | <span data-ttu-id="f3f46-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="f3f46-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="f3f46-120">Обязательный атрибут `name` служит ключом кэша, хранящегося для каждого экземпляра вспомогательной функции тега распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f3f46-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="f3f46-121">В отличие от базовой вспомогательной функции тега кэша, каждому экземпляру которой ключ присваивается на основе имени страницы Razor и расположения вспомогательной функции тега на странице Razor, ключ вспомогательной функции тега распределенного кэша основан только на атрибуте `name`</span><span class="sxs-lookup"><span data-stu-id="f3f46-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="f3f46-122">Пример использования:</span><span class="sxs-lookup"><span data-stu-id="f3f46-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="f3f46-123">Реализации IDistributedCache для вспомогательной функции тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f3f46-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="f3f46-124">В ASP.NET Core есть две встроенные реализации интерфейса `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="f3f46-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="f3f46-125">Одна из них основана на Sql Server, а другая — на Redis.</span><span class="sxs-lookup"><span data-stu-id="f3f46-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="f3f46-126">Сведения об этих реализациях можно найти по адресу <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="f3f46-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="f3f46-127">Обе реализации предусматривают задание экземпляра `IDistributedCache` в файле *Startup.cs* ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3f46-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="f3f46-128">Атрибуты тегов, связанные с использованием определенной реализации `IDistributedCache`, отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="f3f46-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3f46-129">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f3f46-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
