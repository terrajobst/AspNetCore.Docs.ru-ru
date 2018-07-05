---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382463"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом руководстве внесенные изменения базы данных и изменения кода, тестирования изменений в Visual Studio, а затем развернуть обновление в рабочей и тестовой среды.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Добавление нового столбца в таблицу

В этом разделе, добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущностей. Затем вы обновите со страницей, отображающей данные instructor, чтобы он отображал нового столбца.

В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класс (должно быть два закрывающей фигурных скобок после нее):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Затем обновите метод заполнения таким образом, чтобы он предоставлял значение нового столбца. Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, который содержит информацию о дате рождения:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для отображения даты рождения. Между для них назначения даты и office приема на работу, добавьте ее.

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Если отступов кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL-D для автоматически переформатирует файл.)

Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна. Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проект по умолчанию**.

В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проект по умолчанию**, а затем введите следующую команду:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода вы увидите код, создающий новый столбец.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Выполните сборку решения, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что проект ContosoUniversity.DAL по-прежнему выбран):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

После завершения выполнения команды, запустите приложение и выберите страницу преподавателей. При загрузке страницы, вы увидите, что у него есть новое поле даты рождения.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Развертывание обновления базы данных в тестовую среду

В **обозревателе решений** выберите проект ContosoUniversity.

В **веб-публикация одним щелкните** панели инструментов, выберите **теста** профиль публикации, а затем нажмите кнопку **веб-публикации**. (Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)

Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница. Откройте страницу преподавателей, чтобы убедиться, что обновление было успешно развернуто. Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод. Когда отобразится страница, вы увидите ожидаемый **Дата рождения** столбец с датами в нем.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Развертывание обновления базы данных в рабочей среде

Теперь можно развернуть в рабочей среде. Единственное отличие заключается в том, что вы используете *приложения\_offline.htm* чтобы запретить пользователям доступ к сайту и обновление базы данных таким образом, при развертывании изменений. Для развертывания в рабочей среде выполните следующие действия:

- Отправка *приложения\_offline.htm* файл к рабочему сайту.
- В Visual Studio, выберите профиль производства в **веб-публикация одним щелкните** панели инструментов и щелкните **веб-публикации**.
- Удалить *приложения\_offline.htm* файл с рабочего сайта.

> [!NOTE]
> Когда приложение используется в рабочей среде следует реализация плана резервного копирования. То есть вы должны периодически скопировать *School-Prod.sdf* и *aspnet Prod.sdf* файлы с рабочего сайта в место безопасного хранения, и должен сохранение нескольких поколений ОС, таких резервные копии. При обновлении базы данных, следует создайте резервную копию из непосредственно перед изменением. Затем Если вы сделаете ошибку и не обнаружит его до, после его развертывания в рабочей среде, по-прежнему можно восстановить базу данных к состоянию, в котором он находился перед его был поврежден.


Когда Visual Studio открывает URL-адрес домашней страницы в браузере, *приложения\_offline.htm* откроется страница. После удаления *приложения\_offline.htm* файл, можно перейти к домашней странице еще раз, чтобы проверить, что обновление успешно развернут.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Теперь вы развернули обновления приложения, включены изменения базы данных для тестирования и производственная среда. Следующее руководство показано, как перенести базу данных из SQL Server Compact в SQL Server Express и SQL Server.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
