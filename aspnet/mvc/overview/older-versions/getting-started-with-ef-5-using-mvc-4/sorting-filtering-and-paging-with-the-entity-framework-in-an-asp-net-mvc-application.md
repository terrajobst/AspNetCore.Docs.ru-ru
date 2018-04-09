---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Сортировку, фильтрацию и разбиение по страницам с платформой Entity Framework в приложении ASP.NET MVC (3 из 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Сортировку, фильтрацию и разбиение по страницам с платформой Entity Framework в приложении ASP.NET MVC (3 из 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Мы добавили в метод `Index` параметр `searchString`. Вы добавили в инструкции LINQ `where` clausethat выбирает только учащихся, имя или фамилия содержит строку поиска. Строковое значение поиска получается из текстового поля, который вам предстоит добавить в представление индекс. Оператор, который добавляет [где](https://msdn.microsoft.com/library/bb535040.aspx) предложение выполняется только в том случае, если значение для поиска.

> [!NOTE]
> Во многих случаях можно вызвать тот же метод в наборе сущностей Entity Framework или как метод расширения для коллекции в памяти. Обычно такие же результаты, но в некоторых случаях могут различаться. Например, реализация .NET Framework `Contains` метод возвращает все строки, если передать пустую строку, но поставщик Entity Framework для SQL Server Compact 4.0 возвращает нуль строк, наличие пустых строк. Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что получить те же результаты для всех версий SQL Server. Кроме того, реализация .NET Framework `Contains` метод выполняет сравнение с учетом регистра по умолчанию, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра. Таким образом, вызов `ToUpper` метод производится проверка явно без учета регистра гарантирует, что не изменяются при изменении кода позже в репозитории, которая вернет `IEnumerable` коллекции вместо `IQueryable` объекта. (При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Добавление поля поиска на страницу индекса учащихся

В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег, чтобы создать текстовое поле заголовка и **поиска** кнопку.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Откройте страницу, введите строку поиска и нажмите кнопку **поиска** для проверки корректности фильтрации.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Обратите внимание, что URL-адрес не содержит «» строка поиска, это означает, что если закладку для этой страницы вы не сможете получить отфильтрованный список при использовании закладки. Мы изменим **поиска** кнопку, чтобы использовать в критериях фильтра строки запроса, далее в этом учебнике.

## <a name="add-paging-to-the-students-index-page"></a>Добавить разбиение на страницы на страницу индекса студентов

Добавление страницы индекса учащихся разбиения на страницы, вы начнете, установив **PagedList.Mvc** пакет NuGet. Затем будет вносить дополнительные изменения в `Index` метод и добавьте ссылки подкачки `Index` представления. **PagedList.Mvc** является одним из многих хорошо постраничного просмотра и сортировки пакетов для ASP.NET MVC и его использование здесь предназначено исключительно в качестве примера, не в качестве рекомендации по изменению его через другие параметры. На следующем рисунке ссылки разбиения на страницы.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Установка пакета PagedList.MVC NuGet

NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакета в качестве зависимости. **PagedList** пакет устанавливает `PagedList` коллекцию типов и расширения методы для `IQueryable` и `IEnumerable` коллекции. Методы расширения создают одной страницы данных в `PagedList` коллекции из вашего `IQueryable` или `IEnumerable`и `PagedList` коллекция предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы. **PagedList.Mvc** пакет устанавливает подкачки вспомогательный класс, отображающий кнопки разбиения по страницам.

Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки** и затем **управление пакетами NuGet для решения**.

В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **Online** слева, а затем введите «выгружаемого» в поле поиска. При появлении **PagedList.Mvc** пакет, нажмите кнопку **установить**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

В **выберите проекты** щелкните **ОК**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Добавьте в метод индекс разбиения по страницам

В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Замените метод `Index` следующим кодом:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Этот код добавляет `page` параметра, текущий параметр порядка сортировки и текущий параметр фильтра к сигнатуре метода, как показано ниже:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

При первом отображении страницы или если пользователь еще не нажимал на ссылки сортировки и перелистывания, все параметры будут иметь значение null. Если щелкнуть ссылку подкачки `page` переменная будет содержать номер страницы для отображения.

`A ViewBag` свойство предоставляет представление определения порядка сортировки, так как это должны быть включены в ссылки разбиения на страницы для сохранит порядок сортировки во время разбиения на страницы:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Другое свойство `ViewBag.CurrentFilter`, обеспечивает представление с текущей строки фильтра. Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы. Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных. Строка поиска, которая изменяется при нажатии кнопки "Отправить", если ввести значение в текстовом поле. В этом случае `searchString` параметра не равно null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

В конце метода `ToPagedList` метод расширения в слушателей `IQueryable` объект преобразует запрос студента на одной странице учеников в тип коллекции, который поддерживает разбиение по страницам. Страницы учащихся затем передается в представление:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Метод `ToPagedList` принимает номер страницы. Два знака вопроса указывают [оператор объединения с null](https://msdn.microsoft.com/library/ms173224.aspx). Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.

### <a name="add-paging-links-to-the-student-index-view"></a>Добавить разбиение на страницы ссылки на представление Index студента

В *Views\Student\Index.cshtml*, замените существующий код следующим кодом:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.

`using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательным методом MVC для кнопки разбиения по страницам.

В коде используется перегруженная версия [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) , позволяющее указать [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Значение по умолчанию [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) отправки данных с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запросов. При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки. [Рекомендации W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) укажите, должен использовать GET, если действие не приводит к появлению обновления.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки. Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Создание о страницы, отображающей студента статистики

Для Contoso университета веб-сайта о странице как отображать количество студентов регистрации для каждой даты регистрации. Для этого понадобится группировка и выполнение простых расчетов в группах. Для выполнения этой задачи нам потребуется следующее:

- Создать класс модели представления для данных, которые необходимо передать в представление.
- Изменить `About` метод в `Home` контроллера.
- Изменить `About` представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *ViewModels* папки. В этой папке, добавьте файл класса *EnrollmentDateGroup.cs* и замените существующий код следующим кодом:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Необязательно: Развертывание приложения в Windows Azure

Пока приложение работает локально в IIS Express на компьютере разработчика. Чтобы сделать его доступным для других пользователей в Интернете, необходимо развернуть его на веб-поставщик услуг размещения. В этом дополнительном разделе учебника вы развернете его для веб-сайта Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>С помощью Code First Migrations развертывание базы данных

Для развертывания базы данных потребуется использовать Code First Migrations. При создании профиля публикации, который используется для настройки параметров для развертывания из Visual Studio будет предложено выбрать флажок под названием **выполнять миграции Code First (запускается при запуске приложения)**. Этот параметр задан, то процесс развертывания, чтобы автоматически настроить приложение *Web.config* файлов на целевом сервере, чтобы Code First использует `MigrateDatabaseToLatestVersion` класс инициализатора.

Visual Studio не выполняет никаких действий с базой данных во время развертывания. Когда развернутое приложение обращается к базе данных в первый раз после развертывания, Code First автоматически создает базу данных или обновляет схему базы данных до последней версии. Если приложение поддерживает миграцию `Seed` метода, метод выполняется, после создания базы данных или обновления схемы.

Миграции `Seed` метод вставляет данные теста. При развертывании в рабочей среде, необходимо изменить `Seed` метода, так что он вставляет только те данные, необходимые для вставки в рабочей базы данных. Например в текущей модели данных можно иметь реальные курсов, но вымышленной студентов в базе данных разработки. Можно написать `Seed` метод для загрузки в разработке и закомментируйте вымышленной учащихся перед развертыванием в рабочей среде. Или можно написать `Seed` для загрузки только курсы и вводить вымышленные студентов в тестовую базу данных вручную с помощью пользовательского интерфейса приложения.

### <a name="get-a-windows-azure-account"></a>Получить учетную запись Windows Azure

Вам потребуется учетная запись Windows Azure. Если у вас еще нет один, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатная пробная версия Windows](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Создание веб-сайта и базы данных SQL в Windows Azure

Веб-сайте Windows Azure будет выполняться в общей среде размещения, это означает, что он работает на виртуальных машинах (ВМ), которые являются общими с другими клиентами Windows Azure. Способ недорогих в общей среде размещения приступить к работе в облаке. Более поздней версии увеличивается веб-трафика, удовлетворяющее потребностям, запустив на выделенных виртуальных машин можно масштабировать приложение. Если требуются более сложные архитектуры, можно перенести в облачную службу Windows Azure. Облачные службы запустите на выделенных виртуальных машин, которые можно настроить в соответствии с вашими потребностями.

База данных SQL Windows Azure является службой реляционных баз данных на основе облака, основанное на технологии SQL Server. Средств и приложений, работающих с SQL Server также работают с базой данных SQL.

1. В [портала управления Windows Azure](https://manage.windowsazure.com/), нажмите кнопку **веб-сайтов** в левой вкладке, а затем выберите **New**.

    ![Новая кнопка на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Нажмите кнопку **НАСТРАИВАЕМОЕ создание**.

    ![Создать ссылку на базу данных на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **Нового веб-сайта — настраиваемое создание** откроется мастер.
3. В **новый веб-сайт** шаг мастера введите строку в **URL-адрес** поле для использования в качестве уникальный URL-адрес для вашего приложения. Полный URL-адрес будет состоять из, вводимая здесь и суффикса, которое будет отображаться рядом с текстовым полем. На рисунке «ConU», но этот URL-адрес берется скорее всего, будет необходимо выбрать другой.

    ![Создать ссылку на базу данных на портале управления](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. В **область** раскрывающегося списка выберите регион, ближайший к вам. Этот параметр указывает каком веб-сайте будет выполняться в центре обработки данных.
5. В **базы данных** раскрывающегося списка выберите **создать бесплатную базу данных 20 МБ SQL**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. В **имя строки СОЕДИНЕНИЯ DB**, введите *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Щелкните стрелку, указывающую справа внизу окна. В мастере открывается **параметры базы данных** шаг.
8. В **имя** введите *ContosoUniversityDB*.
9. В **сервера** выберите **новой базы данных SQL server**. Кроме того Если вы ранее был создан сервер, этот сервер можно выбрать из раскрывающегося списка.
10. Введите администратор **имя входа** и **пароль**. Если вы выбрали **новой базы данных SQL server** не ввода существующих имя и пароль для этой учетной записи, вводите новое имя и пароль, который вы определяете теперь для последующего использования при доступе к базе данных. Если был выбран сервер, который вы создали ранее, будет введите учетные данные для этого сервера. В этом учебнике не будет выбран ***Дополнительно*** флажок. ***Дополнительно*** параметры можно использовать для настройки базы данных [сортировки](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Выбрать то же **область** , выбранное для веб-сайта.
12. Щелкните значок галочки в правом нижнем углу используется для указания, что по завершении вы сможете.   
  
    ![Шаг настройки базы данных для нового веб-сайта — создать с помощью мастера баз данных](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    На следующем рисунке, используя существующий SQL Server и имя входа.   
  
    ![Шаг настройки базы данных для нового веб-сайта — создать с помощью мастера баз данных](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    На портале управления возвращается на страницу веб-сайтов и **состояние** столбец показывает, что создается сайт. Через некоторое время (обычно меньше, чем через минуту) **состояние** столбец показывает, что узел был успешно создан. На панели навигации слева отображается число сайтов в вашей учетной записи имеется рядом с **веб-сайтов** значок и число баз данных отображается рядом с **баз данных SQL** значок.

## <a name="deploy-the-application-to-windows-azure"></a>Развертывание приложения в Windows Azure

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.  
  
    ![Публикация в контекстном меню проекта](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. В **профиль** вкладке **веб-публикация** мастера, нажмите кнопку **импорта**.  
  
    ![Параметры импорта публикации](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Если вы не добавили ранее подписки Windows Azure в Visual Studio, выполните следующие действия. В этом пошаговом руководстве, чтобы в раскрывающемся списке в группе, добавьте подписку **Импорт из веб-сайта Windows Azure** будет содержать веб-сайте.

    1. В **Импорт профиля публикации** диалоговое окно, нажмите кнопку **Импорт из веб-сайта Windows Azure**, а затем нажмите кнопку **подписки Windows Azure, добавить**.

    ![Добавление подписки Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    2. В **импорта подписки Windows Azure** диалоговое окно, нажмите кнопку **загрузки файла подписки**.

    ![загрузить файл подписки](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    В. В окне браузера, сохраните *.publishsettings* файла.

    ![Загрузите файл PUBLISHSETTINGS](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Безопасность — *publishsettings* файл содержит учетные данные (Незакодированные), используемые для управления подписками Windows Azure и служб. Рекомендациям по безопасности для этого файла требуется временно сохранить за пределами исходных каталогов (например в *Libraries\Documents* папки), а затем удалите его после завершения операции импорта. Злоумышленник, получивший доступ к `.publishsettings` файл можно изменять, создавать и удалять службы Windows Azure.

    Г. В **импорта подписки Windows Azure** диалоговое окно, нажмите кнопку **Обзор** и перейдите к *.publishsettings* файла.

    ![Загрузите sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    Д. Нажмите кнопку **Импортировать**.

    ![импорт](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. В **Импорт профиля публикации** выберите **Импорт из веб-сайта Windows Azure**, выберите веб-сайт из раскрывающегося списка и нажмите кнопку **ОК**.  
  
    ![Импорт профиля публикации](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. В **подключения** щелкните **проверить подключение** чтобы убедиться в том, что настройки указаны верно.  
  
    ![Проверка подключения](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. После проверки подключения отображается рядом с зеленой галочкой **проверить подключение** кнопки. Нажмите кнопку **Далее**.  
  
    ![Успешно проверенные соединения](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Откройте **строку подключения к удаленному** раскрывающегося списка в разделе **SchoolContext** и выберите строку подключения для базы данных, вы создали.
8. Выберите **выполнять миграции Code First (запускается при запуске приложения)**.
9. Снимите флажок **использовать эту строку подключения во время выполнения** для **UserContext (DefaultConnection)**, поскольку это приложение не использует базы данных членства.   
  
    ![Вкладка "Параметры"](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Нажмите кнопку **Далее**.
11. В **предварительного просмотра** щелкните **начать просмотр**.  
  
    ![StartPreview кнопки на вкладке «Предварительный просмотр»](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    Вкладка отображает список файлов, которые будут скопированы на сервер. Предварительного просмотра не требуется для публикации приложения, но полезна функция следует учитывать. В этом случае не нужно выполнять никаких действий со списком файлов, который отображается. При последующем развертывании этого приложения в этом списке будут только измененные файлы.  
  
    ![Выходной файл StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Нажмите кнопку **Опубликовать**.  
    Visual Studio начинает процесс копирования файлов на сервер Windows Azure.
13. **Вывода** окно показывает выполненные действия развертывания и сообщает успешного завершения развертывания.  
  
    ![Успешное развертывание модуля создания отчетов в окне вывода](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. После успешного развертывания браузер по умолчанию автоматически открывается на URL-адрес развернутой веб-сайта.  
    Созданное приложение теперь работает в облаке. Перейдите на вкладку учащихся.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

На этом этапе вашей *SchoolContext* база данных создана в базе данных SQL Windows Azure, так как вы выбрали **выполнять миграции Code First (запускается при запуске приложения)**. *Web.config* файл развернутого веб-сайте был изменен, чтобы [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) инициализатор будет запускать при первом кода считывает или записывает данные в базе данных (который произошло при выборе **учащихся** вкладка):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Процесс развертывания также создается новая строка подключения *(SchoolContext\_DatabasePublish*) для миграции Code First для обновления схемы базы данных и заполнения базы данных.

![Строка подключения Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* строка подключения предназначена для базы данных членства (которая не используются в этом учебнике). *SchoolContext* строка подключения предназначена для ContosoUniversity базы данных.

Можно найти в файл Web.config на локальном компьютере в развернутой версии *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Доступ к развернутой *Web.config* самом файле по протоколу FTP. Инструкции см. в разделе [ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления кода](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Следуйте инструкциям, которые начинаются с «Чтобы использовать средство FTP, необходимы три вещи: URL-адрес FTP, имя пользователя и пароль.»

> [!NOTE]
> Веб-приложение не реализует безопасности, поэтому любой пользователь находит URL-адрес можно изменить данные. Инструкции по защите веб-сайта см. в разделе [развертывание приложения безопасного ASP.NET MVC с членством, OAuth и базы данных SQL для веб-сайта Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Отключать других пользователей, с помощью сайта с помощью портала управления Windows Azure или **обозревателя серверов** в Visual Studio, чтобы остановить сайт.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Инициализаторы первого кода

В разделе «развертывание», вы видели [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) используется инициализатор. Код сначала также предоставляет другие инициализаторы, которые можно использовать, включая [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (по умолчанию), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) и [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Инициализатор может быть полезна при настройке условий для модульных тестов. Можно также написать собственную инициализаторы и инициализатор можно вызвать явным образом, если не нужно подождать, пока приложение для чтения и записи в базу данных. Подробное объяснение инициализаторы в главе 6 книги [программирования платформы Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Джули Лерман и Миллер Роуэном.

## <a name="summary"></a>Сводка

В этом учебнике вы знаете, как создать модель данных и реализации основных CRUD, сортировку, фильтрацию, разбиение по страницам и группировку функциональности. В следующем уроке вы начнете, просмотрев более сложные вопросы, развернув модели данных.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Вперед](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
