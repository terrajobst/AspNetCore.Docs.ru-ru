---
uid: mvc/overview/getting-started/introduction/adding-validation
title: "Добавление проверки | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 8d768727772738264d088315e605cca72db8de0a
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation"></a>Добавление проверки
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

В этом разделе вы добавите логику проверки в `Movie` модель и необходимо убедиться, что правила проверки применяются каждый раз при попытке создать или изменить фильм, с помощью приложения.

## <a name="keeping-things-dry"></a>Чтобы оставить ТОНЕРА

Одна из основных принципов разработки ASP.NET MVC — [ТОНЕРА](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;не повторяться&quot;). ASP.NET MVC рекомендует как можно указать только один раз возможности или поведение и включить его everywhere отражаться в приложении. Это уменьшает объем кода, который необходимо написать и делает код, который вы написали менее подвержен ошибкам и проще.

Поддержка проверки в ASP.NET MVC и Entity Framework Code First является отличным примером ТОНЕРА принципа в действии. Правила проверки можно задать декларативно в одном месте (в класс модели) и правила применяются везде в приложении.

Давайте взглянем на как можно воспользоваться преимуществами этой поддержки проверки в приложении фильма.

## <a name="adding-validation-rules-to-the-movie-model"></a>Добавление правил проверки модели фильма

Сначала добавить некоторую логику проверки для `Movie` класса.

Откройте файл *Movie.cs*. Обратите внимание [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространство имен не содержит `System.Web`. DataAnnotations предоставляет встроенный набор атрибутов проверки, которые декларативно применяются к любому классу или свойству. (Он также содержит атрибуты форматирования, такие как [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , помочь с форматированием и не обеспечивают проверку.)

Теперь обновите `Movie` класса, чтобы воспользоваться преимуществами встроенной [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [регулярное выражение](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), и [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибутов проверки. Замените `Movie` класса следующими строками:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Атрибут задает максимальную длину строки и это ограничение задается в базе данных, поэтому схема базы данных изменится. Щелкните правой кнопкой мыши **фильмов** в таблицу **обозревателя серверов** и нажмите кнопку **Открыть определение таблицы**:

![](adding-validation/_static/image1.png)

В приведенном выше рисунке видно, все поля строки заданы [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Мы будем использовать миграции для обновления схемы. Постройте решение, а затем откройте **консоль диспетчера пакетов** окно и введите следующие команды:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` производного класса с заданным именем (`DataAnnotations`) и в `Up` метод, можно будет увидеть код, который обновляет ограничений схемы:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Поле больше не допускает значения NULL (то есть, необходимо ввести значение). `Rating` Поле не должно превышать 5 и `Title` имеет максимальную длину 60. Минимальная длина 3 на `Title` и диапазон на `Price` не была создана изменения схемы.

Проверьте схему фильма:

![](adding-validation/_static/image2.png)

В полях строки отображаются новые ограничения длины и `Genre` проверяется как допускающая значение NULL.

Атрибуты проверки определяют поведение для свойств модели, к которым они применяются. Атрибуты `Required` и `MinimumLength` указывают, что свойство должно иметь значение. Тем не менее, чтобы удовлетворить требованиям проверки, пользователю достаточно ввести пробел. [Регулярное выражение](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибут используется для ограничения на символы, которые могут быть входных данных. В приведенном выше коде в полях `Genre` и `Rating` можно использовать только буквы (пробелы, числа и специальные символы не допускаются). [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Атрибут ограничивает значение для указанного диапазона. Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства. Типы значений (таких как `decimal, int, float, DateTime`) по своей природе требуются и не нужен `Required` атрибута.

Код сначала проверяет, применение правил проверки, заданные на класс модели перед приложение сохраняет изменения в базе данных. Например, приведенный ниже код вызывает исключение [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) исключение при `SaveChanges` вызывается метод, так как некоторые необходимые `Movie` отсутствуют значения свойства:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Приведенный выше код вызывает следующее исключение:

*Не удалось проверить одну или несколько сущностей. См. Дополнительные сведения о свойстве «EntityValidationErrors».*

С платформой .NET Framework автоматически применяются правила проверки помогает сделать приложение более надежным. Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Ошибка проверки пользовательского интерфейса в ASP.NET MVC

Запустите приложение и перейдите к */Movies* URL-адрес.

Нажмите кнопку **создать новый** ссылку, чтобы добавить новый фильма. Введите в форму какие-либо недопустимые значения. Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> Поддержка проверки jQuery для языков, кроме английского, используйте запятую («,») для десятичной запятой, необходимо включить NuGet глобализации, как описано ранее в этом учебнике.


Обратите внимание, как форма автоматически использовал цвет красной границей выделять текстовые поля, содержащие недопустимые данные и испускаемого соответствующее сообщение об ошибке проверки рядом с каждой. Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).

Реальное преимущество имеет нет необходимости ни одной строки кода в `MoviesController` класса или в *Create.cshtml* представления для включения проверки пользовательского интерфейса. В контроллере и представлениях, создаваемых в рамках этого руководства, автоматически применяются правила проверки, для определения которых к свойствам класса модели `Movie` были применены атрибуты. При проверке с использованием метода действия `Edit` применяются те же правила.

Данные формы передаются на сервер только после того, как будут устранены любые ошибки на стороне клиента. Это можно проверить, разместить точки останова в методе HTTP Post с помощью [инструмента fiddler](http://fiddler2.com/fiddler2/), или IE [средств разработчика F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>О том, как проверка происходит в создания, просмотра и создания метода действия

Вам может быть интересно, как пользовательский интерфейс проверки создается без обновления кода контроллера или представлений. Далее показан что `Create` методы в `MovieController` класса, как будет выглядеть. Они отличается от способа их создания ранее в этом учебнике.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Первый метод действия `Create` (HTTP GET) отображает исходную форму создания. Вторая версия (`[HttpPost]`) обрабатывает передачу формы. Второй метод `Create` (версия `HttpPost`) вызывает `ModelState.IsValid`, который определяет наличие ошибок проверки в фильме. При вызове этого метода оцениваются все атрибуты проверки, которые были применены к объекту. При наличии ошибок проверки в объекте метод `Create` повторно отображает форму. Если ошибок нет, метод сохраняет новый фильм в базе данных. В нашем примере фильма **формы не отправляется на сервер при наличии ошибок проверки, обнаруженных на стороне клиента; второй** `Create` **никогда не вызывается метод**. Если отключить JavaScript в браузере, проверка клиента отключена и HTTP POST `Create` вызовы метода `ModelState.IsValid` , чтобы проверить, имеет ли фильма ошибок проверки.

Вы можете установить точку останова в метод `HttpPost Create` и убедиться, что он не вызывается и данные формы не передаются, если на стороне клиента присутствуют ошибки проверки. Если отключить JavaScript в браузере и отправить форму с ошибками, будет достигнута точка останова. Без JavaScript вы по-прежнему будете получать полную проверку. На рисунке показано, как отключить JavaScript в Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

На следующем рисунке показано, как отключить JavaScript в браузере FireFox.

![](adding-validation/_static/image7.png)

На следующем рисунке показано, как отключить JavaScript в браузере Chrome.

![](adding-validation/_static/image8.png)

Ниже приведен *Create.cshtml* представление шаблон, который вы формирования шаблонов ранее в этом учебнике. Он используется в показанных выше методах действия для отображения исходной формы и повторного вывода формы в случае ошибки.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Обратите внимание на то, как код использует `Html.EditorFor` вспомогательный метод для вывода `<input>` для каждого элемента `Movie` свойство. Рядом с этого вспомогательного объекта представляет собой вызов `Html.ValidationMessageFor` вспомогательный метод. Эти два вспомогательных метода работы с объект модели, который передается по контроллера в представление (в этом случае `Movie` объекта). Они автоматически выполняют поиск проверки атрибутов, указанных в модели отображения сообщения об ошибках и соответствующим образом.

Этот подход удобен тем, что ни контроллер, ни шаблон представления `Create` ничего не знают о фактически применяемых правилах проверки или отображаемых сообщениях об ошибках. Правила проверки и строки ошибок указываются только в классе `Movie`. Такие же правила проверки автоматически применяются к представлению `Edit` и любым другим представлениям модели, которые вы можете создавать или редактировать.

Если вы хотите изменить логику проверки позднее, вы ее можно сделать только в одном месте путем добавления атрибутов проверки для модели (в этом примере `movie` класса). Вам не придется беспокоиться о несогласованности применения правил в различных частях приложения, поскольку вся логика проверки будет определена в одном месте и начнет применяться по всему приложению. Это позволяет максимально оптимизировать код и обеспечить удобство его совершенствования и поддержки. А это означает, что вы будете полностью учета *ТОНЕРА* принцип.

## <a name="using-datatype-attributes"></a>Использование атрибутов DataType

Откройте файл *Movie.cs* и проверьте класс `Movie`. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Пространство имен содержит атрибуты форматирования помимо встроенный набор атрибутов проверки. Мы уже использовали [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) значение перечисления для даты выпуска и поля цены. В следующем коде показано `ReleaseDate` и `Price` свойства с помощью соответствующих [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибута.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты только предоставить подсказки для обработчика представлений для форматирования данных (и укажите атрибуты, такие как `<a>` для URL-адреса и `<a href="mailto:EmailAddress.com">` для электронной почты. Можно использовать [регулярное выражение](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибут для проверки формата данных. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут используется для указания типа данных, который является более точным определением, чем встроенный тип базы данных, они являются ***не*** атрибутов проверки. В этом случае нам нужен только для отслеживания даты, не даты и времени. [Перечисление DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) предоставляет для многих типов данных, таких как *даты, времени, PhoneNumber, валюты, EmailAddress* и многое другое. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении. Например `mailto:` связи могут создаваться для [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), и элемент выбора даты, которые могут быть предоставлены для [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) в браузерах, поддерживающих [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты выдает HTML 5 [от данных](http://ejohn.org/blog/html-5-data-attributes/) (произносится *тире данных*) атрибутов, которые можно понять браузеров HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты не имеют каких-либо проверок.

`DataType.Date` не задает формат отображаемой даты. По умолчанию, поле данных отображается в соответствии с форматы по умолчанию на сервере[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Параметр указывает, что заданное форматирование должен также быть применяется, когда значение отображается в текстовом поле для редактирования. (Не имеет смысла, для некоторых полей, например, для значений валют, может потребоваться обозначение денежной единицы в текстовом поле для редактирования.)

Можно использовать [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут сам, но обычно имеет смысл использовать [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) также атрибут. `DataType` Передает атрибут *семантику* данных как отличие от Подготовка к просмотру его на экране и предоставляет следующие преимущества, которые вы не получаете с `DisplayFormat`:

- Браузер может включить функции HTML5 (например для отображения элемента управления календаря, локализованными обозначение денежной единицы, ссылок по электронной почте, и т. д.).
- По умолчанию браузер будет отображаться с использованием правильного формата на основе данных вашей[языкового стандарта](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибута можно включить MVC выбрать шаблон справа поля для отображения данных ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Если используется сама использует шаблон строки). Дополнительные сведения см. в разделе Брэд Вилсон[ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Хотя предназначено для MVC 2, в этой статье по-прежнему применяется к текущей версии ASP.NET MVC.)

Если вы используете `DataType` атрибут с полем даты, необходимо указать `DisplayFormat` атрибута также для того, чтобы убедиться в правильном отображении поля в браузерах Chrome. Дополнительные сведения см. в разделе [этот поток StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> Проверка jQuery не работает с[диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибута и[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Необходимо отключить проверку jQuery даты для использования [диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибутом[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Это обычно не рекомендуется для компиляции жестких дат в модели, поэтому использование[диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибута и[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) не рекомендуется.


В следующем коде демонстрируется объединение атрибутов в одной строке:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

В следующей части этой серии мы рассмотрим приложение и внесем ряд изменений в автоматически создаваемые методы `Details` и `Delete`.

>[!div class="step-by-step"]
[Назад](adding-a-new-field.md)
[Вперед](examining-the-details-and-delete-methods.md)
