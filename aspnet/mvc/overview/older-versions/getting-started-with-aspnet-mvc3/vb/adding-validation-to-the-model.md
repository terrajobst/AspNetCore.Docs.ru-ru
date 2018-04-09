---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Добавление проверки для модели (VB) | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 86058530aa00ecbc00aeebc6ed7b5cf019fdad72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model-vb"></a>Добавление проверки для модели (Visual Basic)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-validation-to-the-model.md) этого учебника.


В этом разделе вы добавите логику проверки в `Movie` модель и необходимо убедиться, что правила проверки применяются каждый раз при попытке создать или изменить фильм, с помощью приложения.

## <a name="keeping-things-dry"></a>Чтобы оставить ТОНЕРА

Одна из основных принципов разработки ASP.NET MVC — ПРОБНОГО («не повторяться»). ASP.NET MVC рекомендует как можно указать только один раз возможности или поведение и включить его everywhere отражаться в приложении. Это уменьшает объем кода, который необходимо написать и делает код, который вы написали гораздо проще для поддержки.

Поддержка проверки в ASP.NET MVC и Entity Framework Code First является отличным примером ТОНЕРА принципа в действии. Правила проверки можно задать декларативно в одном месте (в класс модели), а затем эти правила будут применены везде в приложении.

Давайте взглянем на как можно воспользоваться преимуществами этой поддержки проверки в приложении фильма.

## <a name="adding-validation-rules-to-the-movie-model"></a>Добавление правил проверки модели фильма

Сначала добавить некоторую логику проверки для `Movie` класса.

Откройте *Movie.vb* файла. Добавить `Imports` инструкции в верхней части файла, который ссылается на [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространство имен:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Пространство имен является частью .NET Framework. Она предоставляет встроенные набор атрибутов проверки, которые декларативно применяются к любому классу или свойству.

Теперь обновите `Movie` класса, чтобы воспользоваться преимуществами встроенной [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), и [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибутов проверки . В качестве примера для применения атрибутов, используйте следующий код.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Атрибуты проверки определяют поведение для свойств модели, к которым они применяются. `Required` Атрибут указывает, что свойство должно иметь значение; в этом образце фильм должен иметь значения для `Title`, `ReleaseDate`, `Genre`, и `Price` свойства, чтобы быть допустимыми. Атрибут `Range` ограничивает диапазон значений. Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства.

Код сначала проверяет, применение правил проверки, заданные на класс модели перед приложение сохраняет изменения в базе данных. Например, в приведенном ниже коде возникает исключение при `SaveChanges` вызывается метод, так как некоторые необходимые `Movie` отсутствуют значения свойств и цена равно нулю (то выходит за пределы допустимого диапазона).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

С платформой .NET Framework автоматически применяются правила проверки помогает сделать приложение более надежным. Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.

Ниже приведен полный код для обновленного *Movie.vb* файла:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Ошибка проверки пользовательского интерфейса в ASP.NET MVC

Повторно запустите приложение и перейдите к */Movies* URL-адрес.

Нажмите кнопку **создания фильма** ссылку, чтобы добавить новый фильма. Заполните форму с некоторые недопустимые значения, а затем нажмите кнопку **создать** кнопки.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Обратите внимание на то, как форма автоматически использовал цвет фона для выделите текстовые поля, содержащие недопустимые данные и испускаемого соответствующее сообщение об ошибке проверки рядом с каждого из них. Сообщения об ошибках соответствует строки ошибок указано, при которых указана `Movie` класса. Ошибки применяются (с использованием JavaScript) на стороне клиента и стороне сервера (если у пользователя есть JavaScript отключена).

Реальное преимущество имеет нет необходимости ни одной строки кода в `MoviesController` класса или в *Create.vbhtml* представления для включения проверки пользовательского интерфейса. Контроллер и представления, созданные ранее в этом учебнике, автоматически менеджера правил проверки, которые указаны с помощью атрибутов на `Movie` класс модели.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>О том, как проверка происходит в создания, просмотра и создания метода действия

Вам может быть интересно, как пользовательский интерфейс проверки создается без обновления кода контроллера или представлений. Далее показан что `Create` методы в `MovieController` класса, как будет выглядеть. Они отличается от способа их создания ранее в этом учебнике.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

Первый метод действия отображает начальной Создание формы. Вторая обрабатывает формы post. Второй `Create` вызовы метода `ModelState.IsValid` , чтобы проверить, имеет ли фильма ошибок проверки. При вызове этого метода оцениваются все атрибуты проверки, которые были применены к объекту. Если объект имеет ошибки проверки `Create` метод повторно отображает форму. Если ошибок нет, метод сохраняет новый фильм в базе данных.

Ниже приведен *Create.vbhtml* представление шаблон, который вы формирования шаблонов ранее в этом учебнике. Он используется в показанных выше методах действия для отображения исходной формы и повторного вывода формы в случае ошибки.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Обратите внимание на то, как код использует `Html.EditorFor` вспомогательный метод для вывода `<input>` для каждого элемента `Movie` свойство. Рядом с этого вспомогательного объекта представляет собой вызов `Html.ValidationMessageFor` вспомогательный метод. Эти два вспомогательных метода работы с объект модели, который передается по контроллера в представление (в этом случае `Movie` объекта). Они автоматически выполняют поиск проверки атрибутов, указанных в модели отображения сообщения об ошибках и соответствующим образом.

Что очень удобно об этом подходе является, что контроллер ни Просмотр шаблона создать знает ничего о применяются правила проверки или отображаются сообщения об ошибках. Правила проверки и строки ошибок указываются только в классе `Movie`.

Если вы хотите изменить логику проверки позже, это можно сделать только в одном месте. Вам не придется беспокоиться о несогласованности применения правил в различных частях приложения, поскольку вся логика проверки будет определена в одном месте и начнет применяться по всему приложению. Это позволяет максимально оптимизировать код и обеспечить удобство его совершенствования и поддержки. Кроме того, таким образом вы будете полностью соблюдать требования принципа "Не повторяйся".

## <a name="adding-formatting-to-the-movie-model"></a>Добавление форматирования к модели фильма

Откройте *Movie.vb* файла. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Пространство имен содержит атрибуты форматирования помимо встроенный набор атрибутов проверки. Примените [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибута и [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) значение перечисления для даты выпуска и поля цены. В следующем коде показано `ReleaseDate` и `Price` свойства с помощью соответствующих [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибута.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Кроме того, можно явно задать [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) значение. В следующем коде показано свойство даты выпуска с помощью строки формата даты (а именно, «d»). Это будет использовать для указания, что вы не хотите времени как часть даты выпуска.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

В следующем примере кода форматы `Price` свойство в денежном формате.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Полный `Movie` класса приведен ниже.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Запустите приложение и перейдите к `Movies` контроллера.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

В следующей части серии мы просмотрите приложение и совершенствовать некоторые автоматически созданный `Details` и `Delete` методы...

> [!div class="step-by-step"]
> [Назад](adding-a-new-field.md)
> [Вперед](improving-the-details-and-delete-methods.md)
