---
title: Пользовательская привязка модели в ASP.NET Core
author: ardalis
description: Узнайте, как привязка модели позволяет действиям контроллера работать непосредственно с типами модели в ASP.NET Core.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 3623a29976a2e2a7b1bdb22d35716b8a3b448958
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891229"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="ad9bf-103">Пользовательская привязка модели в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad9bf-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="ad9bf-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="ad9bf-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ad9bf-105">Привязка модели позволяет действиям контроллера работать непосредственно с типами моделей (передаваемыми в качестве аргументов метода), а не с HTTP-запросами.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="ad9bf-106">Сопоставление между данными входящего запроса и моделями приложений обрабатывается связывателями моделей.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="ad9bf-107">Разработчики могут расширить функциональность привязки встроенных модели путем реализации настраиваемых связывателей (хотя обычно создавать собственный поставщик не требуется).</span><span class="sxs-lookup"><span data-stu-id="ad9bf-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="ad9bf-108">Просмотреть или скачать образец с GitHub</span><span class="sxs-lookup"><span data-stu-id="ad9bf-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="ad9bf-109">Ограничения для связывателя модели по умолчанию</span><span class="sxs-lookup"><span data-stu-id="ad9bf-109">Default model binder limitations</span></span>

<span data-ttu-id="ad9bf-110">Связыватели моделей по умолчанию поддерживают значительную часть распространенных типов данных .NET Core и должны соответствовать большинству требований разработчиков.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="ad9bf-111">Ожидается, что они будут привязывать текстовые входные данные из запроса непосредственно к типам моделей.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="ad9bf-112">Перед привязкой может потребоваться преобразовать входные данные.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="ad9bf-113">Например, у вас есть ключ, используемый для поиска данных модели.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="ad9bf-114">Для выборки данных на основе ключа можно воспользоваться настраиваемым связывателем модели.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="ad9bf-115">Обзор привязки модели</span><span class="sxs-lookup"><span data-stu-id="ad9bf-115">Model binding review</span></span>

<span data-ttu-id="ad9bf-116">Для типов, с которыми работает привязка данных, используются специальные определения.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="ad9bf-117">*Простой тип* преобразуется из одной строки во входных данных.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="ad9bf-118">*Сложный тип* преобразуется из нескольких входных значений.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="ad9bf-119">Платформа определяет их различие в зависимости от наличия `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="ad9bf-120">Если у вас есть простое сопоставление `string` -> `SomeType`, не требующее внешних ресурсов, рекомендуется создать преобразователь типов.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="ad9bf-121">Прежде чем создавать собственный настраиваемый связыватель модели, следует понять реализацию существующих связывателей моделей.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="ad9bf-122">Рассмотрим класс [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), который используется для преобразования строк в кодировке Base64 в массивы байтов.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="ad9bf-123">Массивы байтов часто хранятся в виде файлов или полей больших двоичных объектов базы данных.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="ad9bf-124">Работа с классом ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="ad9bf-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="ad9bf-125">Строки в кодировке Base64 можно использовать для представления двоичных данных.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="ad9bf-126">Например, приведенное ниже изображение можно закодировать в виде строки.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="ad9bf-127">![бот dotnet](custom-model-binding/images/bot.png "бот dotnet")</span><span class="sxs-lookup"><span data-stu-id="ad9bf-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="ad9bf-128">На следующем рисунке показана небольшая часть закодированной строки:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="ad9bf-129">![закодированный бот dotnet](custom-model-binding/images/encoded-bot.png "закодированный бот dotnet")</span><span class="sxs-lookup"><span data-stu-id="ad9bf-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="ad9bf-130">Чтобы преобразовать строку в кодировке Base64 в файл, следуйте инструкциям в [файле README](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md).</span><span class="sxs-lookup"><span data-stu-id="ad9bf-130">Follow the instructions in the [sample's README](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="ad9bf-131">ASP.NET Core MVC может принимать строки в кодировке Base64 и использовать `ByteArrayModelBinder` для их преобразования в массив байтов.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="ad9bf-132">Класс [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider), реализующий интерфейс [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider), сопоставляет аргументы `byte[]` с `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="ad9bf-133">При создании собственного настраиваемого связывателя модели можно реализовать собственный тип `IModelBinderProvider` или воспользоваться [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="ad9bf-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="ad9bf-134">В следующем примере показано, как использовать `ByteArrayModelBinder` для преобразования строки в кодировке Base64 в `byte[]` и сохранить результат в файл:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="ad9bf-135">Строку в кодировке Base64 можно отправить (POST) в этот метод API с помощью такого средства, как [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="ad9bf-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="ad9bf-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="ad9bf-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="ad9bf-137">Привязка модели успешно выполняется до тех пор, пока связыватель может привязывать данные запроса к свойствам или аргументам с соответствующими именами.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="ad9bf-138">В приведенном ниже примере показано, как использовать `ByteArrayModelBinder` с моделью представления.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="ad9bf-139">Образец настраиваемого связывателя модели</span><span class="sxs-lookup"><span data-stu-id="ad9bf-139">Custom model binder sample</span></span>

<span data-ttu-id="ad9bf-140">В этом разделе будет реализован настраиваемый связыватель модели, который выполняет следующий действия:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="ad9bf-141">преобразует данные входящего запроса в строго типизированные аргументы ключа;</span><span class="sxs-lookup"><span data-stu-id="ad9bf-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="ad9bf-142">использует Entity Framework Core для получения связанной сущности;</span><span class="sxs-lookup"><span data-stu-id="ad9bf-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="ad9bf-143">передает связанную сущность в качестве аргумента в метод действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="ad9bf-144">В следующем примере используется атрибут `ModelBinder` в модели `Author`:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="ad9bf-145">В приведенном выше коде атрибут `ModelBinder` указывает тип `IModelBinder`, который следует использовать для привязки параметров действия `Author`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span>

<span data-ttu-id="ad9bf-146">Следующий класс `AuthorEntityBinder` привязывает параметр `Author` путем получения сущности из источника данных с помощью Entity Framework Core и `authorId`:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-146">The following `AuthorEntityBinder` class binds an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> <span data-ttu-id="ad9bf-147">Предыдущий класс `AuthorEntityBinder` предназначен для демонстрации пользовательского связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-147">The preceding `AuthorEntityBinder` class is intended to illustrate a custom model binder.</span></span> <span data-ttu-id="ad9bf-148">Класс не предназначен для демонстрации рекомендаций для использования сценария просмотра.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-148">The class isn't intended to illustrate best practices for a lookup scenario.</span></span> <span data-ttu-id="ad9bf-149">Чтобы выполнить просмотр, привяжите `authorId` и отправьте запрос к базе данных в методе действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-149">For lookup, bind the `authorId` and query the database in an action method.</span></span> <span data-ttu-id="ad9bf-150">Этот подход отделяет ошибки привязки модели от вариантов `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-150">This approach separates model binding failures from `NotFound` cases.</span></span>

<span data-ttu-id="ad9bf-151">В следующем примере кода демонстрируется использование `AuthorEntityBinder` в методе действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-151">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="ad9bf-152">С помощью атрибута `ModelBinder` можно применять `AuthorEntityBinder` к параметрам, которые не используют соглашения по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-152">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="ad9bf-153">Поскольку в этом примере имя аргумента не является `authorId` по умолчанию, оно указывается в параметре с помощью атрибута `ModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-153">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="ad9bf-154">Обратите внимание, что функционал контроллера и метода действия упрощен по сравнению с поиском сущности в методе действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-154">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="ad9bf-155">Логика для выборки автора с использованием Entity Framework Core перемещается в связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-155">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="ad9bf-156">Такой подход позволит существенно упростить работу, если у вас есть несколько методов для привязки к модели `Author`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-156">This can be a considerable simplification when you have several methods that bind to the `Author` model.</span></span>

<span data-ttu-id="ad9bf-157">Чтобы указать определенный связыватель модели или имя только для данного типа или действия, можно применить атрибут `ModelBinder` к свойствам отдельной модели (таким как viewmodel) или к параметрам метода действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-157">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="ad9bf-158">Реализация класса ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="ad9bf-158">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="ad9bf-159">Вместо применения атрибута можно реализовать класс `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-159">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="ad9bf-160">Таким образом реализуются встроенные связыватели платформы.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-160">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="ad9bf-161">При указании типа, с которым работает связыватель, определяется тип создаваемого им аргумента, а **не** принимаемые им входные данные.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-161">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="ad9bf-162">Следующий поставщик связывателей работает с `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-162">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="ad9bf-163">Если он добавляется в коллекцию поставщиков MVC, не нужно использовать атрибут `ModelBinder` в типизированных параметрах `Author` или `Author`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-163">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author`-typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="ad9bf-164">Примечание. Приведенный выше код возвращает `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-164">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="ad9bf-165">`BinderTypeModelBinder` выступает в качестве фабрики для связывателей моделей и обеспечивает внедрение зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="ad9bf-165">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="ad9bf-166">Для доступа к EF Core классу `AuthorEntityBinder` требуется внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-166">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="ad9bf-167">Если связывателю модели необходимы службы из DI, используйте класс `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-167">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="ad9bf-168">Чтобы начать работу с настраиваемым поставщиком связывателей моделей, добавьте его в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ad9bf-168">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="ad9bf-169">При оценке связывателей моделей коллекция поставщиков проверяется в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-169">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="ad9bf-170">Используется первый поставщик, который возвращает связыватель.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-170">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="ad9bf-171">На следующем рисунке показаны заданные по умолчанию связыватели моделей из отладчика.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-171">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="ad9bf-172">![связыватели моделей по умолчанию](custom-model-binding/images/default-model-binders.png "связыватели моделей по умолчанию")</span><span class="sxs-lookup"><span data-stu-id="ad9bf-172">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="ad9bf-173">Добавление поставщика в конец коллекции может привести к вызову встроенного связывателя модели раньше, чем будет вызван ваш собственный настраиваемый связыватель.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-173">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="ad9bf-174">В этом примере настраиваемый поставщик добавляется в начало коллекции, чтобы использоваться для аргументов действия `Author`.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-174">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="ad9bf-175">Рекомендации и советы</span><span class="sxs-lookup"><span data-stu-id="ad9bf-175">Recommendations and best practices</span></span>

<span data-ttu-id="ad9bf-176">Ниже приведены рекомендации относительно настраиваемых связывателей моделей.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-176">Custom model binders:</span></span>

- <span data-ttu-id="ad9bf-177">Связыватели моделей не следует использовать для установки кодов состояния или возвращаемых результатов (например, "404 — не найдено").</span><span class="sxs-lookup"><span data-stu-id="ad9bf-177">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="ad9bf-178">При сбое привязки модели обрабатывать ошибку должен сам [фильтр действий](xref:mvc/controllers/filters) или логика в самом методе действия.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-178">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="ad9bf-179">Связыватели наиболее полезны в сценариях исключения повторяющихся частей кода и решения взаимосвязанных проблем с методами действий.</span><span class="sxs-lookup"><span data-stu-id="ad9bf-179">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="ad9bf-180">Как правило, связыватели не следует использовать для преобразования строки в пользовательский тип. Лучшим вариантом обычно является [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter).</span><span class="sxs-lookup"><span data-stu-id="ad9bf-180">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
