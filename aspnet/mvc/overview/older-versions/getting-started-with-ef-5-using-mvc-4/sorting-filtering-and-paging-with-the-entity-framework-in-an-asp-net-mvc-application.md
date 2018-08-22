---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Сортировка, фильтрация и разбиение по страницам с Entity Framework в приложении ASP.NET MVC (3 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 72180686ea1ee6f6695b24fcedf3af90c1eddcfa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829662"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Сортировка, фильтрация и разбиение по страницам с Entity Framework в приложении ASP.NET MVC (3 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Мы добавили в метод `Index` параметр `searchString`. Мы также добавили в оператор LINQ `where` clausethat отбирает только студентов, чье имя или Фамилия содержат строку поиска. Значение строки поиска получается из текстового поля, который вам предстоит добавить в представление Index. Оператор, который добавляет [где](https://msdn.microsoft.com/library/bb535040.aspx) предложение выполняется только в том случае, если отсутствует значение для поиска.

> [!NOTE]
> Во многих случаях можно вызвать тот же метод, либо на набор сущностей Entity Framework, либо как метода расширения для коллекции в памяти. Обычно такие же результаты, но в некоторых случаях может отличаться. Например, реализация .NET Framework `Contains` метод возвращает все строки, передать пустую строку, когда поставщик Entity Framework для SQL Server Compact 4.0 не возвращает строки, наличие пустых строк. Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что вы получите те же результаты для всех версий SQL Server. Кроме того, реализация .NET Framework `Contains` метод по умолчанию выполняет сравнение с учетом регистра, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра. Таким образом, вызов `ToUpper` метод производится проверка явно регистронезависимым гарантирует, что результаты не изменяются при изменении коду позднее использовать хранилище, которое будет возвращать `IEnumerable` , а не `IQueryable` объекта. (При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Добавление поля поиска на страницу индекса учащихся

В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег для создания заголовка, текстового поля и **поиска** кнопку.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Запустить эту страницу, введите строку поиска и нажмите кнопку **поиска** для проверки работы фильтра.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Обратите внимание на то, что URL-адрес не содержит «» строка поиска, это означает, что если закладку на данной странице, вы не получите отфильтрованного списка при использовании закладки. Вам предстоит изменить **поиска** кнопку, чтобы использовать строки запроса для отбора позже в этом руководстве.

## <a name="add-paging-to-the-students-index-page"></a>Добавление разбиения на страницы на страницу индекса учащихся

Чтобы добавить на страницу указателя учащихся разбиения на страницы, необходимо начать с установки **PagedList.Mvc** пакет NuGet. Затем мы внесем дополнительные изменения в `Index` метод и добавьте ссылки перелистывания, чтобы `Index` представления. **PagedList.Mvc** является одним из многих хороший по страницам и упорядочения пакеты для ASP.NET MVC и его использование здесь предназначен только в качестве примера, не в виде рекомендаций для него другими вариантами. Ниже показан перелистывания.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Установите пакет PagedList.MVC NuGet

NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакет как зависимость. **PagedList** пакет устанавливает `PagedList` коллекции типа и методы расширения для `IQueryable` и `IEnumerable` коллекций. Методы расширения создают одной странице данных в `PagedList` коллекции из вашей `IQueryable` или `IEnumerable`и `PagedList` коллекции предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы. **PagedList.Mvc** пакет устанавливает вспомогательный объект разбиения на страницы, отображающий кнопки перелистывания.

Из **средства** меню, выберите **диспетчер пакетов библиотеки** и затем **управление пакетами NuGet для решения**.

В **управление пакетами NuGet** диалоговом окне щелкните **Online** вкладке слева и введите в поле поиска «, разбитых на страницы». Когда появится **PagedList.Mvc** пакет, нажмите кнопку **установить**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

В **выберите проекты** выберите **ОК**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Добавление разбиения по страницам в метод Index

В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Замените метод `Index` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Этот код добавляет `page` параметра, параметр текущий порядок сортировки и текущий параметр фильтра в сигнатуру метода, как показано ниже:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

При первом отображении страницы или если пользователь еще не нажимал на ссылки сортировки и перелистывания, все параметры будут иметь значение null. При выборе ссылки перелистывания, `page` переменная будет содержать номер страницы для отображения.

`A ViewBag` свойство обеспечивает представление с помощью текущий порядок сортировки, так как он должен быть включен в ссылки перелистывания, чтобы сохранить порядок сортировки, то же, при разбиении по страницам:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Еще одно свойство, `ViewBag.CurrentFilter`, обеспечивает представление текущую строку фильтра. Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы. Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных. Строка поиска изменяется в том случае, когда значение, введенное в текстовое поле и нажатии кнопки отправки. В этом случае `searchString` параметра не равно null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

В конце метода `ToPagedList` метод расширения для учащихся `IQueryable` объект преобразует результат запроса студентов к одной странице студентов в тип коллекции, который поддерживает разбиение по страницам. Этот страница со студентами затем передается в представление:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Метод `ToPagedList` принимает номер страницы. Два вопросительных [оператор объединения с null](https://msdn.microsoft.com/library/ms173224.aspx). Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.

### <a name="add-paging-links-to-the-student-index-view"></a>Добавить ссылки на разбиение на страницы на страницу индекса учащихся

В *Views\Student\Index.cshtml*, замените существующий код следующим кодом:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.

`using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательного приложения MVC для кнопки перелистывания.

Кода используется перегруженная версия [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , позволяющее указать [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Значение по умолчанию [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) отправляет данные формы с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запроса. При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки. [Руководства консорциума W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) указать, что следует использовать GET, когда действие не приводит к обновлению.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки. Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Создать о странице, со статистикой студентов

Для университета Contoso веб сайта о странице как отображать количество учащихся поданы заявки на каждую дату регистрации. Для этого понадобится группировка и выполнение простых расчетов в группах. Для выполнения этой задачи нам потребуется следующее:

- Создать класс модели представления для данных, которые необходимо передать в представление.
- Изменить `About` метод в `Home` контроллера.
- Изменить `About` представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *ViewModels* папки. В этой папке добавьте файл класса *EnrollmentDateGroup.cs* и замените существующий код следующим кодом:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Необязательно: Развертывание приложения в Windows Azure

До сих приложения выполнялся локально в IIS Express на компьютере разработчика. Чтобы сделать его доступным для других пользователей в Интернете, необходимо развернуть его на веб-поставщик услуг размещения. В этом дополнительном разделе этого руководства вы развернете его в веб-сайта Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>С помощью Code First Migrations для развертывания базы данных

Развертывание базы данных будет использовать Code First Migrations. При создании профиля публикации, который можно использовать для настройки параметров для развертывания из Visual Studio, будет предложено выбрать типа "флажок" с меткой **выполнить Code First Migrations (при запуске приложения)**. Этот параметр задан, то процесс развертывания, чтобы автоматически настроить приложение *Web.config* файл на конечном сервере, таким образом, Code First использовал `MigrateDatabaseToLatestVersion` инициализатор класса.

Visual Studio не выполняет никаких действий с базой данных во время развертывания. Когда развернутое приложение обращается к базе данных в первый раз после развертывания, Code First автоматически создает базу данных или обновляет схему базы данных до последней версии. Если приложение реализует миграций `Seed` метод выполнения метода, после создания или обновления схемы.

Миграции `Seed` метод вставляет тестовых данных. При развертывании в рабочей среде, вы должны внести изменения `Seed` метод, чтобы он вставляет только те данные, которые должны быть вставлены в рабочую базу данных. Например в текущей модели данных может потребоваться иметь реальных курсы, но вымышленной учащихся в базе данных разработки. Можно написать `Seed` метод для загрузки в разработке и затем закомментируйте вымышленной учащихся, перед развертыванием в рабочей среде. Или можно написать `Seed` метод для загрузки только курсов и введите вымышленной учащихся в базе данных тестов вручную с помощью пользовательского интерфейса приложения.

### <a name="get-a-windows-azure-account"></a>Получите учетную запись Windows Azure

Потребуется учетная запись Windows Azure. Если ее еще нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатное пробное использование Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Создание веб-сайта и базы данных SQL в Windows Azure

Веб-сайт Windows Azure будет выполняться в общей среде размещения, это означает, что он работает на виртуальных машинах (ВМ), которые являются общими с другими клиентами Windows Azure. Общей среде размещения — это недорогой способ начать работу в облаке. Позже Если ваш веб-трафик увеличится, приложение можно масштабировать для удовлетворения потребностей, выполнив на выделенных виртуальных машинах. Если вам требуется более сложная архитектура, можно перенести в Windows Azure облачную службу. Облачные службы работают на выделенных виртуальных машинах, которые можно настроить в соответствии с потребностями.

База данных SQL Windows Azure — это служба реляционной базы данных на основе облака, которая основана на технологии SQL Server. Средства и приложения, работающие с SQL Server также работают с базой данных SQL.

1. В [портала управления Windows Azure](https://manage.windowsazure.com/), нажмите кнопку **веб-сайтов** в левой вкладке, а затем выберите **New**.

    ![Кнопка "Создать" на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Нажмите кнопку **НАСТРАИВАЕМОЕ создание**.

    ![Создание со ссылкой на базу данных на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Новый веб-сайт — настраиваемое создание** откроется мастер.
3. В **новый веб-сайт** шаг мастера введите строку в **URL-адрес** поле для использования в качестве уникального URL-адреса для вашего приложения. Полный URL-адрес будет состоять из введенной здесь, а также суффикса, который отображается рядом с текстовым полем. На рисунке «ConU», но этот URL-адрес может быть занят, чтобы вы могли выбрать его.

    ![Создание со ссылкой на базу данных на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. В **регион** раскрывающемся списке выберите регион, ближайший к вам. Этот параметр указывает, какой веб-сайт будет выполняться в центр обработки данных.
5. В **базы данных** раскрывающегося списка выберите **создать бесплатную базу данных 20 МБ SQL**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. В **имя строки подключения DB**, введите *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Щелкните стрелку, указывающую справа в нижней части окна. В мастере открывается **параметры базы данных** шаг.
8. В **имя** введите *ContosoUniversityDB*.
9. В **Server** выберите **новую базу данных SQL server**. Кроме того Если ранее вы создали сервер, можно выбрать этот сервер в раскрывающемся списке.
10. Введите администратора **имя входа** и **пароль**. Если вы выбрали **новую базу данных SQL server** не используйте имеющиеся имя и пароль, введите новое имя и пароль, который вы определяете теперь для последующего использования при доступе к базе данных. Если вы выбрали сервера, который вы создали ранее, вы введете учетные данные для этого сервера. В этом учебнике не будет выбран ***Дополнительно*** "флажок". ***Дополнительно*** параметры позволяют задать базе [параметры сортировки](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Выберите ту же **регион** , выбранный для веб-сайта.
12. Щелкните флажок внизу справа от поля, чтобы указать, что Вы завершили.   
  
    ![Шаг настройки базы данных из нового веб-сайта - создание с помощью мастера баз данных](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    На следующем рисунке, с помощью существующего SQL Server и имя входа.   
  
    ![Шаг настройки базы данных из нового веб-сайта - создание с помощью мастера баз данных](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Портал управления возвращает на страницу веб-сайтов и **состояние** столбец показывает, что создается сайт. Через некоторое время (обычно это занимает меньше минуты) **состояние** столбец показывает, что узел был успешно создан. На панели навигации слева, число узлов, у вас есть в вашей учетной записи отображается рядом с полем **веб-сайтов** значок, а также количество баз данных отображается рядом с полем **баз данных SQL** значок.

## <a name="deploy-the-application-to-windows-azure"></a>Развертывание приложения в Windows Azure

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.  
  
    ![Публикация в контекстном меню проекта](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. В **профиль** вкладке **веб-публикации** мастера, нажмите кнопку **импорта**.  
  
    ![Импорт параметров публикации](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Если вы не добавили ранее подписки Windows Azure в Visual Studio, выполните следующие действия. В этом пошаговом руководстве, чтобы в раскрывающемся списке, добавьте подписку **Импорт из веб-сайта Windows Azure** будет включать веб-сайт.

    1. В **Импорт профиля публикации** диалоговом окне щелкните **Импорт из веб-сайта Windows Azure**, а затем нажмите кнопку **подписки Windows Azure, добавьте**.

    ![Добавление подписки Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    2. В **Импорт подписок Azure Windows** диалоговом окне щелкните **загрузки файла подписки**.

    ![Загрузка файла подписки](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    В. В окне обозревателя, Сохранить *.publishsettings* файла.

    ![скачать PUBLISHSETTINGS-файла](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Безопасность — *publishsettings* файл содержит ваши учетные данные (Незакодированные), которые используются для администрирования подписки Windows Azure и служб. Безопасность для этого файла рекомендуется временно хранить его за пределами исходных каталогов (например в *Libraries\Documents* папку) и затем удалять после завершения импорта. Пользователь-злоумышленник, получивший доступ к `.publishsettings` файла можно изменить, создавать и удалять ваши службы Windows Azure.

    Г. В **Импорт подписок Azure Windows** диалоговом окне щелкните **Обзор** и перейдите к *.publishsettings* файла.

    ![Скачайте sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    Д. Нажмите кнопку **Импортировать**.

    ![импорт](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. В **Импорт профиля публикации** выберите **Импорт из веб-сайта Windows Azure**, выберите веб-сайт из раскрывающегося списка и нажмите кнопку **ОК**.  
  
    ![Импорт профиля публикации](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. В **подключения** щелкните **проверить подключение** чтобы убедиться в том, что параметры заданы правильно.  
  
    ![Проверка подключения](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. После проверки подключения зеленая галочка отображается рядом с полем **проверить подключение** кнопки. Нажмите кнопку **Далее**.  
  
    ![Успешно проверенные подключения](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Откройте **строку подключения к удаленному** стрелку раскрывающегося списка в разделе **SchoolContext** и выберите строку подключения для базы данных, вы создали.
8. Выберите **выполнить Code First Migrations (при запуске приложения)**.
9. Снимите флажок **использовать эту строку подключения во время выполнения** для **UserContext (DefaultConnection)**, так как это приложение не использует базу данных членства.   
  
    ![Вкладка "Параметры"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Нажмите кнопку **Далее**.
11. В **предварительной версии** щелкните **начать просмотр**.  
  
    ![Кнопка StartPreview вкладке предварительного просмотра](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    На вкладке отображается список файлов, которые будут скопированы на сервер. В процессе предварительного просмотра не требуется, чтобы опубликовать приложение, но является полезные функции, которые следует учитывать. В этом случае не нужно ничего делать со списком файлов, который отображается. При развертывании этого приложения в этом списке будет только файлы, которые были изменены.  
  
    ![Выходной файл StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Нажмите кнопку **Опубликовать**.  
    Visual Studio начнет процесс копирования файлов на сервер Windows Azure.
13. **Вывода** окно показывает, какие действия по развертыванию были выполнены и передает об успешном завершении развертывания.  
  
    ![Окно вывода, отчетом об успешном развертывании](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. После успешного развертывания в браузере по умолчанию автоматически откроется на URL-адрес развернутого веб-сайта.  
    Созданное приложение теперь работает в облаке. Перейдите на вкладку учащихся.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

На этом этапе ваш *SchoolContext* базы данных в базе данных SQL Windows Azure создан, так как вы выбрали **выполнить Code First Migrations (при запуске приложения)**. *Web.config* файл развернутого веб-узла был изменен, чтобы [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) инициализатор будут выполнены при первом ваш код считывает или записывает данные в базе данных (который произошло после выбора **учащихся** вкладки):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Процесс развертывания также создается новая строка подключения *(SchoolContext\_DatabasePublish*) для Code First Migrations для обновления схемы базы данных и заполнения базы данных.

![Строка подключения Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* является строка подключения для базы данных членства (который мы не используем в этом руководстве). *SchoolContext* является строка подключения для базы данных ContosoUniversity.

Вы найдете развернутой версии файла Web.config на локальном компьютере в *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Доступ к развернутому *Web.config* сам файл с помощью FTP. Инструкции см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Следуйте инструкциям, которые начинаются с «Чтобы использовать средство FTP, необходимы три вещи: URL-адрес FTP, имя пользователя и пароль.»

> [!NOTE]
> Веб-приложение не реализует безопасности, поэтому любой кто найдет этот URL-адрес можно изменить данные. Инструкции о том, как защитить веб-сайт, см. в разделе [развертывание безопасного приложения ASP.NET MVC с членством, OAuth и базой данных SQL для веб-сайта Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Можно запретить другим пользователям с помощью сайта с помощью портала управления Windows Azure или **обозревателя серверов** в Visual Studio, чтобы остановить сайт.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Инициализаторы первого кода

В разделе развертывания вы видели [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) используется инициализатор. Код сначала также предоставляет другие инициализаторы, которые можно использовать, включая [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (по умолчанию), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) и [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Инициализаторов может быть полезна для настройки условий для модульных тестов. Можно также написать собственные инициализаторы и инициализатор можно вызвать явным образом, если вы не хотите подождать, пока приложение для считывания и записи в базу данных. Подробное объяснение инициализаторов, содержатся в главе 6 книги [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) , Джули Лерман и Роуэн Миллер.

## <a name="summary"></a>Сводка

В этом руководстве вы узнали, как создать модель данных и реализации основных CRUD, сортировка, фильтрация, разбиение по страницам и группировка. В следующем учебном курсе вы начнете рассматривать более сложными аспектами, развернув модель данных.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Вперед](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
