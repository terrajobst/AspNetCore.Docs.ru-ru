---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async и хранимых процедур с платформой Entity Framework в приложении ASP.NET MVC | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84cf427c7da7905444568ac34534e9ed98a7d8c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async и хранимых процедур с платформой Entity Framework в приложении ASP.NET MVC
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущих учебниках вы узнали, как для чтения и обновления данных с использованием синхронной модели программирования. В этом учебнике вы научитесь реализации асинхронной модели программирования. Асинхронного кода могут помочь приложения работают лучше, поскольку это повышает эффективность использования ресурсов сервера.

В этом учебнике вы также увидите способ использования хранимых процедур для операций вставки, обновления и delete для сущности.

Наконец будет повторно развернуть приложение в Azure, а также все изменения базы данных, которые реализованы с момента при первом развертывании.

На следующих рисунках изображены некоторые из страниц, с которыми вы будете работать.

![Страница отделов](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Создание подразделения](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Зачем с асинхронного кода

Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки. В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки. В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода. В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов. В результате асинхронного кода включает ресурсы сервера для более эффективного использования и что сервер включен для обработки большего объема трафика без задержек.

В более ранних версиях .NET, создания и тестирования асинхронного кода была сложной задачей, вероятностью ошибок и трудно отлаживать. В .NET 4.5, записи, тестирования и отладки асинхронный код таким образом намного проще, следует обычно написать асинхронный код, если нет причины недоступны для. Асинхронного кода привести небольшого количества временных затрат, но в ситуациях сниженным трафиком, снижение производительности будет незначительным, при в ситуациях с высоким трафиком, является существенным потенциальное улучшение производительности.

Дополнительные сведения об асинхронном программировании см. в разделе [Поддержка асинхронного использования .NET 4.5 во избежание блокирующих вызовов](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Создание контроллера отдела

Создание подразделения контроллера, так же, как это было сделано ранее контроллеров, но в этот раз выберите **Использование асинхронного контроллера** действия флажок.

![Представление формирования отдел контроллера](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Следующие основные особенности Показать, что был добавлен на синхронный код для `Index` метод создания асинхронного:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Четыре изменения были применены к включить запрос к базе данных Entity Framework для асинхронного выполнения:

- Метод помечен атрибутом `async` ключевое слово, которое указывает компилятору создавать обратные вызовы для части тела метода и автоматически создавать `Task<ActionResult>` возвращаемый объект.
- Возвращаемый тип был изменен с `ActionResult` для `Task<ActionResult>`. `Task<T>` Тип представляет выполняющуюся работу с результат типа `T`.
- `await` Была применена ключевое слово для вызова веб-службы. Если компилятор обнаруживает ключевое слово, за кулисами он разбивает метод на две части. Первая часть завершается с операцией, которая запускается в асинхронном режиме. Вторая часть переводится в метод обратного вызова, вызываемый после завершения операции.
- Асинхронная версия объекта `ToList` метод расширения был вызван.

Почему является `departments.ToList` инструкции изменен, но не `departments = db.Departments` инструкции? Причина заключается в асинхронное выполнение только инструкции, вызывающие запросов или команд отправки в базу данных. `departments = db.Departments` Устанавливает инструкции запроса, но запрос не выполняется до `ToList` вызывается метод. Таким образом только `ToList` метод выполняется асинхронно.

В `Details` метод и `HttpGet` `Edit` и `Delete` методы, `Find` метод является тот, который вызывает запрос для отправки в базу данных, то есть метод, который получает асинхронное выполнение:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

В `Create`, `HttpPost Edit`, и `DeleteConfirmed` это методы, `SaveChanges` вызов метода, который вызывает команду для выполнения, не таких инструкциях, как `db.Departments.Add(department)` только благодаря чему сущностей в памяти, чтобы изменить.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Откройте *Views\Department\Index.cshtml*и замените код шаблона с помощью следующего кода:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Этот код изменяет заголовок из индекса по отделам, перемещает имя администратора справа и предоставляет полное имя администратора.

Создания, удаления, сведения и изменения представлений, изменить заголовок для `InstructorID` поля «Administrator» так же, как изменить поля названия отдела «Отдел» в представлениях курса.

Создание и изменение представления используйте следующий код:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

В Delete и сведения о представлениях используйте следующий код:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Запустите приложение и нажмите кнопку **отделы** вкладки.

![Страница отделов](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Все, что работает так же, как и другие контроллеры, но в этом контроллере все запросы SQL выполняются асинхронно.

Некоторые аспекты, которые следует учитывать при использовании асинхронного программирования с платформой Entity Framework.

- Асинхронного кода не является потокобезопасным. Другими словами другими словами, не пытайтесь выполнить несколько операций в параллельном режиме, используя один и тот же экземпляр контекста.
- Если вы хотите использовать преимущества в производительности, которые обеспечивает асинхронный код, убедитесь, что все используемые пакеты библиотек (например, для разбиения на страницы) также используют асинхронный код при вызове любых методов Entity Framework, выполняющих запросы к базе данных.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Использование хранимых процедур для вставки, обновления и удаления

Некоторые разработчики и Администраторы баз данных предпочитают использовать хранимые процедуры для доступа к базе данных. Данные, с помощью хранимой процедуры, можно получить в более ранних версиях платформы Entity Framework [выполнения необработанных запросов SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), но нельзя указать EF с помощью хранимых процедур для операций обновления. EF 6 можно легко настроить Code First с помощью хранимых процедур.

1. В *DAL\SchoolContext.cs*, добавьте выделенный код для `OnModelCreating` метод.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Этот код дает Entity Framework, чтобы использовать сохраненные процедуры для вставки, обновления и удаления операций на `Department` сущности.
2. В консоли управления пакета введите следующую команду:

    `add-migration DepartmentSP`

    Откройте *миграций\&lt; отметка времени&gt;\_DepartmentSP.cs* кода в `Up` метод, который создает вставки, обновления и удаления хранимых процедур:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. В консоли управления пакета введите следующую команду:

     `update-database`
4. Запустите приложение в режиме отладки, нажмите кнопку **отделы** , а затем щелкните **создать новый**.
5. Введите данные для нового отдела и нажмите кнопку **создать**.

     ![Создание подразделения](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. В Visual Studio, просмотрите журналы в **вывода** окна, чтобы увидеть, что хранимая процедура использовался для вставки новой строки отдела.

     ![Отдел Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Код сначала создает имена по умолчанию хранимых процедур. При использовании существующей базы данных, может потребоваться настроить имен хранимых процедур для использования хранимых процедур в базе данных уже определен. Сведения о том, как это сделать, см. в разделе [Entity Framework код первого вставки/обновления/удаления хранимых процедур](https://msdn.microsoft.com/data/dn468673).

Если вы хотите настроить, что созданный хранимых процедур не, можно редактировать формирования шаблонов код для миграций `Up` метод, который создает хранимую процедуру. В этом случае изменения будут отражены каждый раз, когда, выполняется миграция и будут применяться в производственной базе данных, миграция выполняется автоматически в производственной среде после развертывания.

Чтобы изменить существующую хранимую процедуру, которая была создана в предыдущей миграции можно использовать для создания пустого миграции команды Add-Migration и вручную написать код, вызывающий [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) метод .

## <a name="deploy-to-azure"></a>Развертывание в Azure

В этом разделе необходимо выполнить дополнительный **развертыванию приложения в Azure** статьи [миграция и развертывание](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника этой серии. Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот раздел.

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.
2. Нажмите кнопку **Опубликовать**.

    Visual Studio развертывает приложение в Azure, и это приложение открывается в браузере по умолчанию, работающих в Azure.
3. Протестируйте приложение, чтобы проверить его работы.

    При первом запуске страницы, который обращается к базе данных, платформа Entity Framework запускает все миграций `Up` методы, необходимые для перевода базы данных в актуальном состоянии с текущей моделью данных. Теперь можно использовать все веб-страниц, добавленных с момента последнего развертывания, включая страницы отдела, которые вы добавили в этом учебнике.

## <a name="summary"></a>Сводка

В этом учебнике показано, как повысить эффективность работы сервера путем написания кода, которое выполняется асинхронно и как использовать хранимые процедуры для вставки, обновления и удаления операций. В следующем уроке вы увидите, как предотвратить потерю данных, когда несколько пользователей одновременно изменять одну запись одновременно.

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
