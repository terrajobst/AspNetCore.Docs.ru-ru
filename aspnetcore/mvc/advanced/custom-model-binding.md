---
title: Пользовательская привязка модели в ASP.NET Core
author: ardalis
description: Узнайте, как привязка модели позволяет действиям контроллера работать непосредственно с типами модели в ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: b8745241b0699d270bb8f3a56ab614b0ca49e64b
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045540"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="97468-103">Пользовательская привязка модели в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97468-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="97468-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="97468-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="97468-105">Привязка модели позволяет действиям контроллера работать непосредственно с типами моделей (передаваемыми в качестве аргументов метода), а не с HTTP-запросами.</span><span class="sxs-lookup"><span data-stu-id="97468-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="97468-106">Сопоставление между данными входящего запроса и моделями приложений обрабатывается связывателями моделей.</span><span class="sxs-lookup"><span data-stu-id="97468-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="97468-107">Разработчики могут расширить функциональность привязки встроенных модели путем реализации настраиваемых связывателей (хотя обычно создавать собственный поставщик не требуется).</span><span class="sxs-lookup"><span data-stu-id="97468-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="97468-108">Просмотреть или скачать образец с GitHub</span><span class="sxs-lookup"><span data-stu-id="97468-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="97468-109">Ограничения для связывателя модели по умолчанию</span><span class="sxs-lookup"><span data-stu-id="97468-109">Default model binder limitations</span></span>

<span data-ttu-id="97468-110">Связыватели моделей по умолчанию поддерживают значительную часть распространенных типов данных .NET Core и должны соответствовать большинству требований разработчиков.</span><span class="sxs-lookup"><span data-stu-id="97468-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="97468-111">Ожидается, что они будут привязывать текстовые входные данные из запроса непосредственно к типам моделей.</span><span class="sxs-lookup"><span data-stu-id="97468-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="97468-112">Перед привязкой может потребоваться преобразовать входные данные.</span><span class="sxs-lookup"><span data-stu-id="97468-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="97468-113">Например, у вас есть ключ, используемый для поиска данных модели.</span><span class="sxs-lookup"><span data-stu-id="97468-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="97468-114">Для выборки данных на основе ключа можно воспользоваться настраиваемым связывателем модели.</span><span class="sxs-lookup"><span data-stu-id="97468-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="97468-115">Обзор привязки модели</span><span class="sxs-lookup"><span data-stu-id="97468-115">Model binding review</span></span>

<span data-ttu-id="97468-116">Для типов, с которыми работает привязка данных, используются специальные определения.</span><span class="sxs-lookup"><span data-stu-id="97468-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="97468-117">*Простой тип* преобразуется из одной строки во входных данных.</span><span class="sxs-lookup"><span data-stu-id="97468-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="97468-118">*Сложный тип* преобразуется из нескольких входных значений.</span><span class="sxs-lookup"><span data-stu-id="97468-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="97468-119">Платформа определяет их различие в зависимости от наличия `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="97468-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="97468-120">Если у вас есть простое сопоставление `string` -> `SomeType`, не требующее внешних ресурсов, рекомендуется создать преобразователь типов.</span><span class="sxs-lookup"><span data-stu-id="97468-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="97468-121">Прежде чем создавать собственный настраиваемый связыватель модели, следует понять реализацию существующих связывателей моделей.</span><span class="sxs-lookup"><span data-stu-id="97468-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="97468-122">Рассмотрим класс [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), который используется для преобразования строк в кодировке Base64 в массивы байтов.</span><span class="sxs-lookup"><span data-stu-id="97468-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="97468-123">Массивы байтов часто хранятся в виде файлов или полей больших двоичных объектов базы данных.</span><span class="sxs-lookup"><span data-stu-id="97468-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="97468-124">Работа с классом ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="97468-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="97468-125">Строки в кодировке Base64 можно использовать для представления двоичных данных.</span><span class="sxs-lookup"><span data-stu-id="97468-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="97468-126">Например, приведенное ниже изображение можно закодировать в виде строки.</span><span class="sxs-lookup"><span data-stu-id="97468-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="97468-127">![бот dotnet](custom-model-binding/images/bot.png "бот dotnet")</span><span class="sxs-lookup"><span data-stu-id="97468-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="97468-128">На следующем рисунке показана небольшая часть закодированной строки:</span><span class="sxs-lookup"><span data-stu-id="97468-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="97468-129">![закодированный бот dotnet](custom-model-binding/images/encoded-bot.png "закодированный бот dotnet")</span><span class="sxs-lookup"><span data-stu-id="97468-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="97468-130">Чтобы преобразовать строку в кодировке Base64 в файл, следуйте инструкциям в [файле README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md).</span><span class="sxs-lookup"><span data-stu-id="97468-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="97468-131">ASP.NET Core MVC может принимать строки в кодировке Base64 и использовать `ByteArrayModelBinder` для их преобразования в массив байтов.</span><span class="sxs-lookup"><span data-stu-id="97468-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="97468-132">Класс [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider), реализующий интерфейс [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider), сопоставляет аргументы `byte[]` с `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="97468-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="97468-133">При создании собственного настраиваемого связывателя модели можно реализовать собственный тип `IModelBinderProvider` или воспользоваться [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="97468-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="97468-134">В следующем примере показано, как использовать `ByteArrayModelBinder` для преобразования строки в кодировке Base64 в `byte[]` и сохранить результат в файл:</span><span class="sxs-lookup"><span data-stu-id="97468-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="97468-135">Строку в кодировке Base64 можно отправить (POST) в этот метод API с помощью такого средства, как [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="97468-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="97468-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="97468-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="97468-137">Привязка модели успешно выполняется до тех пор, пока связыватель может привязывать данные запроса к свойствам или аргументам с соответствующими именами.</span><span class="sxs-lookup"><span data-stu-id="97468-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="97468-138">В приведенном ниже примере показано, как использовать `ByteArrayModelBinder` с моделью представления.</span><span class="sxs-lookup"><span data-stu-id="97468-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="97468-139">Образец настраиваемого связывателя модели</span><span class="sxs-lookup"><span data-stu-id="97468-139">Custom model binder sample</span></span>

<span data-ttu-id="97468-140">В этом разделе будет реализован настраиваемый связыватель модели, который выполняет следующий действия:</span><span class="sxs-lookup"><span data-stu-id="97468-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="97468-141">преобразует данные входящего запроса в строго типизированные аргументы ключа;</span><span class="sxs-lookup"><span data-stu-id="97468-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="97468-142">использует Entity Framework Core для получения связанной сущности;</span><span class="sxs-lookup"><span data-stu-id="97468-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="97468-143">передает связанную сущность в качестве аргумента в метод действия.</span><span class="sxs-lookup"><span data-stu-id="97468-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="97468-144">В следующем примере используется атрибут `ModelBinder` в модели `Author`:</span><span class="sxs-lookup"><span data-stu-id="97468-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="97468-145">В приведенном выше коде атрибут `ModelBinder` указывает тип `IModelBinder`, который следует использовать для привязки параметров действия `Author`.</span><span class="sxs-lookup"><span data-stu-id="97468-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="97468-146">`AuthorEntityBinder` используется для привязки параметра `Author` получения сущности из источника данных с помощью Entity Framework Core и `authorId`:</span><span class="sxs-lookup"><span data-stu-id="97468-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="97468-147">В следующем примере кода демонстрируется использование `AuthorEntityBinder` в методе действия.</span><span class="sxs-lookup"><span data-stu-id="97468-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="97468-148">С помощью атрибута `ModelBinder` можно применять `AuthorEntityBinder` к параметрам, которые не используют соглашения по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="97468-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="97468-149">Поскольку в этом примере имя аргумента не является `authorId` по умолчанию, оно указывается в параметре с помощью атрибута `ModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="97468-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="97468-150">Обратите внимание, что функционал контроллера и метода действия упрощен по сравнению с поиском сущности в методе действия.</span><span class="sxs-lookup"><span data-stu-id="97468-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="97468-151">Логика для выборки автора с использованием Entity Framework Core перемещается в связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="97468-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="97468-152">Такой подход позволит существенно упростить работу, если у вас есть несколько методов для привязки к модели `Author`, и поможет вам следовать принципу ["не повторяйся"](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="97468-152">This can be a considerable simplification when you have several methods that bind to the `Author` model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="97468-153">Чтобы указать определенный связыватель модели или имя только для данного типа или действия, можно применить атрибут `ModelBinder` к свойствам отдельной модели (таким как viewmodel) или к параметрам метода действия.</span><span class="sxs-lookup"><span data-stu-id="97468-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="97468-154">Реализация класса ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="97468-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="97468-155">Вместо применения атрибута можно реализовать класс `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="97468-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="97468-156">Таким образом реализуются встроенные связыватели платформы.</span><span class="sxs-lookup"><span data-stu-id="97468-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="97468-157">При указании типа, с которым работает связыватель, определяется тип создаваемого им аргумента, а **не** принимаемые им входные данные.</span><span class="sxs-lookup"><span data-stu-id="97468-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="97468-158">Следующий поставщик связывателей работает с `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="97468-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="97468-159">Если он добавляется в коллекцию поставщиков MVC, не нужно использовать атрибут `ModelBinder` в типизированных параметрах `Author` или `Author`.</span><span class="sxs-lookup"><span data-stu-id="97468-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="97468-160">Примечание. Приведенный выше код возвращает `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="97468-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="97468-161">`BinderTypeModelBinder` выступает в качестве фабрики для связывателей моделей и обеспечивает внедрение зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="97468-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="97468-162">Для доступа к EF Core классу `AuthorEntityBinder` требуется внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="97468-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="97468-163">Если связывателю модели необходимы службы из DI, используйте класс `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="97468-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="97468-164">Чтобы начать работу с настраиваемым поставщиком связывателей моделей, добавьте его в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="97468-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="97468-165">При оценке связывателей моделей коллекция поставщиков проверяется в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="97468-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="97468-166">Используется первый поставщик, который возвращает связыватель.</span><span class="sxs-lookup"><span data-stu-id="97468-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="97468-167">На следующем рисунке показаны заданные по умолчанию связыватели моделей из отладчика.</span><span class="sxs-lookup"><span data-stu-id="97468-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="97468-168">![связыватели моделей по умолчанию](custom-model-binding/images/default-model-binders.png "связыватели моделей по умолчанию")</span><span class="sxs-lookup"><span data-stu-id="97468-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="97468-169">Добавление поставщика в конец коллекции может привести к вызову встроенного связывателя модели раньше, чем будет вызван ваш собственный настраиваемый связыватель.</span><span class="sxs-lookup"><span data-stu-id="97468-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="97468-170">В этом примере настраиваемый поставщик добавляется в начало коллекции, чтобы использоваться для аргументов действия `Author`.</span><span class="sxs-lookup"><span data-stu-id="97468-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="97468-171">Рекомендации и советы</span><span class="sxs-lookup"><span data-stu-id="97468-171">Recommendations and best practices</span></span>

<span data-ttu-id="97468-172">Ниже приведены рекомендации относительно настраиваемых связывателей моделей.</span><span class="sxs-lookup"><span data-stu-id="97468-172">Custom model binders:</span></span>
- <span data-ttu-id="97468-173">Связыватели моделей не следует использовать для установки кодов состояния или возвращаемых результатов (например, "404 — не найдено").</span><span class="sxs-lookup"><span data-stu-id="97468-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="97468-174">При сбое привязки модели обрабатывать ошибку должен сам [фильтр действий](xref:mvc/controllers/filters) или логика в самом методе действия.</span><span class="sxs-lookup"><span data-stu-id="97468-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="97468-175">Связыватели наиболее полезны в сценариях исключения повторяющихся частей кода и решения взаимосвязанных проблем с методами действий.</span><span class="sxs-lookup"><span data-stu-id="97468-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="97468-176">Как правило, связыватели не следует использовать для преобразования строки в пользовательский тип. Лучшим вариантом обычно является [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter).</span><span class="sxs-lookup"><span data-stu-id="97468-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
