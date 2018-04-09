---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11, 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как развертывание веб-сайтов Azure для Windows, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этот учебник содержит описание развертывания обновления базы данных для всей базы данных SQL Server. Поскольку Code First Migrations выполняет всю работу по обновление базы данных, процесс идентичен тому, что вы делали для SQL Server Compact в [развертывание обновления базы данных](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) учебника.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Добавление нового столбца в таблицу

В этом разделе учебника будет вносить изменения базы данных и соответствующих изменений в код, затем проверить их в Visual Studio в процессе подготовки для развертывания их в тестовой и рабочей сред. Это изменение включает в себя добавление `OfficeHours` столбец `Instructor` сущности и отображение на новые сведения **инструкторов** веб-страницы.

В проекте ContosoUniversity.DAL откройте *Instructor.cs* и добавьте следующее свойство между `HireDate` и `Courses` свойства:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Обновление инициализатора класса, чтобы он заполняет новый столбец с тестовыми данными. Откройте *Migrations\Configuration.cs* и заменить блок кода, который начинается `var instructors = new List<Instructor>` с следующий блок кода, который включает в себя новый столбец:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

В проекте ContosoUniversity откройте *Instructors.aspx* и добавить новое поле шаблона для часов office непосредственно перед закрывающим тегом `</Columns>` тег в первом `GridView` управления:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Постройте решение.

Откройте **консоль диспетчера пакетов** окна и выберите ContosoUniversity.DAL как **проекта по умолчанию**.

Введите следующие команды:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Запустите приложение и выберите **инструкторов** страницы. Страницы нужно немного больше времени, чем обычно, для загрузки, так как платформа Entity Framework повторно создает базу данных и заполняет его с тестовыми данными.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Развертывание обновления базы данных в тестовой среде

При использовании Code First Migrations метод для развертывания изменений базы данных SQL Server является так же, как SQL Server Compact. Тем не менее, необходимо изменить тест профиль публикации, так как он по-прежнему настройки для миграции из SQL Server Compact в SQL Server.

Первым шагом является удаление преобразования строки подключения, созданных в предыдущем учебнике. Это больше не требуется так, как вы зададите преобразования строки подключения в профиль публикации, как это было сделано, прежде чем вы настроили **упаковка и публикация SQL** tab для перехода на SQL Server.

Откройте *Web.Test.config* файл и удалите `connectionStrings` элемента. Только оставшиеся преобразования в *Web.Test.config* файл `Environment` значение в `appSettings` элемент.

Теперь можно обновить профиль публикации и публикации в тестовую среду.

Откройте **веб-публикация** мастера, а затем переключиться **профиль** вкладки.

Выберите **тест** профиль публикации.

Выберите **параметры** вкладки.

Нажмите кнопку **включить новой базы данных публикации усовершенствования**.

В поле Строка соединения для **SchoolContext**, введите значение, которое используется в *Web.Test.config* файл преобразования в предыдущем учебнике:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Выберите **выполнять миграции Code First (запускается при запуске приложения)**. (В версии Visual Studio, может быть флажок **применить Code First Migrations**.)

В поле Строка соединения для **DefaultConnection**, введите значение, которое используется в *Web.Test.config* файл преобразования в предыдущем учебнике:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Оставить **обновление базы данных** очищен.

Нажмите кнопку **Опубликовать**.

Visual Studio развертывает изменения кода в среде тестирования и открывает в браузере домашнюю страницу Contoso университета.

Выберите страницу инструкторов.

При запуске приложения, эту страницу, она попытается получить доступ к базе данных. Code First Migrations проверяет, является текущей базы данных и обнаруживает, что последний переноса имеет еще не было применено. Code First Migrations применяется последняя миграции, выполняется `Seed` метода, а страница выполняется в обычном режиме. Появится новый столбец время работы с данными задания начальных значений.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Развертывание обновления базы данных в рабочей среде

Необходимо также изменить профиль публикации для производственной среды. В этом случае вы удалите существующий и создайте новое путем импорта обновленных PUBLISHSETTINGS-файл. Обновленный файл будет включать строку подключения для базы данных SQL Server на Cytanium.

Как видно при развертывании в тестовую среду, ненужный преобразований строки подключения в *Web.Production.config* файл преобразования. Открыть этот файл и удалите `connectionStrings` элемента. Остальные преобразования предназначены для `Environment` значение в `appSettings` элемент и `location` элемент, который ограничивает доступ к отчетам об ошибках Elmah.

Прежде чем создавать новый профиль публикации для рабочей среды, загрузить обновленные PUBLISHSETTINGS-файл так же, как это было сделано ранее в [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) учебника. (В панели управления Cytanium щелкните **веб-сайтов**и нажмите кнопку **contosouniversity.com** веб-сайта. Выберите **веб-публикаций** , а затем щелкните **загрузить профиль публикации веб-узла**.) Причина, по которой вы выполняете — выбирает строку подключения базы данных в файле PUBLISHSETTINGS. Загруженный файл, так как используется SQL Server Compact и еще не создана база данных SQL Server на Cytanium еще впервые был недоступен в строке подключения.

Теперь можно обновить профиль публикации и публикации в рабочей среде.

Откройте **веб-публикация** мастера, а затем переключиться **профиль** вкладки.

Нажмите кнопку **Управление профилями**и затем удалить профиль производства.

Закрыть **веб-публикация** мастера, чтобы сохранить эти изменения.

Откройте **веб-публикация** мастер еще раз и нажмите кнопку **импорта**.

На **подключения** измените **URL-адрес назначения** соответствующее значение, если вы используете временные URL-адрес.

Нажмите кнопку **Далее**.

На **параметры** щелкните **включить новой базы данных публикации усовершенствования**.

В раскрывающемся списке строку подключения для **SchoolContext**, выберите строку подключения Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Выберите **миграции выполните Code First (запускается при запуске приложения)**.

В раскрывающемся списке строку подключения для **DefaultConnection**, выберите строку подключения Cytanium.

Выберите **профиль** щелкните **Управление профилями**и переименуйте профиля из «contosouniversity.com - веб-развертывание» в «Production».

Профиль публикации, чтобы сохранить изменения, закройте и снова откройте его.

Нажмите кнопку **Опубликовать**. (Для рабочей веб-сайта, необходимо скопировать *приложения\_offline.htm* для производства и поместить его в папку проекта перед публикацией, затем удалите его после завершения развертывания.)

Visual Studio развертывает изменения кода в среде тестирования и открывает в браузере домашнюю страницу Contoso университета.

Выберите страницу инструкторов.

Code First Migrations обновляет базу данных так же, как и в тестовой среде. Появится новый столбец время работы с данными задания начальных значений.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Успешно развернут обновления приложения, включенные изменения базы данных, база данных SQL Server.

## <a name="more-information"></a>Дополнительные сведения

На этом завершается, серию учебников по развертыванию веб-приложению ASP.NET для стороннего поставщика услуг размещения. Дополнительные сведения об этих подразделах, рассматриваемых в этих учебниках см. в разделе [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) на веб-сайте MSDN.

## <a name="acknowledgements"></a>Благодарности

Я хочу поблагодарить следующих людям, которые вносили свой вклад значительные к содержимому этого учебника ряда:

- [Alberto Poblacion, MVP &amp; MCT, Испания](https://mvp.support.microsoft.com/profile/Alberto)
- Фергюсон Jarod, MVP разработки платформы данных, Соединенных Штатов
- Резкого Mittal, корпорация Майкрософт
- [Олсон Kristina, корпорация Майкрософт](https://blogs.iis.net/krolson/default.aspx)
- [Эти протоколы Майк, корпорация Майкрософт](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Шривастав Mohit, корпорация Майкрософт
- [Raffaele Rialdi, Италия](http://www.iamraf.net/)
- [Рик Андерсон, корпорация Майкрософт](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, корпорация Майкрософт](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Скотт Хантер, корпорация Майкрософт](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Сербия](http://msforge.net/blogs/zmajcek/)
- [Вишал Joshi, корпорация Майкрософт](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
