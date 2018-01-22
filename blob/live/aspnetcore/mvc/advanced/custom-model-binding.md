---
title: "Привязка пользовательских модели"
author: ardalis
description: "Настройка привязки модели в ASP.NET Core MVC."
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: d8b94f53954c5ab63ccf3aab4eb7a7a7dbea487b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="5f456-103">Привязка пользовательских модели</span><span class="sxs-lookup"><span data-stu-id="5f456-103">Custom Model Binding</span></span>

<span data-ttu-id="5f456-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5f456-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5f456-105">Привязка модели позволяет действия контроллера для работы непосредственно с типами моделей (переданное в качестве аргументов метода), а чем HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="5f456-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="5f456-106">Сопоставление между моделями данных и приложений для входящего запроса обрабатывается связыватели моделей.</span><span class="sxs-lookup"><span data-stu-id="5f456-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="5f456-107">Разработчики могут расширить функциональность привязки встроенных модели путем реализации пользовательских связыватели (хотя обычно не требуется указать собственный поставщик).</span><span class="sxs-lookup"><span data-stu-id="5f456-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="5f456-108">Просмотреть или загрузить пример из GitHub</span><span class="sxs-lookup"><span data-stu-id="5f456-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="5f456-109">Ограничения связыватель модели по умолчанию</span><span class="sxs-lookup"><span data-stu-id="5f456-109">Default model binder limitations</span></span>

<span data-ttu-id="5f456-110">Связыватели моделей по умолчанию поддерживают большинство распространенных типов данных .NET Core и должны соответствовать требованиям большинства разработчиков.</span><span class="sxs-lookup"><span data-stu-id="5f456-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="5f456-111">Ожидают, что для привязки входных данных на основе текста из запроса непосредственно с типами моделей.</span><span class="sxs-lookup"><span data-stu-id="5f456-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="5f456-112">Может потребоваться преобразование входных данных до его привязки.</span><span class="sxs-lookup"><span data-stu-id="5f456-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="5f456-113">Например, при наличии ключа, который может использоваться для поиска данных модели.</span><span class="sxs-lookup"><span data-stu-id="5f456-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="5f456-114">Можно использовать настраиваемый связыватель модели для выборки данных, основанный на ключе.</span><span class="sxs-lookup"><span data-stu-id="5f456-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="5f456-115">Проверка привязки модели</span><span class="sxs-lookup"><span data-stu-id="5f456-115">Model binding review</span></span>

<span data-ttu-id="5f456-116">Привязка модели использует конкретные определения для типов, которые он работает.</span><span class="sxs-lookup"><span data-stu-id="5f456-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="5f456-117">Объект *простой тип* преобразуется из одной строки во входных данных.</span><span class="sxs-lookup"><span data-stu-id="5f456-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="5f456-118">Объект *сложный тип* преобразуется из нескольких входных значений.</span><span class="sxs-lookup"><span data-stu-id="5f456-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="5f456-119">Платформа определяет разницу в зависимости от наличия `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="5f456-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="5f456-120">Рекомендуется создать преобразователь типов, если у вас есть простой `string`  ->  `SomeType` сопоставления, не требующий внешние ресурсы.</span><span class="sxs-lookup"><span data-stu-id="5f456-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="5f456-121">Прежде чем создавать собственный настраиваемый связыватель модели, это стоит рецензирования как существующие модели реализуются связывателей.</span><span class="sxs-lookup"><span data-stu-id="5f456-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="5f456-122">Рассмотрим [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) которого может использоваться для преобразования строки в кодировке base64 в массивы байтов.</span><span class="sxs-lookup"><span data-stu-id="5f456-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="5f456-123">Массивы байтов часто хранятся в виде файлов или полям больших двоичных ОБЪЕКТОВ базы данных.</span><span class="sxs-lookup"><span data-stu-id="5f456-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="5f456-124">Работа с ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="5f456-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="5f456-125">Строки в кодировке Base64 можно использовать для представления двоичных данных.</span><span class="sxs-lookup"><span data-stu-id="5f456-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="5f456-126">Например ниже кодируются как строка.</span><span class="sxs-lookup"><span data-stu-id="5f456-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="5f456-127">![Программа-робот DotNet](custom-model-binding/images/bot.png "бот dotnet")</span><span class="sxs-lookup"><span data-stu-id="5f456-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="5f456-128">На следующем рисунке показан малую часть закодированную строку:</span><span class="sxs-lookup"><span data-stu-id="5f456-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="5f456-129">![Программа-робот DotNet кодировке](custom-model-binding/images/encoded-bot.png "бот dotnet кодировке")</span><span class="sxs-lookup"><span data-stu-id="5f456-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="5f456-130">Следуйте инструкциям в [файл README для образца](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) для преобразования строки в кодировке base64 в файл.</span><span class="sxs-lookup"><span data-stu-id="5f456-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="5f456-131">ASP.NET Core MVC можно взять строки в кодировке base64 и использовать `ByteArrayModelBinder` для преобразования его в массив байтов.</span><span class="sxs-lookup"><span data-stu-id="5f456-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="5f456-132">[ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , реализующий [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) сопоставляет `byte[]` аргументы `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="5f456-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="5f456-133">При создании собственных настраиваемый связыватель модели, можно реализовать собственный `IModelBinderProvider` или воспользуйтесь кнопкой [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="5f456-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="5f456-134">В следующем примере показано, как использовать `ByteArrayModelBinder` для преобразования строки в кодировке base64 `byte[]` и сохранить результаты в файл:</span><span class="sxs-lookup"><span data-stu-id="5f456-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="5f456-135">Строка в кодировке base64 в этот метод api, с помощью таких средств, как можно РАЗМЕСТИТЬ [почтальон](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="5f456-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="5f456-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="5f456-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="5f456-137">До тех пор, пока средство привязки можно привязывать данные запроса к соответствующим именем свойства или аргументы, привязка модели будет выполнена успешно.</span><span class="sxs-lookup"><span data-stu-id="5f456-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="5f456-138">В следующем примере показано, как использовать `ByteArrayModelBinder` с модель представлений:</span><span class="sxs-lookup"><span data-stu-id="5f456-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="5f456-139">Образец binder пользовательскую модель</span><span class="sxs-lookup"><span data-stu-id="5f456-139">Custom model binder sample</span></span>

<span data-ttu-id="5f456-140">В этом разделе будет реализован настраиваемый связыватель модели:</span><span class="sxs-lookup"><span data-stu-id="5f456-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="5f456-141">Преобразует данные поступившего запроса в строго типизированный ключа аргументы.</span><span class="sxs-lookup"><span data-stu-id="5f456-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="5f456-142">Entity Framework Core используется для выборки связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="5f456-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="5f456-143">Передает связанной сущности в качестве аргумента для метода действия.</span><span class="sxs-lookup"><span data-stu-id="5f456-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="5f456-144">В следующем примере используется `ModelBinder` атрибут `Author` модели:</span><span class="sxs-lookup"><span data-stu-id="5f456-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="5f456-145">В приведенном выше коде `ModelBinder` атрибута определяет тип `IModelBinder` , следует использовать для привязки `Author` параметров действий.</span><span class="sxs-lookup"><span data-stu-id="5f456-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="5f456-146">`AuthorEntityBinder` Используется для привязки `Author` параметра путем выборки из источника данных с использованием Entity Framework Core сущности и `authorId`:</span><span class="sxs-lookup"><span data-stu-id="5f456-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="5f456-147">Следующий код показывает, как использовать `AuthorEntityBinder` в метод действия:</span><span class="sxs-lookup"><span data-stu-id="5f456-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="5f456-148">`ModelBinder` Атрибут может использоваться для применения `AuthorEntityBinder` к параметрам, которые не используют соглашения по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="5f456-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="5f456-149">В этом примере, так как не указано имя аргумента, значение по умолчанию `authorId`, он указан на параметр с помощью `ModelBinder` атрибута.</span><span class="sxs-lookup"><span data-stu-id="5f456-149">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="5f456-150">Обратите внимание, упрощается метод контроллера и действия по сравнению с поиска сущности в методе действия.</span><span class="sxs-lookup"><span data-stu-id="5f456-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="5f456-151">Логика для выборки с использованием Entity Framework Core автор перемещается связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="5f456-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="5f456-152">Это может быть значительное упрощение, если у вас есть несколько методов, которые можно привязать к модели автора и помогут вам выполнить [ТОНЕРА принцип](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="5f456-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="5f456-153">Можно применить `ModelBinder` атрибут свойства отдельной модели (такие как на viewmodel) или с параметрами метода действия для указания связывателя модели или модели имя для только что такой тип или действие.</span><span class="sxs-lookup"><span data-stu-id="5f456-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="5f456-154">Реализация ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="5f456-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="5f456-155">Вместо применения атрибута, можно реализовать `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="5f456-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="5f456-156">Это реализация связыватели встроенная функция.</span><span class="sxs-lookup"><span data-stu-id="5f456-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="5f456-157">При указании типа работает на связыватель, указать тип аргумента, он создает **не** принимает входные данные вашей связывателя.</span><span class="sxs-lookup"><span data-stu-id="5f456-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="5f456-158">Следующие поставщики связывателей работает с `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="5f456-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="5f456-159">При добавлении MVC в коллекцию поставщиков, нет необходимости использовать `ModelBinder` атрибут `Author` или `Author` типизацией параметров.</span><span class="sxs-lookup"><span data-stu-id="5f456-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="5f456-160">Примечание: Приведенный выше код возвращает `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="5f456-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="5f456-161">`BinderTypeModelBinder`выступает в качестве фабрики для связыватели моделей и предоставляет внедрения зависимости (DI).</span><span class="sxs-lookup"><span data-stu-id="5f456-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="5f456-162">`AuthorEntityBinder` Требует DI для доступа к EF Core.</span><span class="sxs-lookup"><span data-stu-id="5f456-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="5f456-163">Используйте `BinderTypeModelBinder` Если ваш связыватель модели требует службы из DI.</span><span class="sxs-lookup"><span data-stu-id="5f456-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="5f456-164">Чтобы использовать поставщик связывателей модели пользовательские, добавьте его в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5f456-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="5f456-165">При оценке связыватели моделей, в коллекцию поставщиков проверяются в порядке.</span><span class="sxs-lookup"><span data-stu-id="5f456-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="5f456-166">Используется первый поставщик, который возвращает связыватель.</span><span class="sxs-lookup"><span data-stu-id="5f456-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="5f456-167">На следующем рисунке по умолчанию связыватели моделей из отладчика.</span><span class="sxs-lookup"><span data-stu-id="5f456-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="5f456-168">![по умолчанию связыватели моделей](custom-model-binding/images/default-model-binders.png "связыватели моделей по умолчанию")</span><span class="sxs-lookup"><span data-stu-id="5f456-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="5f456-169">Добавление поставщика в конец коллекции может привести к встроенной связыватель вызывается до того, как ваш собственный связыватель вероятность.</span><span class="sxs-lookup"><span data-stu-id="5f456-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="5f456-170">В этом примере пользовательский поставщик добавляется в начало коллекции, чтобы он используется для `Author` аргументов действия.</span><span class="sxs-lookup"><span data-stu-id="5f456-170">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="5f456-171">Рекомендации и советы</span><span class="sxs-lookup"><span data-stu-id="5f456-171">Recommendations and best practices</span></span>

<span data-ttu-id="5f456-172">Пользовательские связыватели:</span><span class="sxs-lookup"><span data-stu-id="5f456-172">Custom model binders:</span></span>
- <span data-ttu-id="5f456-173">Не пытайтесь задать коды состояния или возвращать результаты (например, 404 не найдено).</span><span class="sxs-lookup"><span data-stu-id="5f456-173">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="5f456-174">При сбое привязки модели, [фильтр действий](xref:mvc/controllers/filters) или логику в сам метод должен обрабатывать ошибку.</span><span class="sxs-lookup"><span data-stu-id="5f456-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="5f456-175">Наиболее полезны для исключения повторяющихся частей кода и перекрестные вопросов от методов действий.</span><span class="sxs-lookup"><span data-stu-id="5f456-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="5f456-176">Обычно не используются для преобразования строки в пользовательский тип [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) обычно является лучшим вариантом.</span><span class="sxs-lookup"><span data-stu-id="5f456-176">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
