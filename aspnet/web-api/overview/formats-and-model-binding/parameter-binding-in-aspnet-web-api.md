---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Привязка параметров в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377813"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Привязка параметров в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Когда веб-API вызывает метод на контроллере, его необходимо задать значения для параметров, этот процесс называется *привязки*. В этой статье описывается, как веб-API привязывает параметры и настройки процесса привязки.

По умолчанию веб-API использует следующие правила для привязки параметров:

- Если параметр имеет тип «простой», веб-API пытается получить значение из URI. Простые типы включают в себя .NET [типов-примитивов](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, и т. д), а также **TimeSpan**, **DateTime**, **Guid**, **десятичное**, и **строка**, *, а также* любой тип с преобразователь типов для преобразования из строки. (Дополнительные сведения о преобразователях типов более поздней версии.)
- Для сложных типов, веб-API пытается считать значение из текста сообщения, с помощью [форматирования типа мультимедиа](media-formatters.md).

Например ниже приведен типичный метод контроллера веб-API.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Идентификатор* параметр &quot;простой&quot; введите, после чего пытается получить значение из URI запроса веб-API. *Элемент* параметр — сложный тип, поэтому веб-API использует модуль форматирования типа мультимедиа для чтения значения из текста запроса.

Чтобы получить значение из URI, веб-API выглядит в качестве данных маршрута и строке запроса URI. Данные маршрута заполняется в том случае, если система маршрутизации выполняет синтаксический анализ URI и сопоставляет его с маршрутом. Дополнительные сведения см. в разделе [Маршрутизация и Выбор действия](../web-api-routing-and-actions/routing-and-action-selection.md).

В оставшейся части этой статьи я покажу, как можно настроить процесс привязки модели. Сложные типы Однако рекомендуется использовать модули форматирования типа мультимедиа, по возможности. Ключевой принцип HTTP является то, что ресурсы отправляются в тексте сообщения, используя согласование содержимого для указания представление ресурса. Модули форматирования типа мультимедиа были разработаны для этой цели.

## <a name="using-fromuri"></a>С помощью [FromUri]

Чтобы заставить веб-API для чтения из URI со сложным типом, добавьте **[FromUri]** к параметру. В следующем примере определяется `GeoPoint` типа, а также метод контроллера, который получает `GeoPoint` из URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Клиент можно поместить значения широты и долготы в строке запроса и веб-API будет использовать их для создания `GeoPoint`. Пример:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>С помощью [FromBody]

Чтобы заставить веб-API для чтения к простому типу из текста запроса, добавьте **[FromBody]** к параметру атрибут:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

В этом примере веб-API будет использовать модуль форматирования типа мультимедиа для чтения значение *имя* из текста запроса. Ниже приведен пример запроса клиента.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Если параметр имеет [FromBody], веб-API использует заголовок Content-Type для выбора модуля форматирования. В этом примере — тип содержимого &quot;application/json&quot; и тело запроса является необработанная строка JSON (а не объект JSON).

Не более одного параметра может считывать данные из текста сообщения. Так что это не будет работать:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Причина для этого правила является то, что тело запроса может храниться в потоке без буферизации, которые могут быть прочитаны только один раз.

## <a name="type-converters"></a>Преобразователи типов

Веб-API следует считать класс простой тип (таким образом, веб-API будет пытаться привязать его из URI) можно сделать, создав **TypeConverter** и предоставляя преобразование строк.

В следующем коде показан `GeoPoint` класс, представляющий точку географических плюс строка **TypeConverter** , преобразующий из строк `GeoPoint` экземпляров. `GeoPoint` Класс снабжен **[TypeConverter]** атрибут для указания преобразователь типов. (В этом примере впечатлил Майк стол записи блога [способ привязки для пользовательских объектов в сигнатурах действия в MVC или веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Теперь веб-API будет рассматривать `GeoPoint` как простой тип, означающее, что она попытается привязать `GeoPoint` параметров из URI. Не нужно включать **[FromUri]** параметра.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Клиент может вызвать метод с URI следующим образом:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Связыватели моделей

Более гибкий вариант, чем преобразователь типов для создания настраиваемый связыватель модели. С помощью связывателя модели иметь доступ к HTTP-запроса, описание действия и необработанных значений из данных маршрута.

Чтобы создать связыватель модели, реализовать **IModelBinder** интерфейс. Этот интерфейс определяет единственный метод, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Вот связыватель модели для `GeoPoint` объектов.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Связыватель модели Получает необработанные входные значения из *поставщик значений*. Такая архитектура позволяет разделить две разные функции:

- Поставщик значений принимает HTTP-запроса и заполняет словарь пар "ключ значение".
- Связыватель модели использует этот словарь для заполнения модели.

Поставщик значения по умолчанию в веб-API получает значения из данных маршрута и строку запроса. Например, если URL-адрес является `http://localhost/api/values/1?location=48,-122`, поставщик значений создает следующие пары "ключ значение":

- Идентификатор = &quot;1&quot;
- расположение = &quot;48,122&quot;

(Я предполагаю, шаблон маршрута по умолчанию, который является &quot;api / {controller} / {id}&quot;.)

Имя параметра для привязки хранится в **ModelBindingContext.ModelName** свойство. Связыватель модели ищет ключ с этим значением в словаре. Если значение существует и могут быть преобразованы в `GeoPoint`, связыватель модели присваивает это значение привязанного **ModelBindingContext.Model** свойство.

Обратите внимание на то, что связыватель модели не ограничивается простым типом преобразования. В этом примере связывателя модели в первую очередь в таблицу из известных расположений, и в случае неудачи использует преобразование типов.

**Параметр связывателя модели**

Существует несколько способов задать связыватель модели. Во-первых, можно добавить **[ModelBinder]** к параметру.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Можно также добавить **[ModelBinder]** к типу атрибута. Веб-API будет использоваться связыватель модели указанного все параметры этого типа.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Наконец, можно добавить поставщик связывателей модели для **HttpConfiguration**. Поставщик связывателей модели — просто класс фабрики, создающий связыватель модели. Поставщик можно создать путем наследования от [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) класса. Тем не менее, если связывателю модели обрабатывает одного типа, проще использовать встроенное **SimpleModelBinderProvider**, разработанный для этой цели. В следующем примере кода показано, как это сделать:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

С помощью поставщика привязки модели, по-прежнему необходимо добавить **[ModelBinder]** к параметру, чтобы сообщить веб-API следует использовать связыватель модели и не модуль форматирования типа мультимедиа. Но теперь не нужно указывать тип связывателя модели в атрибуте:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Поставщики значений

Я упомянул, что привязка модели получает значения от поставщика значений. Чтобы создать поставщик пользовательского значения, реализовать **IValueProvider** интерфейс. Ниже приведен пример, который извлекает значения из файлов cookie в запросе.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Необходимо также создать фабрик поставщиков значений путем наследования от **ValueProviderFactory** класса.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Добавление фабрик поставщиков значений для **HttpConfiguration** следующим образом.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Веб-API объединяет все поставщики значений, поэтому при вызове метода связыватель модели **ValueProvider.GetValue**, связыватель модели получает значение из первого поставщика значений, которое сможет его создания.

Кроме того, можно задать фабрик поставщиков значений параметра на уровне с помощью **ValueProvider** атрибут, следующим образом:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Это сообщает веб-API используют привязку модели с помощью фабрики поставщика указанное значение, а не использовать любой из других поставщиков зарегистрированным значением.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Связыватели моделей — это определенный экземпляр более общий механизм. Если взглянуть на **[ModelBinder]** атрибут, вы увидите, что он является производным от абстрактного **ParameterBindingAttribute** класса. Этот класс определяет единственный метод, **GetBinding**, который возвращает **HttpParameterBinding** объекта:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** отвечает за привязывая параметр к значению. В случае использования **[ModelBinder]**, возвращает атрибут **HttpParameterBinding** реализация, которая использует **IModelBinder** для выполнения фактического привязки. Вы также можете реализовать собственные **HttpParameterBinding**.

Предположим, например, чтобы просмотреть теги eTag из `if-match` и `if-none-match` заголовков запроса. Мы начнем с определения класса для представления теги eTag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Также мы определим перечисление, чтобы указать, следует ли получить ETag из `if-match` заголовок или `if-none-match` заголовка.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Вот **HttpParameterBinding** , возвращает ETag из нужного заголовка и привязывает его к параметру типа ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** метод выполняет привязку. В этом методе, добавить значение привязанного параметра **аргумент ActionArgument** словарь в **HttpActionContext**.

> [!NOTE]
> Если ваш **ExecuteBindingAsync** метод считывает текст сообщения запроса, переопределить **WillReadBody** свойство возвращает значение true. Текст запроса могут быть небуферизованный поток, можно прочитать только один раз, поэтому веб-API применяет правило, не более одной привязки можно прочитать тело сообщения.


Чтобы применить пользовательский **HttpParameterBinding**, можно определить атрибут, который является производным от **ParameterBindingAttribute**. Для `ETagParameterBinding`, мы определим два атрибута, один для `if-match` заголовки и один для `if-none-match` заголовки. Оба являются производными от абстрактного базового класса.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Ниже приведен метод контроллера, который использует `[IfNoneMatch]` атрибута.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Помимо **ParameterBindingAttribute**, имеется другой обработчик для добавления настраиваемого **HttpParameterBinding**. На **HttpConfiguration** объекта, **ParameterBindingRules** свойство — это коллекция функций anomymous типа (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Например, можно добавить правила, использующего любой параметр ETag в метод GET `ETagParameterBinding` с `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Данная функция должна возвращать `null` для параметров, где привязка не применимо.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Весь процесс привязки параметров контролируется службой подключаемые **IActionValueBinder**. Реализация по умолчанию **IActionValueBinder** делает следующее:

1. Найдите **ParameterBindingAttribute** в параметре. Сюда входят **[FromBody]**, **[FromUri]**, и **[ModelBinder]**, или настраиваемых атрибутов.
2. В противном случае папка **HttpConfiguration.ParameterBindingRules** для функции, которая возвращает ненулевой **HttpParameterBinding**.
3. В противном случае используйте правила по умолчанию, которые я описал ранее. 

    - Если тип параметра «простой» или преобразователь типов, привязки из URI. Это эквивалентно помещения **[FromUri]** атрибута параметра.
    - В противном случае попробуйте считывать параметр из тела сообщения. Это эквивалентно помещения **[FromBody]** параметра.

При желании, можно заменить весь **IActionValueBinder** с пользовательской реализацией.

## <a name="additional-resources"></a>Дополнительные ресурсы

[Пример привязки пользовательского параметра](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Майк стол написал хороший ряд записей блога о привязке параметра веб-API:

- [Как веб-API работает привязка параметра](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Привязки параметров стиля MVC для веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Как выполнить привязку к пользовательских объектов в сигнатурах действия в MVC или веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Создание поставщика пользовательских значений в веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Привязка параметров веб-API взгляд изнутри](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
