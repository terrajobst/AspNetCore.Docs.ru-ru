---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Сортировка, фильтрация и разбиение по страницам с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: aspnetcontent
ms.date: 06/01/2015
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 231dd6bc9892d855a12289fde681d3f210494af9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839717"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Сортировка, фильтрация и разбиение по страницам с Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебном курсе вы реализовали набор веб-страниц для основных операций CRUD для `Student` сущностей. В этом руководстве вы добавите сортировки, фильтрации и разбиения по страницам для **учащихся** страницы индекса. Здесь также описывается создание страницы с простой группировкой.

На следующем рисунке изображен вид страницы после выполнения задания. Заголовки столбцов являются ссылками, при нажатии на которые происходит сортировка по этому столбцу. При повторном нажатии на заголовок происходит переключение между сортировкой по возрастанию и убыванию.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Добавление ссылок для сортировки в заголовки столбцов на странице указателя учащихся

Добавление сортировки на страницу указателя учащихся изменим `Index` метод `Student` контроллера и добавьте код в `Student` Индексируйте представление.

### <a name="add-sorting-functionality-to-the-index-method"></a>Добавление сортировки в метод Index

В *Controllers\StudentController.cs*, замените `Index` метод следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Этот код принимает параметр `sortOrder` из строки запроса в URL. Значение строки запроса, предоставляемые ASP.NET MVC в качестве параметра метода действия. Имя параметра представляет собой строку, состоящую из "Name" или "Date" с возможным добавлением знака подчеркивания и строки "desc" для указания убывающего порядка сортировки. По умолчанию используется порядок сортировки по возрастанию.

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

Данный метод использует [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) позволяет выбрать столбец для сортировки. Код создает [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) переменной перед `switch` инструкция, изменяет его в `switch` инструкции и вызовы `ToList` метод после `switch` инструкции. После создания и изменения переменных `IQueryable` запрос в базу данных не отправляется. Запрос не выполняется, пока вы не преобразуете `IQueryable` объект в коллекцию путем вызова метода, например `ToList`. Таким образом, этот код вызывает в одном запросе, не выполняется до `return View` инструкции.

Вместо написания различных операторов LINQ для каждого порядка сортировки можно динамически создать оператор LINQ. Сведения о динамических LINQ см. в разделе [динамического LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Добавление гиперссылки на страницу индекса учащихся заголовок столбца

В *Views\Student\Index.cshtml*, замените `<tr>` и `<th>` элементов для строки заголовков с выделенный код:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Этот код использует эти сведения в `ViewBag` свойства для настройки гиперссылок с использованием соответствующего запроса строковые значения.

Откройте страницу и щелкните **Фамилия** и **Дата регистрации** заголовки столбцов, чтобы убедитесь, что Сортировка работает.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

После того как вы щелкнете **Фамилия** заголовок, учащиеся отображаются по убыванию последнее имя.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Добавление поля поиска на страницу индекса учащихся

Для добавления фильтра на страницу указателя учащихся необходимо добавить в представление текстовое поле и кнопку отправки и внести изменения в метод `Index`. Текстовое поле необходимо для ввода строки для поиска в полях имени и фамилии.

### <a name="add-filtering-functionality-to-the-index-method"></a>Добавление функций фильтрации в метод Index

В *Controllers\StudentController.cs*, замените `Index` метод (изменения выделены) следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Мы добавили в метод `Index` параметр `searchString`. Значение строки поиска получается из текстового поля, которое мы добавили в представление Index. Мы также добавили в оператор LINQ `where` предложение, которое отбирает только студентов, чье имя или Фамилия содержат строку поиска. Оператор, который добавляет [где](https://msdn.microsoft.com/library/bb535040.aspx) предложение выполняется только в том случае, если отсутствует значение для поиска.

> [!NOTE]
> Во многих случаях можно вызвать тот же метод, либо на набор сущностей Entity Framework, либо как метода расширения для коллекции в памяти. Обычно такие же результаты, но в некоторых случаях может отличаться.
> 
> Например, реализация .NET Framework `Contains` метод возвращает все строки, передать пустую строку, когда поставщик Entity Framework для SQL Server Compact 4.0 не возвращает строки, наличие пустых строк. Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что вы получите те же результаты для всех версий SQL Server. Кроме того, реализация .NET Framework `Contains` метод по умолчанию выполняет сравнение с учетом регистра, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра. Таким образом, вызов `ToUpper` метод производится проверка явно регистронезависимым гарантирует, что результаты не изменяются при изменении коду позднее использовать хранилище, которое будет возвращать `IEnumerable` , а не `IQueryable` объекта. (При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)
> 
> Обработка NULL также может быть разным для разных поставщиков баз данных или при использовании `IQueryable` сравниваемый объект при использовании `IEnumerable` коллекции. Например, в некоторых сценариях `Where` условия, такие как `table.Column != 0` не может возвращать столбцы, имеющие `null` как значение. Дополнительные сведения см. в разделе [неправильной обработкой null переменных в предложение» where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Добавление поля поиска на страницу индекса учащихся

В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег для создания заголовка, текстового поля и **поиска** кнопку.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Запустить эту страницу, введите строку поиска и нажмите кнопку **поиска** для проверки работы фильтра.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Обратите внимание на то, что URL-адрес не содержит «» строка поиска, это означает, что если закладку на данной странице, вы не получите отфильтрованного списка при использовании закладки. Это также касается ссылки сортировки столбцов, так как они будут отсортированы всего списка. Вам предстоит изменить **поиска** кнопку, чтобы использовать строки запроса для отбора позже в этом руководстве.

## <a name="add-paging-to-the-students-index-page"></a>Добавление разбиения на страницы на страницу индекса учащихся

Чтобы добавить на страницу указателя учащихся разбиения на страницы, необходимо начать с установки **PagedList.Mvc** пакет NuGet. Затем мы внесем дополнительные изменения в `Index` метод и добавьте ссылки перелистывания, чтобы `Index` представления. **PagedList.Mvc** является одним из многих хороший по страницам и упорядочения пакеты для ASP.NET MVC и его использование здесь предназначен только в качестве примера, не в виде рекомендаций для него другими вариантами. Ниже показан перелистывания.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Установите пакет PagedList.MVC NuGet

NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакет как зависимость. **PagedList** пакет устанавливает `PagedList` коллекции типа и методы расширения для `IQueryable` и `IEnumerable` коллекций. Методы расширения создают одной странице данных в `PagedList` коллекции из вашей `IQueryable` или `IEnumerable`и `PagedList` коллекции предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы. **PagedList.Mvc** пакет устанавливает вспомогательный объект разбиения на страницы, отображающий кнопки перелистывания.

Из **средства** меню, выберите **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

В **консоль диспетчера пакетов** окна, убедитесь, что **источник пакета** — **nuget.org** и **проект по умолчанию** — **ContosoUniversity**, а затем введите следующую команду:

`Install-Package PagedList.Mvc`

![Установка PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Выполните построение проекта. 

### <a name="add-paging-functionality-to-the-index-method"></a>Добавление разбиения по страницам в метод Index

В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Замените метод `Index` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Этот код добавляет `page` параметра, параметр текущий порядок сортировки и текущий параметр фильтра в сигнатуру метода:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

При первом отображении страницы или если пользователь еще не нажимал на ссылки сортировки и перелистывания, все параметры будут иметь значение null. При выборе ссылки перелистывания, `page` переменная будет содержать номер страницы для отображения.

Объект `ViewBag` свойство обеспечивает представление с помощью текущий порядок сортировки, так как он должен быть включен в ссылки перелистывания, чтобы сохранить порядок сортировки, то же, при разбиении по страницам:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Еще одно свойство, `ViewBag.CurrentFilter`, обеспечивает представление текущую строку фильтра. Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы. Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных. Строка поиска изменяется в том случае, когда значение, введенное в текстовое поле и нажатии кнопки отправки. В этом случае `searchString` параметра не равно null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

В конце метода `ToPagedList` метод расширения для учащихся `IQueryable` объект преобразует результат запроса студентов к одной странице студентов в тип коллекции, который поддерживает разбиение по страницам. Этот страница со студентами затем передается в представление:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Метод `ToPagedList` принимает номер страницы. Два вопросительных [оператор объединения с null](https://msdn.microsoft.com/library/ms173224.aspx). Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.

### <a name="add-paging-links-to-the-student-index-view"></a>Добавить ссылки на разбиение на страницы на страницу индекса учащихся

В *Views\Student\Index.cshtml*, замените существующий код следующим кодом. Изменения выделены.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.

`using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательного приложения MVC для кнопки перелистывания.

Кода используется перегруженная версия [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , позволяющее указать [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Значение по умолчанию [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) отправляет данные формы с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запроса. При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки. [Руководства консорциума W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) рекомендуем следует использовать GET, когда действие не приводит к обновлению.

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

Откройте страницу.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки. Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Создать о странице, со статистикой студентов

Для университета Contoso веб сайта о странице как отображать количество учащихся поданы заявки на каждую дату регистрации. Для этого понадобится группировка и выполнение простых расчетов в группах. Для выполнения этой задачи нам потребуется следующее:

- Создать класс модели представления для данных, которые необходимо передать в представление.
- Изменить `About` метод в `Home` контроллера.
- Изменить `About` представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *ViewModels* папку в папке проекта. В этой папке добавьте файл класса *EnrollmentDateGroup.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Изменение контроллера Home

В *HomeController.cs*, добавьте следующий `using` инструкций в верхней части файла:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Добавьте переменную класса для контекста базы данных сразу после открывающей фигурной скобки для класса:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Замените метод `About` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Запрос LINQ группирует записи из таблицы студентов по дате зачисления, вычисляет число записей в каждой группе и сохраняет результаты в коллекцию объектов моделей представления `EnrollmentDateGroup`.

Добавление `Dispose` метод:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Изменение представления About

Замените код в *Views\Home\About.cshtml* файла следующим кодом:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Запустите приложение и нажмите кнопку **о** ссылку. Количество зачисленных студентов по дням отображается в таблице.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Сводка

В этом руководстве вы узнали, как создать модель данных и реализации основных CRUD, сортировка, фильтрация, разбиение по страницам и группировка. В следующем учебном курсе вы начнете рассматривать более сложными аспектами, развернув модель данных.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Вперед](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
