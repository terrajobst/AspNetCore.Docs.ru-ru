---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Часть 2: Создание модели домена | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="20b31-102">Часть 2: Создание модели домена</span><span class="sxs-lookup"><span data-stu-id="20b31-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="20b31-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="20b31-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="20b31-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="20b31-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="20b31-105">Добавление моделей</span><span class="sxs-lookup"><span data-stu-id="20b31-105">Add Models</span></span>

<span data-ttu-id="20b31-106">Существует три способа подхода Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="20b31-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="20b31-107">Сначала база данных: запустите с базой данных и Entity Framework создает код.</span><span class="sxs-lookup"><span data-stu-id="20b31-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="20b31-108">«Сначала модель»: начать работу с моделью visual и Entity Framework создает базы данных и кода.</span><span class="sxs-lookup"><span data-stu-id="20b31-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="20b31-109">Сначала код: Начните с кода и Entity Framework создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="20b31-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="20b31-110">Метод сначала код используются, поэтому начнем с определения наши объекты домена как POCO (plain old CLR объекты).</span><span class="sxs-lookup"><span data-stu-id="20b31-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="20b31-111">С данным подходом сначала код объектов домена не требуется любой дополнительный код для поддержки базы данных слоя, например транзакции или сохраняемость.</span><span class="sxs-lookup"><span data-stu-id="20b31-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="20b31-112">(В частности, они не обязательно должны наследовать от [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) класса.) Заметок к данным по-прежнему можно использовать для управления как Entity Framework создает схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="20b31-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="20b31-113">Поскольку POCO без выполнения все дополнительные свойства, описывающие [состояние базы данных](https://msdn.microsoft.com/library/system.data.entitystate.aspx), они могут легко быть сериализован в JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="20b31-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="20b31-114">Тем не менее, это означает, что следует всегда предоставлять модели Entity Framework непосредственно клиентам, как будет показано далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="20b31-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="20b31-115">Мы создадим POCO следующие:</span><span class="sxs-lookup"><span data-stu-id="20b31-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="20b31-116">Продукт</span><span class="sxs-lookup"><span data-stu-id="20b31-116">Product</span></span>
- <span data-ttu-id="20b31-117">Номер</span><span class="sxs-lookup"><span data-stu-id="20b31-117">Order</span></span>
- <span data-ttu-id="20b31-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="20b31-118">OrderDetail</span></span>

<span data-ttu-id="20b31-119">Для создания каждого класса, щелкните правой кнопкой мыши папку модели в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="20b31-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="20b31-120">В контекстном меню выберите **добавить** , а затем выберите **класса.**</span><span class="sxs-lookup"><span data-stu-id="20b31-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="20b31-121">Добавить `Product` следующая реализация класса:</span><span class="sxs-lookup"><span data-stu-id="20b31-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="20b31-122">По соглашению Entity Framework использует `Id` свойство в качестве первичного ключа и сопоставляет столбец идентификаторов в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="20b31-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="20b31-123">При создании нового `Product` экземпляра, не задать значение для `Id`, так как база данных формирует значение.</span><span class="sxs-lookup"><span data-stu-id="20b31-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="20b31-124">**ScaffoldColumn** атрибут сообщает ASP.NET MVC, чтобы пропустить `Id` свойства при создании формы редактора.</span><span class="sxs-lookup"><span data-stu-id="20b31-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="20b31-125">**Необходимые** атрибут используется для проверки модели.</span><span class="sxs-lookup"><span data-stu-id="20b31-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="20b31-126">Указывает, что `Name` свойство должно быть непустой строкой.</span><span class="sxs-lookup"><span data-stu-id="20b31-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="20b31-127">Добавить `Order` класса:</span><span class="sxs-lookup"><span data-stu-id="20b31-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="20b31-128">Добавить `OrderDetail` класса:</span><span class="sxs-lookup"><span data-stu-id="20b31-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="20b31-129">Отношения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="20b31-129">Foreign Key Relations</span></span>

<span data-ttu-id="20b31-130">Заказ содержит многие подробности заказа и подробности каждого заказа ссылается на единицу продукта.</span><span class="sxs-lookup"><span data-stu-id="20b31-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="20b31-131">Для представления таких отношений `OrderDetail` класс определяет свойства с именем `OrderId` и `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="20b31-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="20b31-132">Платформа Entity Framework будет построена, эти свойства представляют внешние ключи что будет добавить в базу данных ограничения foreign key.</span><span class="sxs-lookup"><span data-stu-id="20b31-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="20b31-133">`Order` И `OrderDetail` классы также включают свойства «навигация», которые содержат ссылки на связанные объекты.</span><span class="sxs-lookup"><span data-stu-id="20b31-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="20b31-134">Если заказ, можно перейти к продуктов в порядке, следующие свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="20b31-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="20b31-135">Теперь скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="20b31-135">Compile the project now.</span></span> <span data-ttu-id="20b31-136">Платформа Entity Framework использует отражение для обнаружения свойств моделей, поэтому для него требуется скомпилированную сборку создать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="20b31-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="20b31-137">Настройка модулей форматирования типа мультимедиа</span><span class="sxs-lookup"><span data-stu-id="20b31-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="20b31-138">Объект [форматирования типа мультимедиа](../../formats-and-model-binding/media-formatters.md) — это объект, который выполняет сериализацию данных, когда веб-API записывает текст ответа HTTP.</span><span class="sxs-lookup"><span data-stu-id="20b31-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="20b31-139">Встроенные модули форматирования поддерживает выходных данных JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="20b31-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="20b31-140">По умолчанию оба этих модулей форматирования сериализации всех объектов по значению.</span><span class="sxs-lookup"><span data-stu-id="20b31-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="20b31-141">Сериализация по значению создает проблему, если граф объектов содержит циклические ссылки.</span><span class="sxs-lookup"><span data-stu-id="20b31-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="20b31-142">Это только в случае с `Order` и `OrderDetail` классы, так как каждый содержит ссылку на другой.</span><span class="sxs-lookup"><span data-stu-id="20b31-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="20b31-143">Модуль форматирования выполните ссылок на записи каждого объекта по значению и перейдите по кругу.</span><span class="sxs-lookup"><span data-stu-id="20b31-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="20b31-144">Таким образом нам нужно изменить поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="20b31-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="20b31-145">В обозревателе решений разверните приложение\_запустите папку и откройте файл с именем WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="20b31-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="20b31-146">Добавьте следующий код в класс `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="20b31-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="20b31-147">Этот код задает модуль форматирования JSON для сохранения ссылок на объекты и полностью удаляет модуль форматирования XML из конвейера.</span><span class="sxs-lookup"><span data-stu-id="20b31-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="20b31-148">(Можно настроить модуль форматирования XML для сохранения ссылок на объект, но требует немного усилий и нам требуется только JSON для данного приложения.</span><span class="sxs-lookup"><span data-stu-id="20b31-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="20b31-149">Дополнительные сведения см. в разделе [обработки циклических ссылок объекта](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="20b31-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="20b31-150">[Назад](using-web-api-with-entity-framework-part-1.md)
[Вперед](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="20b31-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
