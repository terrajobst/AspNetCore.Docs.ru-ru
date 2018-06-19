---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Параметр привязки веб-API ASP.NET | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042434"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Параметр привязки веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Когда веб-API вызывает метод на контроллере, его необходимо задать значения для параметров, этот процесс называется *привязки*. В этой статье описывается, как веб-API привязывает параметры и как можно настроить процесс привязки.

По умолчанию веб-API использует следующие правила для привязки параметров:

- Если параметр имеет тип «простой», веб-API пытается получить значение из URI. Простые типы включают .NET [типы-примитивы](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **двойные**, и так далее), плюс **TimeSpan**, **DateTime**, **Guid**, **десятичное**, и **строка**, *, а также* все тип с преобразователь типов для преобразования из строки. (Дополнительные сведения о преобразователях типов более поздней версии.)
- Для сложных типов веб-API пытается считать значение из тела сообщения, с помощью [форматирования типа мультимедиа](media-formatters.md).

Например ниже приведен типичный метод контроллера веб-API:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Идентификатор* параметр &quot;простой&quot; тип, поэтому веб-API пытается получить значение из URI запроса. *Элемент* параметр — это сложный тип, поэтому веб-API с помощью форматирования типа мультимедиа для чтения значения из текста запроса.

Чтобы получить значение из URI, веб-API просматривает данные маршрута и строке запроса URI. Данные маршрута заполняется в том случае, когда система маршрутизации анализирует URI и сопоставляет его с маршрутом. Дополнительные сведения см. в разделе [Маршрутизация и Выбор действия](../web-api-routing-and-actions/routing-and-action-selection.md).

В оставшейся части этой статьи я покажу, как можно настроить привязки модели. Для сложных типов Однако рекомендуется использовать модули форматирования типа мультимедиа, по возможности. Основной принцип HTTP — что ресурсы отправляются в тексте сообщения, используя согласование содержимого для указания представление ресурса. Модули форматирования типа мультимедиа, разработанные для этой цели.

## <a name="using-fromuri"></a>С помощью [FromUri]

Чтобы заставить веб-API для чтения из URI сложного типа, добавьте **[FromUri]** к параметру. В следующем примере определяется `GeoPoint` типа, а также метод контроллера, который получает `GeoPoint` из URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Клиент можно поместить значения широты и долготы в строке запроса и веб-API будет использовать их для создания `GeoPoint`. Пример:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>С помощью [FromBody]

Чтобы заставить веб-API для чтения простого типа в тексте запроса, добавьте **[FromBody]** к параметру:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

В этом примере веб-API будет использовать модуль форматирования типа мультимедиа для чтения значение *имя* в тексте запроса. Ниже приведен пример запроса клиента.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Если для параметра установлено [FromBody], веб-API использует заголовок Content-Type для выбора модуля форматирования. В этом примере — тип содержимого &quot;приложение/json&quot; и необработанные строку JSON (а не объект JSON), текст запроса.

Не более одного параметра может считывать данные из текста сообщения. Поэтому он не будет работать:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Это правило причина заключается в том, что тело запроса может храниться в потоке без использования буфера, которые могут быть прочитаны только один раз.

## <a name="type-converters"></a>Преобразователи типов

Обрабатывает класс как простой тип (при этом веб-API будет предпринята попытка привязать его из URI) веб-API можно сделать, создав **TypeConverter** и преобразования строк.

В следующем коде показано `GeoPoint` класс, представляющий точку географических плюс **TypeConverter** , преобразование из строки для `GeoPoint` экземпляров. `GeoPoint` Декорированных класс **[TypeConverter]** атрибут для указания преобразователь типов. (В этом примере была вдохновлена Майк стол блога [способ привязки для пользовательских объектов в сигнатурах действия в MVC-WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Теперь веб-API будет рассматривать `GeoPoint` как простой тип, то есть, он попытается привязать `GeoPoint` параметры из URI. Не нужно включать **[FromUri]** для параметра.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Клиент может вызывать метод с URI следующим образом:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Связыватели моделей

Параметр более гибким, чем преобразователь типов для создания настраиваемый связыватель модели. С помощью связывателя модели имеется доступ к HTTP-запроса, описание действия и необработанные значения из данных маршрута.

Чтобы создать связыватель модели, реализовать **IModelBinder** интерфейса. Этот интерфейс определяет единственный метод, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Вот связыватель модели для `GeoPoint` объектов.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Связыватель модели Возвращает необработанные входные значения из *поставщик значений*. Такая архитектура позволяет разделить две разные функции:

- Поставщик значений принимает HTTP-запроса и заполняет словарь пар «ключ значение».
- Связыватель модели использует этот словарь для заполнения модели.

Поставщик значений по умолчанию в веб-API возвращает значения из данных маршрута и строку запроса. Например, если URL-адрес является `http://localhost/api/values/1?location=48,-122`, поставщик значений создает следующие пары ключ значение:

- id = &quot;1&quot;
- расположение = &quot;48,122&quot;

(Я буду предполагать шаблон маршрута по умолчанию, который является &quot;api / {controller} / {id}&quot;.)

Имя параметра для привязки хранится в **ModelBindingContext.ModelName** свойство. Связыватель модели ищет ключ с таким значением в словаре. Если значение существует и может быть преобразован в `GeoPoint`, связывателя модели присваивает значение привязки к **ModelBindingContext.Model** свойство.

Обратите внимание, что связыватель модели не ограничивается преобразования простого типа. В этом примере связывателя модели в первую очередь в таблице известных расположений и в случае неудачи использует преобразования типов.

**Установка связывателя модели**

Существует несколько способов, чтобы задать связыватель модели. Во-первых, можно добавить **[ModelBinder]** к параметру.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Можно также добавить **[ModelBinder]** к типу атрибут. Веб-API будет использоваться связыватель модели указанного все параметры этого типа.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Наконец, можно добавить поставщик связывателей модели для **HttpConfiguration**. Поставщик связывателей модели является просто класс фабрики, который создает связыватель модели. Можно создать путем наследования от поставщика [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) класса. Тем не менее, если ваш связыватель модели обработки одного типа, проще использовать встроенные **SimpleModelBinderProvider**, разработанный для этой цели. В следующем примере кода показано, как это сделать:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

С помощью поставщика привязки модели по-прежнему необходимо добавить **[ModelBinder]** к параметру, нужно сообщить веб-API, следует использовать связыватель модели и не модуль форматирования типа мультимедиа. Но теперь не требуется указать тип связывателя модели в атрибуте.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Поставщики значений

Я упоминал, что привязка модели получает значения от поставщика значений. Чтобы создать поставщик пользовательского значения, реализовать **IValueProvider** интерфейса. Ниже приведен пример, который извлекает значения из куки-файлы в запросе:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Также необходимо создать фабрику поставщика значение путем наследования от **ValueProviderFactory** класса.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Добавить фабрики поставщика значение **HttpConfiguration** следующим образом.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Веб-API состоит из всех поставщиков значений, поэтому при вызове связыватель модели **ValueProvider.GetValue**, связывателя модели получает значение из первой воспроизводимых его поставщик значений.

Можно также задать фабрики поставщика значение параметра на уровне с помощью **ValueProvider** атрибута следующим образом:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Это значение определяет веб-API для использования с указанным значением фабрика поставщика привязки модели и отказаться от использования других поставщиков зарегистрированным значением.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Связыватели моделей являются более общий механизм определенного экземпляра. Если взглянуть на **[ModelBinder]** атрибут, вы увидите, является производным от абстрактного **ParameterBindingAttribute** класса. Этот класс определяет единственный метод, **GetBinding**, который возвращает **HttpParameterBinding** объекта:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** отвечает за привязки в значение параметра. В случае использования **[ModelBinder]**, возвращает атрибут **HttpParameterBinding** реализации, которую использует **IModelBinder** для выполнения фактического привязки. Вы также можете реализовать собственные **HttpParameterBinding**.

Например, предположим, вы хотите получить теги eTag из `if-match` и `if-none-match` заголовков запроса. Мы начнем путем определения класса для представления значения eTag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Также мы определим перечисление, указывающее, нужно ли получить ETag из `if-match` заголовок или `if-none-match` заголовок.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Вот **HttpParameterBinding** , возвращает значение ETag из нужного заголовка и привязывает его к параметру типа ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** метод выполняет привязку. В этом методе добавить значение привязанного параметра **аргумент ActionArgument** словаря в **HttpActionContext**.

> [!NOTE]
> Если ваш **ExecuteBindingAsync** метод считывает тело сообщения запроса, переопределите **WillReadBody** свойство возвращает значение true. Текст запроса может быть небуферизованный поток, можно прочитать только один раз, поэтому веб-API принудительно применяет правила, не более одной привязки можно прочитать тело сообщения.


Чтобы применить пользовательское **HttpParameterBinding**, можно определить атрибут, который является производным от **ParameterBindingAttribute**. Для `ETagParameterBinding`, мы определим два атрибута, один для `if-match` заголовки и один для `if-none-match` заголовки. Оба являются производными от абстрактного базового класса.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Ниже приведен метод контроллера, который использует `[IfNoneMatch]` атрибута.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Помимо **ParameterBindingAttribute**, имеется другой обработчик для добавления настраиваемого **HttpParameterBinding**. На **HttpConfiguration** объекта, **ParameterBindingRules** свойство — это коллекция функций anomymous типа (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Например, можно добавить правило, которое использует любой параметр ETag в метод GET `ETagParameterBinding` с `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Данная функция должна возвращать `null` для параметров, когда привязка не применимо.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Процесс привязки параметров контролируется службой подключаемые **IActionValueBinder**. Реализация по умолчанию **IActionValueBinder** делает следующее:

1. Найдите **ParameterBindingAttribute** в параметре. Сюда входят **[FromBody]**, **[FromUri]**, и **[ModelBinder]**, или настраиваемых атрибутов.
2. В противном случае поиск в **HttpConfiguration.ParameterBindingRules** для функции, которая возвращает ненулевое значение **HttpParameterBinding**.
3. В противном случае используйте правила по умолчанию, описанной выше. 

    - Если тип параметра «простой» или преобразователя типов, Привязывайте из URI. Это эквивалентно помещения **[FromUri]** атрибут параметра.
    - В противном случае попробуйте считать этот параметр из текста сообщения. Это эквивалентно помещения **[FromBody]** для параметра.

Если требуется, можно заменить весь **IActionValueBinder** службы с пользовательской реализацией.

## <a name="additional-resources"></a>Дополнительные ресурсы

[Пример настраиваемого параметра привязки](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Майк стол написал хорошо серию записи в блогах о привязки параметров веб-API:

- [Как веб-API осуществляется привязка параметра](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Привязка параметров стиля MVC для веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Привязка пользовательских объектов в сигнатурах действия в MVC или веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Создание поставщика пользовательских значений в веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Привязка параметров веб-API в механизме](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
