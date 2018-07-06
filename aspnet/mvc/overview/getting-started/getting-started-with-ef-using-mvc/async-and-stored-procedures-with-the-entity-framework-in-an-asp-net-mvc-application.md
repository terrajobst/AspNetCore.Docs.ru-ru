---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Асинхронные и хранимые процедуры с помощью Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 211c9c8c66f3eed15121b4dd425fa728c39b7de3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802053"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Асинхронные и хранимые процедуры с помощью Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущих учебниках вы узнали, как для чтения и обновления данных с помощью синхронной модели программирования. В данном учебном курсе реализации асинхронной модели программирования. Асинхронный код может помочь приложения работают лучше, так как он повышает эффективность использования ресурсов сервера.

В этом руководстве вы также узнаете, как использовать хранимые процедуры для вставки, обновления и удаления сущности.

Наконец будет повторно развернуть приложение в Azure, а также все изменения базы данных, реализованные с момента при первом развертывании.

На следующих рисунках изображены некоторые из страниц, с которыми вы будете работать.

![Страница отделов](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Создание подразделения](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>А зачем возиться с асинхронного кода

Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки. В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки. В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода. В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов. Таким образом асинхронный код позволяет ресурсы сервера для более эффективного использования, а на сервере включено обрабатывать больше трафика без задержек.

В более ранних версиях .NET, написании и тестировании асинхронного кода была сложной задачей, ошибкам, поэтому сложно отлаживать. В .NET 4.5 гораздо проще, что следует обычно писать асинхронный код при отсутствии причины не написания, тестирования и отладки асинхронного кода. Асинхронный код вводят немного больше служебных ресурсов, но ситуациях низким трафиком, снижение производительности пренебречь большого объема трафика, является существенным потенциальное улучшение производительности.

Дополнительные сведения об асинхронном программировании см. в разделе [Поддержка асинхронного использования .NET 4.5 во избежание блокирующих вызовов](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Создание контроллера Department

Создание контроллера Department, так же, как более ранних контроллеров, но в этом случае выберите **Использование асинхронного контроллера** действия флажок.

![Каркас контроллера Department](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ниже кратко описан Показать, что был добавлен на синхронный код `Index` для того, асинхронные:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Четыре изменения были применены для включения асинхронного запроса к базе данных Entity Framework:

- Метод помечен атрибутом `async` ключевое слово, которое указывает компилятору создавать обратные вызовы для частей тела метода и автоматически создавать `Task<ActionResult>` возвращаемый объект.
- Возвращаемый тип был изменен с `ActionResult` для `Task<ActionResult>`. `Task<T>` Тип представляет текущую операцию с помощью результата типа `T`.
- `await` Ключевое слово была применена для вызова веб-службы. Когда компилятор обнаруживает ключевое слово, за кулисами он разделяет метод на две части. Первая часть завершается операцией, который запускается в асинхронном режиме. Вторая часть помещается в метод обратного вызова, вызываемый при завершении операции.
- Асинхронная версия `ToList` был вызван метод расширения.

Почему является `departments.ToList` инструкции изменения, но не `departments = db.Departments` инструкции? Причина в том, что только инструкции, вызывающие запросы или команды, отправляемые в базу данных будут выполняться асинхронно. `departments = db.Departments` Устанавливает инструкцию запроса, но запрос не выполняется до `ToList` вызывается метод. Таким образом только `ToList` метод выполняется асинхронно.

В `Details` метод и `HttpGet` `Edit` и `Delete` методы, `Find` метод является тот, который завершает запрос, отправляемые в базу данных, это и есть метод, который выполняется асинхронно:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

В `Create`, `HttpPost Edit`, и `DeleteConfirmed` это методы, `SaveChanges` вызов метода, который вызывает команду для выполнения, не операторы, например `db.Departments.Add(department)` чему сущности только в памяти, чтобы изменить.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Откройте *Views\Department\Index.cshtml*и замените код шаблона следующим кодом:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Этот код изменяет заголовок из индекса в отделах, перемещает имя администратора справа и предоставляет полное имя администратора.

В создания, удаления, просмотра сведений и изменение представлений, изменить заголовок для `InstructorID` поле «Администратор» так же, как вы изменили поле имя отдела «Отдел» в представлениях курса.

В Create и Edit представления используйте следующий код:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

В представлениях Delete и Details используйте следующий код:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Запустите приложение и нажмите кнопку **отделов** вкладки.

![Страница отделов](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Все работает так же, как и в других контроллеров, но в этом контроллере все SQL-запросы выполняются асинхронно.

Некоторые моменты, которые следует учитывать при использовании асинхронного программирования с Entity Framework:

- Асинхронный код не является потокобезопасным. Другими словами другими словами, не следует пытаться выполнять несколько операций параллельно, используя один и тот же экземпляр контекста.
- Если вы хотите использовать преимущества в производительности, которые обеспечивает асинхронный код, убедитесь, что все используемые пакеты библиотек (например, для разбиения на страницы) также используют асинхронный код при вызове любых методов Entity Framework, выполняющих запросы к базе данных.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Использование хранимых процедур для вставки, обновления и удаления

Некоторые разработчики и Администраторы баз данных предпочитает использовать хранимые процедуры для доступа к базе данных. В более ранних версиях Entity Framework можно извлекать данные с помощью хранимой процедуры с [выполнение необработанный SQL-запрос](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), но нельзя указать EF использование хранимых процедур для операций обновления. В EF 6 можно легко настроить Code First с помощью хранимых процедур.

1. В *DAL\SchoolContext.cs*, добавьте выделенный код, который `OnModelCreating` метод.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Этот код указывает, что платформа Entity Framework для использования хранимых процедур для вставки, обновления и удаления операций на `Department` сущности.
2. В консоли управления пакета введите следующую команду:

    `add-migration DepartmentSP`

    Откройте *миграций\&lt; метка времени&gt;\_DepartmentSP.cs* код в `Up` метод, который создает вставки, обновления и удаления хранимых процедур:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. В консоли управления пакета введите следующую команду:

     `update-database`
4. Запустите приложение в режиме отладки, нажмите кнопку **отделов** , а затем щелкните **Create New**.
5. Введите данные для нового отдела, а затем нажмите кнопку **создать**.

     ![Создание подразделения](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. В Visual Studio, просмотрите журналы в **вывода** окно, чтобы увидеть, что хранимая процедура использовалась для вставки новой строки отдела.

     ![Отдел Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Код сначала создает имена по умолчанию хранимых процедур. Если вы используете существующую базу данных, может потребоваться настроить имен хранимых процедур, чтобы использовать хранимые процедуры, уже определенные в базе данных. Сведения о том, как это сделать, см. в разделе [Entity Framework код первого Insert/Update/Delete хранимые процедуры](https://msdn.microsoft.com/data/dn468673).

Если вы хотите настроить, что созданные хранимых процедур, можно изменить сформированный код для миграций `Up` метод, который создает хранимую процедуру. Таким образом изменения будут отражены каждый раз, когда что миграции запускается и будет применяться в производственной базе данных, при миграции автоматически выполняется в рабочей среде после развертывания.

Если вы хотите изменить существующую хранимую процедуру, которая была создана в предыдущей миграции, используйте команду Add-Migration, чтобы создать пустой миграции и вручную написать код, вызывающий [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) метод .

## <a name="deploy-to-azure"></a>Развертывание в Azure

В этом разделе, необходимо завершить необязательный **развертыванию приложения в Azure** статьи [миграции и развертывания](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) руководств этой серии. Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот раздел.

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.
2. Нажмите кнопку **Опубликовать**.

    Visual Studio развертывает приложение в Azure, и приложение откроется в браузере по умолчанию, работающих в Azure.
3. Протестируйте приложение, чтобы проверить его работы.

    При первом запуске страницы, который обращается к базе данных, платформа Entity Framework выполняет все миграции `Up` методы, необходимые для приведения базы данных в актуальном состоянии с текущей моделью данных. Теперь можно использовать все веб-страниц, добавленных с момента последнего развертывания, включая страницы подразделения, добавленные в этом руководстве.

## <a name="summary"></a>Сводка

В этом учебнике вы видели, как повысить эффективность работы сервера путем написания кода, которое выполняется асинхронно и как использовать хранимые процедуры для вставки, обновления и удаления. В следующем учебном курсе будет показано, как для предотвращения потери данных, когда несколько пользователей попытаются изменять одну запись одновременно.

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
