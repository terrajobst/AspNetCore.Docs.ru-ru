---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Добавление проверки | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 22e59a99a179a7a414447036b973e3ad3b462515
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577955"
---
<a name="adding-validation"></a>Добавление проверки
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе вы добавите логику проверки `Movie` модель также вы убедитесь, что правила проверки применяются каждый раз, пользователь пытается создать или изменить фильм в приложении.

## <a name="keeping-things-dry"></a>Чтобы оставить "не повторяйся"

Одним из основных принципов разработки ASP.NET MVC является ["не повторяйся"](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;Don't Repeat Yourself&quot;). ASP.NET MVC рекомендует указать функциональные возможности или поведение только один раз, а затем их в приложении. Это уменьшает объем кода, необходимые для записи и делает код, который вы пишете менее подверженной ошибкам и упрощает поддержку.

Поддержка проверки, реализуемая ASP.NET MVC и Entity Framework Code First является отличным примером принципа "не повторяйся" в действии. Правила проверки декларативно определяются в одном месте (в классе модели) и затем применяются везде в приложении.

Давайте рассмотрим, как можно воспользоваться преимуществами этой поддержки проверки в приложении фильма.

## <a name="adding-validation-rules-to-the-movie-model"></a>Добавление правил проверки к модели фильма

Сначала вы создадите добавить некоторую логику проверки для `Movie` класса.

Откройте файл *Movie.cs*. Обратите внимание, что [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространство имен содержит `System.Web`. Класс DataAnnotations предоставляет набор встроенных атрибутов проверки, которые можно декларативно применяются к любому классу или свойству. (Он также содержит атрибуты форматирования, такие как [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , обеспечивают форматирование и не предназначены для проверки.)

Теперь обновите `Movie` класса, чтобы воспользоваться преимуществами встроенных [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), и [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибутов проверки. Замените `Movie` следующим кодом:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Атрибут задает максимальную длину строки и он устанавливает это ограничение в базе данных, поэтому схема базы данных изменится. Щелкните правой кнопкой мыши **фильмы** в таблицу **обозревателя серверов** и нажмите кнопку **Открыть определение таблицы**:

![](adding-validation/_static/image1.png)

В приведенном выше рисунке видно, все строковые поля задаются для [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Мы будем использовать миграции для обновления схемы. Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна и введите следующие команды:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` производный класс с указанным именем (`DataAnnotations`) и в `Up` метод, можно просмотреть код, который обновляет ограничений схемы:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre` Поле больше не допускает значения NULL (то есть необходимо ввести значение). `Rating` Поле имеет длину не более 5 и `Title` имеет максимальную длину 60. Минимальная длина 3 на `Title` и диапазон на `Price` не создавали изменения схемы.

Изучите схему фильма:

![](adding-validation/_static/image2.png)

Строковые поля Показать новые ограничения длины и `Genre` , больше не проверяется как допускающее значение NULL.

Атрибуты проверки определяют поведение для свойств модели, к которым они применяются. Атрибуты `Required` и `MinimumLength` указывают, что свойство должно иметь значение. Тем не менее, чтобы удовлетворить требованиям проверки, пользователю достаточно ввести пробел. [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибут используется для ограничения, символы, которые могут быть входных данных. В приведенном выше коде в полях `Genre` и `Rating` можно использовать только буквы (пробелы, числа и специальные символы не допускаются). [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) Атрибут ограничивает значений в указанном диапазоне. Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства. Типы значений (таких как `decimal, int, float, DateTime`) являются обязательными и не требуют `Required` атрибута.

Код сначала гарантирует, что правила проверки, определенные на класс модели применяются, прежде чем приложение сохраняет изменения в базе данных. Например, приведенный ниже код создаст исключение [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) исключение при `SaveChanges` вызывается метод, так как некоторые необходимые `Movie` отсутствуют значения свойств:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Приведенный выше код вызывает следующее исключение:

*Не удалось проверить один или несколько сущностей. Свойству «EntityValidationErrors» Дополнительные сведения см.*

Наличие правил проверки, автоматически применяются в .NET Framework помогает сделать приложение более надежным. Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Ошибка проверки пользовательского интерфейса в ASP.NET MVC

Запустите приложение и перейдите к */Movies* URL-адрес.

Нажмите кнопку **Create New** ссылку, чтобы добавить новый фильм. Введите в форму какие-либо недопустимые значения. Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> Поддержка проверки jQuery для английского, используйте запятую («",»") для десятичной запятой, необходимо включить NuGet глобализации, как описано ранее в этом руководстве.


Обратите внимание на то, как формы автоматически использовал цвет красную границу для выделения текстовые поля, которые содержат недопустимые данные и испускаемого сообщение об ошибке соответствующей проверкой рядом с каждого из них. Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).

Реальные преимущество в том, что вам не нужно менять ни единой строчки кода в `MoviesController` класса или в *Create.cshtml* для реализации этой проверки пользовательского интерфейса. В контроллере и представлениях, создаваемых в рамках этого руководства, автоматически применяются правила проверки, для определения которых к свойствам класса модели `Movie` были применены атрибуты. При проверке с использованием метода действия `Edit` применяются те же правила.

Данные формы передаются на сервер только после того, как будут устранены любые ошибки на стороне клиента. Это можно проверить, установите точку останова в методе HTTP Post с помощью [инструмента fiddler](http://fiddler2.com/fiddler2/), или обозреватель IE [средства разработчика F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Проверка происходит в создания, просмотра и создания метода действия

Вам может быть интересно, как пользовательский интерфейс проверки создается без обновления кода контроллера или представлений. Далее показан что `Create` методы в `MovieController` выглядят класса. Они изменились по сравнению с как вы создали их ранее в этом учебнике.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Первый метод действия `Create` (HTTP GET) отображает исходную форму создания. Вторая версия (`[HttpPost]`) обрабатывает передачу формы. Второй метод `Create` (версия `HttpPost`) вызывает `ModelState.IsValid`, который определяет наличие ошибок проверки в фильме. При вызове этого метода оцениваются все атрибуты проверки, которые были применены к объекту. При наличии ошибок проверки в объекте метод `Create` повторно отображает форму. Если ошибок нет, метод сохраняет новый фильм в базе данных. В нашем примере фильма **форма не передается на сервер, при наличии ошибок проверки, обнаруженные на стороне клиента; второй** `Create` **никогда не вызывается метод**. Если отключить JavaScript в браузере, проверка клиента отключена и HTTP POST `Create` вызовы методов `ModelState.IsValid` для проверки, имеет ли фильм ошибок проверки.

Вы можете установить точку останова в метод `HttpPost Create` и убедиться, что он не вызывается и данные формы не передаются, если на стороне клиента присутствуют ошибки проверки. Если отключить JavaScript в браузере и отправить форму с ошибками, будет достигнута точка останова. Без JavaScript вы по-прежнему будете получать полную проверку. Ниже показано, как отключить JavaScript в Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

На следующем рисунке показано, как отключить JavaScript в браузере FireFox.

![](adding-validation/_static/image7.png)

На следующем рисунке показано, как отключить JavaScript в браузере Chrome.

![](adding-validation/_static/image8.png)

Ниже приведен *Create.cshtml* шаблон представления, сформированного ранее в этом руководстве. Он используется в показанных выше методах действия для отображения исходной формы и повторного вывода формы в случае ошибки.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Обратите внимание на то, как в коде используется `Html.EditorFor` вспомогательный метод для вывода `<input>` для каждого элемента `Movie` свойство. Рядом с этого вспомогательного объекта представляет собой вызов `Html.ValidationMessageFor` вспомогательный метод. Эти два вспомогательных метода работы с объектом модели, который передается с помощью контроллера в представление (в этом случае `Movie` объекта). Они автоматически выполняют поиск атрибутов проверки, указанные на модели и отображение сообщения об ошибках соответствующим образом.

Этот подход удобен тем, что ни контроллер, ни шаблон представления `Create` ничего не знают о фактически применяемых правилах проверки или отображаемых сообщениях об ошибках. Правила проверки и строки ошибок указываются только в классе `Movie`. Такие же правила проверки автоматически применяются к представлению `Edit` и любым другим представлениям модели, которые вы можете создавать или редактировать.

Если вы хотите изменить логику проверки позже, вы ее можно сделать в одном месте, добавив атрибуты проверки в модель (в этом примере `movie` класса). Вам не придется беспокоиться о несогласованности применения правил в различных частях приложения, поскольку вся логика проверки будет определена в одном месте и начнет применяться по всему приложению. Это позволяет максимально оптимизировать код и обеспечить удобство его совершенствования и поддержки. И это означает, что вы будете полностью соблюдать требования *"не повторяйся"* принцип.

## <a name="using-datatype-attributes"></a>Использование атрибутов DataType

Откройте файл *Movie.cs* и проверьте класс `Movie`. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Пространство имен предоставляет атрибуты форматирования в дополнение к набору встроенных атрибутов проверки. Мы уже использовали [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) значение перечисления для даты выпуска и поля цены. В следующем коде показан `ReleaseDate` и `Price` свойства с соответствующим [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) атрибута.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты предоставляют только рекомендации обработчик представлений для форматирования данных (а также другие атрибуты, такие как `<a>` для URL-адреса и `<a href="mailto:EmailAddress.com">` для электронной почты. Можно использовать [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибута для проверки формата данных. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут используется для указания типа данных, с более точным определением относительно встроенного типа базы данных, они являются ***не*** атрибутов проверки. В этом случае требуется отслеживать только дату, а не дату и время. [Перечисление DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) представлено множество типов данных, таких как *даты, времени, PhoneNumber, Currency, EmailAddress* и многое другое. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении. Например `mailto:` связи могут создаваться для [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), и можно предоставить селектор даты [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) в браузерах, поддерживающих [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) создает атрибуты HTML 5 [данных —](http://ejohn.org/blog/html-5-data-attributes/) (произносится *dash данных*) атрибуты, которые HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты не предназначены для проверки.

`DataType.Date` не задает формат отображаемой даты. По умолчанию поле данных отображается в соответствии с использованием на сервере форматов [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode` Параметр указывает, что заданное форматирование также должны быть применены при отображении значения в текстовом поле для редактирования. (Вы не хотите, чтобы для некоторых полей, например, для денежных значений не можно символ валюты в текстовом поле для редактирования.)

Можно использовать [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут отдельно, однако он обычно имеет смысл использовать [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) также атрибут. `DataType` Атрибут передает *семантику* данных как и в отличие от способа их вывода на экран и обеспечивает следующие преимущества по сравнению с `DisplayFormat`:

- Браузер можно включить функции HTML5 (например для отображения элемента управления календарем, языковому символа валюты, ссылок электронной почты и т. д.).
- По умолчанию браузер будет обрабатывать данные, используя правильный формат на основе вашего [языкового стандарта](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибута можно включить модель MVC может выбрать подходящий шаблон поля для отображения данных ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) при отдельном использовании базируется на строковом шаблоне). Дополнительные сведения см. в разделе Брэд Вилсон [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Хотя написана для MVC 2, эта статья по-прежнему относится к текущей версии ASP.NET MVC.)

Если вы используете `DataType` атрибут с полем даты, вам нужно будет указать `DisplayFormat` атрибут также, чтобы гарантировать, что и поле правильно отображаются в браузерах Chrome. Дополнительные сведения см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> Проверка jQuery не работает с [диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибут и [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Вам потребуется отключить проверку дат jQuery для использования [диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибут с [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Это обычно не рекомендуется компилировать модели, поэтому использование с фиксированными датами [диапазон](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибут и [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) не рекомендуется.


В следующем коде демонстрируется объединение атрибутов в одной строке:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

В следующей части этой серии мы рассмотрим приложение и внесем ряд изменений в автоматически создаваемые методы `Details` и `Delete`.

> [!div class="step-by-step"]
> [Назад](adding-a-new-field.md)
> [Вперед](examining-the-details-and-delete-methods.md)
