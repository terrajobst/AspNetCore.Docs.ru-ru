---
title: "Привязка пользовательских модели"
author: ardalis
description: "Настройка привязки модели в ASP.NET Core MVC."
keywords: "ASP.NET Core, привязки модели, настраиваемый связыватель модели"
ms.author: riande
manager: wpickett
ms.date: 4/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 2b95073bc0972908d0c0b2158a036e4374c7df4d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="custom-model-binding"></a>Привязка пользовательских модели

По [Стив Смит](https://ardalis.com/)

Привязка модели позволяет действия контроллера для работы непосредственно с типами моделей (переданное в качестве аргументов метода), а чем HTTP-запросов. Сопоставление между моделями данных и приложений для входящего запроса обрабатывается связыватели моделей. Разработчики могут расширить функциональность привязки встроенных модели путем реализации пользовательских связыватели (хотя обычно не требуется указать собственный поставщик).

[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Ограничения связыватель модели по умолчанию

Связыватели моделей по умолчанию поддерживают большинство распространенных типов данных .NET Core и должны соответствовать требованиям большинства разработчиков. Ожидают, что для привязки входных данных на основе текста из запроса непосредственно с типами моделей. Может потребоваться преобразование входных данных до его привязки. Например, при наличии ключа, который может использоваться для поиска данных модели. Можно использовать настраиваемый связыватель модели для выборки данных, основанный на ключе.

## <a name="model-binding-review"></a>Проверка привязки модели

Привязка модели использует конкретные определения для типов, которые он работает. Объект *простой тип* преобразуется из одной строки во входных данных. Объект *сложный тип* преобразуется из нескольких входных значений. Платформа определяет разницу в зависимости от наличия `TypeConverter`. Рекомендуется создать преобразователь типов, если у вас есть простой `string`  ->  `SomeType` сопоставления, не требующий внешние ресурсы.

Прежде чем создавать собственный настраиваемый связыватель модели, это стоит рецензирования как существующие модели реализуются связывателей. Рассмотрим [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) которого может использоваться для преобразования строки в кодировке base64 в массивы байтов. Массивы байтов часто хранятся в виде файлов или полям больших двоичных ОБЪЕКТОВ базы данных.

### <a name="working-with-the-bytearraymodelbinder"></a>Работа с ByteArrayModelBinder

Строки в кодировке Base64 можно использовать для представления двоичных данных. Например ниже кодируются как строка.

![Программа-робот DotNet](custom-model-binding/images/bot.png "бот dotnet")

На следующем рисунке показан малую часть закодированную строку:

![Программа-робот DotNet кодировке](custom-model-binding/images/encoded-bot.png "бот dotnet кодировке")

Следуйте инструкциям в [файл README для образца](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) для преобразования строки в кодировке base64 в файл.

ASP.NET Core MVC можно взять строки в кодировке base64 и использовать `ByteArrayModelBinder` для преобразования его в массив байтов. [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , реализующий [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) сопоставляет `byte[]` аргументы `ByteArrayModelBinder`:

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

При создании собственных настраиваемый связыватель модели, можно реализовать собственный `IModelBinderProvider` или воспользуйтесь кнопкой [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

В следующем примере показано, как использовать `ByteArrayModelBinder` для преобразования строки в кодировке base64 `byte[]` и сохранить результаты в файл:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Строка в кодировке base64 в этот метод api, с помощью таких средств, как можно РАЗМЕСТИТЬ [почтальон](https://www.getpostman.com/):

![почтальон](custom-model-binding/images/postman.png "почтальон")

До тех пор, пока средство привязки можно привязывать данные запроса к соответствующим именем свойства или аргументы, привязка модели будет выполнена успешно. В следующем примере показано, как использовать `ByteArrayModelBinder` с модель представлений:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Образец binder пользовательскую модель

В этом разделе будет реализован настраиваемый связыватель модели:

- Преобразует данные поступившего запроса в строго типизированный ключа аргументы.
- Entity Framework Core используется для выборки связанной сущности.
- Передает связанной сущности в качестве аргумента для метода действия.

В следующем примере используется `ModelBinder` атрибут `Author` модели:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

В приведенном выше коде `ModelBinder` атрибута определяет тип `IModelBinder` , следует использовать для привязки `Author` параметров действий. 

`AuthorEntityBinder` Используется для привязки `Author` параметра путем выборки из источника данных с использованием Entity Framework Core сущности и `authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Следующий код показывает, как использовать `AuthorEntityBinder` в метод действия:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Атрибут может использоваться для применения `AuthorEntityBinder` к параметрам, которые не используют соглашения по умолчанию:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

В этом примере, так как не указано имя аргумента, значение по умолчанию `authorId`, он указан на параметр с помощью `ModelBinder` атрибута. Обратите внимание, упрощается метод контроллера и действия по сравнению с поиска сущности в методе действия. Логика для выборки с использованием Entity Framework Core автор перемещается связывателя модели. Это может быть значительное упрощение, если у вас есть несколько методов, которые можно привязать к модели автора и помогут вам выполнить [ТОНЕРА принцип](http://deviq.com/don-t-repeat-yourself/).

Можно применить `ModelBinder` атрибут свойства отдельной модели (такие как на viewmodel) или с параметрами метода действия для указания связывателя модели или модели имя для только что такой тип или действие.

### <a name="implementing-a-modelbinderprovider"></a>Реализация ModelBinderProvider

Вместо применения атрибута, можно реализовать `IModelBinderProvider`. Это реализация связыватели встроенная функция. При указании типа работает на связыватель, указать тип аргумента, он создает **не** принимает входные данные вашей связывателя. Следующие поставщики связывателей работает с `AuthorEntityBinder`. При добавлении MVC в коллекцию поставщиков, нет необходимости использовать `ModelBinder` атрибут `Author` или `Author` типизацией параметров.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Примечание: Приведенный выше код возвращает `BinderTypeModelBinder`. `BinderTypeModelBinder`выступает в качестве фабрики для связыватели моделей и предоставляет внедрения зависимости (DI). `AuthorEntityBinder` Требует DI для доступа к EF Core. Используйте `BinderTypeModelBinder` Если ваш связыватель модели требует службы из DI.

Чтобы использовать поставщик связывателей модели пользовательские, добавьте его в `ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

При оценке связыватели моделей, в коллекцию поставщиков проверяются в порядке. Используется первый поставщик, который возвращает связыватель.

На следующем рисунке по умолчанию связыватели моделей из отладчика.

![по умолчанию связыватели моделей](custom-model-binding/images/default-model-binders.png "связыватели моделей по умолчанию")

Добавление поставщика в конец коллекции может привести к встроенной связыватель вызывается до того, как ваш собственный связыватель вероятность. В этом примере пользовательский поставщик добавляется в начало коллекции, чтобы он используется для `Author` аргументов действия.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Рекомендации и советы

Пользовательские связыватели:
- Не пытайтесь задать коды состояния или возвращать результаты (например, 404 не найдено). При сбое привязки модели, [фильтр действий](xref:mvc/controllers/filters) или логику в сам метод должен обрабатывать ошибку.
- Наиболее полезны для исключения повторяющихся частей кода и перекрестные вопросов от методов действий.
- Обычно не используются для преобразования строки в пользовательский тип [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) обычно является лучшим вариантом.
