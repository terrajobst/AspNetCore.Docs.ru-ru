---
title: "Распределенного кэша тег вспомогательный | Документы Microsoft"
author: pkellner
description: "Показано, как работать с вспомогательный тег кэша"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="13ea0-103">Вспомогательный тег распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="13ea0-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="13ea0-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="13ea0-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="13ea0-105">Тег вспомогательный объект распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core, кэшируя его содержимого с источником распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="13ea0-106">Вспомогательный распределенного кэша тег наследует тот же базовый класс как вспомогательный тег кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="13ea0-107">Все атрибуты, связанные со вспомогательным методом тег кэша также будет работать в вспомогательный распределенных тег.</span><span class="sxs-lookup"><span data-stu-id="13ea0-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="13ea0-108">Вспомогательный распределенного кэша тег соответствует **явные зависимости принцип** называется **внедрение конструктора**.</span><span class="sxs-lookup"><span data-stu-id="13ea0-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="13ea0-109">В частности `IDistributedCache` интерфейс контейнера передается в конструктор распределенного кэша тег помощника.</span><span class="sxs-lookup"><span data-stu-id="13ea0-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="13ea0-110">Если нет конкретные реализации `IDistributedCache` была создана в `ConfigureServices`, обычно можно найти в файле startup.cs, а затем распределенного кэша тег вспомогательное приложение будет использовать одного поставщика в памяти для хранения кэшированных данных в виде basic вспомогательные тег кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="13ea0-111">Распределенные атрибуты вспомогательного тег кэша</span><span class="sxs-lookup"><span data-stu-id="13ea0-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="13ea0-112">включить истечения срока действия на срок действия истекает после истечения срока действия скользящий различаются по заголовок различаются по запроса различаются по Маршрутизация различаются по cookie изменяться на уровне пользователей различаются с приоритетом</span><span class="sxs-lookup"><span data-stu-id="13ea0-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="13ea0-113">Для определения в разделе вспомогательные тег кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="13ea0-114">Вспомогательный тег распределенного кэша наследует от одного класса в качестве вспомогательной тег кэша, поэтому эти атрибуты являются общими из вспомогательного тег кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="13ea0-115">Имя (обязательно)</span><span class="sxs-lookup"><span data-stu-id="13ea0-115">name (required)</span></span>

| <span data-ttu-id="13ea0-116">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="13ea0-116">Attribute Type</span></span>    | <span data-ttu-id="13ea0-117">Пример значения</span><span class="sxs-lookup"><span data-stu-id="13ea0-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="13ea0-118">string</span><span class="sxs-lookup"><span data-stu-id="13ea0-118">string</span></span>    | <span data-ttu-id="13ea0-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="13ea0-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="13ea0-120">Необходимая `name` атрибут используется в качестве ключа, кэш сохраняется для каждого экземпляра вспомогательный тег распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="13ea0-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="13ea0-121">В отличие от основных кэш тег вспомогательного приложения, назначающий ключа к каждому экземпляру вспомогательный тег кэша на основе имени страницы Razor и расположение тега вспомогательного метода на странице razor вспомогательные распределенного кэша тег только сформирует его ключ на атрибут`name`</span><span class="sxs-lookup"><span data-stu-id="13ea0-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="13ea0-122">Пример использования:</span><span class="sxs-lookup"><span data-stu-id="13ea0-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="13ea0-123">Распределенного кэша тег вспомогательные IDistributedCache реализации</span><span class="sxs-lookup"><span data-stu-id="13ea0-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="13ea0-124">Существует два способа реализации `IDistributedCache` встроенной в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13ea0-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="13ea0-125">Один основан на **Sql Server** и на основе другого **Redis**.</span><span class="sxs-lookup"><span data-stu-id="13ea0-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="13ea0-126">Подробности этих реализаций можно найти в ресурсе, упоминаемой ниже именованный «работа с распределенного кэша».</span><span class="sxs-lookup"><span data-stu-id="13ea0-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="13ea0-127">Обе реализации включают задание экземпляра `IDistributedCache` в ASP.NET Core **файла startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="13ea0-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="13ea0-128">Нет никаких атрибутов тега, в частности, связанные с использованием любой конкретной реализации `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="13ea0-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="13ea0-129">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="13ea0-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
