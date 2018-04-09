---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Добавление нового поля в таблице и модели фильма | Документы Microsoft
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Добавление нового поля в таблице и модели фильма
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.


В этом разделе используется Entity Framework Code First Migrations для миграции некоторые изменения в классы моделей, изменение вступает в силу в базе данных.

По умолчанию при использовании Entity Framework Code First автоматически создать базу данных, как это было сделано ранее в этом учебнике Code First добавляет таблицу в базу данных для отслеживания, является ли синхронизация с помощью классов модели, он был создан из схемы базы данных. Если они не являются синхронизированными, Entity Framework выдает ошибку. Это упрощает для выявления проблем во время разработки, которые может в противном случае только найти (непонятные ошибки) во время выполнения.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Настройка миграции Code First для изменения модели

Если вы используете Visual Studio 2012, дважды щелкните *Movies.mdf* файл в обозревателе решений, чтобы открыть средство базы данных. Visual Studio Express для Web покажет обозреватель баз данных Visual Studio 2012 будет отображаться в обозревателе серверов. Если вы используете Visual Studio 2010, используйте обозреватель объектов SQL Server.

В средстве базы данных (обозреватель баз данных, обозреватель серверов или обозревателе объектов SQL Server), щелкните правой кнопкой мыши `MovieDBContext` и выберите **удалить** удалить базу данных фильмов.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Перейдите обратно в обозревателе решений. Щелкните правой кнопкой мыши *Movies.mdf* файла и выберите **удалить** удаление фильмы базы данных.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Постройте приложение, чтобы убедиться в отсутствии ошибок.

Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

![Добавьте пакет Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

В **консоль диспетчера пакетов** окно в `PM>` строке введите «Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext».

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-Migrations** (как показано выше) команда создает *Configuration.cs* файл в новом *миграций* папки.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio открывает *Configuration.cs* файла. Замените `Seed` метод в *Configuration.cs* файла следующим кодом:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Щелкните правой кнопкой мыши на волнистую красную линию под `Movie` и выберите **Разрешить** затем **с помощью** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Это добавляет следующих инструкций:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations вызовы `Seed` метод после каждой миграции (то есть вызов **базы данных обновления** в консоли диспетчера пакетов), и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существует.


**Нажмите клавиши CTRL-SHIFT-B для сборки проекта.** (Ниже завершится ошибкой, если ваш не на этом этапе построения.)

Следующим шагом является создание `DbMigration` класс для первоначальной миграции. Этот перенос создает новую базу данных, поэтому вы удалили *movie.mdf* файла в предыдущем шаге.

В **консоль диспетчера пакетов** окно, введите команду «Добавить миграции начальным» для создания первоначальной миграции. Имя «Начального» является произвольным и используется для именования создан файл для миграции.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First Migrations создает еще один файл класса в *миграций* папку (с именем *{метки даты}\_Initial.cs* ), и этот класс содержит код, создающий схему базы данных. Filename миграции предварительно фиксированной с меткой времени, чтобы помочь с порядком. Изучите *{метки даты}\_Initial.cs* файл, содержащий инструкции для создания таблицы фильмы для DB фильма. При обновлении базы данных в инструкциях ниже это *{метки даты}\_Initial.cs* файла будет выполняться и создать схему базы данных. Затем **начальное значение** метод работает для заполнения базы данных тестовыми данными.

В **консоль диспетчера пакетов**, введите команду «update-database» для создания базы данных и выполнения **начальное значение** метод.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Если возникает сообщение об ошибке, указывающее таблицу, уже существует и не может быть создан, возможно, будет потому, что приложение запущено после удаления базы данных и до выполнения `update-database`. В этом случае удаление *Movies.mdf* файл еще раз и повторите `update-database` команды. Если ошибки продолжают возникать, удалите папку migrations и содержимое, а затем запустить с инструкциями в верхней части этой страницы (это delete *Movies.mdf* файла, а затем перейдите к Enable-Migrations).

Запустите приложение и перейдите к */Movies* URL-адрес. Отображаются данные начальное значение.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Добавление свойства Rating в модель Movie

Начните с добавления нового `Rating` свойства для существующего `Movie` класса. Откройте *Models\Movie.cs* и добавьте `Rating` свойства следующим образом:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Полный `Movie` класса сейчас выглядит как следующий код:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Построение приложения с помощью **построения** &gt; **построения фильма** меню команды или нажав сочетание клавиш CTRL-SHIFT-B.

Теперь, когда вы обновили `Model` класса, необходимо также обновить *\Views\Movies\Index.cshtml* и *\Views\Movies\Create.cshtml* просмотра шаблонов для отображения новых `Rating`свойства в представлении обозревателя.

Откройте<em>\Views\Movies\Index.cshtml</em> и добавьте `<th>Rating</th>` сразу после заголовка столбца <strong>цены</strong> столбца. Затем добавьте `<td>` столбец ближе к концу шаблона для отображения `@item.Rating` значение. Ниже приведен какие обновленный <em>Index.cshtml</em> Просмотр шаблона выглядит следующим образом:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Затем откройте *\Views\Movies\Create.cshtml* и добавьте следующую разметку в конец формы. Это отрисовывает текстовое поле, рейтинг можно указать при создании новых фильмов.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Теперь вы обновили код приложения, поддерживающие новое `Rating` свойство.

Теперь запустите приложение и перейдите к */Movies* URL-адрес. При этом, вы увидите одно из следующих ошибок:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Вы видите эту ошибку, так как обновленный `Movie` класс модели в приложении теперь отличается от схемы `Movie` существующей базы данных. (В таблице базы данных отсутствует столбец `Rating`.)


Устранить эту ошибку можно несколькими способами:

1. Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели. Этот подход очень удобна при выполнении active разработки на тестовую базу данных; он позволяет быстро меняются схему модели и базы данных вместе. Недостатком такого подхода является потеря существующие данные в базе данных, поэтому вы *не* Чтобы использовать этот подход для производственной базы данных! С помощью инициализатора автоматически инициализировать базу данных с тестовыми данными чаще всего эффективный способ разработки приложения. Дополнительные сведения об инициализаторах базы данных Entity Framework см. в разделе Tom Dykstra [ASP.NET MVC и Entity Framework Учебник](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели. Преимущество такого подхода состоит в том, что сохраняются все данные. Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.
3. Можно обновить схему базы данных с помощью Code First Migrations.


В этом руководстве используется Code First Migrations.

Обновите метод начальное значение, чтобы он предоставляет значение для нового столбца. Откройте файл Migrations\Configuration.cs и добавьте поле «Рейтинг» к каждому объекту фильма.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Постройте решение, а затем откройте **консоль диспетчера пакетов** окно и введите следующую команду:

`add-migration AddRatingMig`

`add-migration` Команда указывает среде миграции для проверки текущей модели фильма с текущей схемой фильма базы данных и создания кода, необходимые для переноса базы данных в новую модель. AddRatingMig является произвольным и используется для именования файл для миграции. Рекомендуется использовать понятное имя для шага миграции.

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` производного класса и в `Up` метода можно просмотреть код, который создает новый столбец.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Постройте решение, а затем введите команду «update-database» в **консоль диспетчера пакетов** окна.

На следующем рисунке показаны выходные данные в **консоль диспетчера пакетов** окна (Отметка даты, добавляя AddRatingMig будет отличаться.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Повторно запустите приложение и перейдите к /Movies URL-адрес. Появится новое поле оценки.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Нажмите кнопку **создать новый** ссылку, чтобы добавить новый фильма. Обратите внимание, что можно добавить оценку.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Нажмите кнопку **Создать**. Новый фильм, включая оценку, теперь отображается список фильмов:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Также следует добавить `Rating` поле для изменения, сведения и SearchIndex шаблонов представлений.

Можно ввести команду «update-database» в **консоль диспетчера пакетов** окно и изменения не будут внесены, так как схема совпадает модели.

В этом разделе вы узнали, как можно изменять объекты модели и синхронизировать базу данных с изменениями. Вы также узнали способ заполнения вновь созданной базы данных с образцами данных, можно опробовать сценариев. Теперь давайте взглянем на как можно добавить расширенные логику проверки в классы модели и включить некоторые бизнес-правила для применения.

> [!div class="step-by-step"]
> [Назад](examining-the-edit-methods-and-edit-view.md)
> [Вперед](adding-validation-to-the-model.md)
