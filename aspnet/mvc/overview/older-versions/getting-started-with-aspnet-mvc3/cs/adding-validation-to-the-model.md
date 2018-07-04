---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Добавление проверки в модель (C#) | Документация Майкрософт
author: Rick-Anderson
description: Создание контроллера
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: bf0a0d96ed41d69f4baa693d35a1a08ef62866c8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388663"
---
<a name="adding-validation-to-the-model-c"></a>Добавление проверки в модель (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.
> 
> 
> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) работы с этим руководством.


В этом разделе вы добавите логику проверки `Movie` модель также вы убедитесь, что правила проверки применяются каждый раз, пользователь пытается создать или изменить фильм в приложении.

## <a name="keeping-things-dry"></a>Чтобы оставить "не повторяйся"

Одним из основных принципов разработки ASP.NET MVC является DRY («Don't Repeat Yourself»). ASP.NET MVC рекомендует указать функциональные возможности или поведение только один раз, а затем их в приложении. Это уменьшает объем кода, необходимые для записи и делает написанный код гораздо проще поддерживать.

Поддержка проверки, реализуемая ASP.NET MVC и Entity Framework Code First является отличным примером принципа "не повторяйся" в действии. Правила проверки декларативно определяются в одном месте (в классе модели) и затем эти правила применяются в рамках всего приложения.

Давайте рассмотрим, как можно воспользоваться преимуществами этой поддержки проверки в приложении фильма.

## <a name="adding-validation-rules-to-the-movie-model"></a>Добавление правил проверки к модели фильма

Сначала вы создадите добавить некоторую логику проверки для `Movie` класса.

Откройте файл *Movie.cs*. Добавить `using` инструкция в верхней части файла, который ссылается на [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространство имен:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Пространство имен является частью платформы .NET Framework. Он предоставляет набор встроенных атрибутов проверки, которые можно декларативно применяются к любому классу или свойству.

Теперь обновите `Movie` класса, чтобы воспользоваться преимуществами встроенных [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), и [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) атрибуты проверки . Используйте следующий код в качестве примера применения атрибутов.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Атрибуты проверки определяют поведение для свойств модели, к которым они применяются. `Required` Атрибут указывает, что свойство должно иметь значение; в этом образце фильм должен иметь значения для `Title`, `ReleaseDate`, `Genre`, и `Price` свойства, чтобы быть допустимыми. Атрибут `Range` ограничивает диапазон значений. Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства.

Код сначала гарантирует, что правила проверки, определенные на класс модели применяются, прежде чем приложение сохраняет изменения в базе данных. Например, приведенный ниже код приведет к возникновению исключения при `SaveChanges` вызывается метод, так как некоторые необходимые `Movie` значения свойств отсутствуют, и стоимость равна нулю (что находится вне допустимого диапазона).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Наличие правил проверки, автоматически применяются в .NET Framework помогает сделать приложение более надежным. Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.

Ниже приведен полный листинг кода для обновленного *Movie.cs* файла:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Ошибка проверки пользовательского интерфейса в ASP.NET MVC

Повторно запустите приложение и перейдите к */Movies* URL-адрес.

Нажмите кнопку **создания фильма** ссылку, чтобы добавить новый фильм. Заполните форму какие-либо недопустимые значения и нажмите кнопку **создать** кнопки.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Обратите внимание на то, как формы автоматически использовал цвет фона для выделения текстовые поля, которые содержат недопустимые данные и испускаемого сообщение об ошибке соответствующей проверкой рядом с каждого из них. Сообщения об ошибках соответствует строки ошибок, которые вы указали при с заметками `Movie` класса. Эти ошибки применяются как (с помощью JavaScript) на стороне клиента, так и на стороне сервера (если у пользователя есть отключает JavaScript).

Реальные преимущество в том, что вам не нужно менять ни единой строчки кода в `MoviesController` класса или в *Create.cshtml* для реализации этой проверки пользовательского интерфейса. В контроллере и представлениях, созданную ранее в этом учебнике, автоматически применяются правила проверки, указанный с помощью атрибутов на `Movie` класс модели.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Проверка происходит в создания, просмотра и создания метода действия

Вам может быть интересно, как пользовательский интерфейс проверки создается без обновления кода контроллера или представлений. Далее показан что `Create` методы в `MovieController` выглядят класса. Они изменились по сравнению с как вы создали их ранее в этом учебнике.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Первый метод действия отображает исходную форму создания. Вторая обрабатывает передачу формы. Второй `Create` вызовы методов `ModelState.IsValid` для проверки, имеет ли фильм ошибок проверки. При вызове этого метода оцениваются все атрибуты проверки, которые были применены к объекту. Если объект имеет ошибки проверки, `Create` метод повторно отображает форму. Если ошибок нет, метод сохраняет новый фильм в базе данных.

Ниже приведен *Create.cshtml* шаблон представления, сформированного ранее в этом руководстве. Он используется в показанных выше методах действия для отображения исходной формы и повторного вывода формы в случае ошибки.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Обратите внимание на то, как в коде используется `Html.EditorFor` вспомогательный метод для вывода `<input>` для каждого элемента `Movie` свойство. Рядом с этого вспомогательного объекта представляет собой вызов `Html.ValidationMessageFor` вспомогательный метод. Эти два вспомогательных метода работы с объектом модели, который передается с помощью контроллера в представление (в этом случае `Movie` объекта). Они автоматически выполняют поиск атрибутов проверки, указанные на модели и отображение сообщения об ошибках соответствующим образом.

Замечательный об этом подходе то, что ни контроллер, ни шаблон представления создать ничего не знают о применяемых правилах проверки или отображаемых сообщениях об ошибках. Правила проверки и строки ошибок указываются только в классе `Movie`.

Если вы хотите изменить логику проверки позже, это можно сделать в одном месте. Вам не придется беспокоиться о несогласованности применения правил в различных частях приложения, поскольку вся логика проверки будет определена в одном месте и начнет применяться по всему приложению. Это позволяет максимально оптимизировать код и обеспечить удобство его совершенствования и поддержки. Кроме того, таким образом вы будете полностью соблюдать требования принципа "Не повторяйся".

## <a name="adding-formatting-to-the-movie-model"></a>Добавление форматирования к модели фильма

Откройте файл *Movie.cs*. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Пространство имен предоставляет атрибуты форматирования в дополнение к набору встроенных атрибутов проверки. Вы примените [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут и [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) значение перечисления для даты выпуска и поля цены. В следующем коде показан `ReleaseDate` и `Price` свойства с соответствующим [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибута.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Кроме того, можно явно задать [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) значение. В следующем коде показано свойство даты выпуска с помощью строки формата даты (а именно, «d»). Это используется для указания, что вы не хотите времени как часть даты выпуска.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

В следующем примере кода форматы `Price` свойства в виде валюты.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Полный `Movie` класса приведен ниже.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Запустите приложение и перейдите к `Movies` контроллера.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

В следующей части этой серии мы рассмотрим приложение и внесем ряд изменений в автоматически создаваемые методы `Details` и `Delete`.

> [!div class="step-by-step"]
> [Назад](adding-a-new-field.md)
> [Вперед](improving-the-details-and-delete-methods.md)
