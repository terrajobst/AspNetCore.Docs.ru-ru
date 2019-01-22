---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Учебник. Добавить сортировку, фильтрацию и разбиение по страницам с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: В этом руководстве описано добавление сортировки, фильтрации и разбиения по страницам для **учащихся** страницы индекса. Можно также создать страницу простой группировкой.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1f18a15d39d58ffb4ac48cfccee6519d33294e85
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444199"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Учебник. Добавить сортировку, фильтрацию и разбиение по страницам с Entity Framework в приложении ASP.NET MVC

В [предыдущем учебном курсе](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), реализован набор веб-страниц для основных операций CRUD для `Student` сущностей. В этом руководстве описано добавление сортировки, фильтрации и разбиения по страницам для **учащихся** страницы индекса. Можно также создать страницу простой группировкой.

Ниже показано, что страница будет выглядеть так, когда все будет готово. Заголовки столбцов являются ссылками, при нажатии на которые происходит сортировка по этому столбцу. При повторном нажатии на заголовок происходит переключение между сортировкой по возрастанию и убыванию.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Добавление ссылки сортировки столбцов
> * Добавление поля поиска
> * Добавление разбиения по страницам
> * Создание страницы About

## <a name="prerequisites"></a>Предварительные требования

* [Реализация базовой функциональности CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Добавление ссылки сортировки столбцов

Добавление сортировки на страницу указателя учащихся изменим `Index` метод `Student` контроллера и добавьте код в `Student` Индексируйте представление.

### <a name="add-sorting-functionality-to-the-index-method"></a>Добавление функции сортировки в метод Index

- В *Controllers\StudentController.cs*, замените `Index` метод следующим кодом:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Этот код принимает параметр `sortOrder` из строки запроса в URL. Значение строки запроса, предоставляемые ASP.NET MVC в качестве параметра метода действия. Параметр — это строка, которая является «Name» или «Date» (необязательно) за которым следует символ подчеркивания и строки «desc», чтобы указать порядок по убыванию. По умолчанию используется порядок сортировки по возрастанию.

При первом запросе страницы Index строка запроса отсутствует. Учащиеся отображаются в порядке возрастания значений `LastName`, который используется по умолчанию, установленные по фамилиям в `switch` инструкции. Когда пользователь щелкает гиперссылку заголовка столбца, в строку запроса подставляется соответствующее значение параметра `sortOrder`.

Два `ViewBag` переменные используются, чтобы представление можно настроить гиперссылок в заголовки столбцов с соответствующими значениями строки запроса:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Это тернарные условные операторы. Первый из них указывает, что если `sortOrder` параметр равен null или пусто, `ViewBag.NameSortParm` должно быть присвоено «имя\_desc»; в противном случае задается пустая строка. Следующие два оператора устанавливают гиперссылки в заголовках столбцов в представлении следующим образом:

| Текущий порядок сортировки | Гиперссылка "Last Name" (Фамилия) | Гиперссылка "Date" (Дата) |
| --- | --- | --- |
| "Last Name" (Фамилия) по возрастанию | по убыванию | по возрастанию |
| "Last Name" (Фамилия) по убыванию | по возрастанию | по возрастанию |
| "Date" (Дата) по возрастанию | по возрастанию | по убыванию |
| "Date" (Дата) по убыванию | по возрастанию | по возрастанию |

Данный метод использует [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) позволяет выбрать столбец для сортировки. Код создает <xref:System.Linq.IQueryable%601> переменной перед `switch` инструкция, изменяет его в `switch` инструкции и вызовы `ToList` метод после `switch` инструкции. После создания и изменения переменных `IQueryable` запрос в базу данных не отправляется. Запрос не выполняется, пока вы не преобразуете `IQueryable` объект в коллекцию путем вызова метода, например `ToList`. Таким образом, этот код вызывает в одном запросе, не выполняется до `return View` инструкции.

Вместо написания различных операторов LINQ для каждого порядка сортировки можно динамически создать оператор LINQ. Сведения о динамических LINQ см. в разделе [динамического LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Добавление гиперссылок в заголовки столбцов в представление Student index

1. В *Views\Student\Index.cshtml*, замените `<tr>` и `<th>` элементов для строки заголовков с выделенный код:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Этот код использует эти сведения в `ViewBag` свойства для настройки гиперссылок с использованием соответствующего запроса строковые значения.

2. Откройте страницу и щелкните **Фамилия** и **Дата регистрации** заголовки столбцов, чтобы убедитесь, что Сортировка работает.

   После того как вы щелкнете **Фамилия** заголовок, учащиеся отображаются по убыванию последнее имя.

## <a name="add-a-search-box"></a>Добавление поля поиска

Для добавления фильтра на страницу индекса учащихся, вы добавите в представление текстовое поле и кнопка отправки и внести соответствующие изменения в `Index` метод. Текстовое поле позволяет ввести строку для поиска в имя полях имени и фамилии.

### <a name="add-filtering-functionality-to-the-index-method"></a>Добавление функций фильтрации в метод Index

- В *Controllers\StudentController.cs*, замените `Index` метод (изменения выделены) следующим кодом:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Код добавляет `searchString` параметр `Index` метод. Значение строки поиска получается из текстового поля, которое мы добавили в представление Index. Он также добавляет `where` предложение в оператор LINQ, которое отбирает только студентов, чье имя или Фамилия содержат строку поиска. Оператор, который добавляет <xref:System.Linq.Queryable.Where%2A> предложение выполняется только в том случае, если отсутствует значение для поиска.

> [!NOTE]
> Во многих случаях можно вызвать тот же метод, либо на набор сущностей Entity Framework, либо как метода расширения для коллекции в памяти. Обычно такие же результаты, но в некоторых случаях может отличаться.
>
> Например, реализация .NET Framework `Contains` метод возвращает все строки, передать пустую строку, когда поставщик Entity Framework для SQL Server Compact 4.0 не возвращает строки, наличие пустых строк. Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что вы получите те же результаты для всех версий SQL Server. Кроме того, реализация .NET Framework `Contains` метод по умолчанию выполняет сравнение с учетом регистра, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра. Таким образом, вызов `ToUpper` метод производится проверка явно регистронезависимым гарантирует, что результаты не изменяются при изменении коду позднее использовать хранилище, которое будет возвращать `IEnumerable` , а не `IQueryable` объекта. (При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)
>
> Обработка NULL также может быть разным для разных поставщиков баз данных или при использовании `IQueryable` сравниваемый объект при использовании `IEnumerable` коллекции. Например, в некоторых сценариях `Where` условия, такие как `table.Column != 0` не может возвращать столбцы, имеющие `null` как значение. Дополнительные сведения см. в разделе [неправильной обработкой null переменных в предложение» where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).

### <a name="add-a-search-box-to-the-student-index-view"></a>Добавление поля поиска в представление Student index

1. В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег для создания заголовка, текстового поля и **поиска** кнопку.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Запустить эту страницу, введите строку поиска и нажмите кнопку **поиска** для проверки работы фильтра.

   Обратите внимание на то, что URL-адрес не содержит «» строка поиска, это означает, что если закладку на данной странице, вы не получите отфильтрованного списка при использовании закладки. Это также касается ссылки сортировки столбцов, так как они будут отсортированы всего списка. Вам предстоит изменить **поиска** кнопку, чтобы использовать строки запроса для отбора позже в этом руководстве.

## <a name="add-paging"></a>Добавление разбиения по страницам

Чтобы добавить на страницу индекса учащихся разбиения на страницы, необходимо начать с установки **PagedList.Mvc** пакет NuGet. Затем мы внесем дополнительные изменения в `Index` метод и добавьте ссылки перелистывания, чтобы `Index` представления. **PagedList.Mvc** является одним из многих хороший по страницам и упорядочения пакеты для ASP.NET MVC и его использование здесь предназначен только в качестве примера, не в виде рекомендаций для него другими вариантами.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Установите пакет PagedList.MVC NuGet

NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакет как зависимость. **PagedList** пакет устанавливает `PagedList` коллекции типа и методы расширения для `IQueryable` и `IEnumerable` коллекций. Методы расширения создают одной странице данных в `PagedList` коллекции из вашей `IQueryable` или `IEnumerable`и `PagedList` коллекции предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы. **PagedList.Mvc** пакет устанавливает вспомогательный объект разбиения на страницы, отображающий кнопки перелистывания.

1. Из **средства** меню, выберите **диспетчер пакетов NuGet** и затем **консоль диспетчера пакетов**.

2. В **консоль диспетчера пакетов** окна, убедитесь, что **источник пакета** — **nuget.org** и **проект по умолчанию** — **ContosoUniversity**, а затем введите следующую команду:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Выполните построение проекта.

### <a name="add-paging-functionality-to-the-index-method"></a>Добавление разбиения на страницы в метод Index

1. В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Замените метод `Index` следующим кодом:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Этот код добавляет `page` параметра, параметр текущий порядок сортировки и текущий параметр фильтра в сигнатуру метода:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   При первом отображении страницы, или если пользователь не щелкнет ссылку перелистывания или сортировки, все параметры имеют значение null. При выборе ссылки перелистывания, `page` переменная содержит номер страницы для отображения.

   Объект `ViewBag` свойство обеспечивает представление с помощью текущий порядок сортировки, так как он должен быть включен в ссылки перелистывания, чтобы сохранить порядок сортировки, то же, при разбиении по страницам:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Еще одно свойство, `ViewBag.CurrentFilter`, обеспечивает представление текущую строку фильтра. Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы. Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных. Строка поиска изменяется в том случае, когда значение, введенное в текстовое поле и нажатии кнопки отправки. В этом случае `searchString` параметра не равно null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   В конце метода `ToPagedList` метод расширения для учащихся `IQueryable` объект преобразует результат запроса студентов к одной странице студентов в тип коллекции, который поддерживает разбиение по страницам. Этот страница со студентами затем передается в представление:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   Метод `ToPagedList` принимает номер страницы. Два вопросительных [оператор объединения с null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.

### <a name="add-paging-links-to-the-student-index-view"></a>Добавить ссылки на разбиение на страницы на страницу индекса учащихся

1. В *Views\Student\Index.cshtml*, замените существующий код следующим кодом. Изменения выделены.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.

   `using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательного приложения MVC для кнопки перелистывания.

   Кода используется перегруженная версия [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) , позволяющее указать [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Значение по умолчанию [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) отправляет данные формы с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запроса. При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки. [Руководства консорциума W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) рекомендуем следует использовать GET, когда действие не приводит к обновлению.

   Текстовое поле инициализируется текущую строку поиска, щелкнув новую страницу можно увидеть текущую строку поиска.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Ссылки в заголовках столбцов передают в контроллер при помощи строки запроса текущее значение строки поиска, чтобы пользователь мог сортировать отфильтрованные данные:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Отображаются текущей страницы и общее число страниц.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Если отсутствуют страницы для отображения, отображается «Страница 0 0». (В этом случае номер страницы больше, чем количество страниц из-за `Model.PageNumber` -1, и `Model.PageCount` равно 0.)

   Кнопки перелистывания отображаются по `PagedListPager` вспомогательный:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   `PagedListPager` Вспомогательный предоставляет ряд возможностей, которые можно настроить, включая URL-адреса и оформление. Дополнительные сведения см. в разделе [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) на сайте GitHub.

2. Откройте страницу.

   Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки. Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.

## <a name="create-an-about-page"></a>Создание страницы About

Для университета Contoso веб сайта о странице как отображать количество учащихся поданы заявки на каждую дату регистрации. Для этого понадобится группировка и выполнение простых расчетов в группах. Для выполнения этой задачи нам потребуется следующее:

- Создать класс модели представления для данных, которые необходимо передать в представление.
- Изменить `About` метод в `Home` контроллера.
- Изменить `About` представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *ViewModels* папку в папке проекта. В этой папке добавьте файл класса *EnrollmentDateGroup.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Изменение контроллера Home

1. В *HomeController.cs*, добавьте следующий `using` инструкций в верхней части файла:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Добавьте переменную класса для контекста базы данных сразу после открывающей фигурной скобки для класса:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Замените метод `About` следующим кодом:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   Запрос LINQ группирует записи из таблицы студентов по дате зачисления, вычисляет число записей в каждой группе и сохраняет результаты в коллекцию объектов моделей представления `EnrollmentDateGroup`.

4. Добавление `Dispose` метод:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Изменение представления About

1. Замените код в *Views\Home\About.cshtml* файла следующим кодом:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Запустите приложение и нажмите кнопку **о** ссылку.

   Количество студентов, зачисленных отображает в таблице.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Получение кода

[Скачивание готового проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Дополнительные ресурсы

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Следующие шаги

В этом учебнике рассмотрены следующие задачи.

> [!div class="checklist"]
> * Добавление ссылки сортировки столбцов
> * Добавление поля поиска
> * Добавление разбиения по страницам
> * Создание страницы About

Перейдите к следующей статье, чтобы сведения об использовании перехвата устойчивость и команду подключения.
> [!div class="nextstepaction"]
> [Перехват подключения устойчивость и команды](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)