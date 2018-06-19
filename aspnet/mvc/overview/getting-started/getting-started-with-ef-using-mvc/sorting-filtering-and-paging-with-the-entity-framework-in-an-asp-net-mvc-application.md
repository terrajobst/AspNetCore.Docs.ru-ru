---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Сортировку, фильтрацию и разбиение по страницам с платформой Entity Framework в приложении ASP.NET MVC | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b7d988202966dc0011eeed32cd632c6e0565b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874687"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Сортировку, фильтрацию и разбиение по страницам с платформой Entity Framework в приложении ASP.NET MVC
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике реализован ряд веб-страниц для основные операции CRUD для `Student` сущностей. В этом учебнике вы добавите сортировку, фильтрацию и разбиения по страницам для **учащихся** страницы индекса. Здесь также описывается создание страницы с простой группировкой.

На следующем рисунке изображен вид страницы после выполнения задания. Заголовки столбцов являются ссылками, при нажатии на которые происходит сортировка по этому столбцу. При повторном нажатии на заголовок происходит переключение между сортировкой по возрастанию и убыванию.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Добавление ссылок для сортировки в заголовки столбцов на странице указателя учащихся

Для добавления, страница индекса студента сортировки, мы изменим `Index` метод `Student` контроллера и добавьте код для `Student` Индексируйте представление.

### <a name="add-sorting-functionality-to-the-index-method"></a>Добавление функции сортировки в Index-метод

В *Controllers\StudentController.cs*, замените `Index` метод следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Этот код принимает параметр `sortOrder` из строки запроса в URL. Значение строки запроса, предоставляемые ASP.NET MVC как параметр методу действия. Имя параметра представляет собой строку, состоящую из "Name" или "Date" с возможным добавлением знака подчеркивания и строки "desc" для указания убывающего порядка сортировки. По умолчанию используется порядок сортировки по возрастанию.

При первом запросе страницы Index строка запроса отсутствует. Студенты, отображаются в возрастающем порядке по `LastName`, используемого по умолчанию, установленные с проходом регистром `switch` инструкции. Когда пользователь щелкает гиперссылку заголовка столбца, в строку запроса подставляется соответствующее значение параметра `sortOrder`.

Два `ViewBag` переменные, которые используются, чтобы представление можно настроить гиперссылки заголовок столбца со значениями строки соответствующего запроса:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Это тернарные условные операторы. Указывает, что если первый `sortOrder` параметра равно null или пусто, `ViewBag.NameSortParm` должно быть равно «имя\_desc»; в противном случае задается равным пустой строке. Следующие два оператора устанавливают гиперссылки в заголовках столбцов в представлении следующим образом:

| Текущий порядок сортировки | Гиперссылка "Last Name" (Фамилия) | Гиперссылка "Date" (Дата) |
| --- | --- | --- |
| "Last Name" (Фамилия) по возрастанию | по убыванию | по возрастанию |
| "Last Name" (Фамилия) по убыванию | по возрастанию | по возрастанию |
| "Date" (Дата) по возрастанию | по возрастанию | по убыванию |
| "Date" (Дата) по убыванию | по возрастанию | по возрастанию |

Этот метод использует [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) для указания столбца, по которому выполняется сортировка. Код создает [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) переменной перед `switch` инструкции, изменяет его в `switch` инструкции и вызовы `ToList` метод после `switch` инструкции. После создания и изменения переменных `IQueryable` запрос в базу данных не отправляется. Запрос не выполняется до преобразования `IQueryable` объект в коллекции, такие как вызов метода `ToList`. Таким образом, этот код вызывает один запрос, который не выполняется до `return View` инструкции.

Как Альтернативой написанию другой инструкции LINQ для каждого порядка сортировки можно динамически создать инструкции LINQ. Сведения о динамических LINQ см. в разделе [динамических LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Добавление гиперссылки в представление индекс студента заголовок столбца

В *Views\Student\Index.cshtml*, замените `<tr>` и `<th>` элементы для строки заголовка с выделенным кодом:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Этот код использует эти сведения в `ViewBag` свойства для настройки гиперссылки с соответствующего запроса строковые значения.

Запустите страницу и щелкните **Фамилия** и **Дата регистрации** заголовки столбцов, чтобы проверить, Сортировка работает.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

После нажатия кнопки **Фамилия** заголовком учащихся, отображаются в порядке убывания последнее имя.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Добавить поле поиска на страницу индекса студентов

Для добавления фильтра на страницу указателя учащихся необходимо добавить в представление текстовое поле и кнопку отправки и внести изменения в метод `Index`. Текстовое поле необходимо для ввода строки для поиска в полях имени и фамилии.

### <a name="add-filtering-functionality-to-the-index-method"></a>Добавьте функцию фильтра в метод индекса

В *Controllers\StudentController.cs*, замените `Index` метод (изменения выделены) следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Мы добавили в метод `Index` параметр `searchString`. Значение строки поиска получается из текстового поля, которое мы добавили в представление Index. Вы добавили в инструкции LINQ `where` предложение, которое выбирает только студентов, имя или фамилия содержит строку поиска. Оператор, который добавляет [где](https://msdn.microsoft.com/library/bb535040.aspx) предложение выполняется только в том случае, если значение для поиска.

> [!NOTE]
> Во многих случаях можно вызвать тот же метод в наборе сущностей Entity Framework или как метод расширения для коллекции в памяти. Обычно такие же результаты, но в некоторых случаях могут различаться.
> 
> Например, реализация .NET Framework `Contains` метод возвращает все строки, если передать пустую строку, но поставщик Entity Framework для SQL Server Compact 4.0 возвращает нуль строк, наличие пустых строк. Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что получить те же результаты для всех версий SQL Server. Кроме того, реализация .NET Framework `Contains` метод выполняет сравнение с учетом регистра по умолчанию, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра. Таким образом, вызов `ToUpper` метод производится проверка явно без учета регистра гарантирует, что не изменяются при изменении кода позже в репозитории, которая вернет `IEnumerable` коллекции вместо `IQueryable` объекта. (При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)
> 
> Обработка значений NULL, также может быть разным для поставщиков другую базу данных или при использовании `IQueryable` объекта по сравнению с при использовании `IEnumerable` коллекции. Например, в некоторых сценариях `Where` условия, такие как `table.Column != 0` не может возвращать столбцы, имеющие `null` как значение. Дополнительные сведения см. в разделе [неправильной обработкой null переменных в предложение «where»](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Добавление поля поиска на страницу индекса учащихся

В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег, чтобы создать текстовое поле заголовка и **поиска** кнопку.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Откройте страницу, введите строку поиска и нажмите кнопку **поиска** для проверки корректности фильтрации.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Обратите внимание, что URL-адрес не содержит «» строка поиска, это означает, что если закладку для этой страницы вы не сможете получить отфильтрованный список при использовании закладки. Это применяется также к ссылкам сортировки столбца, как они будут отсортированы весь список. Мы изменим **поиска** кнопку, чтобы использовать в критериях фильтра строки запроса, далее в этом учебнике.

## <a name="add-paging-to-the-students-index-page"></a>Добавить разбиение на страницы на страницу индекса студентов

Добавление страницы индекса учащихся разбиения на страницы, вы начнете, установив **PagedList.Mvc** пакет NuGet. Затем будет вносить дополнительные изменения в `Index` метод и добавьте ссылки подкачки `Index` представления. **PagedList.Mvc** является одним из многих хорошо постраничного просмотра и сортировки пакетов для ASP.NET MVC и его использование здесь предназначено исключительно в качестве примера, не в качестве рекомендации по изменению его через другие параметры. На следующем рисунке ссылки разбиения на страницы.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Установка пакета PagedList.MVC NuGet

NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакета в качестве зависимости. **PagedList** пакет устанавливает `PagedList` коллекцию типов и расширения методы для `IQueryable` и `IEnumerable` коллекции. Методы расширения создают одной страницы данных в `PagedList` коллекции из вашего `IQueryable` или `IEnumerable`и `PagedList` коллекция предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы. **PagedList.Mvc** пакет устанавливает подкачки вспомогательный класс, отображающий кнопки разбиения по страницам.

Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

В **консоль диспетчера пакетов** окна, убедитесь, что **источник пакета** — **nuget.org** и **проекта по умолчанию** — **ContosoUniversity**, а затем введите следующую команду:

`Install-Package PagedList.Mvc`

![Установить PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Выполните построение проекта. 

### <a name="add-paging-functionality-to-the-index-method"></a>Добавьте в метод индекс разбиения по страницам

В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Замените метод `Index` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Этот код добавляет `page` параметра, текущий параметр порядка сортировки и текущий параметр фильтра к сигнатуре метода:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

При первом отображении страницы или если пользователь еще не нажимал на ссылки сортировки и перелистывания, все параметры будут иметь значение null. Если щелкнуть ссылку подкачки `page` переменная будет содержать номер страницы для отображения.

Объект `ViewBag` свойство предоставляет представление определения порядка сортировки, так как это должны быть включены в ссылки разбиения на страницы для сохранит порядок сортировки во время разбиения на страницы:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Другое свойство `ViewBag.CurrentFilter`, обеспечивает представление с текущей строки фильтра. Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы. Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных. Строка поиска, которая изменяется при нажатии кнопки "Отправить", если ввести значение в текстовом поле. В этом случае `searchString` параметра не равно null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

В конце метода `ToPagedList` метод расширения в слушателей `IQueryable` объект преобразует запрос студента на одной странице учеников в тип коллекции, который поддерживает разбиение по страницам. Страницы учащихся затем передается в представление:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Метод `ToPagedList` принимает номер страницы. Два знака вопроса указывают [оператор объединения с null](https://msdn.microsoft.com/library/ms173224.aspx). Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.

### <a name="add-paging-links-to-the-student-index-view"></a>Добавить разбиение на страницы ссылки на представление Index студента

В *Views\Student\Index.cshtml*, замените существующий код следующим кодом. Изменения выделены.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.

`using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательным методом MVC для кнопки разбиения по страницам.

В коде используется перегруженная версия [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , позволяющее указать [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Значение по умолчанию [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) отправки данных с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запросов. При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки. [Рекомендации W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) рекомендуется, если действие не приводит к появлению обновления следует использовать GET.

При нажатии кнопки новую страницу для отображения текущей искомой строки, текстовое поле инициализируется текущей искомой строки.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Ссылки в заголовках столбцов передают в контроллер при помощи строки запроса текущее значение строки поиска, чтобы пользователь мог сортировать отфильтрованные данные:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Отображаются текущей страницы и общее число страниц.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Если отсутствуют страницы для отображения, отображается «Страница 0 0». (В этом случае номер страницы больше, чем количество страниц из-за `Model.PageNumber` -1, и `Model.PageCount` — 0.)

Отображаются кнопки разбиения на страницы с `PagedListPager` поддержки:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Вспомогательные предоставляет ряд возможностей, которые можно настраивать, включая URL-адреса и оформление. Дополнительные сведения см. в разделе [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) на сайте GitHub.

Запустите страницу.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки. Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Создание о страницы, отображающей студента статистики

Для Contoso университета веб-сайта о странице как отображать количество студентов регистрации для каждой даты регистрации. Для этого понадобится группировка и выполнение простых расчетов в группах. Для выполнения этой задачи нам потребуется следующее:

- Создать класс модели представления для данных, которые необходимо передать в представление.
- Изменить `About` метод в `Home` контроллера.
- Изменить `About` представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *ViewModels* папку в папке проекта. В этой папке, добавьте файл класса *EnrollmentDateGroup.cs* и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Изменение контроллера Home

В *HomeController.cs*, добавьте следующий `using` инструкции в верхней части файла:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Добавьте переменную класса для контекста базы данных сразу же после открывающей фигурной скобки для класса:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Замените метод `About` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Запрос LINQ группирует записи из таблицы студентов по дате зачисления, вычисляет число записей в каждой группе и сохраняет результаты в коллекцию объектов моделей представления `EnrollmentDateGroup`.

Добавить `Dispose` метод:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Изменение представления About

Замените код в *Views\Home\About.cshtml* файла следующим кодом:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Запустите приложение и нажмите кнопку **о** ссылку. Количество зачисленных студентов по дням отображается в таблице.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Сводка

В этом учебнике вы знаете, как создать модель данных и реализации основных CRUD, сортировку, фильтрацию, разбиение по страницам и группировку функциональности. В следующем уроке вы начнете, просмотрев более сложные вопросы, развернув модели данных.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Вперед](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
