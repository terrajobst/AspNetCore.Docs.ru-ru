---
title: "Распределенного кэша тег вспомогательный | Документы Microsoft"
author: pkellner
description: "Показано, как работать с вспомогательный тег кэша"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper
ms.openlocfilehash: 2b260624fb2d85ab1a2625511397bcb4a85b6e77
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2017
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="d4771-104">Вспомогательный тег распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="d4771-104">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="d4771-105">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d4771-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="d4771-106">Тег вспомогательный объект распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core, кэшируя его содержимого с источником распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-106">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="d4771-107">Вспомогательный распределенного кэша тег наследует тот же базовый класс как вспомогательный тег кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="d4771-108">Все атрибуты, связанные со вспомогательным методом тег кэша также будет работать в вспомогательный распределенных тег.</span><span class="sxs-lookup"><span data-stu-id="d4771-108">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="d4771-109">Вспомогательный распределенного кэша тег соответствует **явные зависимости принцип** называется **внедрение конструктора**.</span><span class="sxs-lookup"><span data-stu-id="d4771-109">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="d4771-110">В частности `IDistributedCache` интерфейс контейнера передается в конструктор распределенного кэша тег помощника.</span><span class="sxs-lookup"><span data-stu-id="d4771-110">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="d4771-111">Если нет конкретные реализации `IDistributedCache` была создана в `ConfigureServices`, обычно можно найти в файле startup.cs, а затем распределенного кэша тег вспомогательное приложение будет использовать одного поставщика в памяти для хранения кэшированных данных в виде basic вспомогательные тег кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-111">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="d4771-112">Распределенные атрибуты вспомогательного тег кэша</span><span class="sxs-lookup"><span data-stu-id="d4771-112">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="d4771-113">включить истечения срока действия на срок действия истекает после истечения срока действия скользящий различаются по заголовок различаются по запроса различаются по Маршрутизация различаются по cookie изменяться на уровне пользователей различаются с приоритетом</span><span class="sxs-lookup"><span data-stu-id="d4771-113">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="d4771-114">Для определения в разделе вспомогательные тег кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-114">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="d4771-115">Вспомогательный тег распределенного кэша наследует от одного класса в качестве вспомогательной тег кэша, поэтому эти атрибуты являются общими из вспомогательного тег кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-115">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="d4771-116">Имя (обязательно)</span><span class="sxs-lookup"><span data-stu-id="d4771-116">name (required)</span></span>

| <span data-ttu-id="d4771-117">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d4771-117">Attribute Type</span></span>    | <span data-ttu-id="d4771-118">Пример значения</span><span class="sxs-lookup"><span data-stu-id="d4771-118">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d4771-119">string</span><span class="sxs-lookup"><span data-stu-id="d4771-119">string</span></span>    | <span data-ttu-id="d4771-120">«my-distributed-cache-unique-key-101»</span><span class="sxs-lookup"><span data-stu-id="d4771-120">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="d4771-121">Необходимая `name` атрибут используется в качестве ключа, кэш сохраняется для каждого экземпляра вспомогательный тег распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="d4771-121">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="d4771-122">В отличие от основных кэш тег вспомогательного приложения, назначающий ключа к каждому экземпляру вспомогательный тег кэша на основе имени страницы Razor и расположение тега вспомогательного метода на странице razor вспомогательные распределенного кэша тег только сформирует его ключ на атрибут`name`</span><span class="sxs-lookup"><span data-stu-id="d4771-122">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="d4771-123">Пример использования:</span><span class="sxs-lookup"><span data-stu-id="d4771-123">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="d4771-124">Распределенного кэша тег вспомогательные IDistributedCache реализации</span><span class="sxs-lookup"><span data-stu-id="d4771-124">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="d4771-125">Существует два способа реализации `IDistributedCache` встроенной в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4771-125">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="d4771-126">Один основан на **Sql Server** и на основе другого **Redis**.</span><span class="sxs-lookup"><span data-stu-id="d4771-126">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="d4771-127">Подробности этих реализаций можно найти в ресурсе, упоминаемой ниже именованный «работа с распределенного кэша».</span><span class="sxs-lookup"><span data-stu-id="d4771-127">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="d4771-128">Обе реализации включают задание экземпляра `IDistributedCache` в ASP.NET Core **файла startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="d4771-128">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="d4771-129">Нет никаких атрибутов тега, в частности, связанные с использованием любой конкретной реализации `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="d4771-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="d4771-130">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d4771-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
