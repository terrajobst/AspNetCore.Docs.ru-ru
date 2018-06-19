---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884890"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом учебнике внести изменение базы данных или изменение связанного кода, тестирования изменений в Visual Studio, а затем развернуть обновление для тестирования и в производственной среде.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Добавление нового столбца в таблицу

В этом разделе добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущности. Обновите страницу, отображающую инструктора данных, чтобы он отображал нового столбца.

В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класса (должно быть две фигурные скобки после его закрытия):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Затем обновите Seed-метод, чтобы он предоставляет значение для нового столбца. Откройте *Migrations\Configuration.cs* и заменить блок кода, который начинается `var instructors = new List<Instructor>` с следующий блок кода, который содержит информацию о дате рождения:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

В проекте ContosoUniversity откройте *Instructors.aspx* и добавить новое поле шаблон для отображения даты рождения. Добавьте его от тех, для назначения найма даты и office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Если отступа кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL + D автоматическое переформатирование файл.)

Постройте решение, а затем откройте **консоль диспетчера пакетов** окна. Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проекта по умолчанию**.

В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проекта по умолчанию**, а затем введите следующую команду:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода можно просмотреть код, который создает новый столбец.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Постройте решение, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что по-прежнему выбран проект ContosoUniversity.DAL):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

После завершения команды, запустите приложение и выберите страницу инструкторов. При загрузке страницы, вы видите, что она имеет новый поля «Дата рождения».

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Развертывание обновления базы данных в тестовой среде

В **обозревателе решений** выберите ContosoUniversity проект.

В **веб-публикация одним щелкните** панели инструментов выберите **тест** профиль публикации, а затем нажмите кнопку **веб-публикация**. (Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)

Visual Studio развертывает обновленное приложение, и в браузере будет открыта на домашнюю страницу. Запустите страницу инструкторов, чтобы убедиться, что обновление было успешно развернуто. Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод. При отображении страницы, вы видите ожидаемых **Дата рождения** столбец с датами в нем.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Развертывание обновления базы данных в рабочей среде

Теперь можно развернуть в рабочей среде. Единственное отличие заключается в том, что будет использоваться *приложения\_offline.htm* для запрещения доступа к узлу и таким образом обновляет базу данных при развертывании изменений. Для рабочего развертывания выполните следующие действия:

- Отправка *приложения\_offline.htm* файла рабочего сайта.
- В Visual Studio, выберите профиль на производственных **веб-публикация одним щелкните** инструментов и выберите команду **веб-публикация**.
- Удалить *приложения\_offline.htm* файла из рабочего сайта.

> [!NOTE]
> Приложение во время использования в рабочей среде следует реализация плана резервного копирования. То есть, вы должны периодическое копирование *School-Prod.sdf* и *aspnet Prod.sdf* файлы с рабочего сайта безопасное хранилище, а должен указывать нескольких поколений таких Создание резервных копий. При обновлении базы данных, следует сделать резервную копию из непосредственно перед изменением. Затем Если допущена ошибка и не обнаружит его до, после его развертывания в рабочей среде, по-прежнему можно восстановление базы данных в состояние, в котором она находилась до его был поврежден.


Когда Visual Studio открывает URL-адрес домашней страницы в браузере, *приложения\_offline.htm* -страница. После удаления *приложения\_offline.htm* файл, можно перейти на домашнюю страницу еще раз, чтобы проверить успешность развертывания обновления.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Теперь, после развертывания обновления, включенные изменения базы данных для рабочего и тестового приложения. Далее учебнике показано, как перенести базу данных из SQL Server Compact в SQL Server Express и SQL Server.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
