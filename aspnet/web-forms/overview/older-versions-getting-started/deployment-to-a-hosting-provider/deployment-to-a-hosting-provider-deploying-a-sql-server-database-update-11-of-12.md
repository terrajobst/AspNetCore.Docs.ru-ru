---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816636"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-сайтов Azure для Windows, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этом руководстве показано, как развернуть обновление базы данных для всей базы данных SQL Server. Поскольку Code First Migrations выполняет всю работу по обновления базы данных, процесс идентичен почти что вы сделали для SQL Server Compact, в [развертывание обновления базы данных](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) руководства.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Добавление нового столбца в таблицу

В этом разделе руководства вносятся изменения базы данных и соответствующие изменения кода, затем проверить их в Visual Studio при подготовке к их развертывание в тестовой и рабочей средах. Изменение включает в себя добавление `OfficeHours` столбец `Instructor` сущности и отображение на новые сведения **преподавателей** веб-страницы.

В проекте ContosoUniversity.DAL, откройте *Instructor.cs* и добавьте следующее свойство между `HireDate` и `Courses` свойства:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Обновите класс инициализатора, таким образом, чтобы он заполняет новый столбец с тестовыми данными. Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, включая новый столбец:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для часов office непосредственно перед закрывающим `</Columns>` тег в первом `GridView` управления:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Постройте решение.

Откройте **консоль диспетчера пакетов** окна, а также выберите ContosoUniversity.DAL как **проект по умолчанию**.

Введите следующие команды.

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Запустите приложение и выберите **преподавателей** страницы. На странице используется немного больше времени, чем обычно, чтобы загрузить, так как платформа Entity Framework повторно создает базу данных и заполняет ее тестовыми данными.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Развертывание обновления базы данных в тестовую среду

При использовании Code First Migrations, способ развертывания изменений базы данных в SQL Server такое же, как SQL Server Compact. Тем не менее, вам нужно изменить профиль публикации, так как он по-прежнему настроен для переноса из SQL Server Compact в SQL Server.

Первым шагом является удаление преобразования строки подключения, которые вы создали в предыдущем учебном курсе. Это больше не требуется, так как вы зададите преобразований строк подключения в профиле публикации, как это делалось, прежде чем вы настроили **упаковка и публикация SQL** tab для перехода на SQL Server.

Откройте *Web.Test.config* файл и удалите `connectionStrings` элемент. Единственное преобразование оставшиеся в *Web.Test.config* файл `Environment` значение в `appSettings` элемент.

Теперь можно обновить профиль публикации и опубликовать в тестовую среду.

Откройте **веб-публикации** мастера, а затем переключиться в режим **профиль** вкладки.

Выберите **теста** профиль публикации.

Выберите **параметры** вкладки.

Нажмите кнопку **разрешить улучшения публикации новой базы данных**.

В поле Строка подключения для **SchoolContext**, то же самое значение, выбранное на *Web.Test.config* файл преобразования в предыдущем руководстве:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Выберите **выполнить Code First Migrations (при запуске приложения)**. (В вашей версии Visual Studio, может быть флажок **применить Code First Migrations**.)

В поле Строка подключения для **DefaultConnection**, то же самое значение, выбранное на *Web.Test.config* файл преобразования в предыдущем руководстве:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Оставьте **обновления базы данных** очищен.

Нажмите кнопку **Опубликовать**.

Visual Studio развертывает изменения в коде в тестовую среду и открывает в браузере домашней страницей университета Contoso.

Выберите страницу преподавателей.

При выполнении приложения эту страницу, он пытается получить доступ к базе данных. Code First Migrations проверяет, является текущей базы данных и обнаруживает, что пока еще не примененные последней миграции. Code First Migrations применяется последней миграции, запускает `Seed` метод, а затем страница выполняется в обычном режиме. Появится новый столбец часов Office с помощью задания начальных значений данных.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Развертывание обновления базы данных в рабочей среде

Необходимо также изменить профиль публикации для рабочей среды. В этом случае вы удалите существующий и создайте новую, импортировав обновленную PUBLISHSETTINGS-файла. Обновленный файл будет включать строку подключения для базы данных SQL Server Cytanium.

Как можно было увидеть при развертывании в тестовую среду, вы больше не нужен преобразований строки подключения в *Web.Production.config* файл преобразования. Открыть этот файл и удалите `connectionStrings` элемент. Остальные преобразования предназначены для `Environment` значение в `appSettings` элемент и `location` элемент, который ограничивает доступ к отчетам об ошибках Elmah.

Перед созданием нового профиля публикации для рабочей среды, загрузить обновленные PUBLISHSETTINGS-файла так же, как ранее в [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) руководства. (В панели управления Cytanium щелкните **веб-сайтов**, а затем нажмите кнопку **contosouniversity.com** веб-сайта. Выберите **веб-публикаций** , а затем щелкните **загрузить профиль публикации веб-узла**.) Причина, по которой вы выполняете действия — Выбор строку подключения базы данных в файле PUBLISHSETTINGS. Первый раз, загруженный файл, так как используется SQL Server Compact и еще не создана база данных SQL Server в Cytanium еще был недоступен в строке подключения.

Теперь можно обновить профиль публикации и публикации в рабочей среде.

Откройте **веб-публикации** мастера, а затем переключиться в режим **профиль** вкладки.

Нажмите кнопку **Управление профилями**и удалите профиль производства.

Закрыть **веб-публикации** мастер должен сохранить это изменение.

Откройте **веб-публикации** мастер еще раз, а затем нажмите кнопку **импорта**.

На **подключения** вкладке **URL-адрес назначения** с соответствующим значением, если вы используете временный URL-адрес.

Нажмите кнопку **Далее**.

На **параметры** щелкните **разрешить улучшения публикации новой базы данных**.

В раскрывающемся списке строку подключения для **SchoolContext**, выберите строку подключения Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Выберите **выполнить Code First migrations (при запуске приложения)**.

В раскрывающемся списке строку подключения для **DefaultConnection**, выберите строку подключения Cytanium.

Выберите **профиль** щелкните **Управление профилями**и переименуйте профиль из «contosouniversity.com - веб-развертывание» в «Production».

Закройте профиль публикации, чтобы сохранить изменения, а затем открыть его снова.

Нажмите кнопку **Опубликовать**. (Для реальной рабочей веб-сайта, необходимо скопировать *приложения\_offline.htm* в рабочей среде и поместить его в папку проекта, перед публикацией, удалите его после завершения развертывания.)

Visual Studio развертывает изменения в коде в тестовую среду и открывает в браузере домашней страницей университета Contoso.

Выберите страницу преподавателей.

Code First Migrations обновляет базу данных так же, как и в тестовой среде. Появится новый столбец часов Office с помощью задания начальных значений данных.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Вы успешно развернули обновления приложения, включены изменения базы данных, база данных SQL Server.

## <a name="more-information"></a>Дополнительные сведения

На этом завершается этой серии руководств по развертыванию веб-приложению ASP.NET для стороннего поставщика услуг размещения. Дополнительные сведения о всех подразделах, рассматриваемых в этих учебниках, см. в разделе [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) веб-сайте MSDN.

## <a name="acknowledgements"></a>Благодарности

Я бы хотел поблагодарить следующих специалистов, которые разработали вклад в содержимое этой серии руководств:

- [Альберто Poblacion, обладатель звания MVP &amp; MCT, Испания](https://mvp.support.microsoft.com/profile/Alberto)
- Фергюсон Jarod, MVP разработки платформы данных, США
- Harsh Миттал, Microsoft
- [Олсон Kristina, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Майк эти протоколы, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Мохи Srivastava, Microsoft
- [Raffaele Rialdi, Италия](http://www.iamraf.net/)
- [Рик Андерсон, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Саид Хашими, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Скотт Хантер, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Сербия](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
